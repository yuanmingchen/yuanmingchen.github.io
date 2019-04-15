---
layout: post
title: "Aspect Based Sentiment Analysis with Gated Convolutional Networks"
description: "Aspect Based Sentiment Analysis with Gated Convolutional Networks"
slug: ABSA_GCN
category: paper
tags: []
---
{% include JB/setup %}
**论文地址**：<https://www.aclweb.org/anthology/P18-1234>  
**论文笔记**：<https://zhuanlan.zhihu.com/p/50284374>  
**论文代码**：<https://github.com/wxue004cs/GCAE>  
这篇论文介绍了基于方面的情感分析，提出了一种基于卷积神经网络和门控单元的神经网络，叫做GCAE（Gated Convolutional network with Aspect Embedding），没有使用RNN，大大地提高了模型训练的可并行性，使得训练速度大幅度加快，并且比之前基于RNN（GRU、LSTM等）的模型的效果要有提高，虽然提高不多，但是能在不使用RNN的情况下也算是不错了，毕竟该模型最大的优点是提高训练速度，节省训练时间。
