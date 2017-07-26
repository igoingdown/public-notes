
## 安装virtuoso

Ubuntu系统直接用apt-get就可以安装
```shell
$ sudo apt-get install virtuoso-opensource

# 安装过程中需要设置密码，全部输入为“dba”即可。

```
这种安装方式安装的virtuoso是6.1版本，
需要安装7及以上版本的话可以到下面的链接下载源码进行安装，但是坑比较深……

[virtuoso7源码编译安装教程](https://github.com/dbpedia/dbpedia-docs/wiki/Loading-Data-Virtuoso)

## 启动virtuoso

* 第一种方式

    到virtuoso.ini文件所在目录，开启virtuoso。
    ```shell
    $ sudo virtuoso-t f

    # 如果需要后台运行在后面添加后台运行命令符号 &
    $ sudo virtuoso-t f &
    ```

* 第二种方式

    下面的方式可以开启、关闭、重启virtuoso

    ==！！推荐使用这种方式！！==
    ```shell
    $ sudo service virtuoso-opensource-6.1 {start|stop|force-stop|restart}
    ```

## 导入rdf数据到virtuoso的最全教程


[导入rdf数据到virtuoso最全教程英文原文链接](https://confluence.deri.ie:8443/display/webstar/The+complete+tutorial+for+RDF+data+ingestion+in+Virtuoso)

---


## 抽取数据生成zip文件
```shell
$ zgrep -a '\s<http://rdf\.freebase\.com/ns/common\.topic\.notable_types>' freebase-rdf-2015-01-18-00-00.gz | gzip > freebase-notable_types.gz

# 需要自定义gz文件
```

---

## 导入rdf到virtuoso实战脱坑

* 修改virtuoso.ini文件中的“DirsAllowed”字段

    ```shell
    $ cd <$target_dir>
    # target_dir一般为“/etc/virtuoso-opensource-6.1”
    $ sudo vim virtuoso.ini
    # 将从freebase数据集中抽出的数据生成的.gz文件所在目录添加在“DirsAllowed”字段的末尾,
    # 保存文件即可。
    ```

* 在终端开启SQL解释器

    ```shell
    $ isql-vt 1111 dba dba
    # 之后就进入了SQL 解释器
    ```


* 在SQL解释器下指定要load数据的路径

    ```mysql
    SQL> ld_dir ('<.gz文件所在目录>', '<.gz文件名，多个文件时可以使用正则表达式>', 'http://freebase.com');

    # 示例：
    SQL> ld_dir ('/media/deeplearning/dldisk_dorm', 'freebase-names.gz', 'http://freebase.name.com');
    ```

* 查看要load的数据列表

    ```mysql
    SQL> select * from DB.DBA.load_list;
    ```


* 开始批处理载入

    ```mysql
    SQL> rdf_loader_run();
    ```


* 查看load进度

    * 重新开启一个终端并进入SQL解释器 

        ```shell
        $ isql-vt 1111 dba dba
        ```
    * 查看load进度

        ```mysql
        SQL> select * from DB.DBA.load_list;
        ```

* ==前方终极大坑，请注意！==

    load数据需要比较长的时间，如果按照上一步的方法查看load进度，执行命令发现服务器无响应，这意味着数据导入已经完成了，而且virtuoso进入死锁状态！需要按照下面的步骤做：

    1. Ctrl+C all your isql connections.

    2. Check with `ps -aux | grep virtuoso` command there are no isql connections running.

    3. Use `sudo killall -9 virtuoso-t` to stop the service.

    4. Remove `virtuoso.lck` file located in your database directory,  `virtuoso.lck` is offen at `/var/lib/virtuoso-opensource-6.1/db`.

    5. Use `sudo service virtuoso-opensource-6.1 start` to start the server.


---

## 查询

使用sparql语句查询

查询模板代码如下：

```python
#!/usr/bin/python
# -*- coding:utf-8 -*-

"""
===============================================================================
author: 赵明星
desc:   使用sparql查询并从返回的html中解析数据。
===============================================================================
"""

import sys
reload(sys)
sys.setdefaultencoding('utf-8')

import requests
import re
import zmx_params as params
import traceback
import cPickle

# Setting global variables
# query_url = 'http://10.108.112.28:8890/sparql/'
query_url = 'http://10.210.60.106:8890/sparql/'
PREFIX =  "<http://rdf.freebase.com/ns/>"

# HTTP URL is constructed accordingly with JSON query results format in mind.
def sparql_query_name(subject_id):
    query = '''
        PREFIX ns:%s
        SELECT ?name WHERE {ns:%s ns:type.object.name ?name}
    '''% (PREFIX, subject_id)
    # print query
    params={'default-graph': '', 'query': query.encode('utf8'), 'format': "text/html"}

    r = requests.post(query_url, data=params)
    http_result = r.text
    name_pattern = re.compile("<td><pre>(.*?)@(.*?)</pre></td>", re.S)
    alias_language_list = name_pattern.findall(http_result)
    # print alias_list

    try:
        alias_list = [x[0] for x in alias_language_list]
        lang_list = [x[1] for x in alias_language_list]
        # print "lang_list: ", lang_list
        alias = alias_list[lang_list.index(u"en")]
        return alias
    except Exception as e:
        print "subject_id:\t", subject_id, "\nhttp_result:\n", http_result, "\n\n\n"
        return ""

if __name__ == '__main__':
    with open(params.test_alias_dict_pickle_path) as f:
        alias_dict = cPickle.load(f)
    # sparql_query("m/01vsll")
    for k, v in alias_dict.iteritems():
        # print k, "\n", v, "\n" * 3
        # print k.strip("/").replace("/", ".")
        print sparql_query_name(k.strip("/").replace("/", "."))
    sparql_query_name("m.01_d5")
```

---

## 完整sparql网页查询demo

```mysql
PREFIX :<http://rdf.freebase.com/ns/>
SELECT ?rel ?name WHERE {:m.043tw2m  ?rel ?name}
```
