# EMNLP Paper Review Part5: Speech
- [Adaptive Feature Selection for End-to-End Speech Translation (Biao Zhang et al)](https://arxiv.org/abs/2010.08518)
- [Incremental Text-to-Speech Synthesis with Prefix-to-Prefix Framework (Mingbo Ma et al)](https://arxiv.org/abs/1911.02750)

##  Adaptive Feature Selection for End-to-End Speech Translation
  
- EMNLP 2020  
- Biao Zhang, Ivan Titov, Barry Haddow, Rico Sennrich

### Summary
  
- End-to-End Speech Translation (E2E ST)를 다룬 논문  
- Speech Translation
  + Cascade: 음성 (source) → 음성인식 모델 → 텍스트 (source) → 번역 모델 → 텍스트 (target)
  + E2E: 음성 (source) → 음성번역 모델 → 텍스트 (target)  
- Cascade 방식은 음성인식에서의 오류가 기계번역으로 전파가 되는 단점이 있음
- E2E 번역이 최근 많이 연구되고 있으나, Cascade 방식의 성능을 따라잡지 못하고 있음   
  
![image](https://user-images.githubusercontent.com/42150335/101368190-32294400-38ea-11eb-924b-5b0a2e25d2e6.png)

- E2E ST가 어려운 주된 이유로, 음성마다 단어 발화 길이가 다르며, 노이즈 혹은 중간중간 끊기는 등 일관적이지 않다는 특징 때문이라고 주장  
- 그래서 인코딩 된 피쳐를 선택적으로 사용해야 된다고 주장 (Adaptive Feature Selection)      
- AFS는 인코더 아웃풋에서 필요없는 프레임은 제거하는 역할을 함 (L<sub>0</sub>Drop - Zhang et al., 2020)
- 결과적으로 본 논문은 아래와 같은 파이프라인을 제안함
  
<img src="https://user-images.githubusercontent.com/42150335/101366218-073df080-38e8-11eb-8699-dd6ebc2d70dc.png" width=300>  
  
- Training Pipeline
  1. ASR 모델 학습 (Hybrid Cross Entropy + CTC)  
  2. AFS 모델을 추가해서 ASR 모델 파인튜닝  
  3. ASR & AFS 모델은 Freeze한 채로 ST Encoder, ST Decoder 학습  
- Result on MuST-C En-De  
  
![image](https://user-images.githubusercontent.com/42150335/101370007-4a9a5e00-38ec-11eb-8f41-7f6de1b9d583.png)
  
  + AFS는 모델을 더 빠르게 하면서도 성능을 높였음
  + 성능은 Cascade보다는 살짝 낮음
    
***

## Incremental Text-to-Speech Synthesis with Prefix-to-Prefix Framework  
  
- EMNLP 2020
- Mingbo Ma, Baigong Zheng, Kaibo Liu, Renjie Zheng, Hairong Liu, Kainan Peng, Kenneth Church, Liang Huang  (Baidu Research)  
- [Demo Page](https://inctts.github.io/)
  
### Summary  
  
- 동시번역을 위한 빠른 음성합성 기법 제안  
- 새로 학습할 필요없이 Inference 단에서 수정하여 사용할 수 있는 파이프라인 제안 (Tacotron2 사용)
- 기존 TTS 시스템  
  
![image](https://user-images.githubusercontent.com/42150335/101376816-6bff4800-38f4-11eb-9dca-1592c05c6759.png)
  
> Text2Phoneme → Phoneme2Spectrogram → Spectrogram2Wave 단계를 거침
  
- 위와 같은 Full-sentence TTS는 문장 길이가 길어질수록 latency가 길어지는 고질적인 문제점을 가지고 있음  
- 이러한 문제점 해결을 위해 아래 파이프라인을 제안  
  
![image](https://user-images.githubusercontent.com/42150335/101377884-bf25ca80-38f5-11eb-8098-6f0b206d01f6.png)  
  
- Full-sentence TTS가 아닌, Incremental TTS 방식 제안  
- 먼저 만들어진 오디오를 재생하는 동안 뒷단의 오디오를 만들어나가는 방식  
- 이와 같은 파이프라인이 가능하려면 특정 단위로 쪼개야함 (E.g. Word)  
- 하지만 Word 단위로 TTS를 진행한 후, 오디오를 이어붙이게 되면 굉장히 부자연스러운 음성이 합성됨  
- 이를 극복하기 위해 lookahead-k Policy 제안  
  + t번째 target을 만들때 t+k개의 입력 소스를 통해 생성 (첫 k+1 스텝까지는 wait)    
- 결과적으로 음질이 크게 떨어지지 않으면서도 latency를 줄임 (문장이 길수록 효과가 큼)  
