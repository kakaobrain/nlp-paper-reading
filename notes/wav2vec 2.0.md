# wav2vec 2.0 : A Framework for Self-Supervised Learning of Speech Representations  
  
Alexei Baevski, Henry Zhou, Abdelrahman Mohamed, Michael Auli    
Facebook AI Research (FAIR)  
arXiv (2020.06)  
  
## References
* [arXiv](https://arxiv.org/abs/2006.11477)  
* [source code](https://github.com/pytorch/fairseq/tree/master/examples/wav2vec)
* [BERT](https://arxiv.org/abs/1810.04805)
* [Wav2vec](https://arxiv.org/pdf/1904.05862.pdf)  
* [VQ-Wav2vec](https://arxiv.org/pdf/1910.05453.pdf)  
* [speech book](https://ratsgo.github.io/speechbook/docs/neuralfe/wav2vec)  
  
## Summary
* BERT in Speech Recognition
* Representation learning with 53,000 hours of unlabeled speech data.
* Excellent speech recognition performance with only 40 sentences (10 minutes) of labeled data 
  
## Abstract  
  
* 음성 데이터 자체만으로 학습한 후 Fine-tuning하는 간단한 방법으로 semi-supervised 방법보다 좋은 성능을 냄  
* TIMIT, LibriSpeech 100h 데이터셋에서 State-Of-The-Art (SOTA) 를 달성
* LibriSpeech 100h은 단 1시간의 데이터만으로 기존 SOTA보다 높은 성능을 보임  
* 단 10분의 데이터 (40문장) 으로 LibriSpeech clean 5.7 / noisy 10.1 Word Error Rate (WER) 를 기록  
* LibriSpeech의 전체 학습셋을 사용했을 때 (960h) clean 1.9 / noisy 3.5 WER을 기록 (현재 [wer-are-we](https://github.com/syhw/wer_are_we) SOTA보다 높은 기록)  
  
## Wav2vec
  
* 현재 어느 정도 수준 이상의 음성인식기를 만들기 위해서는 대량의 데이터가 필요함 (수천 시간)
* 세상에는 7,000개 이상의 언어가 존재하는데 모든 언어에 대해 이 정도 수준의 데이터를 구축하기는 어려움  
* 영아 (infant) 들은 단순히듣는 것만으로 음성에 대해 학습함 

### Wav2vec 1.0 (previous work)
  
<img src="https://i.imgur.com/H9X1HiX.png" width=500>    
  
* wav2vec은 크게 *encoder* network *f*와 *context* network *g* 두개의 파트로 구성 (둘 모두 convolution neural network)  
* wav2vec은 해당 입력이 Positive인지 Negative인지 이진 분류(Binary Classification)하는 과정에서 학습  
* Positive : (C<sub>i</sub>, Z<sub>i+1</sub>), Negative : otherwise  
* Negative 쌍은 입력 음성의 i번째 context representation C<sub>i</sub>와 다른 음성의 hidden representation들 중 랜덤 추출  
* 즉, 2개의 네트워크는 입력 음성의 다음 시퀀스가 무엇일지에 관한 정보를 음성 피처에 녹여내도록 학습
  
### VQ-Wav2vec (previous work)
  
<img src="https://i.imgur.com/ivviYL1.png" width=500>  
  
* Wav2vec 아키텍처 중간에 **Vector Quantization** 모듈을 추가한 구조  
* VQ 모듈 : continuous representation Z를 discrete representation Z<sup>^</sup>로 변환  
* Discretization(이산화)는 discrete한 input을 필요로하는 NLP 알고리즘들을 바로 적용할 수 있다는 장점이 있음  
  
#### Vector Quantization  
  
<img src="https://i.imgur.com/y15Qu5Z.png" width=300>  
  
1. Z를 선형변환하여 logit을 만듦  
2. 여기에 Gumbel Softmax와 argmax를 취해 one-hot vector를 만듦  
3. 이후 Embedding matrix를 내적해 Z<sup>^</sup>를 만듦
   
   
### Wav2vec 2.0
  
<img src="https://user-images.githubusercontent.com/42150335/92450554-8a22b280-f1f6-11ea-8f66-0616b29d8c94.png" width=500>  
  
* VQ-wav2vec의 모델을 Transformer로 대체  
* Pre-training 이후 labeled data로 fine-tuning (Connectionist Temporal Classification (CTC) loss 사용)   
* 이전 연구들과의 차이점으로 Filter-Bank와 같은 피쳐추출 과정이 없음  
  
## Model
  
VQ-Wav2vec와 비교하여 Transformer를 사용했다는 특징이 있음  

### Feature Encoder 

* N x [Conv1d, Dropout, GroupNorm, GELU]

 
### Transformer

* Positional Encoding을 conv1d로 대체  
* Convoulation Layer 뒷단에 layer normalization 적용
   
### Quantization module   
* Vector Quantization 부분 참고  
  
## Training  
  BERT의 masked language modeling (MLM)과 유사하게 latent speech의 일부분을 masking하며, 모델은 quantized latent audio representation을 맞추는 방식으로 트레이닝이 진행됨. 이렇게 학습한 후 labeled 된 데이터로 fine-tuning 진행.
  
### Masking  
* Maksing 방법
```
1. 전체 오디오 구간 중 6.5%를 랜덤하게 선택  
  
2. 선택된 구간부터 10 time-step만큼 masking (masking은 중복될 수 있음)   
```
* 전체 오디오 구간 중 약 49% 정도가 masking (평균 14.7 timestep (299ms))
  
### Objective
  
![image](https://user-images.githubusercontent.com/42150335/93301173-0474b780-f833-11ea-8206-5ab40cf418a5.png)  
  
L<sub>m</sub> : Contrastive Loss, L<sub>d</sub> : Diversity Loss, L<sub>f</sub> : L2 penalty, {alpha, beta} : hyperparameter
   
* Contrastive Loss  
  
![contrastive-loss](https://user-images.githubusercontent.com/42150335/92514957-c5040500-f24d-11ea-95c3-1183fa1145b2.png)
  
* Diversity Loss  
  
![diversity-loss](https://user-images.githubusercontent.com/42150335/92514997-df3de300-f24d-11ea-835c-f1367aef5ebe.png)
  
### Fine-tuning  
  
* 이렇게 Pre-train 된 모델을 ASR 태스크로 Fine-tuning (+ projection layer)  
  
* 29 character token    
  
* CTC Loss  
  
* SpecAugment  
  
## Experiment  
  
### Datasets  
  
* Unlabeled data 1 : LibriVox-60k (전처리하여 53.2k 사용) \[[Reference](https://arxiv.org/abs/1912.07875)]   
* Unlabeled data 2 : LibriSpeech 960h
* train-10min, train-1h, train-10h, train-100h, train-960h 설정 (LibriSpeech)
  
### Result
  
* WER on the Librispeech dev/test sets when training on the Libri-light low-resource labeled
data setups of 10 min, 1 hour, 10 hours and the clean 100h subset of Librispeech
  
![image](https://user-images.githubusercontent.com/42150335/92516234-de0db580-f24f-11ea-88f3-485ee579bfda.png)  
  
* WER on Librispeech when using all labeled data of 960 hours
  
![image](https://user-images.githubusercontent.com/42150335/92516409-262cd800-f250-11ea-8e77-42fdc8761d2c.png)
  
  
## Conclusion
  
* 적은 비용으로도 좋은 성능의 음성인식기를 만들 수 있는 연구 방향을 제시함  
* Seq2seq 구조 혹은 word-piece 단위로의 변경을 통해 성능 향상 기대
