## Wrapper
#### 以经典的也是最常用的wrapper为例：

```python
import time
def time_recorder(func):
	def wrapper(*args, **kwargs):
		print "-" * 100
		print "function {0} begin......".format(func.__name__)
		start_time = time.clock()
		res = func(*args, **kwargs)
		end_time = time.clock()
		dur = end_time - start_time
		print "function {0} over......".format(func.__name__)
		print "function {0} used: {1}s\n\n".format(func.__name__, dur)
		print "-" * 100
		return res
	return wrapper
```

---

## 使用python调用C语言函数

python中把计算密集的程序使用C来写，比如for循环，再调用动态链接库就非常爽了。

* 编写C语言文件
```c
// foo.c文件
#include <stdio.h>  
float foo(int a, float b, char str[])  
{  
  printf("you input %d, %f, %s\n", a, b, str);  
  return a+b;  
}  
```

* 编译成动态链接库

```shell
gcc -o libfoo.so -shared -fPIC foo.c
# -shared指定生成动态链接库，而不是可执行程序，
# PIC=position indepedent code，
# 生成动态链接库必须使用这个参数，这样动态链接库才会使用相对地址。
```

* 编写python脚本文件

```python
# test.py文件

import ctypes
 
lib=ctypes.cdll.LoadLibrary('/Users/fun/desktop/libfoo.so')
pstr=ctypes.create_string_buffer(b'abc')
lib.foo.restype=ctypes.c_float
print(lib.foo(2,ctypes.c_float(3.5),pstr))
```

* 运行python脚本文件
```shell
python test.py
```
得到:
```
you input 2, 3.500000, abc
5.5
```
如果c函数返回值是int型，则不需为lib.foo对象指定返回值类型。如果输入参数是int型，也不需进行转换。如果输入参数不是int型就要转换。

---

## python的多进程

---

## 使用params.py定位project中的非代码文件
* params.py文件还是有必要的，可以存完整目录！不同的目录设置了\_\_init\_\_.py 文件之后从外部调用文件中的函数时，其中的文件目录的相对位置就变了！不是当前目录了！很可怕。


---

## python代码风格
* 倾向于用下划线来链接不同单词，使用大小写区别class和object，而java和c++都更倾向于用camelCase。

---

## python中class的field
* python中的class的field不是仅有在\_\_init\_\_.py中才能指定，在任何调用的地方都可以为对象添加新的field，这相当不规范！这就让人想到java和c++的好了！


---



## 查看对象信息
* 使用dir()函数可以查看对象的所有属性和方法。
  ```python
  >>> dir('ABC')
    ['__add__', '__class__', '__contains__', '__delattr__', '__doc__',  
    '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__',  
    '__getnewargs__', '__getslice__', '__gt__', '__hash__', '__init__',  
    '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__',  
    '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__',  
    '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__',
    '_formatter_field_name_split', '_formatter_parser', 'capitalize',  
    'center', 'count', 'decode', 'encode', 'endswith', 'expandtabs',  
    'find', 'format', 'index', 'isalnum', 'isalpha', 'isdigit', 'islower', 
    'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip',  
    'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition',  
    'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip',  
    'swapcase', 'title', 'translate', 'upper', 'zfill']
  ```
     类似“\_\_xxx\_\_”的属性和方法都是有特殊用途的，但是都是可以用改对象调用的！下面的代码是等价的：
    ```shell
    >>> len('ABC')
    3
    >>> 'ABC'.__len__()
    3
    ```

---


## python的反射：
  ```python
  fields = dir(obj)
    for f in fields:
      if f[0] != '_':
        print "{0}: {1}".format(f, getattr(obj, f))
  ```
* 上述方法可访问自定义方法和属性，但是暂时不能调用方法，这挺恶心的！也可能是我暂时还不知道！


---

## 正则表达式

1.  \*：匹配前一个模式0~N次，默认情况下实现贪心算法，即一直匹配到N最大的位置

 2.\+：匹配前一个模式1~N次，默认情况下实现贪心算法，一直匹配到N最大的位置

 3.\？：匹配前一个模式0~1次，默认实现贪心算法，但是最多只匹配1次，在实际应用过程中可以使用？消除其他匹配模式的贪心性质。

 4.\.：匹配一个任意字符，默认情况下不匹配换行符，添加re.S参数时，.可以匹配任意字符，包括换行符。实际应用过程中，（.*?）联合在一起，表示需要截取的内容。生成一个列表。我仿佛知道为什么原来自己爬糗事百科为什么解析出来的全是空白了，应该是没有加re.S或者re.U参数！

 5.I：左右都是模式，一旦左侧模式匹配成功，不在匹配右侧模式（用法还是不明白！）


---


## 数据处理常用的包


* sklearn

    * preprocessing的scale（）是什么用法？为什么要这么用？


---

