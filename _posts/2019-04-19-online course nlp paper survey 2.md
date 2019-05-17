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

#### （2）Identifying Student Leaders from MOOC Discussion Forums through Language Influence   
**论文地址**：<https://www.aclweb.org/anthology/W14-4103>  【EMNLP 2014】  
**论文介绍**：作者的任务是根据语言影响力从mooc论坛中找出学生领导者。从mooc论坛中大规模地识别和理解学生领导者的动机是使得在线学习环境更具有参与性、合作性和指导性的关键。在这篇论文中，我们建议仅根据文本特征来识别学生领袖，或者通过分析他们如何影响其他学生的语言来识别他们。我们提出了一种基于人们对感兴趣的语义话题的词汇选择来衡量语言迁就度的改进方法，并证明了学生领袖确实起到了协调其他学生语言的作用。
一句话说就是，对于给定的主体，通过衡量大家说话时选用的单词来识别出学生中的领导者。  
原理就是人说话时，使用的单词往往会迁就和他们一起说话的人。这种现象叫做语言协调。我个人的理解就是，大家在讨论某个问题时，加入先有人用苹果举了一个例子，那么其他人在讨论的过程中，就更倾向于也用苹果来举例子进行反驳或论证，而不是用菠萝、香蕉之类的，虽然究竟用什么水果举例没有影响。

作者的方法如下：
首先基于语义和语法相似度，通过单词的在句子中相对其他单词出现的统计来把句子构建为词向量。

作者采用以下公式来衡量语言协调度，其中a、b代表两个用户，这个公式表示a向b的语言协调度，$w_k$表示第k个聚类中的一个词$E_{u_{a} \rightarrow u_{b}}^{w_{k}}$表示b使用了$w_k$这个单词，a对b的回复中也使用了$w_k$这个单词，即a对b出现了语言协调。$E_{u_{b}}^{w_{k}}$表示所有使用了$w_k$这个单词的事件。注意，以上只在所有包含k类中单词的评论中进行统计。

$$C^{w_{k}}(a \rightarrow b)=P\left(E_{u_{a} \rightarrow u_{b}}^{w_{k}} | E_{u_{b}}^{w_{k}}\right)-P\left(E_{u_{a} \rightarrow u_{b}}^{w_{k}}\right)$$
$P_{v_a}$



#### （3）Predicting Instructor’s Intervention in MOOC forums
**论文地址**：<https://www.aclweb.org/anthology/P14-1141>  【ACL 2014】   
**论文介绍**：
目的是通过论坛评论来预测教师是否会回复当前这个话题。
使用的是线性回归的方法，使用的特征包括两方面，一方面是当前这个话题的发起者、时间、地点、题目等等总体信息，另一方面是该话题下所有帖子的一个综合信息，例如所有评论的点赞数、所有评论发布时间的均值和方差、某些特定词出现的总数等等。
最终结果：



#### （4）Predicting MOOC Dropout over Weeks Using Machine Learning Methods  
**论文地址**：<https://www.aclweb.org/anthology/W14-4111> 【EMNLP 2014】   
**论文介绍**：

#### （5）Point-of-View Mining and Cognitive Presence in MOOCs:A (Computational) Linguistics Perspective  
**论文地址**：<https://www.aclweb.org/anthology/W14-4105>  【EMNLP 2014】  
**论文介绍**：

#### （6）A Process for Predicting MOOC Attrition
**论文地址**：<https://www.aclweb.org/anthology/W14-4109>  【EMNLP 2014】  
**论文介绍**：

#### （7）Understanding MOOC Discussion Forums using Seeded LDA
**论文地址**：<https://www.aclweb.org/anthology/W14-1804>  【Proceedings of the Ninth Workshop on Innovative Use of NLP for Building Educational Applications 2014】  
**论文介绍**：

#### （8）Towards Identifying the Resolvability of Threads in MOOCs
**论文地址**：<https://aclweb.org/anthology/W14-4104>  【EMNLP 2014】  
**论文介绍**：

