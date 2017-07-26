## 安装过程
* 安装Java，要求版本1.6以上。
* brew install hadoop（Mac环境）
* 查看自己的JAVA_HOME，打开终端输入：
  /usr/libexec/java_home -V

* 修改三个文件
  * > /usr/local/opt/hadoop/libexec/etc/hadoop/hadoop-env.sh,
  * > /usr/local/opt/hadoop/libexec/etc/hadoop/mapred-env.sh and
  * > /usr/local/opt/hadoop/libexec/etc/hadoop/yarn-env.sh

* 修改完文件之后使用三条命令运行hadoop
  1. 格式化namenode
    ```shell
    $ ln -s /usr/local/Cellar/hadoop/2.7.3/bin/ namenode_init
    # 创建软链接，直接到该目录下即可
    # -s选项建立软链接，去掉则建立硬链接

    $ hadoop namenode -format
    # 格式化namenode,命令1，只需运行一次，再次使用需要再运行。
    ```
  2. 开启hadoop相关组件
    ```shell
    $ ln -s /usr/local/Cellar/hadoop/2.7.3/libexec/sbin/ start_hadoop_commands
    $ cd start_hadoop_commands
    $ start-dfs.sh
    # 开启dfs，命令2.

    $ start-yarn.sh
    # 命令3.
    # 安装hadoop并初始化namenode之后，命令2和命令3以及下面的stop命令配套使用。
    ```
  3. 停止hadoop相关服务
    ```shell
    $ cd start_hadoop_commands
    $ stop-all.sh
    # 停止所有hadoop服务。
    ```

---

* 使用下面的链接可以查看本地的各种hadoop服务
  * HDFS status: http://localhost:50070
  * Cluster Status: http://localhost:8088
  * Specific Node Information: http://localhost:8042
  * secondaryNamenode http://localhost:50090

* 也可以使用jps查看当前与hadoop相关的进程

  ```shell
  $ jps
    1809
    2065 Launcher
    2321 DataNode
    2562 ResourceManager
    2229 NameNode
    2661 NodeManager
    2438 SecondaryNameNode
    2697 Jps
    2059
  ```
  这种情况是正常的，说明hadoop的所有组件均已运行起来了。

---

## 使用过程

* 由于hadoop是用homebrew工具安装的，安装目录在
  ```shell
  /usr/local/Cellar/hadoop/
  ```
  而上述的安装目录对intelij idea不可见，最基本的依赖无法添加，使用下面的方式可以解决问题：
  ```shell
  $ cd ~
  $ mkdir HadoopCommon
  # 在intelij idea可见的目录下，创建一个hadoop 专属的文件夹，
  # 里面只存有一个软链接文件！

  $ ln -s /usr/local/Cellar/hadoop/2.7.3/libexec/share/hadoop hadoop_common
  # 创建软链接，将该目录链接到hadoop和核心目录下
  # 后面如果需要，可以重现建一个新的链接文件就叫 hadoop，
  # 将其链接到hadoop的安装目录下 /usr/local/Cellar/hadoop/
  # 这样就可以实现无缝转换！
  ```
  ==这就是我尝试使用hadoop遇到的第一个坑==

---


*  添加依赖第一步添加hadoop-common  
    * File 
    * Project Structure... 
    * Modules 
    * Dependencies 
    * \+ 
    * Library... 
    * New Library...
    * Java 
    * 转换到上述的软链接位置
    * 选择除了httpfs外的全部文件夹
    * Name可以随便写，例如”hadoop-common”，
    * 点击OK结束。


* 添加依赖第二步，添加lib
    * File 
    * Project Structure... 
    * Modules 
    * Dependencies 
    * \+ 
    * Jars or directories...
    * 转到上述软链接位置
    * 选择 *common/lib*
    * OK

此时Dependencies内应该总共增加了一个”hadoop-common”和一个”lib”目录。

---

* 查看hadoop CLASSPATH
  ```
  hadoop classpath
  ```

---


* 为hadoop程序传递输入：
  ```shell
  $ hadoop fs -mkdir /user
  # 在node上创建user目录

  $ hadoop fs -mkdir /user/input
  # 在node的user目录下创建input目录
  # 之后就可以将本地文件传递到node上l

  $ hadoop fs -copyFromLocal ~/Desktop/zmx/hadoop_input_file/dedup.txt /user/input
  # 在程序中就可以处理该输入文件了！

  ```

---


* hadoop程序输出(输出目录在程序运行之前必须不存在)
  ```shell
  $ hadoop fs -cat /output/*
  # 查看程序输出结果。

  $ hadoop fs -rm -r -f /output
  # 删除输出目录，方便下次再次运行程序。
  ```


---

* 常见Warning
  ```
  WARN util.NativeCodeLoader: Unable to load native-hadoop library for your 
  platform... using builtin-java classes where applicable
  ```
  这个异常不必理会！


---

* 一个集群只能运行一个程序，重写一个hadoop程序要重新格式化集群，重新跑程序。


---

## 2017.4.7

* 为什么不能免密登入？？
  ```shell
  $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  ```
  通过上述命令即可解决。


---


* 为什么datanode起不来了？？？ 

  ==可以参考下面链接中的博客==:
  [如何解决多次格式化之后hadoop的datanode无法启动的问题](http://www.cnblogs.com/ilinuxer/p/5052391.html)

  具体方法就是：
    ```shell
    $ cat logs/* | grep "Incompatible"
    # 分析日志文件，找到datanode的位置 target-dir
    
    $ cd target-dir
    $ rm VERSION
    # 这就是博客中的第一种解决方法

    ```

 ---


## 2017.4.8

  ```java
  job.setOutputKeyClass(Text.class);
  job.setOutputValueClass(Text.class);
  ```

  需要注意这两行同时设置了mapper和reducer的输出类型，注意保持一致，算是一个小坑吧。

---

## 组里的hadoop机器

```
ISIGFILESERVER 10.109.247.133 		root	vlywemgz   		hadoop hadoop

zhizhi-Lenovo	10.108.113.19 		

shaohuaxi 2016111470	hadoop hadoop

deeplearning-P8000 10.108.113.208	deeplearning		hadoop hadoop

airfan-Lenovo hadoop hadoop

```

---

## 非常好的hadoop入门demo完整代码

[hadoop demo完整代码](http://penghuaiyi.iteye.com/blog/1943464)

 