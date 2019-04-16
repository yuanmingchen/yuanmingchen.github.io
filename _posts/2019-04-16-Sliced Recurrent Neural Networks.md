---
layout: post
title: "Sliced Recurrent Neural Networks"
description: "Sliced Recurrent Neural Networks论文地址"
category: paper
slug: "SRNN"
tags: []
---
{% include JB/setup %}
论文提出了一种新的RNN结构--切片循环神经网络（SRNN），并且在情感分析任务上取得了remarkable结果。所谓的切片指的是将输入序列分割成若干最小序列(最小序列长度由n和k决定，其中n表示每次分割的子序列个数，k表示分割的次数)。

原文将最小序列作为输入序列输入多层循环神经网络中，并且使用GRU作为递归单元。这样不仅可以做到并行计算大幅度提高训练速度，而且可以将重要的信息完整的从底层传递到顶层，从而更快更准确地完成情感分析。
**论文地址**：<https://arxiv.org/ftp/arxiv/papers/1807/1807.02291.pdf>  
**论文笔记**：<https://zhuanlan.zhihu.com/p/44526426>