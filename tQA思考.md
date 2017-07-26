## 它的表格是怎么输入的，问题是怎么输入的，答案是怎么给出的，即交互过程

表格的输入：  
* 行号，*row*
* 列号，*col*
* 单元格数据，*content*
* 单元格内容对应的token，*content tokens*
* 单元格内容标志，*posTags，part of speech tags，即词性标签*
* 单元格内容ner标志，*nerTags，name entity recognize tags，即命名实体识别标签*  
* 单元格内容ner值，*nerValues  即命名实体识别的结果*
* 单元格中含有的数字，*number*
* 单元格中含有的日期，*date*
* 单元格含有的其他数字（对比分这种数据又奇效），*num2*
* 单元格内容为列表，*list*

例如：
> -1  0   fb:row.row.year Year    year   year    NN  DURATION    P1Y

> 10	3	fb:cell.207	207	207	207	CD	NUMBER	207.0	207.0
-----


## 这个模型是否支持来自外部的用户表格和问题？
支持外部输入表格，但是表格要转换格式。  
问题也可以从外部输入，但是要按照一定的格式：  
* 问题id，*id*
* 问题文本，*utterance*
* 对应表名，*tables*
* 答案，*list*

例如：
> (example (id nt-1) (utterance "in what city did piotr's last 1st place finish occur?") (context (graph tables.TableKnowledgeGraph csv/204-csv/622.csv)) (targetValue       (list (description "Bangkok, Thailand"))))
-----

## 训练好的模型怎么保存，如果我们做了一个简单的网页交互界面，用于读取和显示数据，那么保存好的模型如何跟这两个接口交互；
模型已经训练好了，如果做系统的话，可以从外部输入表格和问题，修改格式后，调用model的evaluate函数。

-----
## ==模型的训练过程，尤其是reinforcement learning具体应用在哪一步，和用它的必要性==
这个还没弄明白。

---
## 后续问题
1. 把模型搞明白
2. 思考如何把模型作为系统的后端。

---


## 2017.3.30
* data怎么生成的？？

  认真看完complete_wiki_processing这个函数应该就明白了！  

  ==当然是不需要看完的！看输出结果就行了啊！==

---

* data每条记录包括以下字段：
  * *question*，问题的token序列
  * *question_attention_mask*， 对于问题token，attention为0，对于padding，attention为-10000。
  * *answer*，问题的calc answer，即scalar answer。

  * 针对问题中出现的数字设计的特征：
    * *question_number*
    * *question_number_1*
    * *question_number_mask*
    * *question_number_one_mask*

  * 针对问题中的序数词设置的特征
    * *ordinal_question*
    * *ordinal_question_one*

  * 和lookup等效？？？这是训练的label？？
    * *print_answer*

  * column_id就是讲列明转换为id
    * *word_column_ids*
    * *column_ids*

  * 列或者单元格的匹配特征：
    * *exact_match*
    * *exact_column_match*
  * 所有column和mask的关系都是一样的，就是保留前面几个column，因为不够
    * *columns*
    * *column_mask*
    * *processed_word_columns*
    * *processed_number_columns*
    * *word_column_mask*
    * *word_column_entry_mask*  

  * ==暂时还没明白的几个属性！==
    * ==*sorted_number_index*==
    * ==*sorted_word_index*==
    * ==*group_by_max*==

  这些数据在feed到graph中的时候，对应的 placeholder的名称前加个batch就是了。data的每条数据表示一个问题，**data是按问题组织的！**

---

* train set: *random-split-1-train.examples*，11321个问题，问题id最大为14151。
* dev set: *random-split-1-dev.examples*，2831个问题，问题最大id为14150。
* *training.annotated*，14152个问题，问题id从0到14151。

* **可以看出train set和dev set是由training.annotated随机分离得到的**。

---

* test set: *pristine-unseen-tables.examples*，4344个问题。
* 可是test set并没有用到！！！ **怎么用呢？？** evaluate的时候用的是dev set！
* question分为lookup question，number-calculate question和bad question。lookup question又分为word-lookup question和word-lookup question。

---


## 2017.4.6

---

*   训练过程函数的调用顺序

    ![image](http://note.youdao.com/yws/public/resource/f8d7e3dff6b1129ff4c32a7cbc985552/xmlnote/WEBRESOURCE2f3f2eb90eb976932c1943a2b8c3a4d6/3414)


---

* 问题RNN(LSTM)得到的结果和history rnn得到的结果不是同步的，先用LSTM得到question的embedding，注意这里question rnn只用了一次。然后再分timestep多次使用history rnn。

---



## 2017.4.13

* 操作的向量化

  > 每个操作都是模型的参数，被表示为一个向量

  这样有什么意义呢？？



* selector模块

  > selector模块推输出是operation和column的概率分布

  > selector模块输入是三个向量的拼接：question经LSTM产生的embedding，当前历史RNN的隐状态和attention，这个attention是使用history  vector对question做attention操作得到的。

  > history rnn的输入是operation和column的概率分布向量，以及对应的权重向量（由上一步推导得出）的拼接。


  怎么做的attention？？


---