#### （9）Semi-Supervised Answer Extraction from Discussion Forums
**论文地址**：<https://www.aclweb.org/anthology/I13-1001>  【International Joint Conference on Natural Language Processing】
**论文介绍**：

#### （10）Predicting Attrition Along the Way: The UIUC Model
**论文地址**：<https://www.aclweb.org/anthology/W14-4110>  【EMNLP 2014】  

**论文介绍**：

#### （11）Countering Position Bias in Instructor Interventions in MOOC Discussion Forums
**论文地址**：<https://www.aclweb.org/anthology/W18-3720v2> 【
Proceedings of the 5th Workshop on Natural Language Processing Techniques for Educational Applications  2018】

**论文介绍**：

### 2、CMU相关论文
#### （1）Sentiment Analysis in MOOC Discussion Forums:What does it tell us?
**论文地址**：<https://www.cs.cmu.edu/~mwen/papers/edm2014-camera-ready.pdf>

**论文介绍**：本文着重研究mooc论坛中的情感分析，具体内容包括以下两个个方面：
- 1、课程级别的情感分析，即分析每个课程的情感随时间的变化。  
- 2、用户级别的情感分析，即分析一个用户在论坛中表达的内容中的情感和浏览论坛时看到的内容中的情感对这个用户辍学的影响。  

**论文目标**：
- 分析论坛情感和辍学之间的关系【课程、用户两个级别的分析】
- 论坛中不同主题的讨论中蕴含的不同情感，以及不同主题表达情感的用词选择
- 主题、情感、辍学率等随时间变化的趋势

**实验数据**：
实验数据是三门mooc课程的数据：
- 1、Accountable Talk: Conversation that works：一门谈话艺术教学课程，简称Teaching课程
- 2、Fantasy and Science Fiction: the human mind, our modern world：一门文学课程，简称Fantasy，讲解科幻小说相关的东西
- 3、 Learn to Program: The Fundamentals：一门Python编程课程
我们有以上三门课程的所有论坛评论，以及评论的用户信息、发布时间。
对于第一门课程，还有所有学生的最后一次登录时间信息。


**研究方法**：
1、对于课程级别的情感分析，作者将论坛内容分为为不同话题，使用 Brown clustering的方法对论坛内容进行自动聚类，每个聚类作为一个话题，然后人工为每个话题从该话题聚类结果中选取关键词，总共选择了四个话题：课程整体（Course），授课（Lecture），作业（Assignment），同学互评（Peer-assessment）。每个主题的关键字这里不再赘述。
然后用这些关键字作为话题标志，如果一个论坛回复中包含该话题中的一个或多个关键词，那么该回复就认为是有该话题有关的。
接下来，为了衡量情感，作者使用正负情感词比例来衡量情感，即第$t$天内发表的所有论坛回复中，积极情感词数/消极情感词数即为第$t$天的情感系数，记作$x_t$，但是一天之内的回复数量不够，会使得情感波动过大，为了平滑，采用7天情感系数的平均值作为指标，即：

$$MA_t=\frac{1}{k}(x_{t-k+1}+x_{t-k+2}+\cdots +x_t)$$

然后作者绘制了整体课程的情感系数与辍学人数随时间变化的图标，探究两者之间的关系。作者使用课程整体（Course）话题的论坛回复中的情感作为学生对这门课的整体情感。

另外，作者还统计了每门课与每个主题最相关的关键情感词，采用PMI对情感词排序，以此找出与当前话题最相关的情感词：
$$PMI_{w, {TopicKeyword}}=\frac{P(w, { TopicKeyword })}{P(w) P({TopicKeyword})}$$


$P(w)$表示情感词$w$在评论中出现的概率，$P(TopicKeyword)$表示当前话题的话题词在回复中出现的概率，一条回复出现一个关键词和多个关键词不做区分，都认为与当前话题有关。$P(w,{TopicKeyword })$表示情感词和话题词共同出现在同一回复中的概率。通过这种方法，作者找出了每个话题最相关的情感词：  
![](/res/images/posts/mooc2-1.png){: width="100%"}

