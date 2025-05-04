```python
计算速度 np数组 > df转化为np数组 > df
df.values # 返回np数组
df.index
df.colums 
df.shape
df.stack()



# 可以是numpy, 也可以是df
np.uniqeu(numpy/df)

# df的统计
df.value_counts()

# 按值排序
df.sort_values(by="排序的列",ascending=False  # 降序
)

# 按轴排序
df.sort_index()  # 按行排序
df.sort_index(axis=1)  # 按列排序

# 查看信息
df.info()
df.head() # 默认数值为5
df.tail() # 默认数值为5
df.describe()
df.dtypes

# 常用计算(统计信息)
df.count() # 计算非空元素的个数
df.idxmax() # 最大值的坐标
df.sum()
df.min()
df.max()
df.mean()
df.median()
df.mode()  # 注意, 返回的是dataframe, 不是series
df.quantile()  # 分位数
# 相关性系数
df.corr()

apply函数非常好用
# 缺失值处理
df.isnull() # 判断是否有缺失值
df.notnull()   
df.dropna() # 删除缺失值的行
df.dropna(axis="columns",how="all") # 全是缺失值才删除
 
value = df.mean() # 每一列的平均值
value = df.stack().mean() # 全部数据的填充 
df.fillna(value=value) # 缺失值填充

df.stack() # 把列索引变成行索引, 会把缺失值舍去


```