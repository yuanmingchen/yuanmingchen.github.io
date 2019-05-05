---
layout: post
title: "在线教育领域NLP论文调研2"
description: "在线教育领域NLP论文调研2"
category: paper
slug: onlEduSur2
tags: []
---
{% include JB/setup %}
基于之前的粗略调研，我们确定关键词并寻找论文，我们继续调研在线教育领域相关论文的研究内容及方法。

### 1、ACL相关论文
#### （1）Your click decides your fate: Inferring Information Processing and Attrition Behavior from MOOC Video Clickstream Interactions
**论文地址**：<https://www.aclweb.org/anthology/W14-4102>  【EMNLP 2014】
**任务简介**：
该论文的任务是以学生看视频时的点击行为序列为基础，进行一系列的结果分析。

作者使用的交互行为有：Play (Pl), Pause (Pa), SeekFw(Sf), SeekBw (Sb), ScrollFw (SSf), ScrollBw(SSb), RatechangeFast (Rf), RatechangeSlow(Rs).  
基于上述点击行为，作者将不同的点击行为序列定义为不同的行为：
```
- Rewatch: PlPaSbPl, PlSbPaPl, PaSbPlSb, SbSbPaPl, SbPaPlPa, PaPlSbPa
- Skipping:SfSfSfSf, PaPlSfSf, PlSfSfSf, SfSfSfPa, SfSfPaPl, SfSfSfSSf, SfSfSSfSf, SfPaPlPa, PlPaPlSf
- Fast Watching: PaPlRfRf, RfPaPlPa, RfRfPaPl, RsPaPlRf, PlPaPlRf (click group of
Ratechange fast clicks while playing or pausing video lecture content, indicating speeding up)
- Slow Watching: RsRsPaPl, RsPaPlPa,PaPlRsRs, PlPaPlRs, PaPlRsPa, PlRsPaPl (click group of Ratechange slow clicks while playing or pausing video lecture content, indicating slowing down)
- Clear Concept: PaSbPlSSb, SSbSbPaPl, PaPlSSbSb, PlSSbSbPa (a combination of SeekBw and ScrollBw clicks, indicating high tussle with the video lecture content)
- Checkback Reference: SbSbSbSb, PlSbSbSb, SbSbSbPa, SbSbSbSf, SfSbSbSb, SbPlSbSb, SSbSbSbSb (a wave of SeekBw clicks)
- Playrate Transition: RfRfRsRs, RfRfRfRs,RfRsRsRs, RsRsRsRf, RsRsRfRf, RfRfRfRf(a wave of ratechange clicks)
```
然后使用模糊字符串匹配算法对比实际的点击序列与上述序列，匹配程度越高则该行为的权重就越大。
最后基于以上信息，使用L2正则和线性回归，作者分析以下几个问题：  
- 根据学生的点击序列预测学生对特定课程的参与时间
- 对于既定点击序列，预测下一个序列是什么可以导致学习不同的参与度
- 通过点击序列交互行为来预测学生看不完这个视频的概率【不能完成当前这一个视频的概率】
- 通过点击序列交互行为来预测学生不能完成整个课程的概率【不能完成整个课程的概率】，不过这个用的是所有课程的交互情况，独立考虑各个课程的点击序列   

### （2）Identifying Student Leaders from MOOC Discussion Forums through Language Influence   
**论文地址**：<https://www.aclweb.org/anthology/W14-4103>  【EMNLP 2014】  
**论文介绍**：作者的任务是根据语言影响力从mooc论坛中找出学生领导者。从mooc论坛中大规模地识别和理解学生领导者的动机是使得在线学习环境更具有参与性、合作性和指导性的关键。在这篇论文中，我们建议仅根据文本特征来识别学生领袖，或者通过分析他们如何影响其他学生的语言来识别他们。我们提出了一种基于人们对感兴趣的语义话题的词汇选择来衡量语言迁就度的改进方法，并证明了学生领袖确实起到了协调其他学生语言的作用。
一句话说就是，对于给定的主体，通过衡量大家说话时选用的单词来识别出学生中的领导者。  
原理就是人说话时，使用的单词往往会迁就和他们一起说话的人。这种现象叫做语言协调。我个人的理解就是，大家在讨论某个问题时，加入先有人用苹果举了一个例子，那么其他人在讨论的过程中，就更倾向于也用苹果来举例子进行反驳或论证，而不是用菠萝、香蕉之类的，虽然究竟用什么水果举例没有影响。

