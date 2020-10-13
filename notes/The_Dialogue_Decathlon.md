## The Dialogue Dodecathlon: Open-Domain Knowledge and Image Grounded Conversational Agents

Kurt Shuster, Da Ju, Stephen RollerEmily Dinan, Y-Lan Boureau, Jason Weston

Facebook AI Research

## Reference

- [arXiv](https://arxiv.org/pdf/1911.03768.pdf)


## Abstract

- Introduce ***dodeca*Dialogue** : a set of 12 tasks that measure a range of skills of a conversational agent in an *open-domain*
    - communicate engagingly with **personality and empathy**
    - ask questions & answer questions by utilizing **knowledge** resources
    - discuss topics and **situations**
    - perceive and converse about **images**
- Throughout multi-tasking on a large-scale dataset, it is expected that *a single unified agent* can possess above abilities

## Introduction

- No single task exists that allows an agent have such abilities at once
- However, a number of distinct large-scale datasets targeting subsets of these skills are available
- Thus, assemble separate tasks and form a single challenge : *dodecaDialogue*, consisting of 12 tasks
- While existing approaches use large-scale pre-training on general text corpora, we show that using dialogue datasets instead is a strong alternative

## The dodecaDialogue Task

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/The_Dialogue_Dodecathlon/image1.png)

An agent should be able to...

- get to know about you (ConvAI2)
- discuss everyday topics (DailyDialog, Reddit, Twitter, Cornell Movie)
- speak knowledgeably at depth (Wizard of Wikipedia, Ubuntu)
- answer questions on topics (ELI5)
- handle various situation and demonstrate empathy (Empathetic Dialog, LIGHT)
- discuss images (Image Chat, IGC)

There are much more datasets which are not included in dodecaDialogue

- choices were made based on tasks that **after training might produce an engaging dialogue agent**
- Hence either **(1) natural datasets** or **(2) crowdsources where crowdworkers were encourage to engage**

## Models

### BERT baseline

- a generative baseline using BERT via adapting a standard auto-regressive loss
- cannot handle image feautres

### Image+Seq2Seq

- additionally adding **image features** from pre-trained ResNeXt model
- add this vector (after a linear projection) to the end of  Transformer encoder's output and feed them all into the decoder
- during fine-tuning, only Transformer is fine-tuned

## Experiements

### Pre-training

- Pre-train the Seq2Seq module of Image+Seq2Seq on **Reddit or Twitter** dataset (which is much larger than other datasets)

### Preliminary Study

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/The_Dialogue_Dodecathlon/image2.png)

- Test on subset of dodecaDialogue : Reddit, ConvAI2, Wizard of Wikipedia and Empathetic Dialogues
- Reveals that training on **Reddit alone is effective at transfer** to other tasks
    - But obviously never as effective as fine-tuning on the task itself
- Training on all four tasks (**i.e. Multi-Tasking**) is the most effective strategy on average !
- Now apply similar approach to dodecaDialogue

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/The_Dialogue_Dodecathlon/image3.png)

### Comparison of PreTraining + FineTuning (**Reddit + Single Task**)

- Pre-training with Reddit corpus is beneficial
- Its dodecaScore (17.1) is much less than **BERT-based**, **Single Task**, **Twitter + Single Task**
- The hypothesis here is that Reddit yields much more effective transfer as it is a dialogue task whereas Wikipedia are not
- It's also impressive that **Reddit Only** result is better than other Single Task results
    - Especially for IGC whose training instance (~4k) is relatively small and likewise for Wiz. of Wikipedia and Empathetic Dialog

### Multi-Task Results (**All Tasks MT**)

- Treat all 12 tasks as a single task
- The best model was chosen based on the average performance across all tasks
- It achieves dodecaScore of 19.1 - superior to **Reddit Only** and all pre-train and fine-tune strategy except **Reddit + Single Task** but only slightly less
- It was reported that no clear improvements were found from **Reddit + All Tasks MT**

### Multi-Task followed by FineTuning (**MT All Tasks + FT Single Task**)

- Train in a multi-task manner on all tasks, before fine-tuning on a single task
- Build a separate model for all tasks, in an attempt to improve single task result further
- It achieves dodecaScore of 16.8 - the best result among all strategies - but only marginally less than **Reddit + Single Task**

### Task Up-Weighting

- apply relative task up-weighting during multi-tasking training was helpful

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/The_Dialogue_Dodecathlon/image4.png)

### Zero-Shot Performance

- Training on all but one of the target tasks
- Its dodecaScore is 31.1 - outperform three Single Tasks and BERT baseline
- its performance drop is mainly due to Reddit task whose PPL was 106.3

## Comparison to Existing Systems

![](https://github.com/kakaobrain/nlp-paper-reading/blob/master/images/The_Dialogue_Dodecathlon/image5.png)

## Summary

- Introduce ***dodeca*Dialogue** : a set of 12 tasks that measure a range of skills of a conversational agent in an open-domain
- Pretrain the agent on Reddit corpus was extremely effective
- Multi-tasking on all of ***dodeca*Dialogue** and then fine-tune on single-task was the best strategy
