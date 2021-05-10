# 一份调查问卷的数据分析过程

*作者：奔跑的青椒*<br />

> 2020 年 4 月 1 日，知识星球【涛哥聊 Python】让粉丝帮忙做了一个付费课程调查问卷，截止到 2020 年 4 月 13 日，这份调查问卷有 107 次浏览，31 次填写记录，这里感谢这些认真填写问卷的粉丝，你们的帮忙促成了这篇文章的产生。

## 为什么做数据分析
做数据分析之前要了解自己的目的，数据分析的环节可以用下图表示：

![](../webimage/4847143E-EC7E-4A26-A0DB-7E152F248356.png)

*图片来源：《深入浅出数据分析》*

我们制作这份调查问卷的目的：<br />
1. 了解粉丝群体特征。
2. 了解粉丝想学习什么内容和需求。

## 分析过程
使用的工具：VSCode、Jupyter、Pandas、matplotlib

### 1. 导入所需要的工具包

```python
import pandas as pd
import matplotlib.pyplot as plt

%matplotlib inline

# 解决 Mac 下 matplotlib 中文显示乱码问题
plt.rcParams['font.sans-serif'] = ['Arial Unicode MS']
```

### 2. 读取数据和查看数据行列信息

```python
df = pd.read_csv('/Users/runforever/CodeRepo/GitRepo/Road/data_exp/src/data.csv')

print('表格行列数:\n', df.shape)
print('每列数据量和类型:'),
print(df.info())
```

![](../webimage/9B69CB91-B4B8-4F87-8DD6-6671DF05C6CC.png)

*总共：31 行数据，15 列*

显示前 5 行数据

```python
df.head()
```

![](../webimage/8A10923C-CA5D-4622-9D10-48A5FCAC476C.png)

### 3.粉丝的男女比例（饼图展示）

```python
print(df['性别'].value_counts(normalize=True))
df['性别'].value_counts(normalize=True).plot.pie(figsize=(6, 6), autopct='%.2f')
plt.show()
```

![](../webimage/73B6CC01-6021-47B2-B96F-2598D3974ADC.png)

**男女粉丝比例大约为 8:2**

### 4. 粉丝的工作经验（饼图）

```python
print(df['你的工作'].value_counts(normalize=True))
df['你的工作'].value_counts(normalize=True).plot.pie(figsize=(6, 6), autopct='%.2f')
plt.show()
```

![](../webimage/630CD6D2-3022-4780-A258-2C233612E578.png)

**其他行业转 Python 和大学生群体占比最大**

### 5. 倾向学习的付费课程（条形图）

```python
courses = [item.split('，') for item in df['你更倾向于下面哪种付费课程呢？'].values]
courses = [course for course_list in courses for course in course_list]
pd.Series(courses).value_counts().plot.barh()
```

![](../webimage/525983EA-76AB-4084-95A2-30E40FA1C040.png)

**大家最想学习使用 Python 进行数据分析**

### 6. 学习 Python 的目的（条形图）

```python
courses = [item.split('，') for item in df['你学Python的目的是？'].values]
courses = [course for course_list in courses for course in course_list]
pd.Series(courses).value_counts().plot.barh()
```

![](../webimage/9BAF9B69-F8BB-4C6B-8CFF-79A2D7A9F229.png)

**大家主要还是为了找工作和提高工作效率，我在分析的过程中对于“提高工作效率”这点有些疑问，这个具体指哪方面呢？欢迎读者留言探讨。**

### 7. 使用的设备（饼图）

```python
print(df['填写设备'].value_counts(normalize=True))
df['填写设备'].value_counts(normalize=True).plot.pie(figsize=(6, 6), autopct='%.2f')
plt.show()
```

![](../webimage/3B0948FB-BE3A-4D62-AC6A-1D53EE31DE36.png)

**Android 用户占比最大，相当一部分粉丝日常在 PC 端挂着微信**

### 8. 大家的期望薪资（条形图）

```python
salary = df['找工作你期望的第一份薪资是？'].dropna()
print(salary.value_counts())
salary.value_counts().plot.barh()
plt.show()
```

![](../webimage/25B07305-6A2E-4449-82BB-3E3C5C9B9BDB.png)

**关于薪资，大家挺克制的，选择 5000-8000 的人最多**

### 9. 是否愿意参与内推（条形图）

```python
recommend = df['如果是找工作，需要帮助内推么？'].dropna()
print(recommend.value_counts())
recommend.value_counts().plot.bar()
plt.show()
```

![](../webimage/715E3B1D-B016-41A0-BA20-F91220CEE0F2.png)

**找工作内推其实效率最高，相信大部分人愿意走内推渠道**

### 10.是否愿意参与推广（条形图）

```python
courses = [item.split('，') for item in df['你是否愿意参与进来'].dropna().values]
courses = [course for course_list in courses for course in course_list]
pd.Series(courses).value_counts().plot.barh()
```

![](../webimage/609CA412-770E-475C-8FBA-E22729602964.png)

**粉丝的热情很高，而且大家愿意互相帮助，感谢你们**

### 11. 填写时长

```python
fill_time = [i.replace('秒', '').split('分') for i in df['填写时长'].values]
fill_seconds = []
for i in fill_time:
    if len(i) == 1:
        fill_seconds.append(int(i[0]))
    else:
        fill_seconds.append(int(i[0]) * 60 + int(i[1]))

fill_time_series = pd.Series(fill_seconds)
print(fill_time_series.describe())
fill_time_series.plot.hist()
plt.show()
```

![](../webimage/E1E866DE-9D75-4865-A5E0-B5C82CAE8A1D.png)

**原本我假设大家填写问卷的时长应该是符合正态分布的，结果完全没有，可能数据量不够多吧**

## 结论
1. 展示了使用 pandas 对于数据分析的一般过程。
2. 关于 pandas API 的使用，展示了 value_counts 函数的用法和如何使用 pandas 画饼图、条形图、直方图。
3. 关于 31 个人填写问卷的样本能否代表总体情况的问题，根据统计学的中心极限定理：一个大型样本的正确抽样与其所代表的群体存在相似关系，只好先假设这是正确抽样了，也欢迎更多的人填写问卷，然后我坐等打脸。

**源码位于**：[https://github.com/runforever/data_exp/blob/master/src/questionnaire.ipynb](https://github.com/runforever/data_exp/blob/master/src/questionnaire.ipynb)

**调查问卷地址**：[https://jinshuju.net/f/N2YpNI](https://jinshuju.net/f/N2YpNI)

**编辑文章使用的工具**：<br/>
1. [docsify](https://docsify.js.org/#/zh-cn/)
2. markdown
3. VSCode
4. 上传图片工具 [iDrag](https://github.com/runforever/iDrag)
5. Mac

## 风险提示
本文分析过程和结论尽可能科学和客观，如有不严谨的地方，还望多指教。
