---
title: 记初次Kaggle比赛
date: 2023-1-7
featured: false
draft: false
comment: true
toc: true
reward: false
pinned: false
carousel: false
math: true
---



## Kaggle-API的使用

> Kaggle官网提供了kaggle-api用于方便在命令行下下载比赛、数据集中的内容，提交比赛结果等操作
>
> 安装与使用说明地址：[https://github.com/Kaggle/kaggle-api](https://github.com/Kaggle/kaggle-api)

### pip安装

```shell
pip install kaggle
```

### 配置kaggle.json

登录Kaggle，进入`https://www.kaggle.com/<username>/account`，点击`Create API Token`，则会自动下载`kaggle.json`文件，然后将文件放入`.kaggle/kaggle.json`即可。

文件内容其实就是`username`和`key`：

```json
{
	username: "",
	key: ""
}
```

现在就可以在命令行下使用kaggle的命令，可以使用`kaggle -v`验证是否安装成功。



## 比赛记录

> 第一次参加，所以先选了个正在进行的，、比较简单的比赛：Kaggle's Playground Series，根据介绍，这个系列会有很多小比赛，目前是最新一届的第一个，该系列的目的是为了提供轻量级的比赛，帮助学习和提高机器学习和数据科学技能。



### 下载比赛数据

```
kaggle competitions download -c playground-series-s3e1
```

> 根据数据描述，比赛的数据均是由基于加州房价数据集训练的模型生成的数据。

### 数据读取

```python
pd.read_csv("./train.csv")
```

### 数据分析

简单的看了下有没有空值：

```python
np.any(train_df.isna())
```

由于本来就是生成的数据，所以并不存在空值，然后给的特征也都是数值类型的，所以这里我也没做什么额外的处理，特征提取之类的。

### 拆分训练集与验证集

```python
from sklearn.modelselection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, shuffle=True, random_state=42)
```

### 模型训练

先使用了决策树，来随便跑了个模型试一下。

```python
from sklearn.tree import DecisionTreeRegressor

regressor = DecisionTreeRegressor(random_state=42)
regressor.fit(X_train, y_train)
pred_y = regressor.predict(X_test)
```

### 模型评估

```python
from sklearn.metrics import mean_squared_error

mean_squared_error(y_test, pred_y)
```

### 提交数据

> 可以使用kaggle提高的api命令提交，也可以直接在官网的比赛提交界面提交文件。

```
kaggle competitions submit -c playground-series-s3e1 -f submission.csv -m "Message"
```



