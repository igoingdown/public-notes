## 问题背景


表格问答，根据表格中的结构化的数据，回答用户的问题，问题范围限定在表格中。
![image](http://note.youdao.com/src/WEBRESOURCEf42ba097c18885f8a2c45bed1e11bcc1)

---

## 方法

自己定义一系列的离散操作，然后通过RNN基于概率预测定长的操作序列中每一步的操作，并在表上执行该操作，逐步缩小答案的范围，最后通过行选择，列选择以及操作选择给出答案。

![image](http://note.youdao.com/yws/public/resource/f8d7e3dff6b1129ff4c32a7cbc985552/xmlnote/WEBRESOURCE94e3181f7507aadcf72b55ff01e18959/2431)

---


### 特点
* 用概率实现柔性选择，
* 弱监督，
* end2end，问题直接到答案
* 可微分，不需要中间的操作作为监督。


---

## 应用示例

每一步会更新Lookup Answer， Row Selector， Scalar Answer的存储的答案。Row Selector是一个与表同行数的向量，Lookup Answer是一个与表同大小的二维向量，Scalar Answer是一个值。
Lookup Answer只跟print操作有关，Scalar Answer之和count操作有关。Row Selector是最重要的参数，而Column的选择也同样重要，解决表格问答的问题只需要确定行和列就可以了。这里分两个部分分			别解决行预测和列预测的问题。每一步，问题的向量表达通过RNN可以得到一个输出，这个输出包含了问题中的词汇的信息，如果列中的信息和词汇的信息一致，该列对应的概率就高一些，同时这个输出包含了操作的信息，模型会根据RNN输出得到各个操作的概率。根据这些概率可以得到每行的概率，这样就可以更新Row Selector对应的行的概率了。

---

## 操作含义：

![image](http://note.youdao.com/yws/public/resource/f8d7e3dff6b1129ff4c32a7cbc985552/xmlnote/WEBRESOURCEbf7d67f6d7197542bc7f08ca16b8d23a/3821)


![image](http://note.youdao.com/yws/public/resource/f8d7e3dff6b1129ff4c32a7cbc985552/xmlnote/WEBRESOURCE2ee3962d0872c71c04793be397677537/2436)




