# Survey of Question Generation

![](https://github.com/kakaobrain/nlp-paper-reading/tree/master/images/QG_Survey/title.png)

## Introduction
- 질문 생성 (QG)는 Raw Text, Database 또는 Semantic Representation, Image 등의 입력 소스에서 질문을 생성하는 작업 (Rus et al. 2008)
- 사람은 어떠한 문헌 혹은 정보로부터 풍부하고 창의적인 질문을 할 수 있고, 이 것을 기계에서 비슷하게 구현해보려고 하고자 하는 시도라고 할 수 있음
- QG는 QA와 밀접하게 관련이 있으며, QA에 대한 도전적인 과제임. QA의 이해능력 + NLG의 문법적으로 올바른 문장을 생성하는 능력도 요구 됨.
- 전통적인 QG (2008 ~ 2012) : 단락에서 사실적인 질문을 생성하는데에 초점을 맞췄으며, Rule based로 구현되었음
- 최근 (2016 ~) : Deep learning의 등장으로 NQG(Neural Question Generation) 기반의 연구가 많이 진행되고 있음
- NQG의 발달로 인해 더 광범위한 애플리케이션을 개발하는 것을 추구하고 있음 (Serban et al. 2016)

## QG For What?
- 교육용 질문을 생성하거나 강의를 위한 시험 문제를 생성할 수 있음 (Heilman and Smith, 2010)
- 지능형 과외 시스템으로 사용 될 수 있음 (Lindberg et al., 2013)
- 대화형 시스템 (챗봇)을 위해서도 QG는 매우 중요한 기술이라고 할 수 있음
- QA 데이터셋을 역으로 생성하여 QA 데이터셋을 만드는데 필요한 노력을 아낄 수 있음

## Fundamental of QG
- QG는 두가지 Objective를 달성할 필요가 있음. 1) 첫번째는 What to ask, 2) 두번째는 How to ask임
- What to ask는 NLU의 일종이며, Summarization처럼 문단에서 중요한 내용을 추출하거나 NER처럼 필요한 개체를 추출해야함
- How to ask는 NLG의 일종이며, 문법상의 정확성, 의미상의 정확성, 언어의 유연성과 같은 자연어 생성 품질에 중점을 둠
- 과거의 연구는 내용 선택과 질문 구성을 통해 What과 How를 두가지 별도의 문제로 생각하고 서로 다른 방법을 취해 구하였음
    - 문장이나 문단이 입력으로 주어지면 내용 선택은 질문 할 가치가있는 특정 주제를 선택하고 질문 유형 (What, When, Who 등)을 결정 
    - 자연어를 템플릿 문장 또는 규칙에 의한 방법으로 변환해서 단어들을 재배열 함 (How to ask)
    - 그러나 이러한 QG 아키텍처는 표현이 다양한 중간 표현, 변환 규칙 또는 템플릿으로 제한되어 있기 때문에 제한적
- 대조적으로 NQG는 End to End 아키텍처에 Motivate하면서 시작했음
- 가장 먼저 등장한 학습 프레임워크(Seq2Seq)에서는 What과 How를 공동으로 학습하는 접근법을 취함
- Attention Mechanism (Bahdanau et al., 2014) 에 의해 디코더에 입력 소스에서 중요한 단어를 선택하는 것이 What을 고르는 것이라고 이야기 함
- Seq2Seq 기반의 QG 모델은 이후 섹션에서 더 자세하게 다룰 예정임

