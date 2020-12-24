## Attention is Not Only a Weight: Analyzing Transformers with Vector Norm

Goro Kokayashi, Tatsuki Kuribayashi, Sho Yokoi, Kentaro Inui

## References

- [[EMNLP 2020](https://arxiv.org/pdf/2004.10102.pdf)]

## 1.  Summary

---

- Study **attention weights** of Transformer and BERT
- Reveal that **attention weights** alone are only **one of the two factors** that determine the output of attention
- The second factor, **the norm of the transformed input vectors** must be taken into account

## 2. Background : Attention Mechanism

---

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img0.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img1.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img2.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img3.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img4.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img5.png)

- The attention mechanism has been designed to **update representations** by gathering ***relevant information*** from the input vectors
- Analysis solely based on attention weight assumes that the **larger the attention weight** of an input vector**, the higher its contribution** to the output (i.e. weight-based)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img6.png)

- However, this **disregards** the **magnitudes of the transformed vectors**

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img7.png)

- The **attention weight** $**a_3**$ of the **first token** $**x_1$** is large, but its **transformed vector**, $f(x_1)$, is small, resulting in low contribution to the output
- On the other hand,  **third token x3** has small **attention weight $a_3$**  but it's **transformed vector, $f(x_3)$**, is large enough to lead high contribution to the output
- Hence, one needs to measure the **norm of the weighted transformed vector, $a||f(x)||$** to analyze attention mechanism behavior

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img8.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img9.png)

## 3. BERT Experiment

---

- Use pre-trained BERT-base
- Wikipedia data where each sequence : [CLS] **PARAGRAPH1** [SEP] **PARAGRAHP2**  [SEP]

### Measure of Coefficient Variation (CV)

- Compute CV to demonstrate the degree to which $a||f(x)||$ differs from $a$

    ![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img10.png)

- There is a clear **variation of** $f(x)$  among attention heads
- Therefore, there is a **difference between $a$ and $a||f(x)||$  due to the dispersion of $f(x)$**

### Weight-based (WB) VS Norm-based (NB)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img11.png)

- The two settings exhibit completely different behavior
    - While **special tokens** *(i.e. CLS, SEP and punctuations)* have **remarkably large attention weights under weight-based**
    - The contributions of vectors corresponding to these tokens are  **very small for norm-based**
    - This leads to the conclusion that the **transformed vector $f(x)$ plays a huge role** in **controlling the amount of information obtained** from these special tokens

### Relationship between $a$ and $||f(x)||$

- We know that **[SEP] token** carries **not much information**
- It's unclear how attention collects only a little information while **assigning high attention to a specific token, [SEP]**
- Below is interesting evidence that $**a$ and $||f(x)||$  cancel each other out**

- Evidence 1

    The **attention weight** and **transformed norm** of [SEP] are negatively correlated

    ![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img12.png)

- Evidence 2

    Layers where the attention weight $a$ are large tend to have small transformed norm $||f(x)||$

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img13.png)

### Relationship between Frequency and $||f(x)||$

- Hypothesize that BERT controls contributions of **highly frequent but less informative words by adjusting $||f(x)||$**
- Strong **positive correlation** between **rank of a token** and $||f(x)||$

       → **Frequent** **tokens** (e.g. [SEP], punctuations) with **low rank tend to have small $||f(x)||$**

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img14.png)

- Conclusion
    - BERT **reduce the information** from highly frequent words by adjusting $||f(x)||$ and not $a$
    - This frequency-based effect is consistent with the intuition that highly frequent words such as **stop words**, are **unlikely to play an important role** in solving the pre-training tasks

    ### Transformer NMT Experiment

    - **Alignment Error Rate** (AER)
        - Low AER indicates that the **extracted word alignments are close to the reference**

            ![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img15.png)

        - Result
            - Norm-based 에서 더 낮은 AER

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img16.png)

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/attention_is_not_only_a_weight/img17.png)