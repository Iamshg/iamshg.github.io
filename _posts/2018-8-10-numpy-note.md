
## numpy 实践笔记  

### dimension 和 shape  

针对 [wiki](https://en.wikipedia.org/wiki/Array_data_structure#Dimension) 上讲述的：  

> The dimension of an array is the number of indices needed to select an element.

就是说：进行几级索引能够索引到该数组的元素，则这个数组的 dimension 就是多少。

~~~python  
In [273]: a = np.random.rand(3,2)

In [274]: a[1,1]
Out[274]: 0.39822106221609188
~~~
上面的例子中 `a` 通过二级索引才可以索引到数组的元素，所以这个数组的 dimension 是 2 .  

对于 shape 就是每一个维度的长度构成的一个元组。
  
~~~python  
In [273]: a = np.random.rand(3,2)

In [274]: a.shape
Out[274]: (3,2)
~~~

dimension 和 shape 的关系可以表示为：  
dimension 就是 shape 的长度， shape 的每一个元素就是该 dimension 的长度。

### numpy 的 Braodcost 机制  

-  Broadcost  rule 1 ： 如果数组的维度不相等，则在短维度的数组中在**左侧**补齐维度，并且补齐的维度为 1 。
-  Broadcost rule 2 ： 如果数组的维度相等，并且两数组的相对应的维度长度如果有一个不相等，并且维度较短的长度为 1 ， 则将 1 补齐为另一数组相对应维度长度。
- Broadcost rule 2 ：如果 rule 1 和 rule 2 都不满足，则跳出异常。

例子：  

~~~python
In [273]: a = np.random.rand(3,2)

In [274]: a[1,1]
Out[274]: 0.39822106221609188

In [275]: a = np.random.rand(3,4)

In [276]: b = np.random.rand(3)

In [277]: # b 先reshape成为 (1,3) ，根据 rule 1;\
     ...: # 此时 b 的 dimension 和 a 一样，都为 2
     ...: # 比较此时的 b 的各个维度的长度和 a 的各
     ...: # 个维度的长度，a 的第一个维度长度是 3 ，
     ...: # b 的第一个维度长度是 1 。1 < 3 , 将 1
     ...: # b 的第一个维度长度 1 拉伸为 3 ，此时 b
     ...: # 的第一个维度为 3 ，同 a 的第一个维度长
     ...: # 度， 对于 a 的第二个维度长度为 4 ，b 的
     ...: # 是 3 ，并且 3 < 4 ,但是 较小的维度长度 3
     ...: # 并不是 1 ，所以不符合 rule2 的情况。 所
     ...: # 以弹出异常。
     ...: a + b
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-277-d94b45f20c8f> in <module>()
     10 # 并不是 1 ，所以不符合 rule2 的情况。 所
     11 # 以弹出异常。
---> 12 a + b

ValueError: operands could not be broadcast together with shapes (3,4) (3,)
~~~

根据上面可以看出，问题出在了`a` 的第二个维度长度大于 `b` 的第二（这里的二代表（1，3）的第二个）个维度长度，并且 `b` 的第二个维度长度不为 1 ，所以才导致的该问题 ， 如果我们使 `a` 的第二个维度长度变为 3 ，或者使得 b 的第二个维度长度变为 1 ，都可以进行运算。  

- `a`的第二个维度长度变为 3 。 `a` 变为 shape （3,3） ，则可以进行运算。  

~~~python
In [278]: a = np.random.rand(3,3)

In [279]: a + b
Out[279]:
array([[ 0.60382884,  1.57606407,  1.44403579],
       [ 0.28414662,  1.34247098,  0.71701922],
       [ 0.08790063,  1.43777143,  0.773714  ]])
~~~

- `b` 的第二个维度变为 1.  

~~~python
In [280]: b = np.random.rand(1)

In [281]: a+b
Out[281]:
array([[ 0.92004311,  1.28013519,  1.37380284],
       [ 0.60036088,  1.0465421 ,  0.64678627],
       [ 0.40411489,  1.14184255,  0.70348104]])
~~~
[参考链接](https://jakevdp.github.io/PythonDataScienceHandbook/02.05-computation-on-arrays-broadcasting.html)    

### matlib 式的创建数组  
在 Matlib 中， 我们可以通过`[1:10,3,2:8]`这样的创建数组，但是在python中的`[1:10]`将会解释成为索引，并不会创建数组。为了方便创建matlib 方式的创建数组，于是numpy中就有了`r_` 和`c_` 实例以及`mgrid` 等等。  
在Stack Overflow上有： 
 
>The motivation for creating `r_` probably comes from Matlab's syntax, which allows to construct arrays in a very compact way, like `x = [1:10, 15, 20:10:100]`. To achieve the same in numpy, you would have to do `x = np.hstack((np.arange(1,11), 15, np.arange(20,110,10)))` 。

其具体的是实现是通过 python 内置的 `__geitem__` 实现的。[详情参考](https://stackoverflow.com/a/18623190)

在 matlib 中，定义 行向量，数组形式为:  

~~~matlib
x = [1:10,9]
~~~~

同样在 numpy 中，定义一维度数组为：

~~~python
In [283]: x = np.r_[1:10,9]

In [284]: x
Out[284]: array([1, 2, 3, 4, 5, 6, 7, 8, 9, 9])
~~~

定义二维数组方便方法为：

~~~python

In [293]: c = np.c_[np.c_[1:10],np.c_[10:19]]

In [294]: c
Out[294]:
array([[ 1, 10],
       [ 2, 11],
       [ 3, 12],
       [ 4, 13],
       [ 5, 14],
       [ 6, 15],
       [ 7, 16],
       [ 8, 17],
       [ 9, 18]])

In [295]: c = np.c_[np.arange(10),np.arange(10,20)]

In [296]: c
Out[296]:
array([[ 0, 10],
       [ 1, 11],
       [ 2, 12],
       [ 3, 13],
       [ 4, 14],
       [ 5, 15],
       [ 6, 16],
       [ 7, 17],
       [ 8, 18],
       [ 9, 19]])
~~~

其中 `np.c_` 和 `np.column_stack()` 有类似用处。
同样类似的实例还有 `np.mgrid` ，`np.meshgrid` , `np.ogrid` 等等。

### np.newaxis in slice 

索引的作用只是将元素的值取出，要要求时不超过索引长度，但是一个索引可以用好多遍，得到一个更加长的数组。  
例如： 

~~~python
In [303]: X = iris.data

In [304]: y = iris.target

In [305]: y_class = iris.target_names

In [306]: y
Out[306]:
array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
       0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
       0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
       2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
       2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2])

In [307]: y_class
Out[307]:
array(['setosa', 'versicolor', 'virginica'],
      dtype='<U10')

In [309]: y_names = y_class[y]

In [310]: y_names
Out[310]:
array(['setosa', 'setosa', 'setosa', 'setosa', 'setosa', 'setosa',
       'setosa', 'setosa', 'setosa', 'setosa', 'setosa', 'setosa',
       'setosa', 'setosa', 'setosa', 'setosa', 'setosa', 'setosa',
       'setosa', 'setosa', 'setosa', 'setosa', 'setosa', 'setosa',
       'setosa', 'setosa', 'setosa', 'setosa', 'setosa', 'setosa',
       'setosa', 'setosa', 'setosa', 'setosa', 'setosa', 'setosa',
       'setosa', 'setosa', 'setosa', 'setosa', 'setosa', 'setosa',
       'setosa', 'setosa', 'setosa', 'setosa', 'setosa', 'setosa',
       'setosa', 'setosa', 'versicolor', 'versicolor', 'versicolor',
       'versicolor', 'versicolor', 'versicolor', 'versicolor',
       'versicolor', 'versicolor', 'versicolor', 'versicolor',
       'versicolor', 'versicolor', 'versicolor', 'versicolor',
       'versicolor', 'versicolor', 'versicolor', 'versicolor',
       'versicolor', 'versicolor', 'versicolor', 'versicolor',
       'versicolor', 'versicolor', 'versicolor', 'versicolor',
       'versicolor', 'versicolor', 'versicolor', 'versicolor',
       'versicolor', 'versicolor', 'versicolor', 'versicolor',
       'versicolor', 'versicolor', 'versicolor', 'versicolor',
       'versicolor', 'versicolor', 'versicolor', 'versicolor',
       'versicolor', 'versicolor', 'versicolor', 'versicolor',
       'versicolor', 'versicolor', 'versicolor', 'virginica', 'virginica',
       'virginica', 'virginica', 'virginica', 'virginica', 'virginica',
       'virginica', 'virginica', 'virginica', 'virginica', 'virginica',
       'virginica', 'virginica', 'virginica', 'virginica', 'virginica',
       'virginica', 'virginica', 'virginica', 'virginica', 'virginica',
       'virginica', 'virginica', 'virginica', 'virginica', 'virginica',
       'virginica', 'virginica', 'virginica', 'virginica', 'virginica',
       'virginica', 'virginica', 'virginica', 'virginica', 'virginica',
       'virginica', 'virginica', 'virginica', 'virginica', 'virginica',
       'virginica', 'virginica', 'virginica', 'virginica', 'virginica',
       'virginica', 'virginica', 'virginica'],
      dtype='<U10')
~~~

np.newaxis 的用法： 
官方网站讲到:  

> Each newaxis object in the selection tuple serves to expand the dimensions of the resulting selection by one unit-length dimension. The added dimension is the position of the newaxis object in the selection tuple.

说明在使用 `np.newaxis` 只是表示在相应的位置上**临时**增加了一个长度为一的维度，结果也相应在也增加一个长度的相应维度。

~~~python

In [315]: iris.data.shape
Out[315]: (150, 4)

In [316]: iris.data[:,2].shape
Out[316]: (150,)

In [317]: iris.data[:,np.newaxis,2].shape
Out[317]: (150, 1)
~~~

在317 的输出上多出来了一个维度，并且长度为1 。

### dtype 

在上面注意到：

~~~python
In [307]: y_class
Out[307]:
array(['setosa', 'versicolor', 'virginica'],
      dtype='<U10')
~~~
其中的数组的 `dtype` 是 `<U10` ，看来可以不仅仅是数字。numpy 数组的元素可以哟很多种。 
详情参照官方[网站](https://docs.scipy.org/doc/numpy-1.13.0/reference/arrays.dtypes.html#arrays-dtypes-constructing).
其中介绍了可以有更多的复杂的形式。包括，number，string，structed data，sub-array 等等。