## Input Modality
- 최근에는 KBQA (Knowledge Base Question Answering")나 VQA (Visual Question Answering) 등 연구가 성장함
- QG는 원래 텍스트 입력을 기반으로 하는 것이 일반적이지만 최근에는 위와 같은 연구의 성장에 힘입어 Knowledge base나 Image 등의 입력을 받는 QG모델도 연구 되고 있음.

## Corpora (a.k.a Dataset)
- QG는 QA의 이중 작업으로 간주 될 수 있으므로 원칙적으로 모든 QA 데이터 세트를 QG에도 사용할 수 있음 
- 그러나 질문 생성의 난이도에 영향을 미치는 말뭉치 관련 요소는 적어도 두 가지가 있음 
- 첫 번째는 이전 섹션에서 논의했듯이 질문에 답하는 데 필요한 인지 수준임 (얼마나 어려운지)
    - 현재 NQG는 SQuAD (Rajpurkar et al., 2016) 및 MS MARCO (Nguyen et al., 2016)와 같은 사실적 질문으로 주로 구성된 데이터 세트에서 유망한 결과를 얻음
    - 그러나 LearningQ (Chen et al., 2018)와 같은 심층 질문 데이터 세트에서는 성능이 크게 떨어짐
- 두번째는 Answer Type임 (입력의 타입)
    1. 정답이 지문 안에 포함된 경우 (Context, Answer Given)
    2. 정답이 지문에 포함되지 않은 추상적인 대답인 경우 (Context Answer Given)
    3. 질문과 함께 여러가지 정답을 함께 만들어야하는 객관식의 경우 (Context, Answer Given)
    4. 정답이 주어지지 않는 경우, 모델이 스스로 학습해야함 (Only Context Given)
    - 자세한 사항은 아래 표에 정리되어있음

![image](https://github.com/kakaobrain/nlp-paper-reading/tree/master/images/QG_Survey/corpora_table.png)

## Evaluation Metric
- 데이터셋은 일반적으로 QG와 QA간에 공유되지만 Metric은 전혀 다름
- 의미 있고, 구문 적으로 정확하고, 의미 적으로 건전하고 자연스러운 것은 모두 유용한 기준이지만 정량화하기는 어려움
- 인간평가 → NLG 메트릭 (BLEU, METEOR, ROUGE-L, NIST) → QA 모델로 평가하는 추세로 발전 됨

## Methodology (LSTM Seq2Seq)
- Attention 기반 BiLSTM (Bahdanau et al., 2014) 으로 주로 연구가 진행되어왔음 (Du et al. ,2017)
- Passage를 주면 BiLSTM으로 문장을 생성해냄. 그러나 여기에는 Answer 정보가 Encoding 되지 않음
- 위와 같은 방법론을 취할 경우 너무 추상적인 질문 (e.g. 무엇이 언급되었나요?)같은 질문이 생성되기도 함.
- 이런 문제를 해결하기 위해 Answer의 정보를 이용하기 시작함
- (1) Answer포지션을 Input의 Extra feature로 넣어주거나  (Zhou et al., 2017 : BIO태깅; Zhao et al., 2018 : Binary Indicator)
- (2) Answer 자체를 인코딩 하여 입력으로 사용하기도 함 (Duan et al., 2017 : ; Kim et al., 2019 : Answer를 answer토큰으로 대체하여 정답이 다시 질문에 나오는 것을 방지) 

## Question Word Generation Methods
- 질문 유형 (e.g. when / how / why ...)는 QG에서 중요한 역할을 함.
- Sun et al. (2018)은 생성 된 질문 단어와 답변 유형 간의 불일치가 현재 NQG 시스템에서 일반적임을 관찰함. 
- 예를 들어, 모델에 의해 "언제 멕시코 전쟁이 끝났나요?"가 나와야하는데 "왜 멕시코 전쟁이 끝났나요?"가 나옴
- 몇몇 논문 (Duan et al., 2017; Sun et al., 2018)은 모델 설계에서 질문 단어 생성을 별도로 고려함
    - Duan et al. (2017)은 나머지 질문을 생성하기 전에 먼저 질문 단어 (예 : "how to answer")를 포함하는 질문 템플릿을 생성하도록 제안
    - 두개의 LSTM Seq2Seq 모델이 사용되는데, 전자는 주어진 텍스트에 대해 위와 같은 질문 템플릿을 생성하는 방법을 배우고, 후자는 완전한 질문을 형성하기 위해 템플릿의 <answer>를 채우는 방법을 학습
    - 2 단계 프레임 워크 대신 Sun et al. (2018)은 제한된 Vocab을 이용해 질문 단어(when, what, how 등)를 생성해내는 추가 모드를 도입하였음
    - 질문단어 생성 모드에서는 answer, context의 embedding을 사용해 "제한된 어휘집합"을 사용하여 질문단어의 분포를 생성하게 학습함
    - 서로 다른 모드 사이의 전환은 각 디코딩 단계에서 모델의 학습 가능한 모듈에 의해 생성된 이산 변수에 의해 제어
    
## Paragraph Level Context Methods
- Du et al., (2017)에 따르면, SQuAD의 20%가 Paragraph 레벨의 이해를 필요로함
- 그러나 Seq2Seq은 긴 입력이 들어가면 Context를 활용하기 위해 너무 많은 시간을 필요로 함
- 이 문제를 해결하기 위해 Zhao et al. (2018)은 중요한 정보와 Context의 Self representation을 융합하여 Context를 구체화하는 gated self attetion을 제안하여 SQuAD에서 Sota를 기록
- 입력 텍스트와 문맥으로 구성된 긴 구절이 Answer Postion과 함께 LSTM을 통해 삽입됨
- 인코딩 된 표현은 Geted self matching network를 타고 입력되며, Feature fusion gate는 전체 context에서 유의미한 부분을 선택함 
- 전체 context를 활용하는 대신 Du and Cardie (2018)는 coreference resolution system을 이용해 pre-filtering을 수행
- answer를 언급하는 (coreffered) 질문들만 gated network로 공급되고, 그 출력이 original input vector와 concate되어 Seq2Seq의 입력으로 제공

## Answer-unaware QG Methods
- Answer unaware QG는 answer없이 context만 입력될 때, Question을 generation하는 환경을 의미함
- 최초의 Seq2Seq처럼 answer 정보를 아예 고려하지 않는 것이 아니라, 일정한 Contents를 선택하는 방법론을 가리킴
- 두개의 논문 (Du and Cardie, 2017; Subramanian et al., 2018)이 이러한 작업을 수행하였는데, 둘다 decomposition 기법을 사용하지만 신경망을 사용해 구현하였음
- 콘텐츠 선택을 위해 Du and Cardie (2017)는 neural sequence tagging 모델을 사용하여 입력 context에서 question-worthy한 문장을 식별하는 문장 선택 작업을 학습함
- Subramanian et al. (2018) neural key phrase extractor를 학습해서 paragraph에서 key phrase를 예측. 질문 구성을 위해 둘 다 Seq2Seq 모델을 사용했으며, 입력은 선택한 문장이거나 대상 답변으로 키 프레이즈가있는 입력 구절이 됨.
- 그러나 질문이 구절 내의 여러 정보에 대한 추론을 요구할 때 어떤 측면에 대해 물어볼 것인지 배우는 것은 매우 어려워짐
- 문제가되는 정보를 검색하는 것 외에도 다양한 추론 패턴 (예 : 귀납적, 연역적, 인과 적 및 유 추적)이 생성 프로세스에 미치는 영향을 연구하는 것이 향후 연구의 한 측면이 될 것임

## Technical Considerations
1. Copy Mechanism
    - 대부분의 NQG 모델 (Zhou et al., 2017; Yuan et al., 2017; Wang et al., 2018; Harrison and Walker, 2018; Kumar et al., 2018a)은 디코딩하는 동안 소스 문장의 관련 단어를 질문으로 그대로 복사하는 copy mechanism을 사용
    - 이 아이디어는 질문을 공식화 할 때 context에 나타나는 구절이나 엔티티를 다시 참조하는 것이 일반적이고, RNN 디코더가 이러한 희귀 단어를 자체적으로 생성하기 어렵기 때문에 널리 받아 들여짐

2. Linguistic Features
    - 단어 대소 문자, POS 및 NER 태그 (Zhou et al., 2017; Wang et al., 2018), Coreference (Harrison and Walker, 2018) 및 종속성 정보를 포함하여 단어 임베딩을 보완하는 추가 언어 기능(Kumar et al., 2018a)을 활용하기도 함
    - 보통 이러한 정보들을 encoding하여 input vector에 concatenation해서 추가 정보로 활용함

3. Policy Gradient
    - 몇몇 논문에서는 (Yuan et al., 2017; Kumar et al., 2018b) Policy Gradient를 사용하여 적절한 보상을 주는 강화학습 기법을 도입
    - 모델이 동등한 표현간에 확률 질량 분포를 학습하므로 생성 된 질문을 다양화하는데 도움이 됨.

## Emerging Trends

1. Multi-task Learning
    - QG가 발전함에 따라서 다른 NLP작업을 돕기 위해 사용되기 시작했고, 몇몇 논문에서는 QG가 데이터 부족 문제를 완화하기 위해 사용됨.
    - semantic parsing (Guo et al.,2018a)과 QA (Sachan and Xing, 2018)에 성공적으로 적용되었음
        - semantic parsing : Guo et al.은 자연어 질문을 SQL쿼리에 맵핑하는 작업을 수행할 때, QG모델로 생성된 수도 라벨링 세트를 포함시켜서 3%의 성능 향상 달성
        - QA : Sachan and Xing (2018)은 self training의 일환으로 QA와 QG를 함께 이용하는 방법을 제안하였음. 먼저 기존 데이터셋을 이용해 QA와 QG를 학습한 뒤, QG가 질문 생성, QA가 정답을 맞춰서 데이터셋을 생성하고, 해당 데이터셋으로 QA, QG모델을 다시 학습시켜서 성능을 향상시켰음
    - Guo et al.은 Question에서 중요한 정보를 찾기 위해 summarization과 QG의 Multi task learning을 수행하였음
    
2. Jointly Learning
    - Wang et al. (2017a)은 QA와 QG를 simultaneously하게 학습시켜서 두 모델의 성능 향상을 기대했음
    - Tang et al. (2018)은 GAN 프레임워크를 이용하여 QA - QG 모델을 학습하였음. 
    - 그러나 Jointly Learning은 분명 효과는 있지만 Sota 모델에 비해서는 좋은 성과가 나오지 않아서 개선의 여지가 있음

## Wider Input Modalities
- 지금까지 QG를 Text Input 위주로 설명했지만, Knowledge based QG나 Visual QG도 많은 발전을 이루었음
- Serban et al. (2016)는 30M의 KB 벤치마크 데이터셋을 생성했고, 이를 이용해 QG를 수행하려는 시도들이 있었음
- 인간은 Visual Input을 보고 더 높은 인지수준의 질문을 많이 할 수 있음. 따라서 Visual QG를 제안하는 몇몇 시도들도 있었음

## Generation of Deep Questions
- 깊은 질문 (높은 인지 수준의 질문)을 만드는 것은 인간에게도 어려움. Graesser and Person (1994)에 의하면 대학생들에게 어떤 질문을 만들어 보라고 했을 때, 1시간 동안 고작 6개의 질문을 만들어냈음
- 기존 NQG 모델들이 Deep question을 생성할 수 있는지 확인하기 위해 Chen et al. (2018)는 60 %가 넘는 질문이 포함 된 심층 질문 중심 데이터 세트 인 LearningQ를 Seq2Seq에 학습시켰으나 결과는 좋지 않았음

## Conclusion (3줄 요약)
- 다양한 환경과 입력을 사용하는 QG연구(Question Word Generation Methods, Paragraph Level Context Methods, Answer-unaware QG Methods)에 대해 알아보았음
- 지금까지 Seq2Seq 등의 모델로 질문을 생성할 때 발생하는 문제들에 대해 다양한 기법과 해결책이 제안되었으며, 특히 QG는 여러가지 태스크들(Summarization, QA 등)과 multi task learning이 잦은 태스크라는 것도 알 수 있었음.
- 단순히 QA 데이터셋을 뒤집어서 Question을 Generation 하는 것 뿐만 아니라, 더 많은 Sub Task들과 Challenge가 존재하며, SQuAD 형태의 데이터셋 이외에도 다양한 Cognitive level, 다양한 Format의 데이터셋이 존재하지만, 아직 많은 연구가 이루어지지 않음.