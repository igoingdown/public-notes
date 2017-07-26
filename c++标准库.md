# 头文件：

~~~c++
# include<algorithm>
~~~


## 排列函数

```c++
next_permutation(beg, end)
next_permutation(beg, end, comp)
```

输入两个迭代器，可以指定比较函数。
序列的所有排列可以规定次序。如123在132前，acb在bac前。调用这一函数时，函数调整输入序列的顺序，生成下一个排列。如果序列已经是最后一个排列中，则`next_permutation`将序列重新排列为最低排列并返回`false`；否则，它将输入序列变换为下一个排列，即字典序的下一个排列，并返回`true`。

---


## C和C++风格的示例代码：

```c++
int a[] = {1, 2, 3};
while (1)
{
    cout << a[0] << a[1] << a[2] << endl;
    if (!next_permutation(a, a + 3)){
        break;
    }
}

vector<string> vecStr{"abc", "xyz", "aeiou"};
while (1)
{
    cout << vecStr[0] << vecStr[1] << vecStr[2] << endl;
    if (!next_permutation(vecStr.begin(), vecStr.end())) {
        break;
    }

}
```
C风格的代码可以显著快于C++风格的代码。


## 排序函数
只能用于顺序容器如vector，deque，list，不能用于键值对容器，这和Python中的sorted函数的限制是一致的。

```c++
sort(beg, end)
//实际是快排,平均时间复杂度为N log(N)。

sort(beg, end, comp)
// comp是函数或者函数指针，签名是 
//   bool comp(elem_type first_arg, elem_type second_arg)，
// 返回的bool值的含义是第一个参数是否应该排在第二个参数的前面。

stable_sort (beg,  end)
//实际是归并排序,在空间足够的情况下时间复杂度为N log(N)。
stable_sort(beg, end, comp)
```

## min、max
```c++
min_element(beg, end)
min_element (beg, end, comp)
max_element (beg, end)
max_element (beg, end, comp)
// 可输入数组指针，也可输入迭代器。
// 输入指针就返回指针，输入迭代器就返回迭代器。
// 另有类似的min和max函数，只能输入迭代器，不能输入指针。
```

## 二分搜索函数

```c++
lower_bound(beg, end, val)
lower_bound(beg, end, val, comp)
// lower_bound是得到与val相同的第一个元素的迭代器

upper_bound(beg, end, val)
upper_bound(beg, end, val, comp)
// upper_bound是得到与val相同的最后一个元素的下一个元素的迭代器
// 输入的数据必须是有序的。


// 若搜索不到，则返回最后一个元素的下一个元素的迭代器，即end()迭代器。
```
```c++
vector<int> vec = { 1, 2, 3, 4, 4, 5, 7, 8 };
auto it = upper_bound(vec.begin(), vec.end(), 4);
printf("%d", it - vec.begin());
// 输出为5。
```

---


## 杂项

只有进行字符串匹配时考虑用find进行搜索，其他情况都用lower_bound和upper_bound进行搜索。

有序序列集合运算
参见《C++ Primer》
一般般，不是特别好用，得到的结果可能和预期的不一样。
