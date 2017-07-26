# YAML 语言（发音 /ˈjæməl/ ）

## 设计目标
方便读写，实质是一种通用的数据串行化格式，比 ==JSON== 强大！

---

# 基本元素
JSON的基本元素是字典和列表，而YAML比JSON多了一个，==纯量==。

* 对象

    键值对的集合，又称为映射（mapping）/ 哈希（hashes） / 字典（dictionary）

* 数组

    一组按次序排列的值，又称为序列（sequence） / 列表（list）

* 纯量（scalars）

    单个的、不可再分的值


---

## 字典

```c++
animal: pets
hash: {name: Steve, foo: bar}

// 转到JS分别如下：
{animal: pets}
{hash: {name: Steve, foo: bar}}
```

---

## 列表


```c++
- aa
- bb
- cc


-
 - a
 - b
 - c
 
// 转到JS分别如下：
[aa, bb, cc]：
[[a, b, c]]
```

---

## 纯量
纯量包括字符串，布尔值，整数，浮点数，Null，时间，日期


```c++
number: 12.30
isSet: true

parent:~
// ~表示null

iso8601: 2001-12-14t21:59:43.10-05:00 
date: 1976-07-31

e: !!str 123
// !!表示强制类型转换

str: 'aaaaa'
str: aaa
    bbb
    ccc

// 转到JS分别如下：
{number: 12.30}
{isSet: true}
{parent: null}
{iso8601: new Date('2001-12-14t21:59:43.10-05:00')}
{date: new Date('1976-07-31')}
{e: '123'}
{str: 'aaaa'}
{str: 'aaa bbb ccc'}
```

---


# 总结 
==YAML支持的格式远不止上述的格式，还有正则表达式和函数等。这就是YAML比JSON强大的原因，但是也带来了一定复杂性！==











