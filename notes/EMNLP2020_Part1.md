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

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled.png" width="70%">

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%201.png" width="70%">

### Aim of Paper

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%202.png" width="70%">

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%203.png" width="70%">

### Model

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%204.png" width="70%">

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%205.png" width="70%">

### Response Contrastive Loss

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%206.png" width="70%">

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%207.png" width="70%">

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%208.png" width="70%">

### Finetune

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%209.png" width="70%">

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2010.png" width="70%">

## Intent Classification (151 classes)

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2011.png" width="70%">

### Dialog Act Classification (13 classes)

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2012.png" width="70%">

### Dialogue State Tracking

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2013.png" width="70%">

### Response Selection

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2014.png" width="70%">
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

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2015.png" width="70%">

### Concatenation could be a problem

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2016.png" width="70%">

### Main Contributions

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2017.png" width="70%">

### Dialogue Extraction

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2018.png" width="70%">

### Thread Encoder

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2019.png" width="70%">

### SOTA and Comparative

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2020.png" width="70%">

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

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2021.png" width="70%">

### Auxilary Tasks

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2022.png" width="70%">

### Model

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2023.png" width="70%">

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2024.png" width="70%">

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2025.png" width="70%">

### Loss Function

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2026.png" width="70%">

### Result

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2027.png" width="70%">

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2028.png" width="70%">

### Further Analysis on Auxiliary Tasks

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2029.png" width="70%">

### Q1 & Q2

- (a) Require at least 5 encoders
- (b) Require at least 7 encoders
- (c) Require at least 4 encoders

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2030.png" width="70%">

### Q3

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2031.png" width="70%">

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

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2032.png" width="70%">

### Strategy

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2033.png" width="70%">

### Data Statistics

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2034.png" width="70%">

### Result

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2035.png" width="70%">

### Effectively Model the Aspect

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2036.png" width="70%">

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2037.png" width="70%">

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2038.png" width="70%">

---

## Utilizing BERT for Aspect-Based Sentiment Analysis via Constructing Auxiliary Sentence

---

- Chi Sun, Luyao Juang, Xipeng Qiu

### Reference

- [NACCL 2019]

### Summary

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2039.png" width="70%">

### Dataset

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2040.png" width="70%">

### Auxiliary Tasks

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2041.png" width="70%">

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2042.png" width="70%">

### Result

<img src="https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/EMNLP_2020/Part1/Untitled%2043.png" width="70%">
