## 推导过程
| $r_{t-1}$   | 
| :-----: |
| 1.0         |
| 1.0         |
| 1.0         |



| $c_{t}$ |
| :-----: |
|  0.05   |
|   0.9   |
|  0.05   |




|$r_t$|
|:-----:|
|0.1|
|0.8|
|0.1|


|$c_{t-1}$  | P|
|:-----:|:-----:|
| c1 | 0.1 |
| c2 | 0.8 |
| c3 | 0.1 |




|$c_{t-1}$ | 
|:-----:|
|0.1 |
|0.8 |
|0.1 |



| c1 | c2 |c2|
|:-----:|:-----:|:-----:|
| s | s | s |
| s | ==B== | s |
| s | s | s |


| | $year$ | $city$ |$house\_price$|
|:-----:|:-----:|:-----:|:-----:|
|$r_1$| 2016 | Beijing | 80000 |
|$r_2$| 2014 | Beijing | ==50000== |
|$r_3$| 2016 | Shanghai | 70000 |

| | $c_1$ | $c_2$ |$c_3$|
|:-----:|:-----:|:-----:|:-----:|
|$r_1$ | 0.18 | 0.18 | 0.48 |
|$r_2$ | 0.24 | 0.24 | ==0.56== |
|$r_3$ | 0.06 | 0.06 | 0.16 |


| | $year$ | $city$ |$house\_price$|
|:-----:|:-----:|:-----:|:-----:|
|$r_1$ | 0.0 | 0.0 | 0.0 |
|$r_2$ | 0.0 | 0.0 | ==1.0== |
|$r_3$ | 0.0 | 0.0 | 0.0 |


| | $year$ | $city$ |$house\_price$|
|:-----:|:-----:|:-----:|:-----:|
|$r_1$ | 0.0 | 0.0 | 0.0 |
|$r_2$ | ==1.0== | 0.0 | 0.0 |
|$r_3$ | 0.0 | 0.0 | 0.0 |


| | $year$ | $city$ |$house\_price$|
|:-----:|:-----:|:-----:|:-----:|
|$r_1$ | 0.0 | 0.0 | 0.0 |
|$r_2$ | ==1.0== | ==1.0== | ==1.0== |
|$r_3$ | 0.0 | 0.0 | 0.0 |


| $column$ | $P$     |
| :-----:  | :-----: |
| $c_1$    | 0.3     |
| $c_2$    | 0.3     |
| $c_3$    | ==0.8== |


| $row$ | $P$   |
| :-------: | :-------: |
| $r_1$ | 0.0     |
| $r_2$ | ==1.0== |
| $r_3$ | 0.0     |


| $row$    | $P$      |
| :-----:  | :------: |
| $r_1$    | $P_1$    |
| $r_2$    | $P_2$    |
| $\cdots$ | $\cdots$ |
| $r_n$    | $P_n$    |


| $operation$ | $P$     |
| :---------: | :-----: |
| $o_1$      | $P_1$    |
| $o_2$      | $P_2$    |
| $\cdots$    | $\cdots$ |
| $o_k$       | $P_k$    |



| $operation$ | $P$      |
| ------------- | ---------- |
| $count$     | $0.0$    |
| $\cdots$    | $\cdots$ |
| ==select==    | ==1.0==    |
| $\cdots$    | $\cdots$ |
| $reset$     | $0.0$    |



| $operation$ | $P$      |
| ------------- | ---------- |
| count         | 0.0        |
| $\cdots$    | $\cdots$ |
| ==print==     | ==1.0==    |
| $\cdots$    | $\cdots$ |
| reset         | 0.0        |


| $column$ | $P$      |
| ---------- | ---------- |
| $c_1$    | $P_1$    |
| $c_2$    | $P_2$    |
| $\cdots$ | $\cdots$ |
| $c_m$    | $P_m$    |


| $column$       | $P$   |
| ---------------- | ------- |
| ==year==         | ==1.0== |
| $city$         | 0.0     |
| $house\_price$ | 0.0     |


| $column$       | $P$   |
| ---------------- | ------- |
| $year$        | 0.0     |
| ==city==         | ==1.0== |
| $house\_price$ | 0.0     |


| $column$       | $P$   |
| ---------------- | ------- |
| $year$         | 0.0     |
| $city$         | 0.0     |
| ==house\_price== | ==1.0== |

| $year$ | $city$ |$house\_price$|
|--- |--- | ---|
|2016 | Beijing | 80000 |
|2014 | ==Beijing== | 50000 |
|2016 | Shanghai | 70000 |

 

| | $c_1$ | $c_2$ | $\cdots$ | $c_m$ |
|--- |--- |--- | ---| --- |
|$r_1$ | $P_{11}$` | $P_{12}$ |$\cdots$ | $P_{1m}$ |
|$r_2$ | $P_{21}$` | $P_{22}$ | $\cdots$ | $P_{2m}$ |
|$\cdots$ | $\cdots$` |$\cdots$ | $\cdots$ | $\cdots$ |
|$r_n$ | $P_{n1}$` | $P_{n2}$ | $\cdots$ | $P_{nm}$ |

| c1 | c2 | c3 |
| --- |--- | ---|
| 1.0 | 1.0 | 1.0 |
| 1.0 | ==1.0== | 1.0 |
| 1.0 | 1.0 | 1.0 |


| c1 | ==c2== | c3 |
| --- |--- | ---|
| 1.0 | ==1.0== | 1.0 |
| 1.0 | ==1.0== | 1.0 |
| 1.0 | ==1.0== | 1.0 |


| c1 | ==c2== | c3 |
| --- |--- | ---|
| 0.5 | ==1.0== | 0.5 |
| 0.5 | ==1.0== | 0.5 |
| 0.5 | ==1.0== | 0.5 |





| c1| c2|c3|
| --- |--- | ---|
| 0.05 | 0.1 | 0.05 |
| 0.4 | ==0.8== | 0.4 |
| 0.05 | 0.1 | 0.05 |



|$o_{t-1}$  | 
|--- |
|0.1 |
|0.8 |
|0.1 |
|……  |
|0.2 |


|$o_{t}$  | 
|--- |
|0.2 |
|0.4 |
|0.1 |
|……  |
|0.1 |


## 论文出处
weak supevision， beam search： Learning to compose neural networks for question answering（NAACL 2016）

Neural enquirer: Learning to query tables with natural language (IJCAI 2016)

$$
\times
$$


$$

p_t\left[i\right] = {row\_select_{t-1}\left[ {i + 1} \right]} , i = 1, \cdots, n - 1; p_t\left[n\right] = 0

$$


$$

count_t = \sum_{i = 1}^{n} {row\_select_{t-1}\left[ {i} \right]}

$$


$$

0.6 + 0.8 + 0.2 = 1.6
$$

$$
P_t(Operation = print) = 1
$$
$$
output_t 

$$