2、对于用户级别的情感分析，作者主要分析了两方面的内容，第一种是一周内用户发布的评论中的情感，第二种是用户一周内回复过的论坛话题中所蕴含的情感。对于积极和消极情感分别计算评分，比如：用户一周发布回复的积极情感词数/用户一周发布回复的所有单词数，即为用户个人发布的积极情感系数，记作Individual Positivity。消极的则记作Individual Negativity，论坛话题影响相应的记作Thread Positivity和Thread Negativity。

作者使用生存模型，预测上述四个因素对下一个星期学生辍学的影响大小。

**实验结果**：  
实验1的实验结果：
![](/res/images/posts/mooc2-3.png){: width="100%"}
可以看到一门课整体的情感系数和辍学的人数有关系。图2是每门课每种话题回复的数量随时间的变化，可以由此看到学生在不同时间段对不同话题的讨论度是不同的。


实验2的实验结果和想象的并不相同，并非积极情感就一定会降低辍学率，消极情感就一定会提高辍学率，需要针对具体课程具体分析，如Fantasy课程中经常出现的一些消极情感词是在讨论科幻小说的内容【僵尸、灾难等】，这体现处理对课程的参与，反而会降低辍学率。所有盲目的对所有课程进行情感分析不可取，因为噪声很多，但是针对一门课程的情感分析是有意义的。
改用mooc相关的情感词典可以提高准确率。
![](/res/images/posts/mooc2-2.png){: width="100%"}



#### （2）Exploring the Effect of Student Confusion in Massive Open Online Courses
**论文地址**：<https://www.cs.cmu.edu/~diyiy/docs/ls15.pdf>

**论文介绍**：主题是mooc论坛中困惑的影响。作者在论文中量化困惑的影响，使用论坛行为和点击流数据设计了一个分类模型来自动识别表达困惑的帖子。使用生存分析量化困惑对学生辍学的影响。
结果表明用户表达和接受到的困惑越多，留下来的可能性越低【辍学的可能性越高】，这些学生接收到指导的话可以帮助减小这种影响。作者探索了困惑在不同上下文中对课程的不同方面的不同影响，总结了设计干预指导措施有主意提高mooc学生的保留率。

探究不同程度的困惑对学生流失的影响。两个贡献：
从理论上讲，通过探究困惑对学生生存的影响，作者发现了学生的困惑与他们退出MOOC课程之间的几个重要联系。在实践上，我们的研究结果为开发潜在的干预措施提供了指导，这些干预措施可能通过为困惑的学生提供及时的帮助和支持来促进学生留下来。

**研究方法**：
我们创建了一个人工标注的语料库，用于开发自动测量学生困惑的方法。其次，我们将每个帖子表示为一组特性，作为机器学习模型的输入。最后，我们建立了一个基于手工编码数据的统计模型，并对其性能进行了评估。

1、分类器衡量困惑程度。设计了一个分类器可以预测学生的困惑程度，这个分类器考虑学生的语言选择和视频观看模式。为了给学生提供识别困惑的机会，我们建立了机器学习模型来自动识别学生在课程论坛上发表的帖子中表达的困惑程度。这些模型使用统计过程将一组输入特性映射到一组输出类别。在我们的工作中，我们从学生的点击行为和他们的发布行为中提取输入特征，包括课程中的点击模式、领域特定内容词的出现以及高级语言特征。输出是一个数值，表示每个帖子中表示的困惑程度。  
使用逻辑回归模型和10次交叉验证的方法评估性能【Acc】。
预测困惑的模型使用的输入特征有3个，分别为：
- 1、点击流模式：比如重看教学视频可能表示困惑。四个动作组成的序列：quiz，lecture，forum，course materials。最大序列长度为4，选取最多的20中序列作为特征模式。
- 2、语言特征：LIWC词典。人称代词、情感词、口语。
- 3、问题特征：这是不是一个问句，问好数量、疑问词开头、常见疑问句式。