作者的方法如下：
首先基于语义和语法相似度，通过单词的在句子中相对其他单词出现的统计来把句子构建为词向量。

作者采用以下公式来衡量语言协调度，其中a、b代表两个用户，这个公式表示a向b的语言协调度，$w_k$表示第k个聚类中的一个词$E_{u_{a} \rightarrow u_{b}}^{w_{k}}$表示b使用了$w_k$这个单词，a对b的回复中也使用了$w_k$这个单词，即a对b出现了语言协调。$E_{u_{b}}^{w_{k}}$表示所有使用了$w_k$这个单词的事件。注意，以上只在所有包含k类中单词的评论中进行统计。

$$C^{w_{k}}(a \rightarrow b)=P\left(E_{u_{a} \rightarrow u_{b}}^{w_{k}} | E_{u_{b}}^{w_{k}}\right)-P\left(E_{u_{a} \rightarrow u_{b}}^{w_{k}}\right)$$
$P_{v_a}$



### （3）Predicting Instructor’s Intervention in MOOC forums
**论文地址**：<https://www.aclweb.org/anthology/P14-1141>  【ACL 2014】   
**论文介绍**：
目的是通过论坛评论来预测教师是否会回复当前这个话题。
使用的是线性回归的方法，使用的特征包括两方面，一方面是当前这个话题的发起者、时间、地点、题目等等总体信息，另一方面是该话题下所有帖子的一个综合信息，例如所有评论的点赞数、所有评论发布时间的均值和方差、某些特定词出现的总数等等。

### （4）Predicting MOOC Dropout over Weeks Using Machine Learning Methods  
**论文地址**：<https://www.aclweb.org/anthology/W14-4111> 【EMNLP 2014】   
**论文介绍**：

### （5）Point-of-View Mining and Cognitive Presence in MOOCs:A (Computational) Linguistics Perspective  
**论文地址**：<https://www.aclweb.org/anthology/W14-4105>  【EMNLP 2014】  
**论文介绍**：

### （6）A Process for Predicting MOOC Attrition
**论文地址**：<https://www.aclweb.org/anthology/W14-4109>  【EMNLP 2014】  
**论文介绍**：

### （7）Understanding MOOC Discussion Forums using Seeded LDA
**论文地址**：<https://www.aclweb.org/anthology/W14-1804>  【Proceedings of the Ninth Workshop on Innovative Use of NLP for Building Educational Applications 2014】  
**论文介绍**：

### （8）Towards Identifying the Resolvability of Threads in MOOCs
**论文地址**：<https://aclweb.org/anthology/W14-4104>  【EMNLP 2014】  
**论文介绍**：

### （9）Semi-Supervised Answer Extraction from Discussion Forums
**论文地址**：<https://www.aclweb.org/anthology/I13-1001>  【International Joint Conference on Natural Language Processing】
**论文介绍**：

### （10）Predicting Attrition Along the Way: The UIUC Model
**论文地址**：<https://www.aclweb.org/anthology/W14-4110>  【EMNLP 2014】  
**论文介绍**：

### （11）Countering Position Bias in Instructor Interventions in MOOC Discussion Forums
**论文地址**：<https://www.aclweb.org/anthology/W18-3720v2> 【
Proceedings of the 5th Workshop on Natural Language Processing Techniques for Educational Applications  2018】
**论文介绍**：


