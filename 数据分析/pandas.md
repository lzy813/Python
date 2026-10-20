# 一、介绍

## 1、简介

- Pandas 是一个开源的<font color="red">**数据分析和数据处理库**</font>，它是基于 Python 编程语言的

- Pandas 提供了易于使用的数据结构和数据分析工具，<font color="red">**特别适用于处理结构化数据**</font>，如表格型数据（类似于Excel表格）
- Pandas 是 Python 数据分析工具链中最核心的库，充当数据读取、清洗、分析、统计、输出的高效工具

- Pandas 是<font color="red">**数据科学和分析领域**</font>中常用的工具之一，它使得用户能够轻松地从各种数据源中导入数据，并对数据进行高效的操作和分析。

- Pandas 主要引入了两种新的数据结构：<font color="red">**Series**</font> 和 <font color="red">**DataFrame**</font>



### 1.1 Series

#### 1.1.1 是什么

- Series 是 Pandas 中的一个核心数据结构，类似于一个一维的数组，<font color="red">**具有数据和索引**</font>。

- Series 可以存储任何数据类型（整数、浮点数、字符串等），并通过标签（索引）来访问元素。

- Series 的数据结构是非常有用的，因为它可以处理各种数据类型，同时保持了高效的数据操作能力，比如可以通过标签来快速访问和操作数据。



#### 1.1.2 Series 特点

- <font color="red">**Series 是 Pandas 中的一种基本数据结构，类似于一维数组或列表，但具有标签（索引）****</font>，使得数据在处理和分析时更具灵活性

  - <font color="red">**一维数组**</font>：Series 中的每个元素都有一个对应的索引值。
  - <font color="red">**索引**</font>： 每个数据元素都可以通过标签（索引）来访问，默认情况下索引是从 0 开始的整数，但是也可以自定义索引。

  - <font color="red">**数据类型**</font>： Series 可以容纳不同数据类型的元素，包括整数、浮点数、字符串、Python 对象等。

  - <font color="red">**大小不变性**</font>：Series 的大小在创建后是不变的，但可以通过某些操作（如 append 或 delete）来改变。

  - <font color="red">**操作**</font>：Series 支持各种操作，如数学运算、统计分析、字符串处理等。

  - <font color="red">**缺失数据**</font>：Series 可以包含缺失数据，Pandas 使用NaN（Not a Number）来表示缺失或无值。

  - <font color="red">**自动对齐**</font>：当对多个 Series 进行运算时，Pandas 会自动根据索引对齐数据，这使得数据处理更加高效。

![Series](图片/pandas/Series.png)

- 可以使用 Pandas 库来创建一个 Series 对象，并且可以为其指定索引（Index）、名称（Name）以及值（Values）：

~~~python
import pandas as pd

# 创建一个Series对象，指定名称为'A'，值分别为1, 2, 3, 4
# 默认索引为0, 1, 2, 3
series = pd.Series([1, 2, 3, 4], name='A')

# 显示Series对象
print(series)

# 如果你想要显式地设置索引，可以这样做：
custom_index = [1, 2, 3, 4]  # 自定义索引
series_with_index = pd.Series([1, 2, 3, 4], index=custom_index, name='A')

# 显示带有自定义索引的Series对象
print(series_with_index)


"""
0    1
1    2
2    3
3    4
Name: A, dtype: int64
1    1
2    2
3    3
4    4
Name: A, dtype: int64
"""
~~~



#### 1.1.3 创建 Series

- 可以使用 pd.Series() 构造函数创建一个 Series 对象，传递一个数据数组（可以是列表、NumPy 数组等）和一个可选的索引数组

~~~python
pandas.Series(data=None, index=None, dtype=None, name=None, copy=False, fastpath=False)
~~~

- 参数说明：

  - data：Series 的数据部分，可以是列表、数组、字典、标量值等。如果不提供此参数，则创建一个空的 Series。

  - index：Series 的索引部分，用于对数据进行标记。可以是列表、数组、索引对象等。如果不提供此参数，则创建一个默认的整数索引。

  - dtype：指定 Series 的数据类型。可以是 NumPy 的数据类型，例如 np.int64、np.float64 等。如果不提供此参数，则根据数据自动推断数据类型。

  - name：Series 的名称，用于标识 Series 对象。如果提供了此参数，则创建的 Series 对象将具有指定的名称。

  - copy：是否复制数据。默认为 False，表示不复制数据。如果设置为 True，则复制输入的数据。

  - fastpath：是否启用快速路径。默认为 False。启用快速路径可能会在某些情况下提高性能。







### 1.2 DataFrame

-  类似于一个二维表格，它是 Pandas 中最重要的数据结构。DataFrame 可以看作是由多个 Series 按列排列构成的表格，它既有行索引也有列索引，因此可以方便地进行行列选择、过滤、合并等操作







## 2、核心理念

- Pandas 是基于 NumPy 构建的专门为处理表格和混杂数据设计的 Python 库，核心设计理念包括：

  - 标签化数据结构：提供带标签的轴

  - 灵活处理缺失数据：内置 NaN 处理机制

  - 智能数据对齐：自动按标签对齐数据

  - 强大 IO 工具：支持从 CSV、Excel、SQL 等 20 + 数据源读写

  - 时间序列处理：原生支持日期时间处理和频率转换



## 3、特点

**高效的数据结构**：

- **Series**：一维数据结构，类似于列表（List），但拥有更强的功能，支持索引。
- **DataFrame**：二维数据结构，类似于表格或数据库中的数据表，行和列都具有标签（索引）。

**数据清洗与预处理**：

- Pandas 提供了丰富的函数来处理缺失值、重复数据、数据类型转换、字符串操作等，帮助用户轻松清理和转换数据。

**数据操作与分析**：

- 支持高效的数据选择、筛选、切片，按条件提取数据、合并、连接多个数据集、数据分组、汇总统计等操作。
- 可以进行复杂的数据变换，如数据透视表、交叉表、时间序列分析等。

**数据读取与导出**：

- 支持从各种格式的数据源读取数据，如 CSV、Excel、JSON、SQL 数据库等。
- 也可以将处理后的数据导出为不同格式，如 CSV、Excel 等。

**数据可视化**：

- 通过与 Matplotlib 和其他可视化工具的集成，Pandas 可以快速生成折线图、柱状图、散点图等常见图表。

**时间序列分析**：

- 支持强大的时间序列处理功能，包括日期的解析、重采样、时区转换等。

**性能与优化**：

- Pandas 优化了大规模数据处理，提供高效的向量化操作，避免了使用 Python 循环处理数据的低效。
- 还支持一些内存优化技术，比如使用 category 类型处理重复的数据。