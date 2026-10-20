# 1、概述

## 1.1 介绍

- numpy 是 Python 中<font color="red">**科学计算的基础包**</font>。 它是一个 Python 库，提供<font color="red">**多维数组对象、各种派生对象（例如掩码数组和矩阵）以及用于对数组进行快速操作的各种方法**</font>，包括数学、逻辑、形状操作、排序、选择、I/O、离散傅里叶变换、基本线性代数、基本统计运算、随机模拟等等。

- numpy 的部分功能如下：

  - ndarray，一个具有矢量算术运算和复杂广播能力的快速且节省空间的多维数组。

  - 用于对整组数据进行快速运算的标准数学函数（无需编写循环）。

  - 用于读写磁盘数据的工具以及用于操作内存映射文件的工具。

  - 线性代数、随机数生成以及傅里叶变换功能。

  - 用于集成由 C、C++、Fortran 等语言编写的代码的 API。



## 1.2 安装

- 安装依赖：pip install numpy
- 添加清华链接：pip install numpy -i https://pypi.tuna.tsinghua.edu.cn/simple



# 二、ndarray

## 1、介绍

- NumPy 最重要的一个特点是其<font color="red">**N 维数组对象 ndarray**</font>，它是一系列同类型数据的集合，以 0 下标为开始进行集合中元素的索引。

- ndarray 对象是用于<font color="red">**存放同类型元素**</font>的多维数组。

- ndarray 中的每个元素在内存中都<font color="red">**有相同存储大小**</font>的区域。

- 跨度可以是负数，这样会使数组在内存中后向移动，切片中 **obj[::-1]** 或 **obj[:,::-1]** 就是如此。

- 创建一个 ndarray 只需调用 NumPy 的 array 函数即可：

~~~python
numpy.array(object, dtype = None, copy = True, order = None, subok = False, ndmin = 0)
~~~

- 参数说明

| 名称   | 描述                                                      |
| :----- | :-------------------------------------------------------- |
| object | 数组或嵌套的数列                                          |
| dtype  | 数组元素的数据类型，可选                                  |
| copy   | 对象是否需要复制，可选                                    |
| order  | 创建数组的样式，C为行方向，F为列方向，A为任意方向（默认） |
| subok  | 默认返回一个与基类类型一致的数组                          |
| ndmin  | 指定生成数组的最小维度                                    |



## 2、核心特性

- **多维性**：支持 0 维（标量）、1 维（向量）、2 维（矩阵）及更高维数组。
- **同质性**：所有元素类型必须一致（通过 dtype 指定）。
- **高效性**：基于连续内存块存储，支持向量化运算。



### 2.1 多维性

- 实例1：0维数组

~~~python
import numpy as np

a = np.array(5)
print(a)
print(f"数组维度：{a.ndim}")


"""
5
数组维度：0
"""
~~~

- 实例2：一维数组

~~~python
import numpy as np

a = np.array([1,2,3])
print(a)
print(f"数组维度：{a.ndim}")

"""
[1 2 3]
数组维度：1
"""
~~~

- 实例3：多维数组

~~~python
import numpy as np

a = np.array([[1, 2, 3],  [4, 5, 6], [7, 8, 9]], ndmin=2)
print(a)
print(f"数组维度：{a.ndim}")


"""
[[1 2 3]
 [4 5 6]
 [7 8 9]]
数组维度：2
"""
~~~



### 2.2 同质性

-  所有元素类型必须一致（通过 dtype 指定）。
- <font color="red">**不同的数据类型会被强制转换成相同的数据类型**</font>
- 实例1

~~~python
import numpy as np

a = np.array([1, 'hello'])
print(a)

"""
['1' 'hello']
"""
~~~

- 实例2

~~~python
import numpy as np

a = np.array([1, 2.5])
print(a)


"""
[1.  2.5]
"""
~~~



### 2.3 高效性

- ndarray 内部由以下内容组成：

  - 一个指向数据（内存或内存映射文件中的一块数据）的指针。

  - 数据类型或 dtype，描述在数组中的固定大小值的格子。

  - 一个表示数组形状（shape）的元组，表示各维度大小的元组。

  - 一个跨度元组（stride），其中的整数指的是为了前进到当前维度下一个元素需要"跨过"的字节数

![ndarray内存结构](图片/ndarray内存结构.png)



## 3、属性

| 属性名称     | 通俗解释                                     | 补充说明                                                | 使用示例     |
| ------------ | -------------------------------------------- | ------------------------------------------------------- | ------------ |
| **shape**    | 数组的形状：行数和列数（或更高维度的尺寸）   | 表示数组在每个轴上的大小；二维数组（矩阵）即行数和列数  | arr.shape    |
| **ndim**     | 维度数量：数组是几维的（1 维、2 维、3 维等） | 数组的秩（rank），即维度数量或轴的数量                  | arr.ndim     |
| **size**     | 总元素个数：数组中所有元素的总数             | 等于 shape 中各轴大小的乘积                             | arr.size     |
| **dtype**    | 元素类型：数组中元素的类型（整数、浮点数等） | 数组中元素的数据类型                                    | arr.dtype    |
| **T**        | 转置：行变列，列变行                         | —                                                       | arr.T        |
| **itemsize** | 单个元素占用的内存字节数                     | 以字节为单位                                            | arr.itemsize |
| **nbytes**   | 数组总内存占用量                             | size * itemsize                                         | arr.nbytes   |
| **flags**    | 内存存储方式：是否连续存储（高级优化）       | 内存布局信息，如是否为 C / Fortran 连续存储、是否只读等 | arr.flags    |
| **real**     | 数组中每个元素的实部                         | 元素类型为复数时生效                                    | arr.real     |
| **imag**     | 数组中每个元素的虚部                         | 元素类型为复数时生效                                    | arr.imag     |
| **data**     | 实际存储数组元素的缓冲区                     | 一般通过索引访问元素，不直接使用该属性                  | arr.data     |

