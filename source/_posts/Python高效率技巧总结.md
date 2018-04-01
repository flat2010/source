---
title: Python算法教程笔记
date: 2016-12-11 21:46:05
tags: [Python, 算法]  
categories: [编程笔记, Python]  
comments: true  
---


><font color=#0000FF face="微软雅黑" size=4>一切只为效率而生.</font>



***


#### 一、ChapterI 引言 
　　无


#### 二、ChapterII 基础 
##### 2.1 字符串操作效率
　　字符串类型的数据在编程中接触的比较多，

 
##### 2.2 Set和List的那些事
　　python中的两种常用对象Set和List应该说在使用上大家都能得心应手，当然也知道这两者的差异：
　　①Set是无序无重复元素的集合;
　　②List则是有序可包含重复元素的集合。
　　在处理的数据量不大的时候，当然是哪个顺手用哪个。然而两者的不同实现决定了二者在性能上的差异，看如下代码：
<!-- more -->

```python
from timeit import timeit
from random import randrange

#分别构造大小和元素完全相同的list和set(约1000个元素)
test_set = set([randrange(10000) for i in range(1000)])
test_list = list(test_set)

#分别在之前构造的list和set中查询'56'是否存在
def list_search():
    if '56' in test_list:
        return True
    else:
        return False
		
def set_search():
    if '56' in test_set:
        return True
    else:
        return False

#输出二者的查询时间		
print(timeit("set_search()", setup='from __main__ import set_search'))
print(timeit("list_search()", setup='from __main__ import list_search'))
``` 
　　输出结果:
　　　　Set>> 0.11968003701895213
　　　　List>> <font color=#FF0000>17.639899451222846</font>　
　　整个运行效率差了约140倍(该值随硬件及软件配置不同会有所不同），增大元素个数，这个差异会更明显。上例还只是单次成员查询，多次成员查询的话，悬殊更大。究其原因，List的本质是“数组”，一块连续的内存区域，而Set的的实现是散列。因此在涉及大量数据操作的时候，如果无顺序要求，都应尽量使用Set而非List。
