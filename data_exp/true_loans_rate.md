# 借贷的真实代价是多少？

*作者：奔跑的青椒*<br />

> 曾经见过被信用卡借款等贷产品缠身的朋友，那种入不敷出，钱越欠越多，钱越还越多的感觉令人绝望和心痛，借贷产品的存在没有任何错误，同时在使用借贷之前我们应该知道所要付出的代价是多少。

### 利率和还款方式

#### 什么是是利率
[百度百科解释](https://baike.baidu.com/item/%E9%93%B6%E8%A1%8C%E8%B4%B7%E6%AC%BE%E5%88%A9%E7%8E%87)，银行贷款利率是指借款期限内利息数额与本金额的比例。

#### 银行贷款利率
为了判断使用这些产品是否过于昂贵，我们需要一个基准作为参考，这里使用银行的贷款利率用做基准，数据源自：[招商银行贷款利率](https://www.cmbchina.com/CmbWebPubInfo/CDRate.aspx?chnl=cdrate)。

|  贷款年限        | 利率  |
|  ----          | ----  |
| 0-6月(含6月)    | 4.35% |
| 6月-1年(含1年)  | 4.35% |
| 1-3年(含3年)    | 4.75% |
| 3-5年(含5年)    | 4.75% |
| 5-30年(含30年)  | 4.90% |

*人民币贷款基准利率 生效日期：2015-10-24*<br/>

#### 日利率，月利率，年利率转换
1. 日利率 = 年利率 / 365
2. 月利率 = 年利率 / 12

#### 还款方式
1. 先息后本（按月付息，到期还本）。
2. 等额本息。
3. 等额本金。
4. 等本等息。

### 等额本金还款方式
[百度百科解释](https://baike.baidu.com/item/%E7%AD%89%E9%A2%9D%E6%9C%AC%E9%87%91%E8%BF%98%E6%AC%BE%E6%B3%95)，等额本金还款，是指贷款人将本金分摊到每个月内，同时付清上一交易日至本次还款日之间的利息。这种还款方式相对等额本息而言，总的利息支出较低，但是前期支付的本金和利息较多，还款负担逐月递减。

案例：<br/>
张三向银行贷款了 48 万元买房，年利率为 4.80%，月利率为 0.4%，贷款 20 年，共 240 期（月），张三选择等额本金方式还款，请问张三需要还的总利息是多少，每月还款额是多少？

#### 1. 第一印象方法计算利息（错误答案）
还款总利息：<br/>
公式：本金 x 年利率 x 期数 = 48万 x 4.8% x 20 = 46.08万，总利息为 **46.08万**。

每月还款金额：<br/>
贷款 240 期，每月还款金额为：(总贷款 + 总利息) / 总期数 = (48万 + 46.08万) / 240 = 3920 元

这是错误答案，用等额本金还款方式计算出的总利息会小于这个数。

#### 2. 等额本金方法计算
每月还款金额计算：<br/>
贷款 240 期，每月还款金额为：总贷款 / 总期数 = 48万 / 240 = 2000 元

每月还款利息计算：<br/>
第 1 个月，张三占用了银行 48万贷款，利息为：48万 x 0.4% = 1920 元 <br/>
第 2 个月，张三上个月还了 2000 元，占用了银行 (48万 - 2千)贷款，利息为：(48万 - 2千) x 0.4% = 1912 元 <br/>
第 3 个月，张三上个月还了 2000 元，总共还了 4000，占用了银行 (48万 - 2千 - 2千)贷款，利息为：(48万 - 2千 - 2千) x 0.4% = 1902 元 <br/>
...<br/>
第 240 个月，张三总共还了 478000，占用银行 2千贷款，利息为：2千 x 0.4% = 8 元<br/>

总共还的利息为：1920 + 1912 + 1908 + ... + 16 + 8，这是个等差数列，根据等差数列的特性，求平均值：(1920 + 8) / 2 = 964，`总利息 = 平均值 x 总期数` = **231360 元**，远小于 46.08万元的利息。

**疑问解答？**<br/>
问：等额本金方法计算利息为什么比直觉法计算利息少这么多？<br/>
答：银行计算利息是按我们占用了多少贷款作为基数计算的，我们每月都在还本金，占用的贷款越来越少，相应的利息也会越来越少。

程序模拟过程如下：

| 期数 | 总贷款额 | 每月本金部分 | 每月利息部分 | 每月还款 |
| ---- | ---- | ---- | ---- | ---- |
| 第 1 期 | 480000.0 | 2000.0 | 1920.0 | 3920.0 |
| 第 2 期 | 478000.0 | 2000.0 | 1912.0 | 3912.0 |
| 第 3 期 | 476000.0 | 2000.0 | 1904.0 | 3904.0 |
| 第 4 期 | 474000.0 | 2000.0 | 1896.0 | 3896.0 |
| 第 5 期 | 472000.0 | 2000.0 | 1888.0 | 3888.0 |
| 第 6 期 | 470000.0 | 2000.0 | 1880.0 | 3880.0 |
| 第 7 期 | 468000.0 | 2000.0 | 1872.0 | 3872.0 |
| 第 8 期 | 466000.0 | 2000.0 | 1864.0 | 3864.0 |
| 第 9 期 | 464000.0 | 2000.0 | 1856.0 | 3856.0 |
| ... | ... | ... | ... | ... |
| 第 234 期 | 14000.0 | 2000.0 | 56.0 | 2056.0 |
| 第 235 期 | 12000.0 | 2000.0 | 48.0 | 2048.0 |
| 第 236 期 | 10000.0 | 2000.0 | 40.0 | 2040.0 |
| 第 237 期 | 8000.0 | 2000.0 | 32.0 | 2032.0 |
| 第 238 期 | 6000.0 | 2000.0 | 24.0 | 2024.0 |
| 第 239 期 | 4000.0 | 2000.0 | 16.0 | 2016.0 |
| 第 240 期 | 2000.0 | 2000.0 | 8.0 | 2008.0 |
| 总计 | -- | 480000 | 231360 | 711360.0 |

*从模拟过程可以看出，等额本金还款方式的特点是每月还款本金部分固定，利息部分由多到少。*

### 等额本息还款方式
[百度百科解释](https://baike.baidu.com/item/%E7%AD%89%E9%A2%9D%E6%9C%AC%E6%81%AF%E8%BF%98%E6%AC%BE%E6%B3%95)，等额本息还款法即把按揭贷款的本金总额与利息总额相加，然后平均分摊到还款期限的每个月中，每个月的还款额是固定的，但每月还款额中的本金比重逐月递增、利息比重逐月递减。

案例：<br/>
使用张三的案例计算一遍等额本息还款方式的总利息和每月还款额。

等额本息还款计算方式较为复杂，推导过程如下：<br/>
月利率为 B，等额本息每月还款是固定的，假设张三每月还款为固定的 X 元 <br />
1. 第 1 个月贷款总额为：A0，还款 X 元后贷款总额 A1 为：`A1 = A0 x (1 + B) - X`，A0 等于 48万
2. 第 2 个月贷款总额为：A1，还款 X 元后贷款总额 A2 为：`A2 = A1 x (1 + B) - X`
3. 第 3 个月贷款总额为：A2，还款 X 元后贷款总额 A3 为：`A3 = A2 x (1 + B) - X`<br/>
...<br/>
4. 第 240 个月贷款总额为：A239，还款 X 元后贷款总额 A3 为：`A240 = A239 x (1 + B) - X`，A240 等于 0

根据上面过程进行数学推导（推导过程省略），得到还款公式如下：

>> X = \frac{AB(1+B)^m}{(1+B)^m-1} >>

X 为每月还款额，m 为总期数。

由公式计算出 X = 3115 元，总还款额为 3115 x 240 = 747600 元，**总利息（贷款的代价）为： 747600 - 480000 = 267600 元**。

**疑问解答？**<br/>
问：等额本息方式对比等额本金方式来说是不是多花了钱？<br/>
答：并不是，银行收取多少利息是根据你每月占用贷款数额的大小，等额本金方式由于每个月还的本金多，占用的贷款金额会越来越少，所以利息也会越来越少，相比之下等额本息方式每月占用的贷款金额更多，所以生成的利息较多。

程序模拟过程如下：

| 期数 | 总贷款额 | 每月本金部分 | 每月利息部分 | 每月还款 |
| ---- | ---- | ---- | ---- | ---- |
| 第 1 期 | 480000 | 1195 | 1920 | 3115 |
| 第 2 期 | 478805 | 1200 | 1915 | 3115 |
| 第 3 期 | 477605 | 1205 | 1910 | 3115 |
| 第 4 期 | 476400 | 1209 | 1906 | 3115 |
| 第 5 期 | 475191 | 1214 | 1901 | 3115 |
| 第 6 期 | 473977 | 1219 | 1896 | 3115 |
| 第 7 期 | 472758 | 1224 | 1891 | 3115 |
| 第 8 期 | 471534 | 1229 | 1886 | 3115 |
| 第 9 期 | 470305 | 1234 | 1881 | 3115 |
| ... | ... | ... | ... | ... |
| 第 234 期 | 21463 | 3029 | 86 | 3115 |
| 第 235 期 | 18434 | 3041 | 74 | 3115 |
| 第 236 期 | 15393 | 3053 | 62 | 3115 |
| 第 237 期 | 12340 | 3066 | 49 | 3115 |
| 第 238 期 | 9274 | 3078 | 37 | 3115 |
| 第 239 期 | 6196 | 3090 | 25 | 3115 |
| 第 240 期 | 3106 | 3103 | 12 | 3115 |
| 总计 | -- | 480000 | 267600 | 747600 |

*从模拟过程可以看出，等额本息还款方式的特点是每月还款额固定。*

### 招商银行掌上取现

规则如下：
1. 手续费为交易金额 1%，最低 10元/笔；
2. 日利率为 0.05%，按月计收复利。

问：按月计收复利是什么？<br/>
答：根据掌上生活规则，若账单日是 12 日，在 2 月 1 日取现 2000，利息每天收取 1 元，截止到 2 月 12 日的利息一共是 12 元，那么从 2 月 13 日（账单日次日）开始按 2012 元计算利息。

案例：老王账单日为每月 12 号，他于 2020 年 1 月 1 日通过招商银行借款 10000，1 年后还款是多少，年利率是多少？<br/>
1. 手续费： 10000 * 1% = 100
2. 每日利息：10000 * 0.05% = 5

程序模拟每月账单过程如下：

|  账单日        | 本金  | 利息 | 计息天数 | 账单 |
|  ----          | ----  | ---- | ---- | ---- |
| 2020-01-12 | 10000 | 60.0 | 12 | 10060.0 |
| 2020-02-12 | 10060.0 | 155.93 | 31 | 10215.93 |
| 2020-03-12 | 10215.93 | 148.13 | 29 | 10364.06 |
| 2020-04-12 | 10364.06 | 160.64 | 31 | 10524.7 |
| 2020-05-12 | 10524.7 | 157.87 | 30 | 10682.57 |
| 2020-06-12 | 10682.57 | 165.58 | 31 | 10848.15 |
| 2020-07-12 | 10848.15 | 162.72 | 30 | 11010.87 |
| 2020-08-12 | 11010.87 | 170.67 | 31 | 11181.54 |
| 2020-09-12 | 11181.54 | 173.31 | 31 | 11354.85 |
| 2020-10-12 | 11354.85 | 170.32 | 30 | 11525.17 |
| 2020-11-12 | 11525.17 | 178.64 | 31 | 11703.81 |
| 2020-12-12 | 11703.81 | 175.56 | 30 | 11879.37 |
| 2021-01-01 | 11879.37 | 118.79 | 20 | 11998.16 |

总还款额为：**12098.16** = 2021-01-01日最终账单 + 手续费 = 11998.16 + 100 <br/>
**总利息（贷款的代价）为：2098.16**<br/>
年利率为：**20.98%** = 2098.16 / 10000 <br/>
20.98% 远大于银行基准利率 4.35%，这笔借款的代价从数字上看无疑是很昂贵的，时间越长越贵。<br/>

### 微信分付
目前网上查询到微信分付的日利率为 0.04%，根据年利率与日利率的转换公式计算：<br/>
年利率：**14.6%** = 日利率 * 365 = 0.04% * 365

14.6% 远大于银行基准利率 4.35%，借贷 10000 一年后归还，**利息（贷款的代价）为：10000 * 14.6% = 1460**。

### 结论
1. 《思考·快与慢》里说了很多种行为偏差，对于借贷问题，抛开产品的宣传带来的锚定效应偏差和启动效应偏差，将利率、还款方式和时间这三者结合来计算出最终成本后，往往可以让你得到与第一印象不同的结论，从而做出更好的决策。
2. 如果读者想要使用花里胡哨的借贷产品，但是仍然不知道怎么计算要付出的代价，欢迎联系我一起探讨。

程序模拟代码位于：[https://github.com/runforever/data_exp/blob/master/src/true_loans_rate.ipynb](https://github.com/runforever/data_exp/blob/master/src/true_loans_rate.ipynb)，请使用 Jupyter 打开。



### 风险提示
本文分析过程和结论尽可能科学和客观，如有不严谨的地方，还望多指教。