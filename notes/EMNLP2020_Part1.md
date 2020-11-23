## TOD-BERT: Pre-trained Natural Language Understanding for Task-Oriented Dialogue

---

- Chien-Sheng Wu, Steven Hoi, Richard Socher, and Caiming Xiong
- Salesforce Researc

### Reference

- [EMNLP2020](https://arxiv.org/pdf/2004.06871.pdf)

### Summary

- There is an **underlying difference** in **linguistic** patterns between **general text** and **TOD**
- This means that **existing pre-trained language mode**l **less useful** in practice
- Hence, they **unify nine TOD datasets** for language modeling while applyingz**contrastive loss**

### Linguistic Difference

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled1.png)

### Aim of Paper

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled2.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled3.png)

### Model

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled4.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled5.png)

### Response Contrastive Loss

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled6.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled7.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled8.png)

### Finetune

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled9.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 10.png)

## Intent Classification (151 classes)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 11.png)

### Dialog Act Classification (13 classes)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 12.png)

### Dialogue State Tracking

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 13.png)

### Response Selection

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 14.png)

---

## Multi-turn Response Selection using Dialogue Dependency Relations

---

- Qi Jia, Yizhu Liu, Siyu Ren, Kenny Q. Zhu
- Shanghai Jiao Tong University

### Reference

- [EMNLP 2020](https://arxiv.org/pdf/2010.01502.pdf)

### Summary

- Simply **concatenating turns** in dialogue **ignores dependencies** between turns
- **Dialogue Extraction** can form a number of threads each of which is related
- Propose  **Thread-Encoder** to encode threads which helps selecting an optimal response

### Turns are dependent

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 15.png)

### Concatenation could be a problem

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 16.png)

### Main Contributions

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 17.png)

### Dialogue Extraction

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 18.png)

### Thread Encoder

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 19.png)

### SOTA and Comparative

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 20.png)

---

## Learning a Simple and Effective Model for Multi-turn Response Generation with Auxiliary Tasks

---

- Yufan Zhao, Can Xu, Wei Wu
- Microsoft Corporation, Beijing

### Reference

- [EMNLP2020](https://arxiv.org/pdf/2004.01972.pdf)

### Summary

- Deep learning models bring successful results in **response generation**
- Yet, their **complexity** hinders the application of the models in real systems
- Propose a **simple** structure yet can effectively leverage **conversation contexts**

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 21.png)

### Auxilary Tasks

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 22.png)

### Model

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 23.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 24.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 25.png)

### Loss Function

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 26.png)

### Result

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 27.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 28.png)

### Further Analysis on Auxiliary Tasks

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 29.png)

### Q1 & Q2

- (a) Require at least 5 encoders
- (b) Require at least 7 encoders
- (c) Require at least 4 encoders

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 30.png)

### Q3

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 31.png)

---

## **Tasty Burgers, Soggy Fries: Probing Aspect Robustness in Aspect-Based Sentiment Analysis**

---

- Xiaoyu Xing, Zhijing Jin, Di Jin, Bingning Wang, Qi Zhang, Xuanjing Huang
- Fudan University, Max Planck Institue, CSAIL, MIT

### Reference

- [EMNLP 2020](https://arxiv.org/pdf/2009.07964.pdf)

### Summary

- Test robustness of various ABSA model on **contrastive** dataset (ARTS)
- Observe huge drop for all models
- BERT models are most robust

### Aspect Robustness Test Set (ARTS)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 32.png)

### Strategy

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 33.png)

### Data Statistics

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 34.png)

### Result

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 35.png)

### Effectively Model the Aspect

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 36.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 37.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 38.png)

---

## Utilizing BERT for Aspect-Based Sentiment Analysis via Constructing Auxiliary Sentence

---

- Chi Sun, Luyao Juang, Xipeng Qiu

### Reference

- [NACCL 2019]

### Summary

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 39.png)

### Dataset

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 40.png)

### Auxiliary Tasks

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 41.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 42.png)

### Result

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled 43.png)
