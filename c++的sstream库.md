## istringstream类

* 问题

    stringstream不主动释放内存(或许是为了提高效率)，如果在程序中用同一个流，反复读写大量的数据，将会造成大量的内存消 耗，这时候，需要适时地用 

    ```stream.str("")```

    清除一下缓冲 

    具体的例子见链接：
    [stream大坑脱坑详细指南](http://blog.csdn.net/chjp2046/article/details/5460462)

---

* 指针失效问题：

   跟python的迭代器不同的是，c++的istringstream的指针在子函数修改istringstream之后，父hanshu的指针会失效，这时候需要调用

    ```cpp
    stream.clear();
    ```
    使指针得到正确值！