结果：Acc：80.3【代数】，60.6%【微观经济学】，发现技术性课程比非技术性课程困惑程度高，还对比了不同群体【性别、年龄、受教育程度】的用户困惑程度不同。

2、生存分析
使用生存分析预测困惑程度对辍学的影响，与上一篇类似，根据用户这一周的困惑程度预测用户下一周的辍学倾向性，从而确实用户困惑程度与辍学之间的关系。具体如下：
- 因变量：辍学与否。
- 自变量
    - 一周内的帖子总数
    - 发起一个线程【话题】
- 独立变量
    - 表达困惑
    - 该学生发起的线程中接受到的困惑
    - 该学生参与的其他线程中接受到的困惑
    - 困惑被解决的线程数
    - 被回复的线程数

**实验数据**：
本文的数据集包括两门Coursera课程:一门数学——代数课和一门微观经济学课。代数课拥有2126个活跃用户(活跃用户指在课程论坛中至少发布一次帖子的用户)和7994个论坛帖子;微观经济学拥有2155名活跃用户和4440个论坛帖子。每个线程提供了一个标志，指示是否解决了问题。两门课程各为期12周。  
除了论坛记录，每个学生与课程材料的互动(学生点击)也记录在点击流中。代数的学生点击量为8,686,230次，微观经济学的点击量为2,709,053次。这个点击流数据为我们提供了一个机会来研究点击模式和学生困惑之间的关系。大约3.4%的代数学生和8.3%的微观经济学学生至少在论坛上发过一次帖子。

另外，为了训练困惑程度预测模型，作者使用MTurk人工标注平台标注数据，每个帖子标注1-4，代表困惑程度评分。

**实验结果**：
接触困惑确实会增加辍学率，但是回复和解决问题有助于缓解这一影响。
用户的点击流模式可以在一定程度上解释用户对哪方面有困惑，可以辅助预测用户发布的线程问题是关于哪一方面的困惑。据此分析学生对不同方面的困惑的不同。
我们的研究结果表明:
- (1)学生在MOOC论坛上表达的困惑越多，接触到的困惑越多，学生在学习社区中保持活跃的可能性就越小;
- (2)帮助解决或提供对学生困惑的反应，减少他们在课程中的退学;
- (3)不同类型的干扰对辍学的影响程度由具体课程决定。在我们的数据中，在技术课程中，测验的混淆导致了最高的辍学率，而在技术含量较低的课程中，对课程的困惑通常会导致辍学率更高。

#### （3）Linguistic Reflections of Student Engagement in Massive Open Online Courses
**论文地址**：<http://www.cs.cmu.edu/~mwen/papers/icwsm2014-camera-ready.pdf>

**论文介绍**：自动分析论坛语言，挖掘学生参与这门课的动机与投入，而学生参与课程的目标与决心与他们的参与度以及辍学息息相关。

作为方法论上的贡献，本文研究了使用计算语言模型从论坛帖子的文本中测量学习者动机和认知投入。从两个角度量化学生参与度：
- 1、学生表现出来的继续学习下去的动力
- 2、学生对课程材料认知理解的程度（讨论参与度）

我们使用生存模型来验证我们的技术，该模型评估了这些变量对于预测随时间推移的学生损耗率的有效性。

这种方法可以有效区分学生，识别危险迹象，找到苦苦挣扎的学生，从而减少辍学率。

**研究方法**：

1、预测学生动机：人工标注数据，标注参与度1-7
使用五种语言特征：
- 应用词：“apply”, “try”, “utilize”, “employ”, “practice”, “use”, “help”, “exploit” and “implement”
- 需要词： “hope”, “want”, “need”, “will”, “would”，“like”, “plan”, “aim” and “goal”
- LIWC-cognitive词，比如 “thinking”, “realized”, “understand”,“insight” and “comprehend”
- 第一人称词
- 积极情感词
2、认知投入水平
衡量语言抽象程度：使用一个抽象程度词典，对句子中的词抽象程度取平均值，作为整个句子的抽象程度。

