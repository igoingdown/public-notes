# 2016.10.9：

1.  通过问题和答案的向量表达来选取候选答案，这就是我接下来的工作？

    筛候选，选最佳答案，计算准确率
    问题在于已知问题的表达的情况下，如何筛选候选答案。将问题的表达输入训练后的模型，会得到什么？是与之相关度很高的答案吗？
    输入所有关系，根据训练好的模型，排序，选topk的关系，找关系对应的头实体，组成pair，算score。

2. 论文中L的表达式中的rs和rq是选取了候选fact和answer之后自动生成的？人工标注怎么用？

3. CBOW体现在`train()`的什么地方？

    CBOW训练的可行性在于通过最大化在字符集中出现的单词组合或者字符组合的概率，
    从而使语法正确的组合的概率远大于错误的组合的概率，这就得到了正确的语法，
    完整的字符集加上正确的语法就是语言。

4. hidden_size表示什么？GRU中的state是什么一样？

    一个rnn单元输出包括output和state，output是中间走过的每一个state。

5. batch_size是一个批次中问题q，描述s，事实f或者答案a的数量？结合图来说明一下

6. 对于oov_words_oov_phrase:

    使用c2w再使用w2p

7. 开始对character， word和phrase打分的时候，怎么评估单个的字符单元和答案的匹配程度的？

    对于每个字符单元应用双向GRU，有每个位置左右位的字符各得到一个位置匹配分数，最终的分数取两者的连接，论文的书写有问题。

8. element-wise是什么意思？

    逐元素地，就是对所有元素执行相同的操作，适用于矩阵运算。

9. 怎么调试tensorflow？怎么让它run起来？

10. `tf.placeholder()`返回一个只能作为数据容器的tensor对象？

   给变量留个坑，需要的时候填坑，但是不能直接求值。

11. tensor对象j就是中间结果，就是节点。等号左边的东西，约等于输出output，但是tensor不是数据的直接引用，而是存储了计算出output的方法或者步骤。
tensorflow的数据流图中的节点是一个操作，即op，这个节点是一个计算节点。

12. wight, bias 和 projection在论文中对应什么？

13. `tf.Variable`

    tensor的一种，在数据流图流动的过程中，每次迭代，必须更新，也可以自己设计更新。

14. tf.operation 是数据流图中的一个节点，需0到多个tensor作为输入，输出也是0到多个tensor。operation等价于tensor，因为等号左边是tensor，右边是flow，即operation。

15. 嵌套是什么意思？代码的注释里多次提到了嵌套？

    就是embedding……

16. scope的作用是什么？意义是增加层，这个层会怎么样影响数据流图？

    scope通过复用其他scope的节点tensor或者边flow，实现bind（一变皆变），从而保证不同层次之间的tensor在数据流图在流动中的一致性。

17. loss计算步骤不太懂，给变量留个个坑，还没填呢，怎么就用lookup读取里面的数据了呢？在哪一步填充的呢？

    在session中，使用feed_dict填充。在设计数据流图的时候可以认为所有占位符都已经填充了数据！直接用就行。

18. concat函数，哪一维连接，哪一维扩充。

19. reshape函数，注意shape参数可以有一个维度的长度是-1的，
就是在其他维度的长度已知的情况下，将原tensor变换为转换为具有目标维度的tensor，
在其他维度的长度已知的情况下，另外一个维度可以推出来。

20. embedding_lookup(params， ids)函数默认将ids以mod（%，另一种方式是div，即/）
方式并行地分配到params中，最后将多个params连接起来，作为输出。


---

# 2016.10.11

1.  shape对象相加，得到的是什么？

    得到的是维度扩充，比如tensor1的shape为[1, 2]，tensor2的shape为[2]，tensor1和tensor相加的结果就是一个shape为[1, 2, 2]的新的tensor。
    list相加和tf.tile操作很像。

2. 师兄的改动在云协作上写个文件，我这边可以手动改动，没有git只能这样了。

3. embedding_lookup的使用可能存在问题：

    返回与查询index tensor对应的embedding tensor。

4. hidden_size在初始化GRUCell时表示单元个数，这个单元个数是什么意思呢？形容rnn什么方面的特征呢？

