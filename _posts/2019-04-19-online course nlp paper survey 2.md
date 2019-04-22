---
layout: post
title: "在线教育领域NLP论文调研2"
description: "在线教育领域NLP论文调研2"
category: paper
slug: onlEduSur
tags: []
---
{% include JB/setup %}
基于之前的粗略调研，我们确定关键词并寻找论文，我们继续调研在线教育领域相关论文的研究内容及方法。

### 1、ACL相关论文
#### （1）Your click decides your fate: Inferring Information Processing and Attrition Behavior from MOOC Video Clickstream Interactions
论文地址：<https://www.aclweb.org/anthology/W14-4102>
任务简介：
该论文的任务是以学生看视频时的点击行为序列为基础，进行一系列的结果分析。

作者使用的交互行为有：Play (Pl), Pause (Pa), SeekFw(Sf), SeekBw (Sb), ScrollFw (SSf), ScrollBw(SSb), RatechangeFast (Rf), RatechangeSlow(Rs).
基于上述点击行为，作者将不同的点击行为序列定义为不同的行为：

- Rewatch: PlPaSbPl, PlSbPaPl, PaSbPlSb,
SbSbPaPl, SbPaPlPa, PaPlSbPa
- Skipping:SfSfSfSf, PaPlSfSf, PlSfSfSf, SfSfSfPa, SfSfPaPl, SfSfSfSSf, SfSfSSfSf, SfPaPlPa, PlPaPlSf
- Fast Watching: PaPlRfRf, RfPaPlPa, RfRfPaPl, RsPaPlRf, PlPaPlRf (click group of
Ratechange fast clicks while playing or pausing video lecture content, indicating speeding
up)
- Slow Watching: RsRsPaPl, RsPaPlPa,
PaPlRsRs, PlPaPlRs, PaPlRsPa, PlRsPaPl
(click group of Ratechange slow clicks while
playing or pausing video lecture content, indicating slowing down)
- Clear Concept: PaSbPlSSb, SSbSbPaPl,
PaPlSSbSb, PlSSbSbPa (a combination of
SeekBw and ScrollBw clicks, indicating high
tussle with the video lecture content)
- Checkback Reference: SbSbSbSb, PlSbSbSb, SbSbSbPa, SbSbSbSf, SfSbSbSb, SbPlSbSb, SSbSbSbSb (a wave of SeekBw
clicks)
- Playrate Transition: RfRfRsRs, RfRfRfRs,
RfRsRsRs, RsRsRsRf, RsRsRfRf, RfRfRfRf
(a wave of ratechange clicks)