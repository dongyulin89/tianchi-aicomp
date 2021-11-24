# 实验室介绍

## XGBoost 的介绍
XGBoost是2016年由华盛顿大学陈天奇老师带领开发的一个可扩展机器学习系统。严格意义上讲XGBoost并不是一种模型，而是一个可供用户轻松解决分类、回归或排序问题的软件包。它内部实现了梯度提升树(GBDT)模型，并对模型中的算法进行了诸多优化，在取得高精度的同时又保持了极快的速度，在一段时间内成为了国内外数据挖掘、机器学习领域中的大规模杀伤性武器。

更重要的是，XGBoost在系统优化和机器学习原理方面都进行了深入的考虑。毫不夸张的讲，XGBoost提供的可扩展性，可移植性与准确性推动了机器学习计算限制的上限，该系统在单台机器上运行速度比当时流行解决方案快十倍以上，甚至在分布式系统中可以处理十亿级的数据。

XGBoost的主要优点：
1. 简单易用。相对其他机器学习库，用户可以轻松使用XGBoost并获得相当不错的效果。
2. 高效可扩展。在处理大规模数据集时速度快效果好，对内存等硬件资源要求不高。
3. 鲁棒性强。相对于深度学习模型不需要精细调参便能取得接近的效果。
4. XGBoost内部实现提升树模型，可以自动处理缺失值。

XGBoost的主要缺点：
1. 相对于深度学习模型无法对时空位置建模，不能很好地捕获图像、语音、文本等高维数据。
2. 在拥有海量训练数据，并能找到合适的深度学习模型时，深度学习的精度可以遥遥领先XGBoost。

## XGBoost 的应用
XGBoost在机器学习与数据挖掘领域有着极为广泛的应用。据统计在2015年Kaggle平台上29个获奖方案中，17只队伍使用了XGBoost；在2015年KDD-Cup中，前十名的队伍均使用了XGBoost，且集成其他模型比不上调节XGBoost的参数所带来的提升。这些实实在在的例子都表明，XGBoost在各种问题上都可以取得非常好的效果。

同时，XGBoost还被成功应用在工业界与学术界的各种问题中。例如商店销售额预测、高能物理事件分类、web文本分类;用户行为预测、运动检测、广告点击率预测、恶意软件分类、灾害风险预测、在线课程退学率预测。虽然领域相关的数据分析和特性工程在这些解决方案中也发挥了重要作用，但学习者与实践者对XGBoost的一致选择表明了这一软件包的影响力与重要性。

# 实验室手册

## 学习目标
- 了解 XGBoost 的参数与相关知识
- 掌握 XGBoost 的Python调用并将其运用到天气数据集预测

## 代码流程
基于天气数据集的XGBoost分类实践
- Step1: 库函数导入 
- Step2: 数据读取/载入 
- Step3: 数据信息简单查看 
- Step4: 可视化描述 
- Step5: 对离散变量进行编码
- Step6: 利用 XGBoost 进行训练与预测
- Step7: 利用 XGBoost 进行特征选择
- Step8: 通过调整参数获得更好的效果

## 算法实战
**基于天气数据集的XGBoost分类实战** 
在实践的最开始，我们首先需要导入一些基础的函数库包括：
- numpy （Python进行科学计算的基础软件包）；
- pandas（pandas是一种快速，强大，灵活且易于使用的开源数据分析和处理工具）；
- matplotlib和seaborn绘图。

```python
#导入需要用到的数据集
!wget https://tianchi-media.oss-cn-beijing.aliyuncs.com/DSW/7XGBoost/train.csv

############### print ##########
--2020-08-22 17:18:54--  https://tianchi-media.oss-cn-beijing.aliyuncs.com/DSW/7XGBoost/train.csv
Resolving tianchi-media.oss-cn-beijing.aliyuncs.com (tianchi-media.oss-cn-beijing.aliyuncs.com)... 47.95.85.21
Connecting to tianchi-media.oss-cn-beijing.aliyuncs.com (tianchi-media.oss-cn beijing.aliyuncs.com)|47.95.85.21|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 11476379 (11M) [text/csv]
Saving to: ‘train.csv’

100%[======================================>] 11,476,379  23.2MB/s   in 0.5s   

2020-08-22 17:18:55 (23.2 MB/s) - ‘train.csv’ saved [11476379/11476379]
```

### step1 函数库导入
```python
##  基础函数库
import numpy as np 
import pandas as pd

## 绘图函数库
import matplotlib.pyplot as plt
import seaborn as sns
```

本次我们选择天气数据集进行方法的尝试训练，现在有一些由气象站提供的每日降雨数据，我们需要根据历史降雨数据来预测明天会下雨的概率。样例涉及到的测试集数据test.csv与train.csv的格式完全相同，但其RainTomorrow未给出，为预测变量。  

