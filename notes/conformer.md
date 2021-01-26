# Conformer: Convolution-augmented Transformer for Speech Recognition  
  
Anmol Gulati et al.  
Google Inc.  
INTERSPEECH, 2020
  
***

## Reference
- [Conformer](https://arxiv.org/pdf/2005.08100.pdf)  
- [Attention Is All You Need](https://arxiv.org/abs/1706.03762)  
- [Transformer-XL](https://arxiv.org/pdf/1901.02860.pdf)
- [Convolution](https://hichoe95.tistory.com/48)
  
***
  
## Summary
- Transformer 기반 모델이 음성인식 분야에서 좋은 성능을 보이고 있음
- Self-attention 기반한 트랜스포머는 global-context 정보를 잘 표현하지만, local-context에서는 부족하다는 단점이 있음  
- 반면, CNN 기반 모델은 local-context는 잘 표현하지만 global-context를 반영하기 위해서는 적당한 dilation과 깊은 구조를 가져야 함  
- 이 두 방법을 결합하여 global-context와 local-context 모두 잘 표현할 수 있도록 하기 위한 transformer + CNN 결합구조인 Conformer 구조 제안  
- Conformer Encoder + Transducer 구조 (논문에서는 인코더만 설명)
  
***

## Conformer Encoder  
  
기존 트랜스포머 블록과 다르게 2개의 Feed Forward Network (FFN)에 쌓인 Sandwich 방식으로 구성

### Conformer encoder model architecture

<img src="https://user-images.githubusercontent.com/42150335/105320076-16af9980-5c09-11eb-86ec-b5146ac65812.png" height=500>   
  
기존 트랜스포머 인코더 블록은 `Multi Head Self Attention (MHSA) → LayerNorm → Feed Forward Network (FFN) → LayerNorm` 구조에서 `FFN Module → MHSA Module → Conv Module → FFN Module → LayerNorm` 구조로 변경  
  
### Multi-Headed Self-Attention Module  
  
<img src="https://images.deepai.org/converted-papers/2005.08100/x3.png">  
  
- Relative positional encoding  
> 절대적인 position 정보를 더하는 방식이 아닌, 상대적인 position 정보를 주는 방식 (Transformer-XL)
> 절대적인 position 정보가 a=1, b=2와 같이 값을 지정하고 그 값의 차이를 계산하는 방식이라면, 상대적인 position 정보는 a=1, b=2이든 a=5, b=6이든 상관없이 두 수(위치)의 차이가 1이라는 것만 알려주면 되는 방식
  
> 이런 방식은 가변적인 시퀀스 길이 인풋에 대해 인코더를 robust하게 만들어 줌

- Pre-norm  
> 기존 트랜스포머는 Post-norm인데 반해, pre-norm 적용  
> 이전 연구들에서 pre-norm은 깊은 모델 학습이 원활하게 되도록 도와주는 효과가 있다고 알려짐  

### Convolution Module
  
<img src="https://user-images.githubusercontent.com/42150335/105454437-30aeb200-5cc5-11eb-8624-1ea49b71c8cd.png">
  
- Pointwise Conv  
  
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb8hxuL%2Fbtqw5f6QxMM%2Fk4gn4DUTEqPkqbJXusPAKk%2Fimg.png">  
  
> kernel size가 1x1로 고정된 convolution
> 차원을 맞출 때 자주 쓰임  
  
- GLU Activation  
  
<img src="https://miro.medium.com/max/1400/1*EwUvi3ATcVoa9Lm-2FwNUA.png" height=50>  
  
<img src="https://miro.medium.com/max/1400/1*4UZTVLQZSDV7gCsw2cn16Q.png" height=200>
  
- Depthwise Conv  
  
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbw6Am5%2Fbtqw4n45UWN%2FqNYnywQjSGkzkOtl5Pkzc1%2Fimg.png">  
  
> 그룹이 채널수와 같은 Group-Convolution. 각 channel마다의 spatial feature를 추출하기 위해 고안된 방법

```python
>>> nn.Conv(in_channels=10, out_channels=10, group=10)
```
  
- Swish activation

<img src="https://blog.kakaocdn.net/dn/QbxpI/btqEHxducIg/hrmYfDLHDT4N1oqCtt74CK/img.png">
  
### Feed Forward Module
  
<img src="https://user-images.githubusercontent.com/1694368/103190710-1b847480-490d-11eb-8ea5-280749a32a24.png">
  
> Pre-norm 적용  
> Swish activation : regularizing에 도움
  
***

## Conformer Block
  
[Macaron-Net](https://arxiv.org/pdf/1906.02762.pdf)에 영감을 받아서 2개의 FFN에 쌓인 Sandwich 구조로 구성.  
FFN 모듈에 half-step residual connection 적용  
  
<img src="https://user-images.githubusercontent.com/42150335/105326425-13b8a700-5c11-11eb-804c-bd8efef6060b.png" height=200>  
  
> 뒤의 Ablation study에서 Macaron-net FFN과 half-step residual connection이 성능 향상에 많은 기여를 했다고 함  
  
***

## Experiment  
  
### Data
- LibriSpeech
- 80 channel filterbank, 25ms window, 10ms stride  
- SpecAugment (F=27), ten time masks (maximum ratio 0.05)
  
### Conformer Transducer
- Three models: 10M, 30M, and 118M params
- Decoder: single LSTM-layer (Transducer)
- Dropout ratio: 0.1
- Adam optimizer, β1=0.9, β2=0.98 and έ=1e-9
- Learning rate scheduler: transformer lr scheduler, 10k warm-up steps
- 3-layer LSTM language model (LM) with 4096 hidden dimension (shallow fusion)

### Results on LibriSpeech
  
<img src="https://user-images.githubusercontent.com/42150335/105327556-5cbd2b00-5c12-11eb-8714-2c0ce2c7a1b0.png">  
  
***

<img src="https://user-images.githubusercontent.com/42150335/105327620-752d4580-5c12-11eb-9091-433ce8700141.png">
  
- Conformer Block vs Transformer Block (without external LM)
  
<img src="https://user-images.githubusercontent.com/42150335/105327876-c9d0c080-5c12-11eb-8b02-948f87c5f47d.png">
  
***

<img src="https://user-images.githubusercontent.com/42150335/105328157-1916f100-5c13-11eb-9473-69ac0c658e15.png">  
  
***

<img src="https://user-images.githubusercontent.com/42150335/105328196-2338ef80-5c13-11eb-9e8a-50ff45bad7b5.png">
  
***
  
<img src="https://user-images.githubusercontent.com/42150335/105328376-54b1bb00-5c13-11eb-9059-38bc7361ba6d.png">

***
  
<img src="https://user-images.githubusercontent.com/42150335/105328408-5aa79c00-5c13-11eb-94b2-8ee455c8daca.png">
  
## Conclusion
  
Transformer + CNN 구조인 Conformer를 제안했고, 이를 잘 결합하기 위한 다양한 실험을 해서 결과를 냄.
