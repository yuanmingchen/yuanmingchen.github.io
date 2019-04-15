---
layout: post
title: "Aspect Based Sentiment Analysis with Gated Convolutional Networks"
description: "Aspect Based Sentiment Analysis with Gated Convolutional Networks"
slug: ABSA_GCN
category: paper
tags: []
---
{% include JB/setup %}
这篇论文介绍了基于方面的情感分析，提出了一种基于卷积神经网络和门控单元的神经网络，叫做GCAE（Gated Convolutional network with Aspect Embedding），没有使用RNN，大大地提高了模型训练的可并行性，使得训练速度大幅度加快，并且模型的效果有提高，虽然提高不多，但是能在不使用RNN的情况下也算是不错了，毕竟该模型最大的优点是提高训练速度，节省训练时间。

**论文地址**：<https://www.aclweb.org/anthology/P18-1234>  
**论文笔记**：<https://zhuanlan.zhihu.com/p/50284374>  
**论文代码**：<https://github.com/wxue004cs/GCAE>  
**数据**：SemEval2014 Task4
### （1）模型创新与贡献
与一般模型不同，作者认为使用RNN和Attention机制都会使得模型的训练过程难以并行运算，这大大影响了训练速度，因此尝试仅仅使用CNN和一些门控单元就在保证模型效果的情况下，提高训练速度，毕竟CNN是可并行运算的。最后，事实证明，作者确实提高了训练速度，并且模型准确率比之前的模型还要高一些。 
与其他模型的收敛时间对比如下如所示：  
![收敛时间对比](/images/posts/absa_with_gcn-1.png)   
另外，作者将传统的ABSA任务分为了两类：分别是ACSA任务和ATSA任务。
- 第一种叫做ACSA任务，即aspect-category sentiment analysis。该种任务的aspect是事先确定的一些类别，并且可能该aspect在句子中不出现，比如service，price这些，“这件衬衫居然要1000块钱！”就是针对“价格”的情感句，但是价格这个词并不会直接出现。  
- 第二种叫做ATSA任务，即aspect-term sentiment analysis。与第一种相反，这种ABSA任务的aspect不是事先确定的，而是从句子中提取出来的，因此该种任务的aspect必然在句子中出现，可能是一个词，也可能是短语，说白了就是找没个短句的评价aspect，这种aspect通常会比较多，毕竟大部分句子的评价aspect都不同。  
举个例子，比如评论“这些泰国菜非常好吃，但是配送太慢了！”。对于ACSA来说，这两个短句的aspect可能分别是“口感”、“服务”。对于ATSA来说，则是“泰国菜”，“配送”。这个例子可能不准确，比较泰国菜不算是aspect，意会即可。

### （2）模型介绍
作者的模型如下图所示：  
![模型结构](/images/posts/absa_with_gcn-2.png){: width="60%"}

如上图所示，首先使用两个卷积层独立对句子进行卷积操作，然后其中一个输出（图中红色输出）再经过一个Tanh门控单元负责提的情感特征，另外一个输出（图中绿色输出）则负责提取aspect的特征，所以它需要与Aspect的向量进行组合【作者采用的是相加的方式，详细见下面的公式】，然后经过ReLU门控单元，作为Aspect特征。这两个门控单元就是作者设计的门控单元。最后让两个门控单元的输出【情感特征和Aspect特征】进行按位乘的操作，其结果再经过最大池化层处理作为最终该Aspect对应的情感信息，把该向量经过输出层进行分类。具体计算过程如下公式：

$$\begin{aligned} a_{i} &=\operatorname{relu}\left(\mathbf{X}_{i : i+k} * \mathbf{W}_{a}+\mathbf{V}_{a} \boldsymbol{v}_{a}+b_{a}\right) \\ s_{i} &=\tanh \left(\mathbf{X}_{i : i+k} * \mathbf{W}_{s}+b_{s}\right) \\ c_{i} &=s_{i} \times a_{i} \end{aligned}$$  

其中${X}_{i : i+k}$代表第$i$个单词到第$i+k$个单词，$*W+b$代表卷积操作。$v_a$代表Aspect的向量，进行线性变换以后与卷积层输出加起来求和。  
显然，上述模型是针对ACSA任务的，因为它需要有一个Aspect的词向量作为输入。针对ATSA任务，作者对上述模型做出了一点改进，使得它也可以用于ATSA任务。思路非常简单，既然没有事先确定的Aspect向量，我们就自己去寻找Aspect，但是不应该使用RNN来找Aspect，这回使得模型难以并行计算，与初衷背道而驰。因此，作者又使用了一个与之前类似的卷积层来找出句子中Aspect，模型结构如下图所示：  
![](/images/posts/absa_with_gcn-3.png){: width:100%}

### （3）实验结果
作者使用的数据集是SemEval2014Task4的数据集，数据分布情况如下：
![](/images/posts/absa_with_gcn-4.png){: width:100%}

最终的实验结果如下所示，首先是与其他模型的正确率Acc的对比：
![](/images/posts/absa_with_gcn-5.png){: width:100%}

然后，作者为了展示门控单元确实是有效的，对数据结果做了可视化，如下图所示，可以看到对于不同的aspect，其对应的评价词与其他评价词的权重有明显差异，可以说，作者的这个门控机制起到了类似Attention的作用：
![](/images/posts/absa_with_gcn-6.png){: width:100%}

另外，作者还通过具体实验证明自己的门控机制比其他常用的门控单元效果要好【如下表所示】，确实可以有效控制模型找到与aspect相关的情感词，忽略不相关的其他情感词。
![](/images/posts/absa_with_gcn-7.png){: width:100%}