5. 一个RNN\_Cell训练的输出包括两部分

    即output和state，其中state是最终的state，output是每一步迭代的中间state。output的是一个 Tensor shaped: [batch_size, max_time, cell.output_size]，state是输出是一个pair，这个pair怎么能和一个二维数组连接呢？，其中cell的state_size， output_size是什么？如果在构造cell的时候没有给定cell.output_size 和 cell.state_size，则output_size就是hidden_size。而cell.state_size 的shape 是除了 batch_size 的shape。和用于初始化的构造函数的参数有关系吗？有的话，是什么关系，和输入sequence的shape有关系吗？有的话，又是什么关系？If cell.state_size is an int, this will be shaped [batch_size, cell.state_size]. If it is a TensorShape, this will be shaped [batch_size] + cell.state_size。现在问题来了，output应该比state大很多，但是维度一样，shape的区别在哪？在哪一维度？


6.  各种embedding后加lf的是什么东东，没找到定义啊？！

7. expand_dims(input, dim)升维，增加一个维度，体现在shape上，比如原tensor的shape为[1,2],expand(0, tensor)后，shape变为（1，1，2），expand(1, tensor)后，shape变为（1，1，2），expand(2, tensor)后，shape变为（1,2,1）。

8. `tf.tile(input, multiples)`将input的第i维复制multiples(i)次。所以input的维度需要与multiples的长度一致。

9. 两个embedding相加是什么结果？应该不是element-wise的吧。

    答案是肯定的，就是element-wise的，而且按照word2vec的思想，两个embedding相加得到的就是词组的embedding。

10. QA_character_pro所代表的v的转置是什么，论文里面也没说明。

   论文里当然是有说明的，没看见而已！

---

# 2016.10.13

1.  max_size的含义：就是指一个问题或者答案可能包含的字符，单词和词组的最大个数，比如一个答案（头实体和关系组成的对）在我们目前的方案中认为最多含两个词组，即头实体和关系在最高level上。

2. 正样例和负样例计算得分的区别

3. `tf.reduce_sum()`和`tf.expend_dim()`执行相反的的操作，会以加和的方式将一个指定维度压扁,在默认情况下，会消除一个维度，如果keep\_dims参数设置为true，则将该维度挤压到长度为1，并保留该维度.比如对一个shape为[2, 3, 4]的tensor(x)执行`reduce_sum(x， 1， keep_dims=true)`的操作，则返回一个shape为[2, 1, 4]的tensor；如果将keep_dims参数去掉，则得到的结果为一个shape为[2, 4]的tensor。

4. tensor的除法

    shape分别为[10， 10， 10]和[10, 10, 1],得到的结果是shape为[10, 10, 10],但是最里面的那个维度中所有元素都会除以shape为[10, 10, 1]该维度的那个数字。


# 2016.10.17

1.  初步把问题定位到，score上，迭代过程中，matching-score增大地过快，最后产生nan问题。

2.  加入`add_check_numerics_ops()`不行，但是可以加入`check_numerics()`，后者是对单个的运算节点进行监测，可以多加几个，看谁最先出现，总是某一个先出现nan就能说明问题了。

3.  目前加入`check_numerics()`成功了，但是还没到出现nan的时候就发生了oom错误。建议将batch_size继续减小。


# 2016.10.18

1.  batch_size对模型的训练有影响吗？

    大的batch_size可以在一个epoch中进行步长较大的训练，缩短收敛需要的时间。


2. 学习心得：先画一张图，这张图中有输入和输出，输入按batch进行，并且要为输出设置占位符，一遍每次迭代都将输入数据填充到占位符中，输出就是loss。连接输入和输出之间的操作就是计算节点，这些操作可以分配到不同的计算资源上进行并行计算。模型的参数定义为variable，这些变量在每次计算loss之后会根据相应的loss处理策略，如梯度下降，来更新参数。

3. 学习速率可能还是偏大了！

4. `dynamic_rnn()`的输出：

    outputs: The RNN output Tensor.
    If time_major == False (default), this will be a Tensor shaped: [batch_size, max_time, cell.output_size（即hidden_size）].
    state: The final state. If cell.state_size is an int, this will be shaped [batch_size, cell.state_size]

5. 猜测由于nl和kb的phrase的不同导致bug，现在还不确定。

6. 计算负样例单词匹配分数时，可能会把batch_size和neg_size混用。

7. phrase为0，是因为把词组放到了oov里面了。这算错误吗？问题出现在非oov词组合oov词组的融合上？

8. 问题出在oov词组和非oov词组的位置对应上。

---

# 2016.10.20:

1.  不能稳定复现，应该是反向传播的锅……


---

# 2016.10.21

