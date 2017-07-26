## 2017.2.8

1.  linux环境下c程序到可执行程序的步骤（以test.c为例）。对于c\+\+程序只要将 *gcc* 改为 *g++*即可。
    * 预处理
      ```shell
      $  gcc -E test.c -o test.i
      # 预处理生成test.i文件

      ```
    * 编译为汇编代码
      ```shell
      $ gcc -S test.i -o test.s
      # 由预处理生成的文件生成汇编语言文件
      # 可以加入编译优化命令，分三个层次
      # -O，第一级优化，-O2第二级优化，-O3第三级优化.
      ```

    * 汇编生成目标文件
      ```shell
      $ gcc -c test.s -o test.o
      ```

    * 链接生成可执行文件
      ```shell

      $ gcc test.o -o test

      ```

    2.mac上安装gdb

    * 执行下面的命令：
    ```shell
    $ ruby -e"$(curl -fsSL  https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```

    * 更新homebrew软件库并安装gdb。
      ```shell
      $ brew search gdb
      # 检查gdb是否在homebrew的软件库中

      $ brew update
      # 更新homebrew软件库

      $ brew install gdb
      # 安装gdb

      ```

    * 修改钥匙串  

        [修改钥匙串详细方法](http://www.jb51.net/os/MAC/405488.html)


    然而gdb和现在的mac系统 sierra不兼容……呵呵呵。
    但是过程还是挺惊险的，还学到了不少linux环境下编译运行C/C++程序的方法。


---

## 2017.2.9

* 使用docker环境玩gdb

  第一个docker不行，我还可以再多弄几个docker出来！
    ```shell
    $ docker run -d -it --security-opt seccomp=unconfined --name ds schickling/rust
    # 这个命令生成的docker
    
    ```

  container应该可以运行gdb

    如果不行换个命令：

    ```shell
    $ docker run --rm -it --security-opt seccomp=unconfined --name ds schickling/rust
    ```
  这个命令生成的docker container是一定可以运行gdb的！

  吼吼。真是开心。又有了一个调程序的利器！

  这些容器都比较低端，没有vim，有的连vi都没有！

  最好使用目录映射，让docker container和宿主机共享一个目录.

  这样就可以在本地用高端的编辑器来写代码，在docker中用gdb调试了！

  所以上述命令可以改为：

    ```shell
    docker run -d -v ~/Desktop/zmx/gdb_share:/gdb_share -it --security-opt seccomp=unconfined --name gdbs schickling/rust
    ```
  但是这么一来docker container中还是没有vim，好烦！

  还是要更新apt-get，然后安装vim！



## 2017.2.10

1.  酷壳上面的文章现在也不一定是对的了，C/C\+\+在更新，一直在更新，在变得更安全！比如

    http://coolshell.cn/articles/4907.html

    中的第一个例子，我在linux上得到的结果就不一样！我这边的输出结果是0，8！系统版本是debian，gcc版本是5.3.0。这个版本的gcc中，局部变量也会自动初始化了！很多东西都要自己试了才知道，这点对于C/C++尤其重要，因为自己看到的跟自己运行的结果可能大相径庭！文章中的第二个例子没分析程序的输出结果，甚至连提都没提……日了狗了！我在debian gcc4.7.2下得到的结果是：使用volatile和不使用volatile没什么区别！！！

    2.无符号整数溢出用最大的该类型最大的数取模，实际就是截断！
    有符号整数溢出是未定义的undefined！undefined是C/C\+\+中最可怕的事情！undefined的行为编译器想怎么干就怎么干！甚至可以玩彩蛋！目前比较流行的C/C\+\+编译器有gcc（g\+\+），这是Unix系列操作系统默认的编译器；Windows的编译器一般是VC++。

    3.http://coolshell.cn/articles/5761.html

    这篇文章中的内存对齐讲的很好，内存对齐（memory alignment）是编译器的干的活，编译器计算一个结构体struct的大小时，是边计算边对齐的，因此结构体元素的声明顺序会影响结构体的大小！原顺序不变，容量需要细细思考，注意每种类型的默认占得字节数和类型变化处！在说明数组的地址时，数组或者指针的地址 + 或 - 操作，必须考虑内部数组的大小，而且高维要变到一维！注意64-bit系统中指针占8B！

    4.编译器干了什么活？每一步又具体干了什么？？？？  

    预处理，编译，汇编，链接。


5.	http://coolshell.cn/articles/11377.html  
    这篇文章说到的访问结构体的实质是（&t + bias），t是结构体实例，即指针。bias就是元素相对于结构体的偏移量！第一个元素的bias为0！花了我好长时间终于理解了！


---

## 2017.2.13：

1.  http://blog.csdn.net/haoel/article/details/24058  

    这篇文章写得太早了，是04年写的了！现在的C\+\+比04年的改变了很多，那时候的copy_on_write已经不能复现！！我在本地跑的结果跟博客中的结果完全不同！

    http://blog.csdn.net/haoel/article/details/1491219

    这篇文章中提到这种copy_on_write（COW）技术在多线程/进程环境下，极易出现错误，并预测在新版的C\+\+标准中被移除，还真是被移除了！

    2.意图，规格，实现！


---

## 2017.2.13：

1.    
    http://coolshell.cn/articles/10115.html

    这篇文章中有一段代码：

    ```c++
    struct {
       char a;
       int b;
    } b = { 2, 4 };
    ```
    这里面b定义了两次，为什么不报错？
    这里并没有定义两次，大括号分开了作用域！！所以这不是重复定义！只有链接的时候才会处理多个文件中重复定义的全局变量！所以那些所谓的抢符号和弱符号都只是相对于链接这个过程而言的！


## 

2017.2.18：

1.  vector的构造函数中用于同时将多个元素初始化为相同值的形式：
    ```c++
    vector<type> vec(elementNum,  elementTypeValue)
    //是个数在前面！！！

    ```

    2.三目运算符的书写格式是：

    ```c++
    condition ？ expression1  ： expression2

    ```



## 2017.4.14

---

* 浮点数转换为int
    * 下行转换
        ```c++
        float x;
        int target = int(x);
        // 普通的强制转换
        ```

    * 几舍几入
        ```c++
        float x;

        int target = int(x + 0.5);
        // 四舍五入

        int target = int (x + 0.7);
        // 二舍三入
        ```

    * 上行转换
        ```c++
        #include <cmath>
        float x;
        int target = ceil(x);

        ```

---


## C++的vector常用接口总结

* 清华的一个女学霸的经验帖写的很棒：

    * [小唯-THU的vector总结](http://www.cnblogs.com/wei-li/archive/2012/06/08/2541576.html)

    **PS**: 这个女学霸的python下matplotlib使用总结写的也非常棒！ 


---

## auto关键字

* 用auto关键字遍历vector

    ```c++
    vector<vector<int> > res;
    for (auto vec: res) {
        vec.push_back(1);
    }
    // 这种方式不能达到修改res的目的！
    // 因为vec是复制生成的新的临时变量！
    // 因此用auto遍历vector只能用于读！
    ```

---


## 使用C的内容

* 使用C的输入输出
    ```c
    # include <cstdio>
    // 不是使用 #include<stdio.h>
    // 前者是后者的模板化版本，也是标准的C++版本。
    // 这样就可以使用C的输入输出函数如
    // printf.
    ```


* 使用C的系统函数
    ```c
    # include <cstdlib>
    // 不适用#inlcude <stdlib.h>.
    // 前者是后者的模板化版本。
    // 这样就可使用system("PAUSE")l .

    ```

---



