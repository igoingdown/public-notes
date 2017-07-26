## du命令

```shell
$ du -h -d 1
# -d option 控制递归深度。
# -h option 会将磁盘空间的单位调整为适当大小，方便查看。
```
---


## ps命令

  ```shell
  $ ps -ax | grep python
  # 可以用这种命令查看每个进程对应的程序名
  # 可以有针对性的干掉一些进程！
  ```

---

## 免密登入

* 基本原理：
  * 在server上的authorized_keys添加client的公钥或者私钥就行。

* 方法：
  * 本地生成公钥
  * 将公钥上传到server上，并添加到authorized_keys末。


---

## deeplearning服务器开启网络服务

  ```shell
  $ bash authBuptGw.sh -i
  ```

---


## 安装tensorflow

安装tensorflow只要联网就行，不需要翻墙！！！

---



## 2017.2.4

1. 什么是linux的component philosophy？？

---

## 2017.2.6

1. 守护进程（deamon process）和僵尸进程（zombie process）
   守护进程（deamon process）：比较复杂，很多服务都是守护进程。
   僵尸进程：子进程退出后，父进程可能仍需要子进程的一些信息，这些信息仍然保留在内存中，只有父进程对子进程表示不感兴趣（SIGNAL）或者已经访问了这些信息之后，系统才会将这些剩余信息删掉，如果只完成了第一步（子进程退出）而没有迟迟不完成第二步（父进程发话），子进程就成了僵尸进程！


2. iNode：其实就代指了一个文件，可以使普通文件，目录或设备文件。
   独立文件系统（independent filesystem）才有iNode，iNode的标号就是iNum。iNode中记录了大量的信息，比如该文件的大小，权限，属组，最近的访问时间、修改时间，引用计数等，下图是iNode具体结构的定义，但是iNode不存储文件名，因为文件名和iNode是映射关系。iNode不记录文件名和文件中的数据，是操作系统可以理解的文件，是一种抽象的数据结构。文件名和相应的iNode在意义上是基本等价的，只是有时候多个文件名可以映射到同一个iNode上。文件名是用户可以理解的，而iNode是FS管理文件时能理解的概念。文件中的数据可以通过iNode中记录的数据块的位置信息来访问。根目录的iNode号即iNum是0，存在整个VFS的superblock中。

![image](http://note.youdao.com/yws/public/resource/f8d7e3dff6b1129ff4c32a7cbc985552/xmlnote/WEBRESOURCE2d2ada814394b31e541239df14f8e900/2106)



3. 文件名存储在目录文件中，存的是文件名到iNode的映射，而多个不同的文件名可以映射到相同的iNode上！这就是链接！！

4. linux的文件系统的图示如下：文件系统只需要也只有iNode，通过逐层访问iNode（广义文件）中的数据，就可以建立dentry。

 ![image](http://note.youdao.com/yws/public/resource/f8d7e3dff6b1129ff4c32a7cbc985552/xmlnote/WEBRESOURCE7149ccfe208e5413de5a4b4fbcfcd2e0/2112)


5. 用户可见的是VFS，VFS管理所有的independent filesystem，每个independent filesystem有一个superblock。

6. dentry维护文件名（广义，包括设备文件，目录文件）和iNode之间的映射。dentry只存在于内存中，就是说在开机时系统会根据iNode（对应广义文件的概念，操作系统能理解的文件的概念！）对于的广义文件的数据来生成dentry。开机读取到的第一个inode就是根目录文件对应的iNode（0号iNode）。读取到了该目录后，内核对象会为该文件对象建立一个dentry，并将其缓存起来，方便下一次读取时直接从内存中取。而目录一个文件（广义文件），目录文件的内容即是该目录下的文件的名字与inode号，目录文件的内容就像一张表，记录的文件名与其iNode之间的映射关系。根据路径即可找到当前需要读取的下一级文件的名字和iNode，同时继续为该文件建立dentry，dentry结构是一种含有指向父节点和子节点指针的双向结构，多个这样的双向结构构成一个内存里面的树状结构，也就是文件系统的目录结构在内存中的缓存了。有了这个缓存，我们在访问文件系统时，通常都非常快捷。


---


## 2017.2.21:

1.  软链接，即符号链接，是把一个目录项链到另一个目录项的文件名，而硬链接是将一个文件链接到另一个已经存在的iNode上。链接文件后缀为@。

2. 先在github上建立仓库，然后本地使用git pull，之后就可以在本地改动，最后通过git push上传文件了。前提是要有github的公钥。


---


## netstat

```shell
$ netstat -ln
# 命令查看系统监听端口
# -l选项表示显示系统监听端口
# -n选项表示按照端口号显示，而不转换为service文件中定义的端口名；若希望了解各个端口都是由哪些进程监听则可以使用p参数
```

---

## linux上删除安装的软件

```shell
$ dpkg --list
# 浏览已安装的程序

# 还可以和awk配合使用
$ dpkg --list | awk '{print $1 "\t" $2}'

$ sudo apt-get --purge remove <$program_name>
# 完全移除的程序

$ sudo apt-get remove <$program_name>
# 只移除程序但保留配置文件

```

---

## 压缩文件命令

```shell
$ gzip -l <$gzip_file_name>
# 对每个压缩文件，显示：
#   压缩文件的大小 
#   未压缩文件的大小 
#   压缩比 
#   未压缩文件的名字 

```


## 查看机器内存信息

1. 查看内存信息
```bash
free -h
# 这是查看内存总览的最好方式

top
# 这个命令很全，但是并不是很好看，参数太多，不容易用

cat /proc/meminfo
# 这个命令对各种设备都好用，比如cpu等。
```

2. 查看cpu信息
```bash
top
# 信息很全，默认按照cpu使用了将进程排序，非常清晰，实时监控最好。

cat /proc/cpuinfo
# 信息很多，但是要结合grep来使用，不是很容易使用
```
要查看cpu的更具体的信息，
请点击下面的链接：
 
[ubuntu如何查看cpu的具体信息](http://www.cnblogs.com/xd502djj/archive/2011/02/28/1967350.html)



---



```
graph LR
A-->B
```

```
sequenceDiagram
A->>B: How are you?
B->>A: Great!
```

```
gantt
dateFormat YYYY-MM-DD
section S1
T1: 2014-01-01, 9d
section S2
T2: 2014-01-11, 9d
section S3
T3: 2014-01-02, 9d
```



| header 1    | header 2    |
| ----------- | ----------- |
| row 1 col 1 | row 1 col 2 |
| row 2 col 1 | row 2 col 2 |
