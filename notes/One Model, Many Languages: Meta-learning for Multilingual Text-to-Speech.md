# One Model, Many Languages: Meta-learning for Multilingual Text-to-Speech  
  
Tomáš Nekvinda, Ondřej Dušek  
Charles University  
INTERSPEECH, 2020  
  
## Reference
  
* [ArXiv](https://arxiv.org/abs/2008.00768)  
* [Source Code](https://github.com/Tomiinek/Multilingual_Text_to_Speech)  
* [Demo Webpage](https://tomiinek.github.io/multilingual_speech_samples/)  
* [Tacotron](https://arxiv.org/abs/1703.10135), [Tacotron2](https://arxiv.org/abs/1712.05884)   
* [DC-TTS](https://arxiv.org/pdf/1710.08969.pdf)
* [CSS 10 Dataset](https://github.com/Kyubyong/css10)  
* [Common Voice Dataset](https://commonvoice.mozilla.org/en/datasets) 
  
## Summary
  
* Multilingual Speech Synthesis  
* Meta-learning   
* Voice Cloning : Speech in multiple languages with the same voice  
* Code switching : Speak two (or more) languages with a single utterance.  
* Tacotron2 base architecture
  
## Tacotron
  
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FwnGyQ%2FbtqDblNauXg%2FJsSXkwgQY1yc3lIHtdgIP0%2Fimg.png" width=700>

* 딥러닝 기반 음성합성의 대표적인 모델  
* Attention + Sequence-to-Sequence의 TTS 버전  
* Griffin-Lim Vocoder 사용 (빠르지만 성능은 좋지 못함)  


## Tacotron2  
  
<img src="https://user-images.githubusercontent.com/42150335/94840259-1cfbe900-0453-11eb-8803-cac2ea30b425.png" width=470>  
  
* Mel-Prediction Network : Attention based Sequence-to-Sequence Network  
  + 인코더에서 Bi-directional LSTM 적용
  + Location Sensitive Attention 적용 (음성 Alignment에 강한 어텐션)
  + 인코더, 디코더에 Convolution Layer 적용  
* Stop Token 사용
* Vocoder : WaveNet  
  + 장점 : 상당히 고품질의 음성으로 변환
  + 단점 : 엄청나게 느림
  

## Model Architecture
  
<img src="https://github.com/Tomiinek/Multilingual_Text_to_Speech/raw/master/_img/generated.png" width=800>
  
* Tacotron2 기반의 모델들로 실험 진행  
* WaveRNN Vocoder 사용

### This Paper`s Model: Generated (GEN)  
  
* **Parameter Generation Convolutional Encoder**   
  + 이 논문에서는 Fully convolutional encoder를 사용 (from DC-TTS)  
  + Cross-lingual knowledge-sharing을 가능하게 하기 위해 인코더 컨볼루션 레이어의 파라미터를 생성하여 사용  
  + 입력되는 Language ID에 따라 Fully Connected 레이어를 통해 다른 다른 파라미터를 생성  
  
* **Speaker Embedding**  
  + Multi-speaker, Cross-lingual voice cloning을 위해 Speaker Embedding을 사용  
  + 인코더 아웃풋에 Concatenate하여 스펙트로그램 생성시에 반영되도록 함

* **Adversarial Speaker Classifier**  
  + 이상적으로 Voice cloning을 위해서는 텍스트(언어)로부터 화자의 정보가 반영되면 안됨  
  + Speaker Classifier와 나머지 모델(인코더, 디코더)은 forward에서는 독립적이지만,  backpropagation을 진행할 때, 두 loss (L2 of predict spectrogram, cross entropy of predicted speaker ID)가 인코더 파라미터 업데이트에 영향을 미침  
  + Gradient reversal layer를 통해 인코더가 speaker에 대한 정보를 반영 못하도록 학습

### Baselines: Shared, Separate & Single  
  
※ GEN과 다른점만 비교  
  
* **Single (SGL)** 
  + Monolingual Vanilla Tacotron 2 (Code-switching에 사용 X)
* **Shared (SHA)** 
  + GEN과 다르게 Tacotron 2의 인코더 사용 (Multilingual)  
* **Separate (SEP)** 
  + GEN과 같이 Multiple convolution layer를 사용
  + Parameter generation 사용 X
  + Adversarial speaker classifier 사용 X


## Dataset
  
10개의 언어로 구성된 CSS10과 Common Voice 데이터셋의 일부를 사용
Code-switching을 학습하기 위해 multi-speaker 데이터가 필요 (언어와 화자 일치를 없애기 위해)   
  
<img src="https://user-images.githubusercontent.com/42150335/95888064-9680c900-0dbb-11eb-9967-a30b21dbfa80.png" width=600>  

## Experiment  
  
SGL, SHA, SEP, GEN을 비교했을 때 GEN이 거의 모든 결과에서 우수한 성능을 보임
  
![image](https://user-images.githubusercontent.com/42150335/95888889-a816a080-0dbc-11eb-81a9-9a2d036f2def.png)  
  
<img src="https://user-images.githubusercontent.com/42150335/95888982-cbd9e680-0dbc-11eb-984f-1524ab3a9f38.png" width=400>
  
## Conclusion
  
* 본 논문에서 제안하는 모델은 Multilingual Voice cloning, Code-switching에 우수한 성능을 보임
* 추후 연구로 어텐션 모듈을 수정하는 것을 생각중이라고 함
  