1.  神经网络中的激励函数有什么用？

    将输入数据转换为输出数据同时保证输出数据的范围不至于过大，以免整个数据的输出范围过大。另外激励函数理论上来说只能是非线性的函数，这是由于在不限制神经元的个数的情况下，一层神经网络可以模拟所有函数，即使是非常复杂的函数。但是激励函数是线性函数时不可以。

2. 如何判断神经元是否处于激活状态？

3. 反向传播的时候是否仅仅涉及被激活的神经元？还是对所有的神经元都有传播？

4. dropout是干什么的？

    干掉一些神经元之间的一些边，避免过拟合，用大部分的神经元的规律来记录学到的经验，而保持一些少量的神经元以备学习新的经验。


---

# 2016.10.22

1.  oov_embedding 的index0是无意义的，其他的index都有意义？


2.  代码的运算逻辑可以看懂，但是为什么要这么做，不懂

    还是要看论文！

3. 数据中不仅有nan，还有inf！

---


# 2016.10.23

1.  看莫烦的tensorflow视频很有收获，终于明白了scope的作用，以及name参数的作用！这些东西使用来做可视化的！工具就是tensorboard！

---

# 2016.11.1

1.  GRUcell和普通的神经网络有什么区别？

    GRUcell也有output，也可以加入dropout，关键在于cell在神经网络中的地位和作用。GRUcell的结构比一般RNNcell的结构更加复杂，更能记忆长久的东西。

2. dynamic_rnn输出的是oov的embedding，我们没有传递参数给rnn，为什么给的输出却有着应有的shape？

3. 为什么要用到biGRU输出的state，这些state表示的是什么含义？是下层结构的表达？

    单向GRU可以学习该单词前方或者后方的上下文信息，将通过两个方向的上下文信息得到的不同的表达（embedding）结合起来，形成一个结合了两个方向的上下文信息的embedding就是双向GRU的作用。

4. 测试什么？为什么要测试？

    测试准确率，先筛选候选集，将候选集和问题转换为可以输入神经网络的格式，得到每个candidate的final_score，进行排序（话说我还是不太明白怎么排序呢……）

5. pycharm远程编辑，怎么设置？

    已经加入收藏夹，然而用着并不舒服……

6. tensorboard画出来的图，怎么感觉怪怪的？各个score直接的关系有点乱啊！

7. answer和fact有重叠吗？

8. 生成q的每个word到subjects的dict时，q只包括测试集中的q？

9. 不好弄啊，把graph写到函数里了，这就压根没考虑过test啊……这下有点麻烦了。


---

# 2016.11.5

1.  word_subjects_dict的生成有了新方法，更高效的方法。

2. 及时把做出来的结果给师兄看看！别做出来的东西和师兄想的不一样 ！

3. kb_phrase_dict没有保存到文件中，input_data要重新跑一遍……

    确实要重新跑一遍，还要把各种字典存到不同的路径下，苦死鸟……

4. 问题怎么办？还要按照input_data中那样重新跑一遍？

5. anaconda + pycharm


---

# 2016.11.9

1.  input_data重新跑一遍，把所有可能用到的字典全部存下了，而且全部分开存！重要信息还要存一份txt，方便查看是否正确。

    已经跑完，结果也保存了，并将`word_subjects_dict`以txt方式存了下来，方便查看。

2. 一个问题，多个candidate，可以把原placeholder的batch_size改为None。

3. qa_path中的s和r注释掉。

4. `after_raw_data`中除了neg都要生成！

5. 为了在训练的时候区分relation和subject，策略是将relation设为upper，而subject设置为lower。这样relation中的单词就有了自己的表达，而且这种表达更能体现这些单词在relation中的真实语义。这一点构思很巧妙，一个很好的trick！

6. 检查`word_2_subject_dict`的key不可以是标点符号

7. 测试一下正则表达式的功能

    经测试发现，师兄新设计的那种正则表达式不太好用，可能也怪我不太理解“|”这个符号的用法。


---

# 2016.11.14

1.  内存爆掉，这个问题怎么解决呢？
    分batch存储……

      这导致读的时候又是问题！很恶心。

2. 写到txt文件里


3.  写几个API：
    * 得到所有candidate和question的overlap
    * 计算overlap连续匹配长度，选出top_k个作为候选，这样就有效降低了数据量！嘿嘿嘿，开心。这是最简单的方式，也是最要紧的方法，赶快实现！
    * 统计overlap在question中的位置
    * 统计overlap长度占question长度的比例
    * 统计overlap长度占subject长度的比例


