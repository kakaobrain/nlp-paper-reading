# Wikipedia2Vec
- Yamada et al.
- Studio Ousia, RIKEN AIP, University of Washington, The University of Tokyo, Nara Institute of Science and Technology, National Institute of Informatics, Keio University
- Submitted on 15 Dec 2018 to arXiv

### Reference
- [arXiv](https://arxiv.org/pdf/1812.06280.pdf)
- [code](https://github.com/wikipedia2vec/wikipedia2vec)
<br><br>

## Summary
- Wikipedia를 이용해 Entity Embedding을 학습해서 좋은 결과를 얻었으며 관련 오픈소스 툴킷(파이썬)을 공개했다.
- 엔티티만 학습하는 것이 아니라 단어와 엔티티를 모두 학습했는데, 단어-단어, 엔티티-엔티티 뿐만 아니라 유사한 단어-엔티티도 가깝게 위치하게 하여 학습했다.
<br><br>

## Methodology
![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/Wikipedia2Vec/methodology.png)

<br><br>

### 1. word-based Skip Gram
우리가 이미 알고 있는 단어와 단어 간의 Skip Gram Word2Vec을 학습한다. 
<br><br>
<img src="https://latex.codecogs.com/svg.latex?L_w=-\sum_{i=1}{\sum_{-c\leq%20j\leq%20c,%20j%20\neq%200}{\log%20p(w_{i+j}|w_{i})}}" width=500>
<br><br>

- i번째 단어를 주어졌을 때, 주변의 단어들, 즉 i+j번째 단어들을 예측한다.
<br><br>

### 2. Anchor Context Model
Anchor Context Model은 Word와 Entity의 거리를 좁히는 Skip-Gram 모델이다. 
<br><br>
<img src="https://latex.codecogs.com/svg.latex?L_a=-\sum_{(e_i,%20Q)\in%20A}{\sum_{w_c\in%20Q}{\log%20p(w_c|e_i)}" width=500>
<br>

- 하이퍼링크가 달린 단어를 Entity로 규정하고, Entity가 주어질 때, 앞 뒤의 단어들을 예측한다.
- A는 아티클 전체, Q는 엔티티 e_i의 surrounding word들을 의미한다.
<br><br>

### 3. Link Graph Model
Link Graph Model은 Wikipedia Link Graph를 이용하여 연결된 Entity간의 임베딩을 좁히는 모델인데,
여기에서 Wikipedia Link Graph란 Wikipedia 내의 하이퍼링크들의 참조 관계를 나타내는 무방향 그래프이다.
이 그래프 관계를 이용해 가까운 참조관계의 엔티티를 예측한다.

<br><br>
<img src="https://latex.codecogs.com/svg.latex?L_e=-\sum_{e_i\in%20E}{\sum_{e_o\in%20C_{e_i}}{\log%20p(e_o|e_i)}" width=500>
<br>

- E는 Vocab 내의 모든 엔티티들의 집합이다.
- C_e는 Link Graph 내에서 엔티티 e와 인접한 이웃 엔티티들을 의미한다.
<br><br>

### Final Model (Loss)
위의 세가지 Loss를 모두 더해서 최종 Loss를 계산하고, 이 Loss를 Minimize하는 모델을 최종적으로 사용함

<br><br>
<img src="https://latex.codecogs.com/svg.latex?L=L_w+L_a+L_e" width=400>
<br><br>

- 추가로 같은 Wikipedia 내에서 동일한 엔티티의 하이퍼링크가 딱 한번만 등장하기 때문에, 페이지마다 엔티티 단어들의 하이퍼링크를 직접 만들었음
<br><br>

## Experiments
- KORE entity relatedness dataset를 사용함 (아래와 같은 데이터셋임)

<img src=https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/Wikipedia2Vec/KORE.png width=500>

<br><br>

- KORE 데이터셋에서 다른 모델들 대비 가장 우수한 성적을 얻었으며, Hyperlink Generation(한번만 등장하는 것 방지)과 link graph 모델을 수행하지 않으면 성능이 떨어졌음
- 또한 Word Embedding의 성능을 평가하기 위해 Word Analogy 태스크 (Athens, Greece) -> (Seoul, ?)를 위해 Semantic(SEM), Syntactic(SYN) 성능에 대한 실험을 하였으며, Word Similarity 데이터셋인 SL과 WS 등의 데이터셋에서도 성능을 측정했는데 fastText 등에 비해 우수했다.

<img src=https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/Wikipedia2Vec/result.png width=500>

- 아래와 같은 방식으로 사용할 수 있다. `get_entity()` 메서드와 `get_word()` 메서드가 존재하고, 각각에 대해 검색 가능하다.
- [여기](https://wikipedia2vec.github.io/demo/) 에서 데모를 직접 해볼 수 있다.

```python
from wikipedia2vec import Wikipedia2Vec
>>> wiki2vec = Wikipedia2Vec.load(MODEL_FILE)
>>> wiki2vec.get_entity_vector("Scarlett Johansson")
memmap([-0.1979, 0.3086, ..., ], dtype=float32)

>>> wiki2vec.get_word_vector("tokyo")
memmap([ 0.0161, -0.0332, ..., ], dtype=float32)

>>> wiki2vec.most_similar(wiki2vec.get_entity("Python (programming language)"))[:3]
[(<Word python>, 0.7265),
(<Entity Ruby (programming language)>, 0.6856),
(<Entity Perl>, 0.6794)]

>>> wiki2vec.most_similar(wiki2vec.get_word('yoda'), 5)
[(<Word yoda>, 1.0),
 (<Entity Yoda>, 0.84333622),
 (<Word darth>, 0.73328167),
 (<Word kenobi>, 0.7328127),
 (<Word jedi>, 0.7223742)]

>>> wiki2vec.most_similar(wiki2vec.get_entity('Scarlett Johansson'), 5)
[(<Entity Scarlett Johansson>, 1.0),
 (<Entity Natalie Portman>, 0.75090045),
 (<Entity Eva Mendes>, 0.73651594),
 (<Entity Emma Stone>, 0.72868186),
 (<Entity Cameron Diaz>, 0.72390842)]
```
