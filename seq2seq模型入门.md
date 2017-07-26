## 2017.1.11

1. 分布式tensorflow的运行机制是什么样的？
2. 能在自己的主机上使用分布式的tensorflow吗？怎么测试？
    可以做测试，但是没有写出具体流程。
3. flag的作用是什么？
4. app的作用哪呢？
5. 先按train的流程看一遍，把这个流程搞通再说！


---


## 2017.1.12

1. 参数初始化语句	
    ```python
    tf.initialize_all_variables()
    # tf.initialize_all_variables()用于单机版tensorflow

    tf.global_variables_initializer()

    # 在分布式版本的tensorflow中，
    # 分global_variable和local_variable。
    # 在分布式版本的tensorflow中，global_variable通常指一些全局的变量，这些变量由所有机器共享，
    # 如模型的参数，比如权重w和偏移值b。
    # local_variable通常指一些用于数学运算的计数器，如每台机器上的世代参数epoch

    ```
2. 三层的RNN-cell的工作原理是什么样的呢？

    就是低层的每个cell得到的隐状态不仅会传给本层的下一个cell，还会传给上一层同一位置的cell。

3. bucket的作用是什么?

    就是一个encoder和decoder的输入都定长的序列模型。神经网络在处理定长的序列的时候非常给力，效率很高，这是因为定长数据利于并行处理，GPU的优势就在于并行处理。所以而翻译模型的输入数据（sentence， word seq）长度差异非常大，当然也可以不用bucket，全部用最长的序列长度装载这些序列也可以，只是这样效率不高，因为要填充大量的UNK。在分布式系统中，设计多个不同size的bucket，分别不同长度范围的输入输出序列。然后将不同size的bucket扔到不同的机器上跑，这样效率非高出很多。这是很好的

4. 训练seq2seq模型的时候，编码器的输出作为解码器的输入。实际解码的时候编码器的输出不作为解码器的输入，两者作为独立的系统。

5. 数据和代码的分离要更加彻底！那就是把数据放到URL上，而且每次生成新数据都要考虑如果数据已经有了的情况。


---

## 2017.1.14

1.  https://codegists.com/code/beam-search-tensorflow/

2. 杜绝代码的复制！


---

## 2017.1.15

1.  为什么encoder的输入要在padding之后执行reverse操作？而decoder的输入却不进行reverse操作？这都是什么悬念啊？

2. 为什么要进行维度变换，把batch_id作为二级维度，而把encoder_id作为主维度？把batch_id作为主维度不是更好理解吗？
    
    难道是为了并行？这么理解倒是可以，但是暂时没可靠的证据！

3. 投影过程不太清楚，必要性大概明白，但是具体工程不清楚，为什么这样会work？？？

4. lambda函数：

   lambda x: f(x)，x为参数，f(x)为单句函数。

   ```python
   num_name_dict = {
               1: "赵明星",
               2: "陈琨",
               3: "蒲黎明",
               4: "张庆恒",
               5: "康家鹏",
               6: "郭明",
               7: "肖宗阳",
               8: "王刘"
               }
   l = sorted(num_name_dict.iteritems(), key = lamda x: x[1])
   # 将上述dict转为list之后按value排序
   
   ```


---

## 2017.1.16
1.  多用enumerate函数，用这个函数就不需要用counter了！多方便！

2. 凡是要用到key的地方，基本都会用到hash，因此要注意这个问题！byte和text的区别有一点就是由此而来。str类是byte类的子类。

3. symbol表示的是什么意思？就是token？

   就是单词的代号？



## 2017.1.17

1.  attention是什么意思？

    就是指PAD_ID, GO, STOP这些symbol？


---

## 2017.1.19

1.	beam是什么意思？？？



## 2017.3.8：

1.  前会神经网和RNN的区别？？？

    前会神经网没有时间的概念，记忆功能微弱，只对神经网形成的过程中的输入敏感，即对最初的几次输入比较敏感，因为这几次的输入造成的梯度比较大！而对较近几次的输入不敏感，梯度下降时会导致梯度弥散。可是RNN也有这样的问题！！前会神经网的结构如下：

    ![image](http://note.youdao.com/yws/public/resource/f8d7e3dff6b1129ff4c32a7cbc985552/xmlnote/WEBRESOURCE093c126c939d530ecd799d0614ce5110/2340)

    而RNN一个简单的示例结构如下：
    ![image](http://note.youdao.com/yws/public/resource/f8d7e3dff6b1129ff4c32a7cbc985552/xmlnote/WEBRESOURCE67a835f1b18da7613b180a5d538de593/2344)

    RNN的隐状态的公式为:

    $$
    \mathbf{h_{t}} = \phi \left( \mathbf{Wx_{t} +} + \mathbf{Uh_{t-1}} \right)

    $$

    可见本步输入和上一步输出的权重不是一样的！有专门的参数。W决定本轮输入和上轮输出（隐状态）的重要程度，这些重要程度随机初始化，经过训练过程的反向传播，最后把重要程度矩阵训练到一定程度，loss不能再减小，就代表这些参数所代表的权重很好地表示了序列的模式。U相当于马氏链中的转移矩阵。


2.  为什么tensorflow要使用非scalar的summary，就是因为这可以查看weight中的值是否都集中到1或-1附近了，一般情况下应该集中在中间！！这可以保证较好的学习效果。

3. 神经网络中的time和layer
 
    time就是指RNN的一步，layer就是指网络的一层。

4. N-GRAM的核心公式：c可以用频率代替。

   $$
   P_{ML} \left( e_{t} \left. \right| e_{1}^{t-1} \right) = \frac {c_{prefix} \left( {e_1^t} \right)} {c_{prefix} \left( {e_1^{t-1}} \right)}
   $$
   但是这并不现实，对于没有出现过的句子，概率都是0，这样的模型效果会大打折扣。于是有了下面的近似模型。这样就可用一连串的条件概率和第一个词出现的概率相乘表示一个句子出现的概率。
   $$
   P_{ML} \left( e_{t} \left. \right| e_{1}^{t-1} \right) \approx P_{ML} \left( e_{t} \left. \right| e_{1}^{t-1} \right) 
   $$

5. python太慢了！在本地的笔记本上跑读取2G大小是文件的程序，居然花了1天！！我都服了！我要写C++了！！

   ==事实证明我代码写错了！==

6. dev这个数据集是干嘛的！！！握草！？？？

7. distribution of high or low order 什么意思？？？

8. 为什么要设置unk？？？
 
    模拟测试集中将会出现的生词！


## 2017.3.14：


1. 变量含义	
    ```python
    encode_ntoken = 250
    decode_ntoken = 62
    ```
    这两个变量到底是什么含义？



​    