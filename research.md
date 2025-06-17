---
layout: default
---

Medicine: Safe+Natal
============
I am working with [safe+natal](https://safenatal.org/){:target="_blank"} (joint project with Emory University and [Wuqu' Kawoq](https://www.wuqukawoq.org){:target="_blank"}) on improving pregnancy healthcare in rural Guatemala through ML-based diagnostics and triage. More specifically, the goal of this project is to provide a simple system that enables rural midwives from underserved communities to conduct basic pregnancy monitoring and identify cases that need to be referred to hospitals. We distributed $200 health monitor kits (including smartphone and very basic devices such as blood pressure monitors and 1-dimensional doppler ultrasounds) to local midwives. Indigenous Maya communities do not commonly use written language, so we automatically transcribe and interpret readings from the health devices to enable timely healthcare decisions. Safe+Natal is tremendously successful: integrated with a Mayan hospital, it reduced maternal mortality from one of the highest in the Western Hemisphere to nearly zero! 

My role in this larger project is to develop and test image recognition software that transcribes readings from an image of a blood pressure monitor (taken on the smartphones given to midwives). The challenge in this is to create an accurate system that works with all types of lighting and backgrounds.

[Paper linked here](https://arxiv.org/abs/2503.09659){:target="_blank"}.


Crypto: Private Repair of User-Flagged Failures in Text-to-Image Services
============
This project focuses on using cryptographic tools (namely secure multiparty computation) to securely and privately remove bias from generative AI models. Consider the setting where a private pre-trained model may be biased, e.g., it only generates images of men (not women) in leadership positions. The goal is to use an external party’s advice (which is also private) to fine-tune the model to eliminate the specific bias. We achieve this by computing on encrypted data.

Work done as a part of an internship with [Prof. Xiao Wang (Northwestern University)](https://wangxiao1254.github.io){:target="_blank"}; the resulting paper was submitted to NeurIPS 2025.


Digital Humanities: Identifying the author of a Medieval text
============
The goal of this work is to identify the author of a medieval text - the *Passio Et Miracula Sancti Eadwardi Regis Et Martyris*. Our work proposes, with much evidence, an author for this text, disproving previous beliefs. 

Our approach to authorship identification is three-pronged: through traditional human analysis, stylometric analysis, and machine learning. I was responsible for the machine learning part of this project; I trained an accurate model to distinguish the author (among four possible choices) of Latin text. When running the Passio through my model, it classified the text identically to my co-authors' traditional and stylometric analyses, thus corroborating their results. 

The resulting paper is in preparation for submission to a history journal.


Misc. individual projects
============
- [Some coding projects](https://github.com/ellenkolesnikova/projects){:target="_blank"} - several projects ranging from basic data structures to assembly code and AI
- [A simple technical introduction to ZK proofs](https://ellenkolesnikova.github.io/assets/files/zkintro.pdf){:target="_blank"} - I wrote this when studying zero-knowledge proofs
- [A paper introducing backpropagation in neural networks](https://ellenkolesnikova.github.io/assets/files/neuralnetworks.pdf){:target="_blank"} - I wrote this for my AP calc class