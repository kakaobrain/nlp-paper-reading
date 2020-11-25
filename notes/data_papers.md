## Age Suitability Rating
* https://www.aclweb.org/anthology/2020.lrec-1.166.pdf
* MPAA rating: 영화 등급
* 영화 대본 같은 정보를 넣고 영화 등급을 맞추는 게임이라고 생각하면 됨
* 모델적인 Contribution은 없음
* 각 영화에 대한 폭력지수, 선정성 지수 등이 있음.
* 영화 대본을 썼을 때, 이걸 보고 관련 등급 및 장르를 예측해주면 수요가 있지 않을까?
* 혹은 영화 대본이 아니더라도, 영상물 등급이 나오면 좋겠다

## Online Near-Duplicate Detection of News Articles
* https://www.aclweb.org/anthology/2020.lrec-1.156.pdf
* 신문 데이터가 많은데, 신문끼리 거의 같은 내용에 토씨 하나 바꾼 내용인 것이 많음
* 같은 데이터를 넣는 것보다 다양하게 넣어주는 것이 좋기 때문에 중요한 주제일 수 있음
* shingles : list of N-gram
* 어떤 텍스트가 들어왔을 때, 간단한 숫자로 변환한 sketch로 남겨두는 방식
* 점수자체는 큰 차이 없지만, 온라인으로 사용 가능함

## Long Range Arena : A Benchmark for Efficient Transformers
* https://arxiv.org/abs/2011.04006
* 데이터셋을 새로 만든 것 같음
* 긴 컨텍스트를 봐야만 하는 태스크들의 집합 (3개는 자연어, 3개는 이미지)
* => 긴 컨텍스트를 봐야만 하는 태스크들을 잘 해결하기 위함
* Reformer, Longformer 등 여러개가 나왔는데, 뭐가 뭔지 모르겠다. 우리가 딱 정해줄게! 하는 논문
* 제약조건도 딱 걸어놓고, 실험 진행
* {MAX 4 3 {MIN 2 3 1 1 0 {MEDIAN 1 5 8 9, 2}} 요런 문제
* Byte-level로 풀자! 서브워드 말고
* 이전 문맥을 보는 것들 중에서 어떤 것들은 이전 문맥이 똑같이 중요한 것도 있고, 지날수록 중요도가 떨어지는 태스크도 있을 수 있음 (떨어짐는 태스크 : 자연어, 앞이 더 중요한 경우: ListOps)
* 결과적으로 얘기하면 Base-Transformer가 짱이다
* 성능은 BigBird가 제일 좋았음
* Sparse Trans와 Performer의 경우는 ListOps에서는 폭망했지만, 나머지 태스크에서는 매우 좋은 성능을 보임 => ListOps 같이 앞이 더 중요한 경우를 제외하고는 이 2개가 더 좋은 것 아닐까? (Ryan 의견)
* 어쨌든 현존하는 모델은 아직은 성능이 좋지 않다.
* 이 논문이 나옴으로써 추후에 많은 모델들이 나올 것 같다
