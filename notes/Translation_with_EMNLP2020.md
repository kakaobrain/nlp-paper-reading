# Machine Translation with EMNLP2020
![image](https://user-images.githubusercontent.com/38183241/100951475-f725ba00-3551-11eb-8d9d-c79d832bf33b.png)
<br><br><br>

## 1. [Shallow-to-Deep Training for Neural Machine Translation](https://arxiv.org/abs/2010.03737) (EMNLP 2020)
1. 인코더를 깊게 쌓는 것이 번역에 도움이 되는 이유
2. 깊은 트랜스포머 네트워크를 빠르게 학습시키는 방법
3. 기타 여러가지 학습을 돕는 방법들
<br><br><br>

### Background : [Non Auto-regressive Machine Translation](https://arxiv.org/abs/1711.02281) (ICLR 2018)
![image](https://user-images.githubusercontent.com/38183241/100951460-eecd7f00-3551-11eb-8e7d-07d4f0e97f2b.png)
![image](https://user-images.githubusercontent.com/38183241/100952560-1e7d8680-3554-11eb-9919-ae05ed79172c.png)
- 인코더 레이어의 마지막 단에 Fertility Classifier를 붙여서 각 토큰을 디코더의 입력으로 몇번 사용할지 결정
- 디코더에 있던 Casual Attention을 제거함 (디코더가 모든 입력 토큰을 볼 수 있음)
- Non Auto-regressive하기 때문에 빔서치 등의 방법을 사용하지 않고 이로 인해 디코딩이 훨씬 빠름.
- 성능이 Auto-regressive에 비해 소폭 하락하였으나 속도 대비 굉장히 좋은 성능을 보여줌.
<br><br><br>

### Background : [Deep Encoder - Shallow Decoder](https://arxiv.org/abs/2006.10369) (ICLR 2021)
![image](https://user-images.githubusercontent.com/38183241/100952321-b038c400-3553-11eb-85d1-9da7512a2ba5.png)
- 인코더를 깊게 쌓고 디코더를 얕게 쌓으면 Auto-regressive의 고성능을 유지하면서 속도를 빠르게 할 수 있다.
- 기존의 6-6 Auto-regressive 아키텍처를 12-1 아키텍처로 변경하고 Non Auto-regressive 모델들과 속도를 비교한다.
- S1은 한 문장, SMax는 배치단위 번역시의 시간, AT는 Auto-regressive, NAT는 Non AT, CMLM과 Disco는 NAT계열의 모델이다.
- S1에서 Deep Encoder Shallow Decoder가 기존 NAT 모델들과 비슷한 수준의 속도를 보였다.
- SMax에서는 오히려 NAT 모델들보다 AT 모델이 훨씬 빠르다. 여기에 12-1 아키텍처를 적용하면 더 빨라진다.
- **Question : 그러면 번역에서는 인코더가 디코더보다 중요한걸까? 인코더를 깊게 쌓으면 왜 번역이 잘 되는 걸까?**
<br><br><br>

#### 그런데 잠깐, 왜 유독 Translation 태스크에서만 Transformer 아키텍처를 변경하려고 할까? (from [T5](https://arxiv.org/abs/1910.10683))
![image](https://user-images.githubusercontent.com/38183241/100953463-f98a1300-3555-11eb-91a4-104c2d486e0d.png)
- T5 저자들에 의하면 다른 생성 태스크들과 달리 번역 태스크의 경우 Pretrained LM의 힘이 별로 크게 작용하지 않는다.
- Pretrain-Finetune 방식으로 성능이 안올라가니까 다른 방식으로 번역 성능을 올려보기 위해서 아키텍처를 변경하는 시도가 많다.
<br><br><br>

### Shallow-to-Deep Training for Neural Machine Translation (본 논문)
![image](https://user-images.githubusercontent.com/38183241/100953327-b7f96800-3555-11eb-80d2-77d34c18dac9.png)
- Emb-Similarity : 인코더가 깊어질수록 입력 임베딩과 i번째 Layer의 출력값이 비슷하지 않게 됨 → 입력 표현을 더 많이 변화시킨다.
- Adjcent-Similarity : 그러나 인코더가 깊어질수록 i번째 Layer의 출력과 i-1번째 Layer의 출력은 거의 비슷하다 → 인접한 레이어들의 표현이 거의 비슷하다.
- Inter-Similarity : 인코더가 깊어질수록 각 포지션의 Representation과 이들의 평균 Representation이 비슷해진다. → Noisy한 입력에 강해진다.
```
# Inter-Similarity
This can be seen as smoothing the representations over different positions. 
A smoothed representation makes the model more robust and is less sensitive to noisy input
```
- 인코더가 깊을수록 표현력이 강해지고, Noisy한 입력에 대해 robust해진다는 것은 알겠다. 그런데 학습이 너~무 오래걸린다.
![image](https://user-images.githubusercontent.com/38183241/100954490-fe4fc680-3557-11eb-8993-8a66c17fb73f.png)
- 위 처럼 모델의 Weight를 복사해가면서 점점 깊은 모델을 학습시키면 훨씬 빨리 학습시킬 수 있다.
  - 가장 베이스라인인 6-6 모델을 학습시킨다.
  - 6개 layer를 그대로 사용하고, 이 weight를 그대로 복사해와서 12-6 모델로 만들고 학습시킨다.
  - 12개 layer를 그대로 사용하고 가장 최근에 추가한 6개의 인코더 weight를 복사해서 18-6 아키텍처를 만든다.
  - 이를 계속 반복하여 깊은 네트워크를 빨리 학습할 수 있었다고 함.
- 이 방법에 대한 근거는 Adjcent-Similarity에서 찾을 수 있다. 
- 인접한 레이어의 Represenation은 비슷하기 때문에, 이전 레이어들을 복사하면 더 좋은 시작점에서 학습이 진행될 수 있다.
- 이외에 추가적으로 Pre-norm, Residual connection, LR initialization에 대해 다루었는데, 자세한 내용은 [여기](https://arxiv.org/pdf/2010.03737.pdf)를 참고.
<br><br><br>

## 2. [Multi-task Learning for Multilingual Neural Machine Translation](https://arxiv.org/abs/2010.02523) (EMNLP 2020)
- MLM과 DAE 등의 태스크를 번역과 동시에 학습하는 멀티 태스크 러닝을 통해 번역 성능을 개선해보자는 논문이다.
![image](https://user-images.githubusercontent.com/38183241/100960829-6d331c80-3564-11eb-8c95-0553a40616eb.png)
- 성능이 꽤 많이 오른다고 한다. 추가로 MLM, DAE 등울 Pretrain 단에서 수행한 mBART와 성능을 비교한다.
![image](https://user-images.githubusercontent.com/38183241/100961203-43c6c080-3565-11eb-8b4f-bf34086949df.png)
- 번역을 진행하면서 MLM과 DAE를 동시에 진행하는게 MLM pretraining 하는 것 보다 낫다고 한다.
![image](https://user-images.githubusercontent.com/38183241/100961301-74a6f580-3565-11eb-9181-cd6efc511778.png)
<br><br><br>

## 번외편 (+α) : Multilingual Machine Translation (MMT)를 잘하기 위해서는?
### [Beyond English-Centric Multilingual Machine Translation](https://arxiv.org/abs/2010.11125)
- English Centric한 데이터가 아니라, non-English ↔ non-English 데이터가 많아야 한다.
<br>
![image](https://user-images.githubusercontent.com/38183241/100961723-48d83f80-3566-11eb-8734-9c9b4e4e5d60.png)
<br>

- 본 논문에서는 LASER와 FAISS를 이용하여 대규모 병렬 데이터를 구축할 수 있었다고 한다.
<br>
![image](https://user-images.githubusercontent.com/38183241/100961835-7f15bf00-3566-11eb-954a-c53cb07bdfde.png)
<br>

- 핵심 1 : 고품질의 데이터 큐레이팅이 필요하다.
<br>
![image](https://user-images.githubusercontent.com/38183241/100962131-1844d580-3567-11eb-81a7-19878b8fd522.png)
<br>

- 핵심 2 : 적은 리소스의 언어 데이터를 크게 늘려야한다.
<br>
![image](https://user-images.githubusercontent.com/38183241/100962182-33afe080-3567-11eb-9895-de0203307379.png)
<br><br><br>

### [Complete Multilingual Neural Machine Translation](https://arxiv.org/abs/2010.10239)
<br>
- Low resource한 언어들 사이에서도 Zero-shot이 아니라 Complete Translation이 되어야한다.
![image](https://user-images.githubusercontent.com/38183241/100962469-b2a51900-3567-11eb-9492-c23e8f9d4651.png)
<br>

- 아래 같이 3중 병렬 데이터가 있으면, 영어를 제외한 병렬 데이터를 만들어야한다.
<br>
![image](https://user-images.githubusercontent.com/38183241/100962571-e7b16b80-3567-11eb-83b8-1049fced8d08.png)
<br>

- Multilingual Translation에서 Pivoting보다 Complete Translation을 하는 것이 더 좋은 성능을 보일 수 있다.
<br>
![image](https://user-images.githubusercontent.com/38183241/100962750-4a0a6c00-3568-11eb-8f4a-a75f281f88ea.png)
