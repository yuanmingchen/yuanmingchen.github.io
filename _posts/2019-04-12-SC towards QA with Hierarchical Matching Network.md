---
layout: post
title: "Sentiment Classification towards Question-Answering with Hierarchical Matching Network 论文阅读笔记"
description: "Sentiment Classification towards Question-Answering with Hierarchical Matching Network 论文阅读笔记"
categories: paper
slug: SCtoQa
---
{% include JB/setup %}
&#160; &#160; &#160; &#160;这篇论文介绍的是关于电商平台问答的情感分析，类似淘宝的“问大家”这种形式的问题对，根据问题答案对来分析其中的情感。

**论文地址**：<https://aclweb.org/anthology/D18-1401>

**数据链接地址**：<https://github.com/clshenNLP/QASC/>

**代码地址**：暂无代码。
#### （1）该论文的贡献主要有两个：
 1. 提出了一个新问题，即问答情感分析。并且上传了一份用于研究该问题的标注数据。数据链接地址：<https://github.com/clshenNLP/QASC/>
 2. 对这个新问题提出了一种专门的解决方法，即题目中的分层匹配神经网络。

#### （2）论文概括
&#160; &#160; &#160; &#160;该论文首先介绍了这个任务描述，然后分析这种任务为什么不适合直接用传统的情感分析技术来进行研究，并提出了一种专门针对这种问答的情感分析研究方法，叫作分层匹配神经网络，该方法分为三步：
 1. 将问题和答案都分解为一个个短句，然后对于每个Q和A中的短句构建 [Q-sentence, A-sentence]单元。加入Question有N个句子，Answer有M个句子，那么我们就有N*M个这样的短句匹配单元。
 2. 使用一个QA双向匹配层，将每个[Q-sentence, A-sentence]匹配pair单元编码为一个向量，以便用于后续情感分析。注意这里的双向的含义，并不是指使用的是双向LSTM，而是指作者在计算匹配pair单元表示向量的时候，使用的Attention机制是双向的，问题短句和答案短句彼此互相做Attention，计算问题短句的时候使用答案短句的表示与之来做Attention，计算答案短句的表示的时候使用问题短句的表示来做Attention，这就是所谓的双向匹配。
 3. 使用自我匹配注意力层（self-matching attention layer）让模型自动捕捉每个[Q-sentence, A-sentence]匹配向量的重要程度，以便更好的推断Q-A的情感极性。这里是针对第二步生成的一个个匹配向量又做了一次Self-Matching Attention，这一次是句子级别的Attention。
 
#### （3）分层匹配神经网络结构详细说明
 分层匹配神经网络的网络结构如下图所示：
![分层神经网络结构](/images/posts/post1-1.png){: width="100%"}
下面是其中QA双向匹配机制的详细结构图：
![QA双向匹配机制的详细结构图](/images/posts/post1-2.png){: width="100%"}
&#160; &#160; &#160; &#160;注意上图是针对问题的第i个句子与回答的第j个句子所组成的问答短句pair来进行分析的，以这样一个短句pair作为输入，最后输出的是这个问答短句pair的一个相关性向量。所以下面的文字中提到的问题句子指的就是问题的第i个句子，回答句子就是回答中的第j个句子，都只是一个短句而已。 

&#160; &#160; &#160; &#160;问题句子和答案句子匹配向量的计算思路很简单，实际上就是把问题句子和答案句子的最终表示拼接在一起，就表示它俩的匹配向量。计算句子表示向量的方法也很简单，就是把句子作为BiLSTM的输入，然后对各个时刻的输出加权求和，权重是通过Attention机制计算出来的，关键点也就在这个Attention的计算上，使用的是问题句子表示和答案句子表示彼此互相做Attention的方法，也就是所谓的双向匹配机制。

&#160; &#160; &#160; &#160;$D_{[i,j]}$中的第[a,b]个元素代表问题句子的第a个单词与回答句子中的第b个单词的语义相关性评分。作者采用了两个Attention，第一种是Answer-to-Question Attention，也就是使用答案句子对问题句子进行Attention。把$D_{[i,j]}$的每一行经过神经网络处理成权重（上角标r代表row），$D_{[i,j]}$的第k行代表问题句子的第k个单词与答案句子的每个单词的相关性，是词级别的Attention，实际上相当于用$H_{A_j}$与$H_{Q_i}$的每个时刻的输出进行Attention，然后计算每个时刻的权重，得到$H_{Q_i}$的加权后的表示$V_{[i,j]}^r$。

&#160; &#160; &#160; &#160;其中$H_{Q_i}$是问题Q的第i个句子经过BiLSTM后的表示，$N_i$是时刻数，即问题句子的单词数；$H_{A_j}$是回答第j个句子经过BiLSTM后的表示，$M_j$是时刻数，即回答第j个句子的单词数。其中$h_{j,m}\in R^{d'}$，即每个单词（时刻）的表示都是$d'$维的。具体计算公式如下：

