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
### 2、Identifying Student Leaders from MOOC Discussion Forums through Language Influence 
**论文地址**：<https://www.aclweb.org/anthology/W14-4103>  【EMNLP 2014】  