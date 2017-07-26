## tensorflow

#### 安装

* mac： python3 + tensorflow0.11 + virtualenv

```bash
export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/cpu/tensorflow-0.11.0-py3-none-any.whl
pip3 install --upgrade $TF_BINARY_URL
```

* ubuntu： python3 + tensorflow + virtualenv
```bash
export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-0.11.0rc0-cp35-cp35m-linux_x86_64.whl
pip3 install --ignore-installed --upgrade $TF_BIN
```



#### 基本知识
1. tf.Graph类：包括一组Operation对象和一组Tensor对象，Operation对象表示计算单元，Tensor对象表示在计算单元之间流动的数据单元。
2. tf.Tensor 类：表示一个Operation的多个输出中的一个。最终会被Output覆盖。
3. nan错误主要是数据流图中有除零操作，认真检查
4. 注意使用激活函数，控制神经网中数据的范围，避免溢出
5. with device不像你想象的那样，神经网络中有些内置op必须使用cpu，如果在外层使用语句强制使用gpu会报错，建议打开softreplace。
6. 每一个操作（option）都可以命名，并在tensorboard中显示。
7. 可以设定一个大的变量，每个batch选取其中的一部分，先look_up然后对look_up的结果进行操作和更新，继而更新被look_up的大的变量中相应的值。
8. scalar和vector相对，分别表示标量和向量，在tensorflow中向量表述为tensor，而标量就是scalar，在summary中，分别对应scalar_summary和histogram_summary,在tensorboard中分别才event和histogram项目下查看。
9. num_units，即rnn_cell的单元数，在神经网络中表示什么含义？边？我自己理解的神经网络是分层的，但是这种层次和使用rnn_cell构建的神经网络对应不上，尴尬……
10. n-gram的作用和原理
11. rnn和dynamic_rnn的区别，dynamic_rnn和rnn函数功能相同，但是dynamic_rnn对输入数据进行完全动态展开的处理。	
