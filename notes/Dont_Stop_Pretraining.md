## Title
Don’t Stop Pretraining: Adapt Language Models to Domains and Tasks (ACL 2020)

### References
- arXiv: https://arxiv.org/abs/2004.10964
- code: https://github.com/allenai/dont-stop-pretraining

### Summary
- DAPT (Domain Adaptive PreTraining): Continue to pretrain on some text of the domain relevant to the target downstream task.
  - For example, suppose we tackle CHEMPROT (relation classification of chemicals), then we keep training on some biomdecial text.
  - DAPT is effective particularly when the target domain is specialized such as the biomed.
- TAPT (Task Adaptive PreTraining): Continue to pretrain on the training data of the target task.
  - For example, we keep training on the training set of the CHEMPROT task.
  - TAPT is cheaper than DAPT but still effective.
- DAPT + TAPT is the most powerful in most cases.
- If large amounts of unlabeled data for TAPT are not available, simple data selection technique based on kNN is helpful.