---


# 2016.11.17

1.  指定目录空间不足？

2.  安装过后怎样查看是否安装成功，virtuoso这玩意是个什么鬼，有什么用途？  


---

# 2016.11.20

1.  python的dict底层使用hash表实现，查找时间为常数，很吃内存！我一直以为Python的dict的实现是搜索树！我擦，跟师兄说错了啊……很尴尬。

2. 时间都花在查询overlap上了，查找一个问题的所有overlap需要1到2分钟！20000个问题，这不得了啊！

    把所有cpu和内存都用上，完全计算所有的overlap花了两天的时间……还是挺慢的，但是好在跑出来了，后面再说优化的问题吧。

3. subject竟然是问句！subject的真实名字有问题！

    `alias_dict`有问题！很有问题！还要往前查`alias_dict`的生成过程！好烦！

4. Python的效率太低了！看来真的要用C++重新写那些代码了！想想还真是有点小激动呢！

    使用多进程而不是多线程，快了很多，cpu和内存完全占有的感觉真好。

---

# 2016.11.25

1.  晚上睡觉没睡好，把virtuoso的思路整理了一下，结合暑假实习的经历，感觉应该可以解决，就是看浏览器发出的包的信息。

2.  前期准备的代码，就是分批处理的代码，还没想好，可是我跟师兄说的进度可不是这样的……啪啪啪打脸

3.  同一个option还可以有不同的参数？可是这个参数在哪呢？

4.  要到服务器上的torch环境中试验一下lua环境

---

# 2016.12.12

1.  改subject_relations

    ```python
    import zmx_params as params
    import cPickle
    def load_dicts_from_file():
        """
        test_subject_relations_dict may need to modified,
        because it only contains the sujects in the test_set!
        """
        with open(params.test_subject_relations_dict_pickle_path, 'rb') as f:
            subject_relations_dict = cPickle.load(f)
        with open(params.test_candidate_subjects_for_all_questions_pickle_path_multiprocessing, 'rb') as f:
            candidate_subjects_for_all_questions = cPickle.load(f)
        return subject_relations_dict, candidate_subjects_for_all_questions
        
    ```

    2.调test_session_4把代码调通，测出accuracy。

    3.修改test_batch中函数调用的问题，将false改为ture。
    ```
    test_questions_phrases_natural_language = \
            [self.identify_phrases(self.nl_word4phrase_dict, x, False) 
             for x in test_questions_words_natural_language]

    ```

---

# 2016.12.14

1.  把train，validate，test和fb5m中的所有id都到数据库中查一遍，能查到的记入dict，不能查到记入list，都存下来，注意分别m中的尾实体不止一个。
    已经解决的差不多了。

2. fb5m数据中有大量的重复，导查询速度很慢，需要去重，这也导致返回的没找到name的实体id大量重复，需要去重！这很关键！

      fb5m因为数据量大而且大量重复导致查到一半服务器拒绝提供服务了……真尴尬
    经过去重，还是出了同样的bug，不知道为啥？真是奇怪！不知道查了什么就导致服务器死掉了，好奇怪。
    在两个版本的virtuoso都遇到了相同的问题，好奇怪啊。
    fb5m中的id也是只能查到1/3，真是烦啊！
    为什么都只能查到1/3，这是什么原因啊？
    因为数据被删了！freebase在不同时期的发布的数据不一样！


---

# 2016.12.16

1.  graph是什么概念？

2. prefix有什么用？


---

# 2016.12.17

1.  valid_set中有非asc字符！程序异常退出！尴尬，但是我不想再debug了，很烦！

    还是要三码合一，Python程序内部可以改动的有解释器编码和代码编码，剩下的就看系统的编码了。不过Python内部能改变的两个固定为utf-8之后，基本问题就不大了。


---

# 2016.12.23

1.  实现N-Gram+，测召回率，准确率，F1值。关注候选实体，控制s-r pair的数量。

2. 配合郎爽弄明白条件随机场和RNN的原理、实现。把基于torch的RNN模型run起来，测准确率，召回率和F1值。


---

# 2016.12.26

1.  拿到了问题不知道答案（s-r pair）。

    郎爽给我的新版本的标注数据还是真实值和预测值对不上号……

2. 部分含有unknown， 预测和实际问题对应关系不明确。

    预测数据的句子中有unknown无所谓，我们是从真实数据找实体名。只需要从预测数据中得到对应位置的分数就行。


​	
