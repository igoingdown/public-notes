# 2017.5.4

* precision和recall算的有问题吧？？？？

* train过程函数调用关系

    * train()多线程调用trainMode
    * trainMode调用train_bags(x)
    * train_bags(x)先调用train(xxx)，再调用train\_gredient(xxx)

* 论文中的公式 $\mathbf{s} = \sum_{i}a_i\mathbf{X}_i$ 和代码对应不上！！

* 不应该是预测结果吗？？？但是好像没用的测试结果呢？？？


---

# 2017.5.9

* tot不变，记录的是test数据集中关系非NA的数据的条数。

* $sum[j]$和论文中的$P_r$对应。`weight`对应论文中的$\alpha$,`r`对应论文中的$\mathbf s$, `matrixRelationsPr`对应论文中的$\mathbf d$

* b_sum存的是sentence的数量，就是数据的条数。是个递增数列，$bagSentenceNum_i = b\_sum_i - b\_sum_{i - 1}$


