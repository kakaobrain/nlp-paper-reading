## When BERT Plays the Lottery, All Tickets Are Winning

Sai Prasanna, Anna Rogers, Anna Rumshisky

## References

- [[EMNLP 2020](https://arxiv.org/pdf/2005.00561.pdf)]

## Summary

---

- Apply **Magnitude** Pruning and **Structured** Pruning to explore "**good**" subnetworks
- There exists "**good**" subnetworks for each downstream task
- But they are **unstable** and cannot be explained by self-attention

## 1. Abstract

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img0.png)

## 2. Related Work

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img1.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img2.png)

## 3. Methodology

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img3.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img4.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img5.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img6.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img7.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img8.png)

## 4. BERT Plays the Lottery

### 4.1 The "Good" Subnetworks

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img9.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img10.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img11.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img12.png)

### 4.2 Testing LTH for BERT Fine-tuning

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img13.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img14.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img15.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img16.png)

### 4.3 How Bad are the "Bad" Subnetworks?

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img17.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img18.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img19.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img20.png)

## 5. Interpreting BERT's Subnetworks

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img21.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img22.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img23.png)

## 7. Conclusion

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/when_bert_plays_the_lottery_all_tickets_are_winning/img24.png)