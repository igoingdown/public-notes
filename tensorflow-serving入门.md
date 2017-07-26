## tensorflow serving 入门

1. 安装bazel及tensorflow serving的依赖
    * 下载bazel安装脚本文件
    
        [链接在这里](https://github.com/bazelbuild/bazel/releases),
        注意选择正确的系统版本和JDK版本，如果安装了JDK8，
        可以选择不含JDK的版本。最新的bazel版本是`0.5.1 `。
        ==但是这个版本不好，总是会编译错误！最好使用`0.4.5`版本！==
        
    * 安装bazel
    
        ```bash
        chmod +x bazel-0.4.5-installer-linux-x86_64.sh
        ./bazel-0.4.5-installer-linux-x86_64.sh --user
        ```
    
    * 安装依赖
    
        ```bash
        sudo apt-get update && sudo apt-get install -y \
        build-essential \
        curl \
        libcurl3-dev \
        git \
        libfreetype6-dev \
        libpng12-dev \
        libzmq3-dev \
        pkg-config \
        python-dev \
        python-numpy \
        python-pip \
        software-properties-common \
        swig \
        zip \
        zlib1g-dev
        ```
      
2. 下载tensorflow serving

    ```bash
    git clone --recurse-submodules https://github.com/tensorflow/serving
    ```
    
3. 设置build所需的参数

    ```bash
    cd serving
    cd tensorflow
    ./configure
    cd ..
    ```
    我就是因为没有运行`./configure`这行代码，直接浪费了两天时间！
    
4. 构建demo模型(mnist)

    ```bash
    bazel build //tensorflow_serving/example:mnist_saved_model
    bazel-bin/tensorflow_serving/example/mnist_saved_model /tmp/mnist_model
    ```
    不出意外这里会遇到一个bug，提示如下：
    ```
    tensorflow.python.framework.errors_impl.NotFoundError: 
        /home/deeplearning/zmx/serving/bazel-bin/tensorflow_serving/
        example/mnist_saved_model.runfiles/org_tensorflow/tensorflow/
        contrib/image/python/ops/_single_image_random_dot_stereograms.so: 
        undefined symbol: 
        _ZN6google8protobuf8internal10LogMessageC1ENS0_8LogLevelEPKci
    ```
    解决方法参考[这里](https://github.com/tensorflow/serving/issues/421)。
    简单总结就是注释掉文件`bazel-bin/tensorflow_serving/example/mnist_saved_model.runfiles/org_tensorflow/tensorflow/contrib/image/__init__.py`中的一行。
    把下面的代码注释掉就行。
    ```python
     from tensorflow.contrib.image.python.ops.single_image_random_dot_stereograms import single_image_random_dot_stereograms
    ```
    再次运行
    ```bash
    bazel-bin/tensorflow_serving/example/mnist_saved_model /tmp/mnist_model
    ```
    训练模型，这次可以正确执行。执行结果是保存训练好的模型到`/tmp/mnist_model/`中，
    
    
5. 构建将模型作为服务开启的模块
    ```bash
    bazel build //tensorflow_serving/model_servers:tensorflow_model_server
    ```
    
6. 开启模型服务
    ```bash
    bazel-bin/tensorflow_serving/model_servers/tensorflow_model_server --port=9000 --model_name=mnist --model_base_path=/tmp/mnist_model/
    ```
    
7. 编译客户端
    ```bash
    bazel build //tensorflow_serving/example:mnist_client
    ```
8. 开启客户端程序，连接服务端并测试
    ```bash
    bazel-bin/tensorflow_serving/example/mnist_client --num_tests=1000 --server=localhost:9000
    ```
    
* 注意 

    ==中间运行服务端和客户端代码的时候可能需要安装一下缺少的package，直接安装然后继续运行就好。==
    
