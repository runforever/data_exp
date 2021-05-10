# A股中股票型基金也只有 10% 的基金赚钱吗？

*作者：奔跑的青椒*<br />

> 交易市场存在一句话，交易市场中 70% 的人亏钱，20% 的人不亏不赚，10% 的人赚钱，交易市场的玩家有散户、机构和游资，这当中机构和游资属于市场中的优秀玩家，优秀玩家是否也是只有 10% 的人赚钱呢？

### 数据源和工具
1. 数据源来自 [聚宽量化平台 JQData](https://www.joinquant.com/help/api/help?name=JQData)。
2. 取股票型基金数据用作分析。
3. 分析工具 Python、Jupyter、Pandas。

### 分析方案
1. 获取所有股票型基金。
2. 从所有股票型基金中随机抽 30% 的数据用作分析。
3. 获取抽选基金 3 年的单位累积净值收益，计算其中赚钱和亏钱的比例。
4. 获取抽选基金 5 年的单位累积净值收益，计算其中赚钱和亏钱的比例。

**疑问解答？**<br/>
问：为什么随机抽取 30% 的数据就可以让结论正确？<br/>
答：根据统计学中心极限定理，一个大型样本的正确抽样与其所代表的群体存在相似关系。

问：单位累积净值是什么，为什么用它来表示盈亏是有效的？<br/>
答：[百度百科](https://baike.baidu.com/item/%E7%B4%AF%E8%AE%A1%E5%87%80%E5%80%BC)，基金单位累计净值是基金单位净值与基金成立后历次累计单位派息金额的总和，反映该基金自成立以来的所有收益的数据。基金单位累计净值=基金单位净值+基金历史上累计单位派息金额(基金历史上所有分红派息的总额/基金总份额）。

### 分析过程
1. 使用 Jupyter 进行探索，导入相关包。

```python
import datetime

import pandas as pd
from jqdatasdk import *

auth('your account', 'your password')
%matplotlib inline
```

2. 获取所有股票型基金数据。

```python
# 获取所有成立超过 3 年的股票型基金数据
year_range = 3
now = datetime.date.today()

start_year = now.year - year_range
fund_end_date = datetime.datetime(start_year, 1, 1).strftime('%Y-%m-%d')

# 结果集
result = None

page = 0
page_size = 3000
while 1:
    df = finance.run_query(
        query(finance.FUND_MAIN_INFO).filter(
            finance.FUND_MAIN_INFO.underlying_asset_type_id=='402001',
            finance.FUND_MAIN_INFO.start_date<fund_end_date,
        ).offset(page * page_size).limit(3000)
    )
    if df.empty:
        break
    if result is None:
        result = df
    else:
        result = pd.concat([result, df], ignore_index=True)
    page += 1

print(result.shape)

```

输出

```python
(1020, 11)
# 表示总共获取到了 1020 个运作时间大于 3 年的股票型基金
```

3. 抽样，打乱结果集顺序，抽取前 30% 的数据来分析

```python
random_count = round(result.shape[0] * 0.3)
random_result = result.sample(frac=1).iloc[:random_count]
```

4. 获取样本中 3 年时间段初始累积净值和结束累积净值

```python
start_values = []
end_values = []

for _, row in random_result.iterrows():
    code = row['main_code']
    start_date = row['start_date']

    perf_start_date = start_date.strftime('%Y-%m-%d')
    perf_end_date = datetime.datetime(start_date.year+year_range, start_date.month, start_date.day).strftime('%Y-%m-%d')

    q = query(finance.FUND_NET_VALUE).filter(
        finance.FUND_NET_VALUE.code==code,
        finance.FUND_NET_VALUE.day>=perf_start_date
        ).order_by(finance.FUND_NET_VALUE.day.asc()).limit(1)
    df = finance.run_query(q)
    start_values.append(df.sum_value.values[0] or df.net_value.values[0])

    q = query(finance.FUND_NET_VALUE).filter(
        finance.FUND_NET_VALUE.code==code,
        finance.FUND_NET_VALUE.day<=perf_end_date
        ).order_by(finance.FUND_NET_VALUE.day.desc()).limit(1)
    df = finance.run_query(q)
    end_values.append(df.sum_value.values[0] or df.net_value.values[0])
```

5. 计算样本收益

```python
random_result['start_value'] = start_values
random_result['end_value'] = end_values
random_result['profit'] = (random_result['end_value'] - random_result['start_value'])/random_result['start_value']
```

6. 获取收益的均值、标准差等统计信息

```python
random_result.profit.describe()
```

输出

```python
count    306.000000      # 样本总数
mean       0.082556      # 均值
std        0.477168      # 标准差
min       -0.986800      # 最小值
25%       -0.196875      # 1/4 位值
50%        0.067647      # 中位数
75%        0.325325
max        3.993000      # 最大值
Name: profit, dtype: float64
```

7. 计算样本的盈亏比例

```python
profits = random_result.profit.values
total = len(profits)
loss = sum(1 for i in profits if i <= 0)
win = sum(1 for j in profits if j > 0)
print('样本总数: %s, 亏损数: %s, 盈利数: %s, 亏损比例: %s, 盈利比例: %s' % (total, loss, win, round(loss/float(total), 2), round(win/float(total), 2)))
```

输出

```python
样本总数: 306, 亏损数: 130, 盈利数: 176, 亏损比例: 0.42, 盈利比例: 0.58
```

8. 查看样本盈亏直方图

```python
random_result.profit.hist(bins=50)
```

![收益直方图](../webimage/8C24CA16-940D-49E9-A463-60F06EA63C5A.png)

9. 盈利基金每年的平均收益

```python
avg_profit = sum(j for j in profits if j) / win
i = (1 + avg_profit) ** (1/3) - 1
print(i)

# 0.045
```

分析代码位于 [https://github.com/runforever/data_exp/blob/master/src/Astock_fund.ipynb](https://github.com/runforever/data_exp/blob/master/src/Astock_fund.ipynb)

### 结论
1. 经过多次抽样，样本 3 年业绩中亏损和盈利的比例为 4:6，60% 左右的基金是盈利的，零假设 10% 的基金赚钱不成立。
2. 成立 5 年以上的基金 462家，直接分析总数据中 5 年业绩中亏损和盈利的比例为 2:8，80% 左右的基金是盈利的，零假设 10% 的基金赚钱不成立。
3. 即使大多数股票型基金是盈利的，不代表无脑投资股票基金是好的，样本数据显示，所有股票型基金的平均盈利为 0.08，盈利基金的平均收益为 4%，购买股票型基金想要获得超额收益需要投资人具备识别优秀基金的能力。

### 风险提示
1. 本文分析过程和结论尽可能科学和客观，如有不严谨的地方，还望多指教。
2. 本文不构成任何投资建议，教人炒股，天打雷劈。