3、生存实验：
因变量：和之前一样，又是辍学与否。
控制变量：
- Cohort：二元变量，是否在第一周发表过帖子。
- 一周发布帖子的数量
- 一周在论坛中的评论数
- 一周发布帖子的平均动机，使用实验1中的模型预测动机
- 一周发布帖子的平均抽象程度


**实验数据**：
三门Coursera课程数据，和第一篇情感分析的那个用的数据一样：
- 1、Accountable Talk: Conversation that works：社交谈话教学课程，1146个活跃用户，5107个帖子
- 2、Fantasy and Science Fiction: the human mind, our modern world：文学课程，771个活跃用户，6520个帖子
-3、Learn to Program: The Fundamentals：编程课程，3590个活跃用户和24963个帖子。

**实验结果**：
参与度越高，辍学率越低；个人解释越多，抽象程度越高，辍学率越低。
#### （4）Towards triggering higher-order thinking behaviors in MOOCs
**论文地址**：<http://delivery.acm.org/10.1145/2890000/2883964/p398-wang.pdf>

**论文介绍**：
题目是激发mooc中更高层次的思考行为。目的是研究**什么样的讨论更有助于学习**，探索高阶的思考行为比仅仅集中与课程材料的讨论更加有助于学习。与上一篇中的语言抽象程度有点像。为了研究这种效果是由于相关行为的参与，还是学生个人的执着的性格。作者使用两种方法：
- 1、使用倾向性得分配对在其他课程活动中表现出相似参与度的学生
- 2、探索个体在几个星期内参与高阶思考行为的差异
两项分析的结果都支持将这种效应归因于行为解释？？？  

作者还研究了**什么样的学习材料更容易引发高阶思考讨论**：使用LDA对课程材料的进一步分析表明，更多的社会导向的话题比更多的生物心理学导向的话题引发了更丰富的讨论。

**研究方法**：
构建了一个编码手册【一系列规则】，标注每个帖子是不是高阶思考行为。
1、把学生分成三个群体：高阶思考、专注课件、不讨论。使用回归模型，因变量是测后成绩，控制变量包括测前成绩、帖子数、是否注册、是否点击视频、是否点击测验、课程网站点击数、论坛点击数。

我们利用前面介绍的所有控制变量作为学生参与课程的特征，建立了逻辑回归模型，预测学生在课程中出现高阶思维行为的概率。

**实验数据**：
cmu的课程《Introduction to Psychology as a Science》，该数据集共包含27750名注册用户的数据，以及7990篇文章和评论。其中，有1079名学习者同时进行了前测和后测，其中491人参加了论坛，共发表帖子和评论3864条。

除了论坛记录，点击流数据中还记录了与课程材料相关的学生点击量。课程记录包含1487665次学生点击。这些数据为我们提供了一个机会，监控学生与课程材料的互动过程。
流程：逻辑回归训练模型，实验组和对照组的人进行相似行为匹配，然后预测测后成绩。高阶思维成绩更高。

**实验结果**：

#### （5）Towards Identifying the Resolvability of Threads in MOOCs
**论文地址**：<https://www.aclweb.org/anthology/W14-4104>

**论文介绍**：
分析mooc中问题线程被解决与哪些因素有关。mooc的问题论坛与其他百度知道等问答网站的不同之处在于：
- 1、没有最佳回答这一选项，只能标识问题已解决或者未解决。
- 2、没有奖励机制，回答者不能获得奖励
- 3、mooc允许低水平的提问，更注重大家对问题的讨论互动过程，而非单纯性的回答问题而已。

作者把一个线程中的参与者分成了以下几种用户角色：
- 1、发起者（Starter）：线程问题的发起者，提问者
- 2、导师（Instructors）：老师和助教同城导师
- 3、参与者（Participants）：所有参与讨论的人，回复和评论都算。