- 例子

~~~python
import numpy as np

a = np.array([[1, 2, 3], [4, 5,6]])
print(a)
print(f"a的形状：{a.shape}")
print(f"a的维度：{a.ndim}")
print(f"a的元素个数：{a.size}")
print(f"a的元素类型：{a.dtype}")
print(f"a的单个元素占用的内存字节数：{a.nbytes}")
print(f"a的单个元素占用的内存存储方式：{a.flags}")
print(f"a的转置：{a.T}")


"""
[[1 2 3]
 [4 5 6]]
a的形状：(2, 3)
a的维度：2
a的元素个数：6
a的元素类型：int64
a的单个元素占用的内存字节数：48
a的单个元素占用的内存存储方式：  C_CONTIGUOUS : True
  F_CONTIGUOUS : False
  OWNDATA : True
  WRITEABLE : True
  ALIGNED : True
  WRITEBACKIFCOPY : False

a的转置：[[1 4]
 [2 5]
 [3 6]]
"""
~~~



## 4、创建方式

- ndarray创建：

  - 基础构造：适用于手动构建小规模数组或复制已有数据。对应方法：array()、copy()

  - 预定义形状填充：用于快速初始化固定形状的数组（如全0占位、全1初始化）。对应方法：zeros()、ones()、empty()、full()

  - 基于数值范围生成：生成数值序列，常用于模拟时间序列、坐标网格。对应方法：arange()、linspace()、logspace()

  - 特殊矩阵生成：数学运算专用（如线性代数中的单位矩阵）。对应方法：eye()、diag()

  - 随机数组生成：模拟试验数据、初始化神经网络权重等场景。对应方法：random.rand()、random.randn()、random.randint()

  - 高级构造方法：处理非结构化数据（如文件、字符串）或通过函数生成复杂数组。对应方法：array()、loadtxt()、fromfunction()。



### 4.1 基础构建



```python
import numpy as np

# 基础创建
arr = np.array([1,2,3], dtype=np.float64) # 使用dtype参数可以指定强制转换成某个类型
print(arr)

# 复制
arr1 = np.copy(arr) # 从arr拷贝出一个数组arr1,不是同一个数组了（深拷贝，拷贝后修改arr不会影响arr1的值）
print(arr1)

"""
[1. 2. 3.]
[1. 2. 3.]
"""
```



### 4.2 预定义形状填充

~~~python
import numpy as np

# 预定义形状

# 创建一个2行3列的全0数组
arr = np.zeros((2,3), dtype=int) # 参数为一个元组，指定数组的形状。默认的所有元素都是浮点型，可以使用 dtype指定元素类型
print(arr)

# 创建一个3行5列全1的数组
arr = np.ones((3,5), dtype=int)
print(arr)

# 创建一个未初始化的2行3列数组（不推荐，这个空数组的值是内存中残留的值）
arr = np.empty((2,3)) # 创建一个未初始化的数组
print(arr) # 也能打印出内容，打印的是内存中残留的数值

# 创建一个3行4列数组，数组每个元素都是2035
arr = np.full((3,4), 2025)
print(arr)

# 创建一个全0的数组，形状和arr一样
# empty_like、ones_like、full_like 功能类似
arr1 = np.zeros_like(arr)
print(arr1)

"""
[[0 0 0]
 [0 0 0]]
 
[[1 1 1 1 1]
 [1 1 1 1 1]
 [1 1 1 1 1]]
 
[[0. 0. 0.]
 [0. 0. 0.]]
 
[[2025 2025 2025 2025]
 [2025 2025 2025 2025]
 [2025 2025 2025 2025]]
 
[[0 0 0 0]
 [0 0 0 0]
 [0 0 0 0]]
"""
~~~



### 4.3 基于数据范围生成

```python
import numpy as np

# 等差数列
arr = np.arange(1, 11, 2) # 指定起始值（包含）、结束值（不包含）、步长
print(arr)

# 等间隔数列
arr = np.linspace(1, 10, 3) # 指定起始值、结束值、从其中等间隔取几份
print(arr)

# 对数间隔数列
arr = np.logspace(0, 10, 3, base=2) # 指定起始值、结束值、取几个数、对数的底数
print(arr) # 实际上等同于：先做 np.linespace(0, 10, 3)，然后对结果中的每一项作为指数求2的对应幂


"""
[1 3 5 7 9]
[ 1.   5.5 10. ]
[1.000e+00 3.200e+01 1.024e+03]
"""
```
