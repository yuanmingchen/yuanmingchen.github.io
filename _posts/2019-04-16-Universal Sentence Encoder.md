---
layout: post
title: "Universal Sentence Encoder"
description: "Universal Sentence Encoder论文笔记"
category: paper
slug: "USE"
tags: []
---
{% include JB/setup %}
论文主要是提出了一个统一的句子编码框架，句子级别的encode比Word2vec使用起来会更加的方便，因为可以直接拿来做句子分类等任务。

本文主要提出了两个句子encode的框架，一个是之前《attention is all you need》里面的一个encode框架，另一个是DAN（deep average network）的encode方式。两个的训练方式较为类似，都是通过多任务学习，将encode用在不同的任务上，包括分类，生成等等，以不同的任务来训练一个编码器，从而实现泛化的目的。

**论文地址**：<https://arxiv.org/pdf/1803.11175v2.pdf>
**论文笔记**：
- [paperweekly-很简略](https://www.paperweekly.site/papers/notes/577)
- [知乎-木子李](https://zhuanlan.zhihu.com/p/35174235)
 
**论文代码**：
- 作者将代码用TensorFlow实现并上传到了TF Hub，详见： <https://tfhub.dev/google/universal-sentence-encoder/2>
- [更多代码实现点这里](https://paperswithcode.com/paper/universal-sentence-encoder)  【不过我看paperswithcode列出的代码好像都不是论文实现】