线程问题是否被解决由发起者负责标记。作者研究的就是给定一个问题和一系列的回复，影响该问题是否会（被标记为）已解决的因素。


**研究方法**：

作者把一个线程中的参与者分成了以下几种用户角色：
- 1、发起者（Starter）：线程问题的发起者，提问者
- 2、导师（Instructors）：老师和助教同城导师
- 3、参与者（Participants）：所有参与讨论的人，回复和评论都算。


作者尝试分析了几个可能对mooc问题可解决性有影响的因素：
- 1、**启动影响**：问题的发起者吸引其他人来参与回答讨论的倾向性。使用线程发起者的影响程度衡量这个指标，包括四个方面：
    - 问题爱好者：根据用户发起的线程数对用户分级
    - 用户参与度：根据用户参与讨论的线程数对用户分级
    - 解决优先级：根据由当前用户发起的线程并且已解决的数量对用户分级
    - 回复被表扬度：该用户的回复中获赞的比例
- 2、专家参与：回答者贡献有用回答的倾向性。专家的定义有两类群体：
    - （1）导师：包括所有的老师和助教
    - （2）声誉排名前1%的学生，衡量学生$u$声誉的方式如下：
    
    $$\begin{aligned} \operatorname{score}(u)=& \alpha u_{Pst}+\beta u_{Rep}+\gamma u_{Res}+(1-\alpha-\beta-\gamma) u_{Uvt} \end{aligned}$$
    其中$u_{Pst}、u_{Rep}、u_{Res}、u_{Uvt}$分别对应于前面提到的问题爱好者、用户参与度、解决优先级和回复被表扬程度。
    
- 3、线程流行度：线程的受欢迎程度、流行度与线程是否可以被解决有关，以总共获赞数和最高获赞数来作为衡量指标。
- 4、友好程度。回答者的回答的友好程度对新手能否继续学习下去很重要，讥讽的态度更容易打击初学者。使用礼貌词匹配的方法衡量：
    - 以感谢开头：发布问题时是否用了感谢
    - 以感谢结尾：收到别人的回复时是否表示了感谢
    - 感谢数量：衡量该线程总体的友好程度，计算整个线程感谢相关的个数
    - 表示尊重的礼貌用语词数：比如Nice、Great或Awesome等词
    - 友好问话词数：捕捉友好问话词的使用个数，比如please、will等
- 5、内容匹配度。用来表述回答是否是针对用户发起的问题，描述回答与问题的相关性。使用Eigenword bipartite graph描述语义相似度。二部图中的每个节点都是给定单词对应的特征字3，左边表示线程启动器中出现的单词，右边表示给定回复中的单词。使用余弦相似度度量计算问题和回复的相似度评分，使用余弦相似度排名前三的相似值和TF-IDF相似度以及最大问题长度辅助计算匹配度得分。

作者使用结构方程模型Structural Equation Model (SEM)来验证每个潜在因素对线程可解决性的影响。使用预测模型评估识别问题可解决性研究的通用性。

之后作者使用全体逻辑回归模型，预测一个线程的可解决性。对五个影响因素分别构建5个逻辑回归模型，然后再使用下面的公式集成这些子模型的输出结果，相当于是结合了5个不同方面预测可解析性。

$$\ddot{R}_{j}=\frac{1}{1+e^{-\sum_{i=1}^{k} \alpha_{i} \cdot \dot{R}_{ij}}}$$


**实验数据**：
Python编程课程论坛，将论坛集限制为集中于讨论课程内容的子论坛【有一个已解决按钮】，在最终的数据集中，共有2508个线程(1244个已解析线程)，2896个用户(12名教师和工作人员)至少有一个帖子。每个问题都与一个标签相关联，该标签指示是否解决了问题

**实验结果**：
影响系数：启动影响（0.62）、专家参与（0.59）、内容匹配度（0.18）、友好程度（0.08）、线程欢迎度（0.05）。

预测实验结果：

![预测实验结果](/res/images/posts/mooc2-3.png){: width="100%"}



