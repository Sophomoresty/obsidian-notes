## 在获取文本的时候, 为什么都是要设置编码格式?

```python
with open(file_path,mode='rt',encoding='utf-8') as file_object:
  ...				
res = requests.get(url,headers=headers)
res.encoding('utf-8').text
```

> 二进制/十六进制  通过解码 得到 文本;文本 通过编码 得到 二进制/十六进制.
>
> 从这里来看, 不是应该要设置解码格式吗?

实际上, 我们的思路是没问题的.

获取文本确实是需要对二进制进行解码, 也就是需要获得解码格式,

如何获取呢? 只要知道这段代码是怎么编码的就行了, 所以我们需要告诉它编码格式, 它自然就获得了解码格式, 然后对二进制进行解码, 获取文本

## 正则问题

*也是贪婪匹配, 如果只想出现一次的话

```python
pattern = re.compile(r'.*?')
```

豆瓣抓取图片的时候, 想用.代替\s,结果发现多了大一部分内容, 

```pyhon
pattern_2 = re.compile(r'''<img\s+?src=\"https://img\d.doubanio.com/mpic/s\d+.jpg\"/>''')  # 正确写法
pattern_2 = re.compile(r'''<img.+?src=\"https://img\d.doubanio.com/mpic/s\d+.jpg\"/>''',re.S)  # 错误写法
```

`.` 代表的任意字符, 这里的字符包括`\` `*` `空格` 这些字符, 除了换行符的所有字符

## 字符串方法

### 字符串的strip(), 第一不会改变原字符串, 第二不会返回值

### 字符串的replace不会改变原字符串, 会返回值

## 列表的复制

```python
list1 = list2  # 直接复制, 指向同一块地址
# 列表生成式和for循环遍历等同于copy(), 都是浅拷贝
list.copy()  # 浅拷贝
list.deepcopy()  # 深拷贝
```

列表的copy方法, 对于第一层是深拷贝, 即重新创建内存地址, 对于嵌套的是浅拷贝

列表的remove

列表的del

## 夜猫知识点

### 提取的经典知识点

字符串的join方法可以把列表抓换为字符串, 效果更加方便

字符串的strip(), 会改变原字符串, 不会返回值

list的sort方法, 会改变原列表,不会返回值

list的appende方法, list必须提前定义

切片索引和range函数可以理解为左闭右开

```python
data_list[3:5]
range(1,10)
```

list()方法可以把字符串直接转换为列表, 每个字符为单独的元素



### Pycharm知识点

#### 1.修改运行配置 - 使用pyhton控制台运行

既能在终端运行, 使用pycharm的补齐功能; 又能像调试一样看到变量结构



<img src="E:\MD\Typora\assets\image-20241228125704519.png" alt="image-20241228125704519" style="zoom: 50%;" />

<img src="E:\MD\Typora\assets\image-20241228125721795.png" alt="image-20241228125721795" style="zoom: 50%;" />

#### 2.使用软件包进行安装(当然也可以用pip)

<img src="E:\MD\Typora\assets\image-20241228130025600.png" alt="image-20241228130025600" style="zoom: 33%;" />

<img src="E:\MD\Typora\assets\image-20241228130037809.png" alt="image-20241228130037809" style="zoom:33%;" />