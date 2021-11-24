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
![image](https://user-images.githubusercontent.com/44680953/143263883-9db57d30-c980-4ec0-86a0-f13826a8f6ad.png)
![image](https://user-images.githubusercontent.com/44680953/143263899-20fcd8e2-991c-469f-8e25-7ecec7cc343c.png)
![image](https://user-images.githubusercontent.com/44680953/143263926-4b1aa717-9332-418a-af06-e38195e7a487.png)
![image](https://user-images.githubusercontent.com/44680953/143263943-2ecc2836-4f04-443a-bbac-8851d4161795.png)
![image](https://user-images.githubusercontent.com/44680953/143263951-617d9704-f14d-4edc-8450-782de44db4c9.png)
![image](https://user-images.githubusercontent.com/44680953/143263970-0c91fd8f-2fd9-4a68-8d2b-8ec7d1f682d5.png)
![image](https://user-images.githubusercontent.com/44680953/143263980-7faf0cb3-cc88-4eea-b01c-3d70a2dfc0e9.png)
![image](https://user-images.githubusercontent.com/44680953/143263989-d14f99a6-0a9c-4b59-a02d-e2b9eac7885e.png)
![image](https://user-images.githubusercontent.com/44680953/143264006-c119ef24-c589-4b74-b3f7-e03ec8ba199f.png)
![image](https://user-images.githubusercontent.com/44680953/143264024-a7f7423d-92cc-4418-94fd-c03d56e480aa.png)
![image](https://user-images.githubusercontent.com/44680953/143264031-29934ce4-54f7-4ebf-98ce-02651695354c.png)
![image](https://user-images.githubusercontent.com/44680953/143264047-579b2a7c-541a-47e2-bce9-03744ef43a34.png)
![image](https://user-images.githubusercontent.com/44680953/143264068-355a7547-2992-40e9-b997-e7198af680d7.png)
![image](https://user-images.githubusercontent.com/44680953/143264085-2b6e53c0-ca3c-4cdd-9468-87107aa9a0bd.png)
![image](https://user-images.githubusercontent.com/44680953/143264092-6d8896f0-24b0-4885-ab3b-7d5b83017c37.png)  

利用箱型图我们也可以得到不同类别在不同特征上的分布差异情况。我们可以发现Sunshine,Humidity3pm,Cloud9am,Cloud3pm的区分能力较强。

```python
tlog = {}
for i in category_features:
    tlog[i] = data[data['RainTomorrow'] == 'Yes'][i].value_counts()
flog = {}
for i in category_features:
    flog[i] = data[data['RainTomorrow'] == 'No'][i].value_counts()

plt.figure(figsize=(10,10))
plt.subplot(1,2,1)
plt.title('RainTomorrow')
sns.barplot(x = pd.DataFrame(tlog['Location']).sort_index()['Location'], y = pd.DataFrame(tlog['Location']).sort_index().index, color = "red")
plt.subplot(1,2,2)
plt.title('Not RainTomorrow')
sns.barplot(x = pd.DataFrame(flog['Location']).sort_index()['Location'], y = pd.DataFrame(flog['Location']).sort_index().index, color = "blue")
plt.show()
```
![image](https://user-images.githubusercontent.com/44680953/143264423-3b80900c-0059-4813-907c-dd3d87d8537c.png)  
从上图可以发现不同地区降雨情况差别很大，有些地方明显更容易降雨。

```python
plt.figure(figsize=(10,2))
plt.subplot(1,2,1)
plt.title('RainTomorrow')
sns.barplot(x = pd.DataFrame(tlog['RainToday'][:2]).sort_index()['RainToday'], y = pd.DataFrame(tlog['RainToday'][:2]).sort_index().index, color = "red")
plt.subplot(1,2,2)
plt.title('Not RainTomorrow')
sns.barplot(x = pd.DataFrame(flog['RainToday'][:2]).sort_index()['RainToday'], y = pd.DataFrame(flog['RainToday'][:2]).sort_index().index, color = "blue")
plt.show()
```
![image](https://user-images.githubusercontent.com/44680953/143264559-ac0274be-deda-451a-8479-5cf8d7fe9f93.png)  
上图我们可以发现，今天下雨明天不一定下雨，但今天不下雨，第二天大概率也不下雨。

### step5 对离散变量进行编码
由于XGBoost无法处理字符串类型的数据，我们需要一些方法将字符串数据转化为数据。一种最简单的方法是把所有的相同类别的特征编码成同一个值，例如女=0，男=1，狗狗=2，所以最后编码的特征值是在$[0, 特征数量-1]$之间的整数。除此之外，还有**独热编码**、**求和编码**、**留一法编码**等等方法可以获得更好的效果。  
```python
## 把所有的相同类别的特征编码为同一个值
def get_mapfunction(x):
    mapp = dict(zip(x.unique().tolist(),
         range(len(x.unique().tolist()))))
    def mapfunction(y):
        if y in mapp:
            return mapp[y]
        else:
            return -1
    return mapfunction
for i in category_features:
    data[i] = data[i].apply(get_mapfunction(data[i]))
    
## 编码后的字符串特征变成了数字
data['Location'].unique()

# array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16,
#        17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33,
#        34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48])
```

### step6 利用 XGBoost 进行训练与预测
```python
## 为了正确评估模型性能，将数据划分为训练集和测试集，并在训练集上训练模型，在测试集上验证模型性能。
from sklearn.model_selection import train_test_split

## 选择其类别为0和1的样本 （不包括类别为2的样本）
data_target_part = data['RainTomorrow']
data_features_part = data[[x for x in data.columns if x != 'RainTomorrow']]

## 测试集大小为20%， 80%/20%分
x_train, x_test, y_train, y_test = train_test_split(data_features_part, data_target_part, test_size = 0.2, random_state = 2020)
```

```python
## 导入XGBoost模型
from xgboost.sklearn import XGBClassifier

## 定义 XGBoost模型 
clf = XGBClassifier()

# 在训练集上训练XGBoost模型
clf.fit(x_train, y_train)

# XGBClassifier()
```

```python
## 在训练集和测试集上分布利用训练好的模型进行预测
train_predict = clf.predict(x_train)
test_predict = clf.predict(x_test)
from sklearn import metrics

## 利用accuracy（准确度）【预测正确的样本数目占总预测样本数目的比例】评估模型效果
print('The accuracy of the Logistic Regression is:',metrics.accuracy_score(y_train,train_predict))
print('The accuracy of the Logistic Regression is:',metrics.accuracy_score(y_test,test_predict))

## 查看混淆矩阵 (预测值和真实值的各类情况统计矩阵)
confusion_matrix_result = metrics.confusion_matrix(test_predict,y_test)
print('The confusion matrix result:\n',confusion_matrix_result)

# The accuracy of the Logistic Regression is: 0.8517142354802789
# The accuracy of the Logistic Regression is: 0.8469689155609733
# The confusion matrix result:
#  [[15759  2470]
#  [  794  2306]]
```

```python
# 利用热力图对于结果进行可视化
plt.figure(figsize=(8, 6))
sns.heatmap(confusion_matrix_result, annot=True, cmap='Blues')
plt.xlabel('Predicted labels')
plt.ylabel('True labels')
plt.show()
```
![image](https://user-images.githubusercontent.com/44680953/143265585-c63310e3-1c96-46ba-8b85-86eb4be00a48.png)   
我们可以发现共有15759 + 2306个样本预测正确，2470 + 794个样本预测错误。

### step7 利用 XGBoost 进行特征选择
XGBoost的特征选择属于特征选择中的嵌入式方法，在XGboost中可以用属性feature_importances_去查看特征的重要度。
```shell
? sns.barplot

############# output
Signature:
 sns.barplot(
    x=None,
    y=None,
    hue=None,
    data=None,
    order=None,
    hue_order=None,
    estimator=<function mean at 0x7f5030185950>,
    ci=95,
    n_boot=1000,
    units=None,
    seed=None,
    orient=None,
    color=None,
    palette=None,
    saturation=0.75,
    errcolor='.26',
    errwidth=None,
    capsize=None,
    dodge=True,
    ax=None,
    **kwargs,
)
Docstring:
Show point estimates and confidence intervals as rectangular bars.

A bar plot represents an estimate of central tendency for a numeric
variable with the height of each rectangle and provides some indication of
the uncertainty around that estimate using error bars. Bar plots include 0
in the quantitative axis range, and they are a good choice when 0 is a
meaningful value for the quantitative variable, and you want to make
comparisons against it.

For datasets where 0 is not a meaningful value, a point plot will allow you
to focus on differences between levels of one or more categorical
variables.

It is also important to keep in mind that a bar plot shows only the mean
(or other estimator) value, but in many cases it may be more informative to
show the distribution of values at each level of the categorical variables.
In that case, other approaches such as a box or violin plot may be more
appropriate.


Input data can be passed in a variety of formats, including:

- Vectors of data represented as lists, numpy arrays, or pandas Series
  objects passed directly to the ``x``, ``y``, and/or ``hue`` parameters.
- A "long-form" DataFrame, in which case the ``x``, ``y``, and ``hue``
  variables will determine how the data are plotted.
- A "wide-form" DataFrame, such that each numeric column will be plotted.
- An array or list of vectors.

In most cases, it is possible to use numpy or Python objects, but pandas
objects are preferable because the associated names will be used to
annotate the axes. Additionally, you can use Categorical types for the
grouping variables to control the order of plot elements.    

This function always treats one of the variables as categorical and
draws data at ordinal positions (0, 1, ... n) on the relevant axis, even
when the data has a numeric or date type.

See the :ref:`tutorial <categorical_tutorial>` for more information.    

Parameters
----------
x, y, hue : names of variables in ``data`` or vector data, optional
    Inputs for plotting long-form data. See examples for interpretation.        
data : DataFrame, array, or list of arrays, optional
    Dataset for plotting. If ``x`` and ``y`` are absent, this is
    interpreted as wide-form. Otherwise it is expected to be long-form.    
order, hue_order : lists of strings, optional
    Order to plot the categorical levels in, otherwise the levels are
    inferred from the data objects.        
estimator : callable that maps vector -> scalar, optional
    Statistical function to estimate within each categorical bin.
ci : float or "sd" or None, optional
    Size of confidence intervals to draw around estimated values.  If
    "sd", skip bootstrapping and draw the standard deviation of the
    observations. If ``None``, no bootstrapping will be performed, and
    error bars will not be drawn.
n_boot : int, optional
    Number of bootstrap iterations to use when computing confidence
    intervals.
units : name of variable in ``data`` or vector data, optional
    Identifier of sampling units, which will be used to perform a
    multilevel bootstrap and account for repeated measures design.
seed : int, numpy.random.Generator, or numpy.random.RandomState, optional
    Seed or random number generator for reproducible bootstrapping.    
orient : "v" | "h", optional
    Orientation of the plot (vertical or horizontal). This is usually
    inferred from the dtype of the input variables, but can be used to
    specify when the "categorical" variable is a numeric or when plotting
    wide-form data.    
color : matplotlib color, optional
    Color for all of the elements, or seed for a gradient palette.    
palette : palette name, list, or dict, optional
    Colors to use for the different levels of the ``hue`` variable. Should
    be something that can be interpreted by :func:`color_palette`, or a
    dictionary mapping hue levels to matplotlib colors.    
saturation : float, optional
    Proportion of the original saturation to draw colors at. Large patches
    often look better with slightly desaturated colors, but set this to
    ``1`` if you want the plot colors to perfectly match the input color
    spec.    
errcolor : matplotlib color
    Color for the lines that represent the confidence interval.
errwidth : float, optional
    Thickness of error bar lines (and caps).         
capsize : float, optional
    Width of the "caps" on error bars.

dodge : bool, optional
    When hue nesting is used, whether elements should be shifted along the
    categorical axis.    
ax : matplotlib Axes, optional
    Axes object to draw the plot onto, otherwise uses the current Axes.    
kwargs : key, value mappings
    Other keyword arguments are passed through to
    :meth:`matplotlib.axes.Axes.bar`.

Returns
-------
ax : matplotlib Axes
    Returns the Axes object with the plot drawn onto it.    

See Also
--------
countplot : Show the counts of observations in each categorical bin.    
pointplot : Show point estimates and confidence intervals using scatterplot
            glyphs.    
catplot : Combine a categorical plot with a :class:`FacetGrid`.    

Examples
--------

Draw a set of vertical bar plots grouped by a categorical variable:

.. plot::
    :context: close-figs

    >>> import seaborn as sns
    >>> sns.set(style="whitegrid")
    >>> tips = sns.load_dataset("tips")
    >>> ax = sns.barplot(x="day", y="total_bill", data=tips)

Draw a set of vertical bars with nested grouping by a two variables:

.. plot::
    :context: close-figs

    >>> ax = sns.barplot(x="day", y="total_bill", hue="sex", data=tips)

Draw a set of horizontal bars:

.. plot::
    :context: close-figs

    >>> ax = sns.barplot(x="tip", y="day", data=tips)

Control bar order by passing an explicit order:

.. plot::
    :context: close-figs

    >>> ax = sns.barplot(x="time", y="tip", data=tips,
    ...                  order=["Dinner", "Lunch"])

Use median as the estimate of central tendency:

.. plot::
    :context: close-figs

    >>> from numpy import median
    >>> ax = sns.barplot(x="day", y="tip", data=tips, estimator=median)

Show the standard error of the mean with the error bars:

.. plot::
    :context: close-figs

    >>> ax = sns.barplot(x="day", y="tip", data=tips, ci=68)

Show standard deviation of observations instead of a confidence interval:

.. plot::
    :context: close-figs

    >>> ax = sns.barplot(x="day", y="tip", data=tips, ci="sd")

Add "caps" to the error bars:

.. plot::
    :context: close-figs

    >>> ax = sns.barplot(x="day", y="tip", data=tips, capsize=.2)

Use a different color palette for the bars:

.. plot::
    :context: close-figs

    >>> ax = sns.barplot("size", y="total_bill", data=tips,
    ...                  palette="Blues_d")

Use ``hue`` without changing bar position or width:

.. plot::
    :context: close-figs

    >>> tips["weekend"] = tips["day"].isin(["Sat", "Sun"])
    >>> ax = sns.barplot(x="day", y="total_bill", hue="weekend",
    ...                  data=tips, dodge=False)

Plot all bars in a single color:

.. plot::
    :context: close-figs

    >>> ax = sns.barplot("size", y="total_bill", data=tips,
    ...                  color="salmon", saturation=.5)

Use :meth:`matplotlib.axes.Axes.bar` parameters to control the style.

.. plot::
    :context: close-figs

    >>> ax = sns.barplot("day", "total_bill", data=tips,
    ...                  linewidth=2.5, facecolor=(1, 1, 1, 0),
    ...                  errcolor=".2", edgecolor=".2")

Use :func:`catplot` to combine a :func:`barplot` and a :class:`FacetGrid`.
This allows grouping within additional categorical variables. Using
:func:`catplot` is safer than using :class:`FacetGrid` directly, as it
ensures synchronization of variable order across facets:

.. plot::
    :context: close-figs

    >>> g = sns.catplot(x="sex", y="total_bill",
    ...                 hue="smoker", col="time",
    ...                 data=tips, kind="bar",
    ...                 height=4, aspect=.7);
File:      /opt/conda/lib/python3.6/site-packages/seaborn/categorical.py
Type:      function
```



## 重要知识点
