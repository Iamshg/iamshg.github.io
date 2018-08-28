## matlib's lsqnonlin 和 scipy.optimize's least_square

### 问题

有三个点 $A,B,C$ , 经过一个线性变换 $T$ , 变为了 $A^',B^',C^'$ 三点，现在知道 $A,B,C$ , $A^',B^',C^'$ 六个点坐标， 求出相应的变换 $T$ ?

###  matlab

matlab 使用的都是矩阵运算，所以一切在都要化成矩阵形式。直接将问题写成矩阵形式有一些不直观。所以首先将问题写成一些线性方程组的形式，根据线性方程组的形式再写出矩阵的形式。
假设 

$$
A = 
    \begin{bmatrix}
    x_{a1} & y_{a1}\\
     x_{a2} & y_{a2} \\
    x_{a3} & y_{a3} \\
    \end{bmatrix},
R =     \begin{bmatrix}
    x_{1} & y_{1}\\
     x_{2} & y_{2} \\
    x_{3} & y_{3} \\
    \end{bmatrix}\\

$$

- 线性方程组的形式 为：


$$
\left\{ 
\begin{array}{c}
ax_1+by_1+c=x_{a1} \\ 
dx_1+ey_1+f=y_{a1} \\
ax_2+by_2+c=x_{a2} \\ 
dx_2+ey_2+f=y_{a2} \\
ax_3+by_3+c=x_{a3} \\ 
dx_3+ey_3+f=y_{a3} 
\end{array}
\right. 
$$

其中前两个一组两个一组，第一个和第二个方程组组成的矩阵形式为：

$$
\left\{ 
\begin{array}{c}
ax_1+by_1+c=x_{a1} \\ 
dx_1+ey_1+f=y_{a1} \\
\end{array}
\right.    ==> \\
\begin{bmatrix}
    a & b & c\\
    e & d & f \\
    \end{bmatrix}
*    
\begin{bmatrix}
    x_{a1} \\
    y_{a1} \\
    1
    \end{bmatrix} = 
\begin{bmatrix}
    x_{1} \\
    y_{1} \\
    \end{bmatrix}
$$

将六个方程组合并成为：

$$
T =     \begin{bmatrix}
    a & b  \\
    c & d   \\
     e  & f 
    \end{bmatrix}\\

\left\{ 
\begin{array}{c}
ax_1+by_1+c=x_{a1} \\ 
dx_1+ey_1+f=y_{a1} \\
ax_2+by_2+c=x_{a2} \\ 
dx_2+ey_2+f=y_{a2} \\
ax_3+by_3+c=x_{a3} \\ 
dx_3+ey_3+f=y_{a3} 
\end{array}
\right.  ==>
\begin{bmatrix}
    a & b & c\\
    e & d & f \\
    \end{bmatrix}
*    
\begin{bmatrix}
    x_{a1} & x_{a2} & x{a3} \\
    y_{a1} & y_{a2} & y_{a3} \\
    1 & 1 & 1
    \end{bmatrix} = 
\begin{bmatrix}
    x_{1} & x_{2} & x_{3} \\
    y_{1} & y_{2} & y_{3}
    \end{bmatrix}  \\

==>  T^T * R = A

$$

矩阵的表示形式之后，使用 MATLAB 的程序 lsqnonlin 优化程序得到 $T$ .

lsqnonlin 的输入是要优化的函数句柄和初始参数向量 $X$ ，输出是一个结果向量。**注意**这里输入和输出都是向量。当要优化的变量是矩阵形式的时候，例如本例子中的 $T$ , 需要人为的将其变为向量作为参数向量传入要被 lsqnonlin 优化的函数，同样，当结果也是矩阵形式时候，也要做类似转换。

针对上面的公式 和 分析 ，得出的 MATLAB 代码如下：

~~~matlab
function F = foo(T)
    T = reshape(T,3,2);
    R = [-2^0.5/2,0,2^0.5 ;
          -2^0.5/2+1,0+1,0+1;
          1,1,1];
    A = [1+3,0+3,-1+3;
        0+1,0+1,1+1];
    F = reshape(T'*R-A,1,[]); %向量化

% 命令行调用函数
lsqnonlin(@foo,ones(3,2))

ans =

   -0.7071    0.7071
   -0.7071   -0.7071
    3.7071    1.7071
~~~

由上面返回的 `ans` 结果可以得到 $T$ 。

### python's scipy.optimize

scipy 中的数值计算不同于 MATLAB ，是基于元素的 ， 所以我们没必要将线性方程组化为矩阵形式 。
坐标的线性变换为 ， 

$$
\left\{ 
\begin{array}{c}
ax+by+c=x^' \\ 
dx+ey+f=y^' 
\end{array}
\right.
$$

其中 $x,y$ 可以分别是列向量， 
所以假设

$$ 
T = \begin{bmatrix}
    a & b & c & e & d & f 
    \end{bmatrix} \\

A = 
\begin{bmatrix}
    x_{a1} & x_{a2} & x{a3} \\
    y_{a1} & y_{a2} & y_{a3}
\end{bmatrix}

R = 
\begin{bmatrix}
    x_{1} & x_{2} & x_{3} \\
    y_{1} & y_{2} & y_{3}
\end{bmatrix}  

$$

则可以写成

$$ 

\left\{ 
\begin{array}{c}
ax+by+c=x^' \\ 
dx+ey+f=y^' 
\end{array}
\right. ==>
[ax,ey] + [by,dx] + [c,f] = [x^',y^'] ==> \\
[a,e].*[x,y] + [b,d].*[y,x] + [c,f] = [x^',y^'] \\
T[:2].*R^T + T[2:4].*R^T[:,[1,0]] + T[4:].*R^T 

$$

为了方便写代码，这里将$T$ 的各个元素的顺序改变了一下。其中 `x[:,[1,0]]` 是将两列换一下。

所以对应的代码为：

~~~python
In [9]: def foo1(T):
   ...:
   ...:     R = np.array([ [-2**0.5/2,0,2**0.5 ],
   ...:           [-2**0.5/2+1,0+1,0+1]]).T
   ...:     A = np.array([ [1+3,0+3,-1+3],
   ...:         [0+1,0+1,1+1]]).T
   ...:
   ...:     return np.ndarray.flatten(T[:2]*R+T[2:4]*A[:,[1,0]]+T[4:])
   ...: from scipy.optimize import least_squares as l_s
   ...: T0 = np.ones(shape=(6,))
   ...: print(l_s(foo1,T0))
   ...:
 active_mask: array([ 0.,  0.,  0.,  0.,  0.,  0.])
        cost: 6.1972286281607316e-46
         fun: array([ -2.62791580e-23,  -1.37051771e-24,   4.13590306e-24,
         0.00000000e+00,   2.27798139e-23,  -3.30872245e-24])
        grad: array([  5.07976927e-23,  -3.71013779e-24,   2.34163730e-23,
        -1.20995158e-23,   6.36559009e-25,  -4.67924016e-24])
         jac: array([[-0.70710678,  0.        ,  1.        ,  0.        ,  1.        ,
        -0.        ],
       [ 0.        ,  0.29289322, -0.        ,  4.        ,  0.        ,
         1.        ],
       [ 0.        ,  0.        ,  1.        ,  0.        ,  1.        ,
        -0.        ],
       [ 0.        ,  1.        , -0.        ,  3.        ,  0.        ,
         1.        ],
       [ 1.41421356,  0.        ,  2.        ,  0.        ,  1.        ,
        -0.        ],
       [ 0.        ,  1.        , -0.        ,  2.        ,  0.        ,
         1.        ]])
     message: '`gtol` termination condition is satisfied.'
        nfev: 3
        njev: 3
  optimality: 5.0797692652871677e-23
      status: 1
     success: True
           x: array([  4.30133919e-23,   6.61744490e-24,  -4.21862112e-23,
         3.30872245e-24,   4.63221143e-23,  -1.65436123e-23])
~~~

## lsqnonlin 和 least_square 的函数封装方法

对于这两个函数的传入的要优化的函数 ， 该函数的输出是一个向量 ，假设该向量为：

$$

y = [y_1,y_2,y_3,y_4,\cdots,y_n]

$$

则通过lsqnonlin 和least_square 之后的包裹为：

$$

c*\sum_i{y_i^2}

$$

其中 $c$ 是一个常量。
