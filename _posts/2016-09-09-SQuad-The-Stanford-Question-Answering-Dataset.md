---
layout: post
title:  "SQuad: The Stanford Question Answering Dataset"
date:   2016-09-09 00:00:00 +0900
categories: jekyll update
type: Tensorflow
excerpt_separator: <!--more-->
---
<!--more-->

What is SQuAD?
---
>Stanford Question Answering Dataset (SQuAD) is a new reading comprehension dataset, consisting of questions posed by crowdworkers on a set of Wikipedia articles, where the answer to every question is a segment of text, or span, from the corresponding reading passage. With 100,000+ question-answer pairs on 500+ articles, SQuAD is significantly larger than previous reading comprehension datasets.


Getting Started with the Dataset
---
To get a feel of the dataset, you can explore it visually:

- [Explore SQuAD][R3]

Download a copy of the dataset (distributed under the [CC BY-SA 4.0][R6] license):

- [Training Set v1.1 (30 MB)][R4]
- [Dev Set v1.1 (5 MB)][R5]

To evaluate your models, we have also made available the evaluation script we will use for official evaluation, along with a sample prediction file that the script will take as input. To run the evaluation, use `python evaluate-v1.1.py <path_to_dev-v1.1> <path_to_predictions>`.

- [Evaluation Script v1.1][R7]
- [Sample Prediction File (on Dev v1.1)][R8]

Once you have a built a model that works to your expectations on the dev set, you submit it to get official scores on the dev and a hidden test set. To preserve the integrity of test results, we do not release the test set to the public. Instead, we require you to submit your model so that we can run it on the test set for you. Here's a tutorial walking you through official evaluation of your model:

- [Submission Tutorial][R9]


Sweetie of the Day
---
![Imgur](http://i.imgur.com/uiobsPb.jpg)
#ladyM #Seoul


Reference:
---
[SQuAD，斯坦福在自然语言处理的野心][R1]<br />
[SQuAD][R2]

[R1]: http://www.leiphone.com/news/201608/ftBdq445PzC1kxbF.html
[R2]: https://rajpurkar.github.io/SQuAD-explorer/
[R3]: https://rajpurkar.github.io/SQuAD-explorer/explore/1.1/dev/
[R4]: https://rajpurkar.github.io/SQuAD-explorer/dataset/train-v1.1.json
[R5]: https://rajpurkar.github.io/SQuAD-explorer/dataset/dev-v1.1.json
[R6]: http://creativecommons.org/licenses/by-sa/4.0/legalcode
[R7]: https://worksheets.codalab.org/rest/bundles/0xbcd57bee090b421c982906709c8c27e1/contents/blob/
[R8]: https://worksheets.codalab.org/rest/bundles/0xc83bf36cf8714819ba11802b59cb809e/contents/blob/
[R9]: https://worksheets.codalab.org/worksheets/0x8403d867f9a3444685c344f4f0bc8d34/