数据的各个特征描述如下：  
| 特征名称      | 意义            | 取值范围 |
|---------------|-----------------|----------|
| Date          | 日期            | 字符串   |
| Location      | 气象站的地址    | 字符串   |
| MinTemp       | 最低温度        | 实数     |
| MaxTemp       | 最高温度        | 实数     |
| Rainfall      | 降雨量          | 实数     |
| Evaporation   | 蒸发量          | 实数     |
| Sunshine      | 光照时间        | 实数     |
| WindGustDir   | 最强的风的方向  | 字符串   |
| WindGustSpeed | 最强的风的速度  | 实数     |
| WindDir9am    | 早上9点的风向   | 字符串   |
| WindDir3pm    | 下午3点的风向   | 字符串   |
| WindSpeed9am  | 早上9点的风速   | 实数     |
| WindSpeed3pm  | 下午3点的风速   | 实数     |
| Humidity9am   | 早上9点的湿度   | 实数     |
| Humidity3pm   | 下午3点的湿度   | 实数     |
| Pressure9am   | 早上9点的大气压 | 实数     |
| Pressure3pm   | 早上3点的大气压 | 实数     |
| Cloud9am      | 早上9点的云指数 | 实数     |
| Cloud3pm      | 早上3点的云指数 | 实数     |
| Temp9am       | 早上9点的温度   | 实数     |
| Temp3pm       | 早上3点的温度   | 实数     |
| RainToday     | 今天是否下雨    | No，Yes  |
| RainTomorrow  | 明天是否下雨    | No，Yes  |

### step2 数据读取/载入
```python
## 我们利用Pandas自带的read_csv函数读取并转化为DataFrame格式
data = pd.read_csv('train.csv')
```

### step3 数据信息查看
```python
## 利用.info()查看数据的整体信息
data.info()

################## print ##################
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 106644 entries, 0 to 106643
Data columns (total 23 columns):
Date             106644 non-null object
Location         106644 non-null object
MinTemp          106183 non-null float64
MaxTemp          106413 non-null float64
Rainfall         105610 non-null float64
Evaporation      60974 non-null float64
Sunshine         55718 non-null float64
WindGustDir      99660 non-null object
WindGustSpeed    99702 non-null float64
WindDir9am       99166 non-null object
WindDir3pm       103788 non-null object
WindSpeed9am     105643 non-null float64
WindSpeed3pm     104653 non-null float64
Humidity9am      105327 non-null float64
Humidity3pm      103932 non-null float64
Pressure9am      96107 non-null float64
Pressure3pm      96123 non-null float64
Cloud9am         66303 non-null float64
Cloud3pm         63691 non-null float64
Temp9am          105983 non-null float64
Temp3pm          104599 non-null float64
RainToday        105610 non-null object
RainTomorrow     106644 non-null object
dtypes: float64(16), object(7)
memory usage: 18.7+ MB
```

```python
data.head()
```
![image](https://user-images.githubusercontent.com/44680953/143262024-2a229ca8-632a-45d5-8059-8c3ee50b3a1c.png)
![image](https://user-images.githubusercontent.com/44680953/143262171-ebd2429c-4bd2-4ab1-93af-57ee1ba69bfa.png)  
5 rows × 23 columns

这里我们发现数据集中存在NaN，一般的我们认为NaN在数据集中代表了缺失值，可能是数据采集或处理时产生的一种错误。这里我们采用-1将缺失值进行填补，还有其他例如“中位数填补、平均数填补”的缺失值处理方法有兴趣的同学也可以尝试。
```python
data = data.fillna(-1)
```

```python
data.tail()
```
![image](https://user-images.githubusercontent.com/44680953/143262584-5fecf65f-9dcf-4b66-82c4-898696339447.png)
![image](https://user-images.githubusercontent.com/44680953/143262658-5736d7df-9363-4232-8dfb-d57952b18c43.png)  

```python
## 利用value_counts函数查看训练集标签的数量
pd.Series(data['RainTomorrow']).value_counts()

# No     82786
# Yes    23858
# Name: RainTomorrow, dtype: int64
```
我们发现数据集中的负样本数量远大于正样本数量，这种常见的问题叫做“数据不平衡”问题，在某些情况下需要进行一些特殊处理。  

```python
## 对于特征进行一些统计描述
data.describe()
```
![image](https://user-images.githubusercontent.com/44680953/143262968-80c94425-c3fd-4b7a-93a3-4c2ad4ddc7d8.png)
![image](https://user-images.githubusercontent.com/44680953/143263019-f6d42569-7a7f-42b8-89d1-3bc1f0d3fecb.png)  


### step4 可视化描述
为了方便，我们先纪录数字特征与非数字特征：  
```python
numerical_features = [x for x in data.columns if data[x].dtype == np.float]

category_features = [x for x in data.columns if data[x].dtype != np.float and x != 'RainTomorrow']

## 选取三个特征与标签组合的散点可视化
sns.pairplot(data=data[['Rainfall',
'Evaporation',
'Sunshine'] + ['RainTomorrow']], diag_kind='hist', hue= 'RainTomorrow')
plt.show()
```
![image](https://user-images.githubusercontent.com/44680953/143263369-ed5e57d5-060f-4679-aa8b-e053a218cd30.png)  
从上图可以发现，在2D情况下不同的特征组合对于第二天下雨与不下雨的散点分布，以及大概的区分能力。相对的Sunshine与其他特征的组合更具有区分能力。

```python
for col in data[numerical_features].columns:
    if col != 'RainTomorrow':
        sns.boxplot(x='RainTomorrow', y=col, saturation=0.5, palette='pastel', data=data)
        plt.title(col)
        plt.show()
```
![image](https://user-images.githubusercontent.com/44680953/143263683-9cd5c5f5-590a-4b34-b01d-5e29d008dde6.png)


## 重要知识点
