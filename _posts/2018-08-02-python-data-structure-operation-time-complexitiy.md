---
layout: post
title: "python 内置数据结构的操作的时间复杂度"
date: 2018-08-02
categories: 实践
---

### python 的内置数据结构
其中 python 内置的数据结构有链表(list),字典(dict),集合(set),元组(tuple),队列(dequen)等等.其中对 list 和 dict 的使用最为频繁,其下就详细记录一下,自己对 list 操作的时间复杂度的理解.

### python 数据结构操作的[时间复杂度][python time complexity]
#### list 的操作的时间复杂度  
- 索引操作:a[i],$O(1)$
- 赋值操作:a[i]=b,$O(1)$
- 获取长度操作:len(a),$O(1)$
- 切片索引操作:a[i:i+k],$O(k)$,因为要进行单索引操作k次,所以复杂度要是$O(k)$.
- 弹出pop(index)操作:pop(i),$O(len(a)-i)$先对索引为i的元素进行弹出,在将索引为i元素的右边元素依次进行向前移动索引.
  - 如果是弹出的最后一个索引,那么是时间的复杂度就是$O(1)$.
  - 如果对中间的元素进行弹出,那么就是$O(len(a)-i)$.
  - 平均情况:假想,如果有 n 个的长度为 n 的链表,假如每个链表依次从后向前弹出一项元素,第一个链表用$O(1)$时间复杂度,第二个链表用$O(2)$时间复杂度,等等,一直是到第一个元素用$O(n)$时间复杂度,则一共$n$次弹出用了$O(1+2+\cdots+n)=O(\frac{n^2+n}{2})$复杂度,则每一次的复杂度为$O(\frac{n+1}{2})=O{n}$.
  - 最坏的情况:假设这 n 个链表弹出的都是第一个元素,则每一个链表都要用上$O(n)$的时间复杂度,则表示的最终的时间复杂度为$O(n)$.
- 删除(del)操作: 因为 del 操作的传入的参数也是索引,所以也和pop 弹出的情况一样.
- 插入(insert)操作: 因为插入操作传入的也是索引,所以和删除操作的时间复杂度一致.
- 移除(remove)操作: remove(elements),因为 remove 的传入参数为元素的值,所以对与弹出和删除操作多了一项查找元素 elements 的操作,该项操作在加上后续的后面索引向前移位的操作一共正好是$O(n)$.所以对于传入参数是元素值的操作无论是平均还是最好都是 O(n) 时间复杂度 .
- 判断是否存在操作: x in a , 这个操作相当于传入参数是元素值x,所以时间复杂度单项操作来说是$O(k)$ .假设 x 在 a 中的索引为k . 平均来讲是$O(\frac{n}{2})$,最坏的平均来讲是$O(n)$.
- 获取最大(max)最小(min)值:max(a),min(a).$O(n)$
- 深度拷贝(copy):b = a[:],$O(n)$,相当于执行了 n 次赋值操作.
- 拼接(+)和乘法(\*)操作:k\*a,$O(k \times n)$.
- 排序(sort)操作:sorted(a),$O(nlogn)$  

#### dict 的操作的复杂度:
dict 除了Iteration 和 copy 是 $O(n)$ 时间复杂度.其余都是$O(1)$,包括设置item值,得到item值,删除键值对.
### 附加资料:
python [官方][python time complexity]的内置结构操作的复杂度,和,MIT 的[python cost model] [6006 introduce to algorithms python cost Model] 的资料.

[python time complexity]: https://wiki.python.org/moin/TimeComplexity "time compexity"
[6006 introduce to algorithms python cost Model]: https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/readings/python-cost-model/ "6006"


