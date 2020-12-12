# If beam search is the answer, what was the question?
* https://arxiv.org/abs/2010.02650
* EMNLP 2020

## 핵심 내용
* Greedy search: 매 step 1등만 뽑음. 심플. 성능 별로. beam width=1
* Beam Search: cut off. 성능 good. 복잡.
* MAP: cut off 없음. 즉 참가자(vocab) 전원 고려. beam width=len(vocab). intractable. 성능 별로. empty가 많이 생성.
* 왜 Beam Search가 MAP보다 잘 되나?
* UID: 정보의 분포가 골고루 있을때 사람들이 더 좋아한다.
* MAP는 전체 문장의 스코어만 고려하지 분산을 고려하지 않는다.
* Beam search는 local 정보에 집중하기 때문에 전체적으로 고른 정보량을 갖게 된다.
* 이를 이용해 이 논문에서는 정보의 분산을 작게 하기 위해 regularizers를 도입해 beam search 성능을 향상시켰다.
* 매우 흥미로운 생각.
* 그러나, 최고 성능을 내는 beam width를 줄인 것은 아니다. 즉, 복잡도나 속도 면에서의 향상은 없다.
* beam search path를 예측할 수 있다면 정말 좋겠다.
