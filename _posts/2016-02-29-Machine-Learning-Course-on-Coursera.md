---
layout: post
title:  "Machine Learning Course on Coursera"
date:   2016-02-29 00:00:00 +0900
categories: jekyll update
type: Tensorflow
excerpt_separator: <!--more-->
---
For the two months that I have sacrificed my spare time to do this :-)
<!--more-->

*A bit summery for main topics I’ve learned from this machine learning course:*

### supervised learning:
- linear regression
- logistic regression
- neural networks
- SVMs

### unsupervised learning:
- kmeans
- PCA
- anomaly detection

### special topics:
- recommender system
- large scale machine leaning(mapReduce, parallel computing, CUDA)

### Insight:
- bias/variance, regularization
- deciding what to do next: evaluation of leaning algorithms
- learning curves, error analysis, ceiling analysis.


What's Next?
---
Next step is deciding what you would like to focus on.  Someone just gave a few that are interesting to pursue but they're by no means encompassing all that is going on in the field right now.

### 1. Computer Vision

For building vision system, whether for photos or videos.

Course:

- [CS231n Convolutional Neural Networks for Visual Recognition][R6]

What you will learn in the course:

- the state-of-art convolutional neural networks
- set up GPU instance in AWS

Follow up would be like:

- scene labeling by combining with Recurrent Neural networks such as LSTM.

### 2. Natural Language Processing

For machine translation, question and answering, sentiment analysis, etc.

Course:

- [CS224d: Deep Learning for Natural Language Processing][R7]

What you will learn in the course:

- word embedding (word2vec, GLove)
- bi-directional LSTMs for translation, etc.

### 3. Deep Reinforcement Learning

- Deepmind's publication section: [Publications - Google DeepMind][R8]
- [Professor Rich Sutton's book][R9] is must go-to place to start learning RL
- and supplement it with David Silver's (prominant researcher at Google Deepmind) UCL video lectures: [Advanced Topics: RL][R10]
- [LEARNING REINFORCEMENT LEARNING (WITH CODE, EXERCISES AND SOLUTIONS)][R11]


Others You Could Do
---

Suggested by [サービスをつくるエンジニアが機械学習を学ぶべき3つの理由][R12], you could probably read the following resources.

- [Rules of Machine Learning: Best Practices for ML Engineering][R13]
- [Kaggle Data Science Use Cases][R14]
- [DataQuest][R15]
- [Hands-On Machine Learning with Scikit-Learn and TensorFlow][R16]
- [Python機械学習プログラミング][R17] ([監訳者による読み方][R18]が参考になる)
- [戦略的データサイエンス入門][R19]


See What Other Engineers Read
---
- [Machine Learning Reading resources][R20]
    - [9 Lessons Learned from a Decade Spent Developing Machine Learning Products][R21]
    - [CNN(卷积神经网络)、RNN(循环神经网络)、DNN(深度神经网络)的内部网络结构有什么区别？][R22]
- [ML Materials Collection (by a self-driving car engineer)][R23]
- [Lessons Learned from Deploying Deep Learning at Scale][R24]


Reference
---
- [Stanford University’s Machine Learning course on Coursera][R1]
- [Stanford Unsupervised Feature Learning and Deep Learning Tutorial][R2]
- [7 Steps to Mastering Machine Learning With Python][R3]
- [Neural Networks and Deep Learning free online book][R4]
- [I have completed Andrew Ng's Coursera class on Machine Learning. What should I do next? What *can* I do next?][R5]
- [サービスをつくるエンジニアが機械学習を学ぶべき3つの理由][R12]


[R1]: https://www.coursera.org/learn/machine-learning/
[R2]: http://ufldl.stanford.edu/tutorial/
[R3]: http://www.kdnuggets.com/2015/11/seven-steps-machine-learning-python.html/2
[R4]: http://neuralnetworksanddeeplearning.com/
[R5]: https://www.quora.com/I-have-completed-Andrew-Ngs-Coursera-class-on-Machine-Learning-What-should-I-do-next-What-*can*-I-do-next
[R6]: http://cs231n.github.io/
[R7]: http://cs224d.stanford.edu/
[R8]: http://deepmind.com/publications.html
[R9]: https://webdocs.cs.ualberta.ca/~sutton/book/ebook/the-book.html
[R10]: http://www0.cs.ucl.ac.uk/staff/d.silver/web/Teaching.html
[R11]: http://www.wildml.com/2016/10/learning-reinforcement-learning/
[R12]: http://developers.freee.co.jp/entry/3-reasons-machine-learning-for-application-engineers
[R13]: http://martin.zinkevich.org/rules_of_ml/rules_of_ml.pdf
[R14]: https://www.kaggle.com/wiki/DataScienceUseCases
[R15]: http://dataquest.io/
[R16]: http://shop.oreilly.com/product/0636920052289.do
[R17]: https://www.amazon.co.jp/dp/4844380605
[R18]: https://thinkit.co.jp/article/9926
[R19]: https://www.amazon.co.jp/dp/4873116856
[R20]: https://github.com/justlaputa/reading
[R21]: http://media.bemyapp.com/lessons-learned-decade-spent-developing-machine-learning-products/
[R22]: https://www.zhihu.com/question/34681168/answer/84061846
[R23]: http://www.elvawyf.com/2016/09/17/en/Materials/#more
[R24]: http://blog.algorithmia.com/deploying-deep-learning-cloud-services/