$$
\begin{aligned} H_{Q_{i}} &=\left[h_{i, 1}, h_{i, 2}, \ldots, h_{i, n}, \ldots, h_{i, N_{i}}\right] 
\\ H_{A_{j}} &=\left[h_{j, 1}, h_{j, 2}, \ldots, h_{j, m}, \ldots, h_{j, M_{j}}\right] \end{aligned}
\\D_{[i, j]}=\left(H_{Q_{i}}\right)^{\top} \cdot\left(H_{A_{j}}\right)
\\\begin{array}{c}{U_{[i, j]}^{r}=\tanh \left(W_{r} \cdot D_{[i, j]}^{\top}\right)} 
\\ {\alpha_{[i, j]}^{r}=\operatorname{softmax}\left(w_{r}^{\top} \cdot U_{[i, j]}^{r}\right)}\end{array}
\\ V_{[i, j]}^{r}=\left(H_{Q_{i}}\right) \cdot \alpha_{[i, j]}^{r}
$$

其中$H_{Q_i}\in R^{d'\times N_i},H_{A_j}\in R^{d'\times M_j}$，所以$D_{[i,j]}\in R^{N_i\times M_j},W_r\in R^{d'\times M_j},w_r \in R^{d'}$，所以$U_{[i,j]}^r\in R^{d'\times N_i},\alpha_{[i, j]}^{r} \in \mathbb{R}^{N_{i}}，V_{[i,j]}^r\in R^{d'}$。

实际上：
$$

$$


&#160; &#160; &#160; &#160;而第二种显然就是Question-to-Answer Attention，也就是使用问题句子对回答句子进行Attention。使用问题句子的表示$H_{Q_i}$对答案句子$H_{A_j}$的每个时刻进行Attention，把$D_{[i,j]}$的每一列经过神经网络处理成权重（上角标c代表column），同理我们最后可以得到答案句子的新表示向量$V_{[i,j]}^c \in R^{d'}$，计算公式如下：

$$
\\\begin{array}{c}{U_{[i, j]}^{c}=\tanh \left(W_{c} \cdot D_{[i, j]}\right)} 
\\ {\alpha_{[i, j]}^{c}=\operatorname{softmax}\left(w_{c}^{\top} \cdot U_{[i, j]}^{c}\right)}\end{array}
\\ V_{[i, j]}^{c}=\left(H_{A_{j}}\right) \cdot \alpha_{[i, j]}^{c}
$$

&#160; &#160; &#160; &#160;最后，这个Q-A短句匹配pair的表示向量$V_{[i,j]}$由$V_{[i,j]}^r$和$V_{[i,j]}^c$拼接表示起来，如下公式所示，$V_{[i,j]}\in R^{2d'}$其中$\oplus$代表连接操作符：
$$V_{[i, j]}=V_{[i, j]}^{r} \oplus V_{[i, j]}^{c}$$

&#160; &#160; &#160; &#160;使用上面的双向匹配网络，我们把$N\times M$个短句匹配单元中的每一个单元作为输入，都可以输出一个匹配向量，所有现在我们就得到了$N\times M$个$2d'$维的向量，我们把它们拼成一个向量，然后做一个简单的句子级别的self-Attention，得到最终的的表示，最终再经过一个输出层即可，$p$就是最终分类的输出结果（四分类），如下所示：

$$
\begin{array}{c}{V=\left[V_{[1,1]}, V_{[1,2]}, \ldots, V_{[i, j]}, \ldots, V_{[N, M]}\right]} 
\\ {U=\tanh \left(W_{h} \cdot V\right)} 
\\ {\alpha=\operatorname{softmax}\left(w_{h}^{\top} \cdot U\right)}\end{array}
\\ R=V\cdot \alpha
\\p=softmax(W_l\cdot R+B_l)
$$


#### （4）关于作者分析的标注数据集：
**数据来源**：淘宝的“问大家”，主要包括美妆、鞋和电子产品这三个领域，每个领域收集了10000条问答对。

**标注说明**：对于情感分类的标注结果有四类，分别是positive, negative, neutral,conflict。其中conflict代表这个问答对中既包含对整体评价对象的积极情感，又包含消极情感。比如：“Q：这个手机好用吗？  A：手机使用起来手感很好，非常流畅。但是电池不太好，一会儿就没电了！”。这个Q-A就会被标注为“conflict”。
而neutral并不一定是中立的，按照作者描述的规则，以下这些情况都会被标注为“neutral”：

 1. 答非所问。比如“Q：屏幕清楚吗？ A：电池寿命很长！”
 2. 不确定的回答，“我不知道”这种回答。比如“Q：这款手机怎么样？ A：不知道，买来送人的”
 3. 不包含感情的客观事实。比如“Q：手机什么颜色？ A：蓝色”
 4. 对比两个或多个产品的问答。比如“Q：这款手机和iPhone6相比怎么样？ A：那决定于你，它们是不可比较的”
下面是数据分布情况：
![标注数据分布情况](/images/posts/post1-3.png)
 
