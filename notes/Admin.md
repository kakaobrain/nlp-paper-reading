# Admin Initialization
## What should we do to train Transformer well?
- 학습시에 Post LN 보다 Pre LN 구조가 더 안정적이다.
- 그러나 Pre LN 구조는 Residual Branch에 대한 의존성이 떨어져서 모델의 잠재력을 충분히 살리지 못한다.
- 두 방식의 장점을 모두 취하는 Admin이라는 Initialization 기법을 이용하여 Translation 분야에서 SoTA를 기록했다.
- 참고로 아래 Admin 논문의 1저자인 Liu는 RAdam의 1저자이며 Microsoft 소속 연구원이라고 카더라...

<br>

## 1. On Layer Normalization in the Transformer Architecture
- Xiong et al.
- [arXiv](https://arxiv.org/abs/2002.04745)

### Layer Normalization
![image](https://user-images.githubusercontent.com/38183241/103609599-882cee00-4f61-11eb-81ea-49a989c3f9a6.png)
- Transformer 학습의 핵심 요소인 Layer Normalization
- 기본적으로 Batch Normalization과 비슷히지만, 적용되는 Dimension이 다름.
- 시퀀스 처리에 보다 적합한 Normalization 기법임.

### Post LN vs Pre LN
- 일반적으로 우리가 알고 있는 Transformer는 Post LN (Multi-head Attention과 Skip Connection 이후에 Normalization을 진행)의 형태를 띔.
- 이 논문에서는 Post LN에 문제가 있음을 언급하고, Pre LN이 더 좋다는 결론을 보여줌.
- 특히, 우리가 자주 사용하는 Pytorch는 Post LN, Tensor2Tensor는 Pre LN으로 구현되어있음.

![](https://jeongukjae.github.io/images/2020/07-16-preln/fig1.png)

![image](https://user-images.githubusercontent.com/38183241/103536537-0cd22a80-4ed6-11eb-99af-0029b46517d6.png)
- 특히 Pre LN은 가장 마지막 레이어 출력 전에 Layer Norm을 한번 더 수행한다. (Final Layer Norm)
<br>

### Post LN에는 어떤 문제가 있을까?
- 일반적으로 Transformer를 학습시킬 때는 CNN, RNN 등의 모델보다 더욱 신중하게 최적화 해야함.
- 특히 Transformer를 scratch 부터 학습시킬 때는 learning rate warm up이 필수적임.
- 그러나 이런 warm up 과정은 연구자에게 hyperparameter 선택을 강요하며, 학습을 더 느리게 만듦.

![](https://hoya012.github.io/assets/img/bag_of_trick/2.PNG)

- 논문에서는 Layer Normalization의 위치가 gradient scale에 큰 영향을 미치며, Post LN의 경우 output layer 근처의 gradient scale이 크기 때문에
warm up 과정 없이 학습이 잘 안된다고 주장함.

- Adam Optimizer의 경우

![](https://user-images.githubusercontent.com/38183241/103535619-69cce100-4ed4-11eb-82ce-5ea3dab3d28c.png)

- SGD Optimizer의 경우

![](https://user-images.githubusercontent.com/38183241/103535644-72251c00-4ed4-11eb-91ca-91ca5cea83e0.png)

- 첫번째로 두 옵티마이저 모두 warm up이 필수적이라는 것을 알아냄. Adam Optimizer의 경우, warm up을 하지 않으면 BLEU 스코어 8점을 기록한데 비해, 
warm up을 적용하면 34점을 기록하여 매우 큰 성능 차이를 보였음. SGD도 마찬가지로 warm up을 한 경우 성능이 훨씬 좋음. 
  - RAdam 논문에서, AdaXXX 계열의 adaptive LR optimizer들이 초반에 너무 적은 수의 배치(잘못 된 분산)만 보고 lr을 설정하기 때문에 warm up이 도움된다고 했는데, 이상하게 SGD에서도 잘 됨... 이는 2번째 논문에서 설명함.
- 두번째로 t_warmup (warm up 스텝 수)가 중요하다는 것을 알 수 있음. Adam Optimizer의 경우 lr이 le-3 일 때, t_warmup을 500으로 설장하면 거의 학습이 진행되지 않은 것에 비해
t_warmup을 4000으로 설정하면 학습이 잘 진행됨.
- 결과적으로 warm up에 대한 하이퍼파라미터 설정이 성능에 대단히 중요한 영향을 미친다는 것을 알 수 있고, Post LN의 경우 warm up을 수행하지 않고 처음부터 높은 LR을 부과하면 학습이 잘 안되는 것을 알 수 있음.

### 그렇다면 왜 Pre LN이 더 나을까?
![](https://jeongukjae.github.io/images/2020/07-16-preln/fig3.png)
- ed proof : Given e>0, there exists delta>0 such that |x-a|<delta일 때 |f(x)-L|<e를 만족하면 f는 x는 a일 때 L로 수렴.
- 현재 함수에서 수렴 점이 Z의 기댓값이기 때문에, 만약 Z가 ed bounded 되어 있으면 높은 확률로 그 기댓값에서 멀리 떨어져 있지 않음.

![](https://jeongukjae.github.io/images/2020/07-16-preln/fig4.png)

- 위 Theorem 1에 의하면, Post LN의 출력과 Pre LN의 출력이 모든 i에 대해 ed bound 되어 있으면,
각 방식의 Gradient의 frobenius norm(행렬 norm)이 0.99 - δ - (δ / 0.9 + δ)의 확률로 위를 만족함. (Single-head attention 기준)
- 여기서 중요한 것은 **Pre LN의 경우 gradient의 frobenius norm이 L(레이어 수)에 반비례 함**
- 이를 이해하기 위해 3가지 lemma가 등장함.
  - lemma-1 : gaussian distribution을 따르는 벡터 X가 존재할 때, ReLU(X)의 L2 norm의 기댓값은 1/2 * (σ^2) * d가 됨.
  - lemma-2 : lemma-1에 의하면 Post LN의 모든 레이어에 대해 최총 출력 기댓값은 3/2 d가 됨. (layer와 무관)
  그러나 Pre LN의 경우 (1 + l/2) <= 기댓값 <= (1 + 3l/2)가 됨. (여기서 l은 layer임)
  - lemma-3 : Layer Normalization이 걸리는 Gradient는 X의 norm에 반비례가 됨. (||J_ln|| = sqrt(d) / ||x||)
- (lemma와 Theorem에 대한 내용은 Appendix에 유도식이 자세하게 나오니 관심 있으시면 보시길...)
- 즉, **Pre LN은 layer가 깊어질 수록 X의 norm이 커지는데, Layer norm이 걸리는 Gradient는 그와 반비례이기 때문에, Layer가 깊어질 수록 Gradient가 작아짐.**

### Experiments
![image](https://user-images.githubusercontent.com/38183241/103556767-980ee880-4ef5-11eb-8bcd-7c0f36ac5d7b.png)

- 위 그래프는 사이즈(레이어)별 Transformer 모델들의 output gradient 기댓값을 나타냄.
- 위의 정리에서 보인 것과 정확히 일치함.

![image](https://user-images.githubusercontent.com/38183241/103556289-e66fb780-4ef4-11eb-876a-ed2fc125bab8.png)

- 위 그래프는 6-6 Transformer의 각 레이어 별 output gradient 기댓값을 나타냄.
- Post LN은 깊은 레이어로 갈수록 Gradient가 점점 Exploding / Vanishing 하는 현상이 일어나는 것에 비해 Pre LN은 Gradient가 Layer가 깊어짐에 따라
작아지기 때문에 상쇄(?)되는 것 처럼 보임.

![image](https://user-images.githubusercontent.com/38183241/103557649-d953c800-4ef6-11eb-939c-b39351967dd7.png)

- 기계번역의 경우 Pre LN이 훨씬 더 효율적으로 학습됨을 볼 수 있다.

![image](https://user-images.githubusercontent.com/38183241/103557940-41a2a980-4ef7-11eb-9817-f5f354e9ee78.png)

- BERT의 경우 Pre-train과 두가지 downstram task에 대해서 Pre LN이 더 좋은 모습을 보였음.

<br><br>

## 2. Understanding the Difficulty of Training Transformers
- Liu et al.
- [arXiv](https://arxiv.org/abs/2004.08249)

<br>

### 보아하니 Post LN의 Decoder만 문제인 것 같다?
![](https://user-images.githubusercontent.com/38183241/103564593-20938600-4f02-11eb-9baf-3410ad179f37.png)
- 위에서 언급했던 Gradient Exploding / Vanishing 문제는 Post LN Transformer의 디코더만 겪는다. 즉, 인코더는 위의 현상이 발생하지 않는다.
- 특히, Decoder의 Causal Attention → Enc-Dec Attention 부분에서 문제가 심한 것 같다.
- 또 한가지 발견한 사실은, 단순히 Gradient Exploding / Vanishing 문제를 해결하는 것 만으로 학습을 안정화 시킬 수 없었다는 것이다.

![](https://user-images.githubusercontent.com/38183241/103593399-18a30880-4f39-11eb-9ae7-1c4403b7c05a.png)
- 그림을 보면 Post LN Encoder + Pre LN Decoder를 사용했을 때는 Gradient Exploding / Vanishing 현상이 발생하지 않았다.
- 그러나 학습 결괄르 보면 수렴하지 않고 발산했다. 오직 Pre LN Encoder + Pre LN Decoder를 사용했을 때만이 수렴에 성공했다.
- 물론 **Gradient Exploding / Vanishing을 해결하는 것은 중요한 문제지만, 단순히 그것만으로 학습을 안정화시킬 수는 없다.**
- 따라서 Gradient Exploding / Vanishing를 넘어서는 개념인 **Amplification Effect**에 대해 이야기한다.

### Residual branches?
![image](https://user-images.githubusercontent.com/38183241/103604953-d3410400-4f55-11eb-915b-f608aa2ee97e.png)
- 논문을 처음 읽을 때 용어가 헷갈렸는데, Residual branch는 x + f(x)에서 f(x) 부분을 의미하는 것임.

### Visualization
![image](https://user-images.githubusercontent.com/38183241/103604828-8b21e180-4f55-11eb-93a2-700b24540a45.png)
- β는 residual branch와 short cut branch 출력 값의 크기 비율을 의미하고, 위는 그 비율을 시각화 한 자료임.
- Post LN은 Pre LN보다 β가 훨씬 크게 학습 됨. 즉, Post LN은 residual branch(= f(x))가 큰 영향을 미침.
- 그에 비해 Pre LN은 residual branch가 영향을 덜 미치게 됨. (It is harder for Pre-LN layer to depend too much on their residual branches)
- **Residual Branch에 더 많이 의존하면, 모델이 더 높은 포텐을 가지게 되지만, 당연하게도 파라미터 변화에 의한 값의 변동이 커진다. (output change가 커진다.)** (불안해진다)
(Although depending more on residual branches allows the model to have a larger potential, it amplifies the fluctuation brought by parameter changes.)

![image](https://user-images.githubusercontent.com/38183241/103600936-f070d500-4f4b-11eb-9471-422533a4ae4d.png)
- 길게 설명되어 있는데, 핵심은 `Var[변화 이전(x,W) - 변화 이후(x,W*)]`의 값 차이를 비교할 때, Post LN은 O(N), Pre LN은 O(log N)이라는 것임.
- Pre LN의 경우 파라미터 변동 이전과 이후의 차이가 적고, Post LN은 변동 이전과 이후의 차이가 크다. 
  - 직관적으로 파라미터 변화(= W와 W*의 차이)가 크니까 당연히 output 값 사이의 차이도 클 수 밖에 없음.
- 이에 대한 자세한 설명과 수식 풀이는 Appendix에 있으니, 관심 있으신 분들은 보시길... (핵심은 아닌 것 같아서 생략함.)

![image](https://user-images.githubusercontent.com/38183241/103601290-ab00d780-4f4c-11eb-83c3-563762bec8ce.png)
- **large output change가 Post LN을 destabilize시키는 주요 원인**이라고 함.
- warm up을 적용하면 SGD에도 도움이 되는 이유는 output change가 작아지기 때문 인 것 같으며, 이는 Appendix B에 설명 되어있음.
- 따라서 Post-LN의 output change를 조절하여 네트워크를 안정적으로 만들어 볼 것임.

### Admin initialization
![image](https://user-images.githubusercontent.com/38183241/103602695-0a141b80-4f50-11eb-8ae0-4b6a0e00f66f.png)
- Post LN의 Residual Branch에 대한 종속성을 제어할 ω라는 변수를 도입하고, 이 변수를 이용하여 Post LN의 output change를 O(log N)으로 만들 것임.
- **네트워크를 output = x · ω + f(x)와 같이 구성함. 즉, ω를 short cut branch에 곱하게 하여 그 크기를 조절하기 위함.**
- 학습에 사용되는 세부적인 옵션들 (activation 함수나 drop out ratio)등이 다르기 때문에 universal한 initialization은 불가능. 
- 대신 이 초기화 기법을 Profiling과 Initialization이라는 두가지 과정으로 분해하여 진행시킬 것임.

#### Profiling Stage
- 일단 전체 네트워크를 (기존과 같이) gaussian distribution을 따르도록 초기화 시킨 뒤, ω를 1로 설정하여 기존과 동일하게 네트워크를 초기화 시킴.
- 그 다음에 네트워크를 update 없이 forward propagate 시켜서 각 레이어의 residual branch의 분산(Var[f(x)])를 미리 계산해놓음.
- 현재는 각 Layer가 independent한 분포를 따르기 때문에, 이 부분에서 적은 수의 데이터 샘플(8192 토큰 이하)만 이용해서 forward 해도 충분하다고 함.

#### Initialization Stage
![image](https://user-images.githubusercontent.com/38183241/103603559-3e88d700-4f52-11eb-9923-56b72e6574e1.png)
- ω를 위와 같이 초기화 시키게 되면 학습 초반에는 output change가 O(log N)으로 보장되는데, 학습 후반에는 Residual Branch에 많이 의존하게 된다고 함.
- 따라서 학습 초반에는 Pre LN처럼 안정적으로 학습하면서 학습 후반에는 점점 Residual branch를 의존하게 되어서 모델의 포텐을 충분히 살릴 수 있다고 함.
- 인퍼런스 할 때는 이 ω를 reparameterize (제거) 시키고 사용한다고 함.
  - 직관적으로만 생각해보면 초반에는 random하게 초기화 시켜놨기 때문에 Var[f(x)]의 값들이 당연히 클 것이다. (값들이 자기 맘대로니까 분산이 크다.) 따라서 short cut branch에 힘이 실린다.
  - 그러나 학습이 진행되면서 각 레이어의 파라미터 분포들이 어느정도는 유사해지고, 그에 따라 분산이 작아지게 되면 short cut에 곱해지는 ω가 작아지고 short cut보다는 residual branch에 힘이 실리는 것?
  - (논문에 언급된 내용은 아니고 본인 그냥 생각입니다..)

### Experiments
![](https://user-images.githubusercontent.com/38183241/103604401-4fd2e300-4f54-11eb-91ca-29f87cddeefe.png)
- 위 처럼 학습 초반에는 Pre LN처럼 Residual Branch의 의존도가 낮지만, 학습 후반에는 Post LN처럼 residual branch에 대한 의존도가 높아짐.

![](https://user-images.githubusercontent.com/38183241/103603900-08982280-4f53-11eb-983f-fba95334c473.png)
- 앞서 언급한대로, Post LN은 15개 세팅 중에서 7개의 세팅에서 발산할 정도로 불안정하지만, 수렴한 세팅에서는 대부분 Pre LN보다 성능이 좋음.
- Pre LN은 15개 세팅에서 전부 수렴하였지만 대부분의 경우 성능이 좋지 못함. 안정적이긴 하지만 모델의 포텐이 제한되는 경향이 있음.
- Admin을 적용하게 되면 Pre LN처럼 모든 경우 발산하지 않고 학습에 성공하는데 성능이 Post LN 수준으로 좋다.

<br><br>

## 3. Very Deep Transformers for Neural Machine Translation
- Liu et al.
- [arXiv](https://arxiv.org/pdf/2008.07772.pdf)

### Admin의 파워를 실험해보기 위한 간단한 실험 논문!
![image](https://user-images.githubusercontent.com/38183241/103605790-287e1500-4f58-11eb-9912-84a1a756ee6c.png)
- Admin을 사용하면 Encoder 60 Layer, Deocder 12 Layer 모델도 학습이 가능하다. (Bleu가 2.5점씩 올랐다.)

![image](https://user-images.githubusercontent.com/38183241/103605885-67ac6600-4f58-11eb-93b0-1d0df3df7c7f.png)
- Tran PPL 비교 결과 확실히 Admin을 적용한 것이 확실히 학습도 빨리 되고 우수하다.

![image](https://user-images.githubusercontent.com/38183241/103606282-5748bb00-4f59-11eb-8f2c-ebd841b99964.png)
- 인코더 디코더 레이어수에 대한 성능 비교, 이전 연구들에서 언급한 것 처럼 Encoder Layer가 깊을수록 성능이 좋다.
- 이는 E60-D12 모델과 E12-D60 모델의 성능 비교를 보면 확실히 차이가 나는데, 심지어 E24-D12모델이 E12-D60 모델을 out perform했다.

![image](https://user-images.githubusercontent.com/38183241/103606532-f66db280-4f59-11eb-99fa-a7a7d57cbe85.png)
- 실제로 인코더가 깊어지면 저빈도 단어에 대한 정확도가 크게 개선되며, 긴 문장에 대한 번역 성능도 개선 됨을 알 수 있음.

![image](https://user-images.githubusercontent.com/38183241/103606617-39c82100-4f5a-11eb-8932-22aac4825c7f.png)
- SoTA에 도전, 모델의 파라미터를 조절하여 d_model을 768로 키우고 인코더 레이어를 36개로 줄임. 추가로 BT를 수행함.

![image](https://user-images.githubusercontent.com/38183241/103606945-18b40000-4f5b-11eb-9444-439009cc6f6e.png)
- 현재 (2020년 1월 5일)기준으로 WMT En-Fr SoTA 성능을 보유하고 있음. Admin + BT를 En-Fr에서만 도전했기 때문에 이 데이터셋에서만 SoTA임.
- En-Ge의 경우 Understanding Backtranslation as Scale이 1위, T5 11B모델이 2위인 것을 감안하면, En-Ge에서 Admin + BT를 했으면 이 모델이 SoTA를 찍었을 가능성이 높음.

### ReZero is All You Need: Fast Convergence at Large Depth
![image](https://user-images.githubusercontent.com/38183241/103607260-cf17e500-4f5b-11eb-80d4-9116e325cc0f.png)
- 관련해서 레딧을 뒤지다가 발견한건데, 이 메커니즘이 이 논문에서 처음 제안한 것은 아닌 것 같음.
- CNN도 Transformer와 유사하게 BN + Residual Connection 구조를 사용했기 때문에 Admin과 같은 접근법이 가능. (ReZero가 먼저 나왔다.)
- 이 논문에서는 성능이나 Convergence 여부보다는 속도에 초점을 맞추고 개선했다고 주장하는 것 같음. (CNN은 깊어도 잘 수렴하니까..?)
- 이 ReZero라는 논문이 사용한 기법과 Admin Initialization이 매우 유사해보인다.

![image](https://user-images.githubusercontent.com/38183241/103607421-333aa900-4f5c-11eb-8a15-80234cdf85c0.png)
- 이 논문에서도 CNN과 Transformer를 모두 실험했음. 
- CNN의 경우 CIFAR-10에서 성능을 비교했는데 성능이 모두 개선된 것을 볼 수 있고, 80% 정확도에 도달하는 시간을 비교하고 있음.
- Transformer는 속도 이야기만 하고 성능 향상에 대한 언급이 없음. (약간 아쉽다.)

<br><br>

## Conclusion
- 이전에 나도 동일하게 36-2(번역), 36-12(요약) 등 깊은 Transformer를 학습하려고 시도했었는데 모두 발산했던 경험이 있다. 이 논문들은 깊은 Transformer도 안정적으로 학습시키는 방법을 연구해준 고마운 논문들이다. 
- Admin이 Fairseq이나 Transformers에 기본으로 탑재되면 거의 Adam, BatchNorm급의 치트키가 될 수도...?
- mBART와 T5의 논문에서 언급 되어 있는 것처럼 Translation은 PLM으로 성능을 개선하기가 어렵다.
  - 그래서 Facebook과 Google 진영에서는 PLM이 아닌 Data Curation 위주로 번역 성능을 향상하는 방법에 대해 연구했는데, 그건 Facebook이나 Google같은 공룡기업들이 할 수 있는 것이고, 실제로 현업에 적용하기는 어렵다.
  - 이 논문은 추가 데이터 없이 모델의 구조만 간단하게 변경하여 벤치마크 데이터셋에 대해 성능을 개선하였다는 것에 의의가 있다. 그러나 Layer를 60-12까지 극적으로 키웠는데도 BLEU score가 2.5점 밖에 안올랐다는 것은 약간 실망이다. 
- 올해에도 번역 관련해서 여러 논문들이 나오고 있긴한데 (e.g. [Data-Diversification](https://arxiv.org/pdf/1911.01986v4.pdf), [Bert-Fused](https://arxiv.org/pdf/2002.06823v1.pdf) 등등..) BT 이후로는 모두 성능을 극적으로 개선하지는 못하고 있는 것 같다.
- NLU에서 BERT가 보여줬던 것 처럼 뭔가 번역에서도 성능을 극적으로 개선할만한 그런 novel한 방법은 없는걸까...?
