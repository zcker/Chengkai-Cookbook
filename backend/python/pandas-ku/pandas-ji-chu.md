---
cover: >-
  https://images.unsplash.com/photo-1597953601374-1ff2d5640c85?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw0fHxwYW5kYXxlbnwwfHx8fDE2NDg2MzI3NTY&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 🍪 pandas基础

## 2.0 如何解决Windows下pandas报错问题

在anaconda中的base\_root中创建环境，别用3.7版本

常用包：

* xlrd
* pandas
* pytables

## 2.1 用Pandas导入数据

* [pandas.read\_csv — pandas 1.4.1 documentation (pydata.org)](https://pandas.pydata.org/docs/reference/api/pandas.read\_csv.html#pandas.read\_csv)

```python
df = pd.read_csv(
    # 要注意字符串转义符号 \ ，可以使用加r变为raw string或者每一个进行\\转义
    filepath_or_buffer=r'',
    # 编码格式，不同的文件有不同的编码方式，一般文件中有中文的，编码是gbk，默认是utf8
    encoding='gbk',
    # 该参数代表数据的分隔符，csv文件默认是逗号。其他常见的是'\t'
    sep=',',
    # 该参数代表跳过数据文件的的第1行不读入
    skiprows=1,
    # nrows，只读取前n行数据，若不指定，读入全部的数据
    nrows=15,
    # 将指定列的数据识别为日期格式。若不指定，时间数据将会以字符串形式读入。一开始先不用。
    parse_dates=['day'],
    # 将指定列设置为index。若不指定，index默认为0, 1, 2, 3, 4...
    index_col=['day'],
    # 读取指定的这几列数据，其他数据不读取。若不指定，读入全部列
    usecols=['day', 'work'],
    # 当某行数据有问题时，报错。设定为False时即不报错，直接跳过该行。当数据比较脏乱的时候用这个。
    error_bad_lines=False,
    # 将数据中的null识别为空值
    na_values='NULL',
)
```

## 2.2 用Pandas查看数据

* [Options and settings — pandas 1.4.1 documentation (pydata.org)](https://pandas.pydata.org/pandas-docs/stable/user\_guide/options.html)

```python
print(df.shape)  # 输出dataframe有多少行、多少列
print(df.shape[0])  # 取行数量，相应的列数量就是df.shape[1]
print(df.columns)  # 顺序输出每一列的名字
print(df.index)  # 顺序输出每一行的名字
print(df.dtypes)  # 数据每一列的类型不一样，比如数字、字符串、日期等。该方法输出每一列变量类型
print(df.head(3))  # 看前3行的数据，默认是5。与自然语言很接近
print(df.tail(3))  # 看最后3行的数据，默认是5
print(df.sample(n=3))  # 随机抽取3行，想要去固定比例的话，可以用frac参数
print(df.describe())  # 非常方便的函数，对每一列数据有直观感受；只会对数字类型的列有效
pd.set_option('expand_frame_repr', False)  # 当列太多时显示不清楚
pd.set_option("display.max_rows", 1000)  # 设定显示最大的行数
pd.set_option('precision', 1)  # 浮点数的精度
print(df['开盘价'])  # 根据列名称来选取，读取的数据是Series类型
print(df[['交易日期', '收盘价']])  # 同时选取多列，需要两个括号，读取的数据是DataFrame类型

loc操作：通过label（columns和index的名字）来读取数据
print(df.loc['2000-03-30': '2000-04-30'])  # 选取在此范围内的多行，和在list中slice操作类似，读取的数据是DataFrame类型
print(df.loc['2000-03-30': '2000-07-31', '开盘价':'收盘价'])  # 读取指定的多行、多列。逗号之前是行的范围，逗号之后是列的范围。读取的数据是DataFrame类型
print(df.loc[:, :])  # 读取所有行、所有列，读取的数据是DataFrame类型
print(df.at['2000-03-30', '开盘价'])  # 使用at读取指定的某个元素。loc也行，但是at更高效

iloc操作：通过position来读取数据
print(df.iloc[1:3])  # 选取在此范围内的多行，读取的数据是DataFrame类型
print(df.iloc[:, 1:3])  # 选取在此范围内的多列，读取的数据是DataFrame类型
print(df.iloc[1:3, 1:3])  # 读取指定的多行、多列，读取的数据是DataFrame类型
print(df.iat[1, 3])  # 使用iat读取指定的某个元素。使用iloc也行，但是iat更高效
```

## 2.3 Pandas中的列操作

```python
列操作
print(df['交易日期'] + ' 15:00:00')  # 字符串列可以直接加上字符串，对整列进行操作
df['交易日期2'] = df['交易日期'] + ' 00:00:00'	# 新增一列

统计函数
print(df[['收盘价', '成交量']].mean())  # 求两列的均值，返回两个数，Series。会自动排除空值
print(df[['收盘价', '成交量']].mean(axis=1))  # 求两列的均值，返回DataFrame。axis=0或者1要搞清楚
# axis=1，代表对整几列进行操作。axis=0（默认）代表对几行进行操作。实际中弄混很正常，到时候试一下就知道了
print(df['最高价'].max())  # 最大值
print(df['最低价'].min())  # 最小值
print(df['收盘价'].std())  # 标准差
print(df['收盘价'].count())  # 非空的数据的数量
print(df['收盘价'].median())  # 中位数
print(df['收盘价'].quantile(0.25))  # 25%分位数

shift类函数、删除列的方式
df['下周期收盘价'] = df['收盘价'].shift(-1)  # 读取上一行的数据，若参数设定为3，就是读取上三行的数据；若参数设定为-1，就是读取下一行的数据；
del df['下周期收盘价']  # 删除某一列的方法
df['涨跌'] = df['收盘价'].diff(1)  # 求本行数据和上一行数据相减得到的值
df.drop(['涨跌'], axis=1, inplace=True)  # 删除某一列的另外一种方式，inplace参数指是否替代原来的df
df['涨跌幅'] = df['收盘价'].pct_change(1)  # 类似于diff，但是求的是两个数直接的比例，相当于求涨跌幅

cum(cumulative)类函数
df['累计成交量'] = df['成交量'].cumsum()  # 该列的累加值
print((df['涨跌幅'] + 1.0).cumprod())  # 该列的累乘值，此处计算的就是资金曲线，假设初始1元钱。
df['收盘价_排名'] = df['收盘价'].rank(ascending=True, pct=False)  # 输出排名。ascending参数代表是顺序还是逆序。pct参数代表输出的是排名还是排名比例
print(df['收盘价'].value_counts())  # 计数。统计该列中每个元素出现的次数。返回的数据是Series
```

## 2.4 用Pandas筛选数据

```python
print(df['股票代码'] == 'sh600000')  # 判断交易股票代码是否等于sh600000,返回的是true or false
print(df[df['股票代码'] == 'sh600000'])  # 将判断为True的输出：选取股票代码等于sh600000的行
print(df[df['股票代码'] == 'sh600000'].index)  # 输出判断为True的行的index
print(df[df['股票代码'].isin(['sh600000', 'sh600004', 'sz000001'])])  # 选取股票代码等于'sh600000'或'sh600004'或'sz000001'的都行
print(df[df['收盘价'] < 10.0])  # 选取收盘价小于10的行
print(df[(df['收盘价'] < 10.0) & (df['股票代码'] == 'AIDUSD')])  # 两个条件，或者的话就是|
```

## 2.5 Pandas空缺值处理

```python
1.删除缺失值
print(df.dropna(how='any'))  # 将带有空值的行删除。how='any'意味着，该行中只要有一个空值，就会删除，可以改成all。
print(df.dropna(subset=['月头', '收盘价'], how='all'))  # subset参数指定在特定的列中判断空值。
all代表全部为空，才会删除该行；any只要一个为空，就删除该行。

2.补全缺失值
print(df.fillna(value='N'))  # 直接将缺失值赋值为固定的值
df['月头'].fillna(value=df['收盘价'], inplace=True)  # 直接将缺失值赋值其他列的数据
print(df.fillna(method='ffill'))  # 向上寻找最近的一个非空值，以该值来填充缺失的位置，全称forward fill，非常有用
print(df.fillna(method='bfill'))  # 向下寻找最近的一个非空值，以该值来填充确实的位置，全称backward fill

3.找出缺失值
print(df.notnull())  # 判断是否为空值，反向函数为isnull()
print(df[df['月头'].notnull()])  # 将'月头'列为空的行输出
```

## 2.6 Pandas数据整理

```python
--排序函数
print(df.sort_values(by=['交易日期'], ascending=1))  # by参数指定按照什么进行排序，acsending参数指定是顺序还是逆序，1顺序，0逆序
print(df.sort_values(by=['股票代码', '交易日期'], ascending=[1, 0]))  # 按照多列进行排序

--df上下合并操作
print(df1.append(df2))  # append操作，将df1和df2上下拼接起来。注意观察拼接之后的index。index可以重复
df3 = df1.append(df2, ignore_index=True)  # ignore_index参数，用户重新确定index

--对数据进行去重
df3.drop_duplicates(
    subset=['交易日期', '股票代码'],  # subset参数用来指定根据哪类类数据来判断是否重复。若不指定，则用全部列的数据来判断是否重复
    keep='first',  # 在去除重复值的时候，我们是保留上面一行还是下面一行？first保留上面一行，last保留下面一行，False就是一行都不保留
    inplace=True
)

--其他函数
df.reset_index(inplace=True, drop=True)  # 重置index
df.rename(columns={
    '收盘价': 'close',
    '开盘价': 'open',
    '股票代码': 'code',
    '股票名称': 'name',
    '交易日期': 'date',
    '最高价': 'high',
    '最低价': 'low',
    '前收盘价': 'prev_close',
    '成交量': 'volume',
    '成交额': 'money'}, inplace=True)  # rename函数给变量修改名字。使用dict将要修改的名字传给columns参数
print(df.empty)  # 判断一个df是不是为空，此处输出不为空
print(pd.DataFrame().empty)  # pd.DataFrame()创建一个空的DataFrame，此处输出为空
print(df.T)  # 将数据转置，行变成列，很有用
```

## 2.7 Pandas中的字符串操作

* [Working with text data — pandas 1.4.1 documentation (pydata.org)](https://pandas.pydata.org/pandas-docs/stable/user\_guide/text.html)

```python
print('sh600000'[:2])   # 取前两个字符
print(df['股票代码'].str[:2])   # 全部列取前两个字符
print(df['股票代码'].str.upper())  # 加上str之后可以使用常见的字符串函数对整列进行操作
print(df['股票代码'].str.lower())   # 小写
print(df['股票代码'].str.len())  # 计算字符串的长度,length
df['股票代码'].str.strip()  # strip操作，把字符串两边的空格去掉
print(df['股票代码'].str.contains('767'))  # 判断字符串中是否包含某些特定字符
print(df['股票代码'].str.replace('sz', 'sh'))  # 进行替换，将sz替换成sh
```

## 2.8 Pandas中的时间处理

* [Time series / date functionality — pandas 1.4.1 documentation (pydata.org)](https://pandas.pydata.org/pandas-docs/stable/user\_guide/timeseries.html)

```python
--时间处理
print(pd.to_datetime('1999年1月11日'))  # pd.to_datetime函数：将字符串转变为时间变量
print(df['交易日期'].dt.year)  # 输出这个日期的年份。相应的month是月份，day是天数，还有hour, minute, second
print(df['交易日期'].dt.week)  # 这一天是一年当中的第几周
print(df['交易日期'].dt.dayofyear)  # 这一天是一年当中的第几天
print(df['交易日期'].dt.dayofweek)  # 这一天是这一周当中的第几天，0代表星期一
print(df['交易日期'].dt.weekday)  # 和上面函数相同，更加常用
print(df['交易日期'].dt.days_in_month)  # 这一天所在月份有多少天
print(df['交易日期'].dt.is_month_end)  # 这一天是否是该月的开头，是否存在is_month_end？
print(df['交易日期'] + pd.Timedelta(days=1))  # 增加一天，Timedelta用于表示时间差数据，[weeks, days, hours, minutes, seconds, milliseconds, microseconds, nanoseconds]
print((df['交易日期'] + pd.Timedelta(days=1)) - df['交易日期'])  # 增加一天然后再减去今天的日期
```

## 2.9 Pandas的滚动操作

```python
--rolling、expanding操作
# 使用rolling函数
df['收盘价_3天均值'] = df['收盘价'].rolling(3).mean()
# rolling(n)即为取最近n行数据的意思，只计算这n行数据。后面可以接各类计算函数，例如max、min、std等
# rolling可以计算每天的最近3天的均值，如果想计算每天的从一开始至今的均值，应该如何计算？
# 使用expanding操作
df['收盘价_至今均值'] = df['收盘价'].expanding().mean()
print(df[['收盘价', '收盘价_至今均值']])
# expanding即为取从头至今的数据
```
