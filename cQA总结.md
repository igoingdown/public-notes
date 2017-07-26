# cQA总结


1.  典型社区：

    知乎，quora，stackoverflow。（thread就是一个问题及其所有回答，这是cQA的典型组织方式。）

    ==知乎==

    ![image](http://note.youdao.com/yws/public/resource/f8d7e3dff6b1129ff4c32a7cbc985552/xmlnote/WEBRESOURCE60926d4d191aae5077574de06872289e/2644)

    ==Quora==

    ![image](http://note.youdao.com/yws/public/resource/f8d7e3dff6b1129ff4c32a7cbc985552/xmlnote/WEBRESOURCE3c6c5a73e8b4f5f42cb7a72a989caa2e/2646)


---

2.  两大问题：

    噪音与重复，一些基本概念，新问题，候选问题，候选答案，thread。

    噪音：鱼龙混杂，好答案，坏答案并存，因此答案的呈现顺序很重要。引入用户投票机制可以一定程度上解决这个问题啊，“群众的眼睛是雪亮的”。但是这对较新的答案是不公平的！举例来说，如果一个答案是最好的，但是同时又是最新的答案，那么这个答案很有可能永远不见天日！而且投票机制可以轻易用于造假！

    重复：用户倾向于问问题而不是搜索问题的答案，喜欢问问题的人远多于喜欢并且有能力回答问题的人。小白用户更是如此，不会搜索答案，倾向于问一下非常简单而且已经有答案的问题，cQA产品的绝大多数用户是这种用户……这就导致了大量的重复问题（语义的重复或包含）。比如最好的编程语言是什么？？什么语言是深度学习最好的入门语言？？？各种编程语言的优缺点是什么？？什么编程语言对初学者比较友好？？？那么对于新提出的问题，可不可以先尝试从已经有大量答案的问题中找答案呢？？但是这个问题又很有解决的价值，方便用户的同时，也能为厂家节省存储空间和计算资源。这篇论文为了解决这个问题，首选选择候选问题（怎么选择的），然后选择问题的排名靠前的答案（选择依据是什么？），考虑原问题q，候选问题q'和候选答案c之间的相互关系（就是相似性，tango含义）。这种相似性用特征向量来衡量，后面有比较细致的讲解特征向量的构造。使用RNN来进行预测，最后发现问题之间的相似性最为关键，有着决定性的意义。

    可是对于原问题q来说非常好的的答案在候选问题q'中可能排名比较低，文章采用top10的答案，这样会遗漏很多有潜力的答案。


---

3.  三种关系（tango）： 

    要解决的新问题q和候选答案c之间的匹配程度：relevance，（模型中的hq1和hq2）

    新问题q和候选问题q'之间的相似性：relatedness

    候选问题q'和候选答案c之间的匹配程度：appropriateness

    候选问题是用google搜索引擎在社区内部发现与原问题最相近的问题。

    这些自己提取的特征是不用经过隐层的，直接从输入层到达输出层。

---

4. 一个重要的概念：

    thread，就是一个问题和这个问题下的所有答案，这就是一个thread。

---

5. 每种关系（特征）的表现包括embedding（distributed representation），涵盖了语法语义信息， 词汇匹配（lexical），涵盖了字符信息， 自定义domain-specific特征（涵盖thread级别的相似度信息）三种特征，感觉这里面面含了一定的多粒度表达的思想！！！为了实验需要会进行对照实验。实验表明词汇匹配级别的特征>domain-specific特征>embedding.

---

6. 文章的亮点，使用一种pairwise的神经网络结构来比较哪个答案更好，这种神经网络架构可以为各种关系（相似性）之间的关系建模。网络的输入是新问题q，两个候选答案c1， c2及答案对应的候选问题q1'和q2'。输出是0或者1，1表示c1比c2好，0表示c2比c1好。其实也可以用概率来实现soft selection！！！感觉我好像有了一个绝好的idea……哈哈哈哈
   h12表示候选问题q1'和候选问题q2',已经候选答案c1和c2的相似度。

---

7. 模型架构

    ![image](http://note.youdao.com/yws/public/resource/f8d7e3dff6b1129ff4c32a7cbc985552/xmlnote/WEBRESOURCE040fb16f5ac8941fa451cc9d2651f239/2580)

    这其实是一个传统神经网络，加入一些传统分类器所需的自定义特征来实现的！！！三种关系都可以使用lexical级的特征。

    生成lexical特征的方法：MTFEATS(BLEU, NIST, TER, METEOR, UNIGRAM PRECISION,  RECALL)和BLEUcomp。

    分散表达级别的特征：GOOGLE_VEC， QL_VEC, SYNTAX_VEC，三种关系的特征都可以用这种方法来生成。


---

8. domain-specific特征：

    SAME_AUTHOR,适用于q'和c之间的相似度

    CQ'RANK_FEAT:score = 1/r，适用于q'和c之间的相似度

    QQ'RANK_TEAT:相互排序的倒数，绝对排序的倒数，适用于q和q'之间的相似度

    CQRNAK_FEAT：绝对排序倒数，适用于q和c之间的相似度。

    TASK_FEAT：链接数，图片数等，各类词的数目。适用于q和c，以及q'和c。


9. 模型融合

    ![image](http://note.youdao.com/yws/public/resource/f8d7e3dff6b1129ff4c32a7cbc985552/xmlnote/WEBRESOURCE027c74c08a5e4d9c2e966b62d794af3c/2590)



10. 实验结果：

    ![image](http://note.youdao.com/yws/public/resource/f8d7e3dff6b1129ff4c32a7cbc985552/xmlnote/WEBRESOURCE48015f3c2339bd9cf21b8ef29d87a90f/2638)

    ![image](http://note.youdao.com/yws/public/resource/f8d7e3dff6b1129ff4c32a7cbc985552/xmlnote/WEBRESOURCE422b2a1772e8ae146e9560d6e573243b/2641)