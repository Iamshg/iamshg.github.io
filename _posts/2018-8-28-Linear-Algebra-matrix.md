
### Centering matrix

根据 wiki 上的定义 : 

> the centering matrix is a symmetric and idempotent matrix, which when multiplied with a vector has the same effect as subtracting the mean of the components of the vector from every component.

也就是是说 中心矩阵 是对称并且 幂等的 ，他与一个向量相乘等于将其中心化，也就是相当于每个元素减去这个向量元素的均值。

对于中心矩阵的形式为：

$$
C_n = I_n - \frac{1}{n}\mathbb{O}
$$

其中 $I_n$ 是 identity 矩阵，对角线全为 1 方阵。 其中 $ \mathbb{O} $ 是全为 1 的方阵。

应用：


假设有一个 m by n 的矩阵 M，需要将其列中心化：
则为 $ C_m \times M_{m \times n} $,如果要将其行中心化，则为$ M_{{m} \times {n}} \times  C_n$


### Idempotent matrix

幂等矩阵，定义为矩阵自身相乘认为自己。

$$
M \times M = M
$$


