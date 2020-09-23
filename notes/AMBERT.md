## Title
AMBERT: A Pre-trained Language Model with Multi-Grained Tokenization by Xinsong Zhang, Hang Li

### References
- arXiv: https://arxiv.org/abs/2008.11869

### Summary
- Fine-grained tokenization (prevalent)
  - subword / character / byte
  - no risk of ‘incorrect’ tokenization (+)
  - easier to learn (+)
  - less complete as lexical units (-)
- Coarse-grained tokenization
  - phrases in English and words in Chinese
  - e.g., New York, ice cream
  - complete as lexcial units (+)
  - no guarantee that tokenization is perfect (-)
  - difficult to learn (-)
- AMBERT: A Multi-grained BERT comes into play.
- Basic idea
  - 하나의 input 문장에 대해 위 두 가지 서로 다른 tokenization 기법을 적용한다.
  - e.g., input: A new chapel in York Minister was built in 1154.
  - Fine-grained: a | new | chapel | .. | 1154 | .
  - Coarse-grained: a new | chapel | in | york minister | ... | 1154 | .
  - parameter가 shared된 두 BERT-encoder가 각각을 encoding한다.
  - 그 둘을 합친다.
  - 일종의 앙상블.
- Coarse-grained techniques
  - 설명이 부실하다
  - 영어는 n-gram 중 빈도가 높은 것들을 하나로 묶었다고 나와 있다
  - 중국어는 내부 word tokenizer를 썼다고 되어 있다.
  - 성능에 이 technique이 주요 변수가 될지 안 될지가 명확하지 않다.