* pandas读取csv、excel

    * `read_csv()`函数很好用，没有表头，将header参数设为None，即`read_csv(header=None)`。 
        * 有个问题是读取纯数值文件时占用内存太多，不如手写读文件代码效率高。这是因为这个函数直接以string来读文件，每个数值都会转为一个string，string默认初始capacity为32 \* 4B，而一个float或int只有4B或者8B！这一下就多出32倍的内存使用量！
    * `to_csv(index=False)`也很好用，这对dataframe类型数据很好用，非这种类型，还是用pickle比较好。


---
## 2016.11.20

1.  dict的底层

    dict底层是hash表，存储键值（key-value）对，要对键（key）进行hash化，而这个hash后得到的值可以直接得到value的存储位置，因此key只能是不可变类型。

    2.set底层

    set底层也是hash表，只是key和value相同，set的key和dict的key类似，也要对key进行hash化，因此set的元素只能是不可变类型。

---

## google代码阅读

1.  dict的get方法非常好用
    ```python
    d = {}
    with open(file_name) as f:
        for line in f:
            l = line.split()
            d.get(l[0], {})

    # 用这个函数就不需要事先判断key是否在dict中了
    # 非常方便，代码更加简洁了！
    ```
    google的代码就是好！还是要多看大神的代码，不断思考，才能快速成长！光看API文档没用，大神的代码实现简单而高效，自己实现的代码过分冗长！

    2.多用join和[x for x in list]，
    ```python
    str_list = ["2012", "05", "06"]
    s = "-".join(str_list)
    print s
    # 输出：2012-05-06

    l = [x[0] for x in str_list]
    print l
    # 输出：list: ["2", "0", "0"]
    ```

---

## 2017.5.13

1. 拼接多个list
    ```python
    l1 = [1, 2]
    l2 = [3, 4]
    l1[len(l1):len(l1)] = l2
    print l1
    # 输出l1: [1, 2, 3, 4]
    # 这是从尾部插入,也可以从头部和中间插入，将索引的位置改一下就可以了。

    # 直接相加也行！
    print l1 + l2
    # 输出[1, 2, 3, 4]

    ```

---


## 2017.5.23

1. gensim是个很好用的库

    * 将文本取出，分词之后，生成列表或者二维列表，可以直接生成字典,以后就不用自己生成相应的字典了！

        ```python

        import gensim
        import jieba

        def foo():
            lines = []
            with open("weibo_texts/test.txt") as f:
                for line in f:
                    lines.append(jieba.cut(line))
            d = gensim.corpora.Dictionary(lines)
            for word, index in d.token2id.iteritems():
                print word, index
        ```


## 2017.6.1

1. collections包中自带有Counter类，非常方便，以后再也不用写那种非常丑的统计频率的代码了！

    ```python
    from collections import Counter
    
    def foo():
        s = "here we go"
        word_counter = Counter()
        for w in s.split():
            word_counter[w] += 1
        for k, v in word_counter.iteritems():
            print k, v
        # 输出为:
        # here 1
        # we 1
        # go 1
    ```


## django入门

1. 有着多对多关系的实体注意：
   
   不能直接在构造对象的时候传递参数，只能在构造完对象，并且使用`save()`
   函数将实体存入数据库之后，再为实体添加具有多对多关系的子实体。
   ```python
    t = Tag.objects.get(name="tag test")
    p = Post()
    p.title = "aa"
    p.save()
    p.tags.add(t)
    ```

2. 数值类型的`Field`必须设置默认值
    ```python
    talk_direction = models.IntegerField(blank=True, default=0)
    ```
    

## 遍历目录下的文件或目录

遍历目录有两种方式：

1. `os.walk`方法：
    
```python
import os
list_dir_and_files = os.walk(dir)
for root, dirs, files in list_dir_and_files:
    # 遍历目录下的文件 
    for item in files:
       pass
    
    # 遍历目录下的目录
    for item in dirs:
        pass
```

2. 第二种方法忘掉了！


## str类型和Unicode类型互换

就是下面的方法：
```
str  -> decode('the_coding_of_str') -> unicode
unicode -> encode('the_coding_you_want') -> str
```


## json包的主要用法

`dump()`方法和`load()`方法一起用，用于文件和dict的转换。
而`dumps()`和`loads()`方法一起用，用于str和dict的转换。

```python

import json

d = {'a': 10,
     'c': [20, 30]}
json_d = json.dumps(d)
print json.loads(json_d)

json.dump(d, open("file_name", "w"))
print json.load(open("file_name", "r"))

```
`load`和`dump`都使用了反序列化，这样更加高效。

## django经验
* 在本地开启服务，使用ip和端口访问服务

    先将本机ip加入ALLOWED_HOSTS中
    ```python
    ALLOWED_HOSTS = ['10.205.42.227', 'localhost', '127.0.0.1']
    ```
    
    之后启动服务，指定端口号，之后就可以使用ip和端口号从任何设备访问服务了！
    ```bash
    sudo python manage.py runserver 0.0.0.0:9999
    ```
















