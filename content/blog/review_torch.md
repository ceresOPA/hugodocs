---
title: "Pytorch常用操作"
date: 2022-11-27
draft: false
toc: true
featured : true
tags:
  - deep learning
categories: ["学习"]
---

介绍了从torch的数据类型，基本操作，到如何使用torch进行模型构建、训练、测试的过程，算是写了个基本的模板，方便随时记忆。

<!--more-->

## 一、数据类型

> Pytorch中的数据都是Tensor类型的，即张量，是向量、矩阵的扩展

### 1. 几种创建Tensor变量的方法

```python
torch.randn((3,2)) # 服从高斯分布的随机数randn
torch.randn(3,2)
# 上面两种都是一样的，会生成3行2列的默认数据类型为float32的Tensor (pytorch默认float32，是为了节省资源)
torch.randn((3,2),dtype=torch.float64)
```

下面给出了所有的dtype，但是要注意的是像上面的randn是指定不了dtype为torch.int的(会报错`RuntimeError: "normal_kernel_cpu" not implemented for 'Int'`)，这是因为要符合高斯分布的原因，想要这样做可以选择torch.randint方法。解决方案可参考：[RuntimeError: “normal_kernel_cpu” not implemented for ‘Byte’](https://discuss.pytorch.org/t/runtimeerror-normal-kernel-cpu-not-implemented-for-byte/139962)

```
========================== ===========================================   ===========================
Data type                  dtype                                         Legacy Constructors
========================== ===========================================   ===========================
32-bit floating point      ``torch.float32`` or ``torch.float``          ``torch.*.FloatTensor``
64-bit floating point      ``torch.float64`` or ``torch.double``         ``torch.*.DoubleTensor``
64-bit complex             ``torch.complex64`` or ``torch.cfloat``
128-bit complex            ``torch.complex128`` or ``torch.cdouble``
16-bit floating point [1]_ ``torch.float16`` or ``torch.half``           ``torch.*.HalfTensor``
16-bit floating point [2]_ ``torch.bfloat16``                            ``torch.*.BFloat16Tensor``
8-bit integer (unsigned)   ``torch.uint8``                               ``torch.*.ByteTensor``
8-bit integer (signed)     ``torch.int8``                                ``torch.*.CharTensor``
16-bit integer (signed)    ``torch.int16`` or ``torch.short``            ``torch.*.ShortTensor``
32-bit integer (signed)    ``torch.int32`` or ``torch.int``              ``torch.*.IntTensor``
64-bit integer (signed)    ``torch.int64`` or ``torch.long``             ``torch.*.LongTensor``
Boolean                    ``torch.bool``                                ``torch.*.BoolTensor``
========================== ===========================================   ===========================
```

除了randn常用的还有：

```python
torch.zeros((3,2))
torch.ones((3,2))
torch.Tensor([[2.0,3.2],[3,2]])
torch.from_numpy(a) # a = np.random.randn((2,3)) 将numpy的narray类型转换为tensor类型
torch.arange(6).reshape(2,3)
```

### 2. 更改已创建变量的dtype

```python
a = torch.randint((3,2))
a = a.to(torch.float32) # to这个方法还挺常用的，包括可以给数据指定cpu还是gpu a.to("cuda:0")
a = a.int() #其实a.int()就是调用了a.to(torch.int32)
```

## 二、torch中的基本运算

```python
"""
对于相同大小维度的张量相加减乘的时候，就是对应位运算了，但也是可以利用传播机制，张量可以与标量进行运算
或者只要有一维相同就可以实现运算,下面假设a和b都是shape大小为(2,3)的tensor,且dtype相同。
"""
a+b 
a-b
a*b #对应位相乘 等价于a.mul(b)
a.pow(2)
torch.mm(a,b.reshape(3,2))# 矩阵相乘，n*m m*p -> n*p 这里需注意的是两张量的dtype需完全一致，即使是int32和int64也会报错
```

## 三、常用的张量维度变换方法

- 减少或增加一维

```python
a = torch.randn((1,3,2,1))
a = a.squeeze() # a.shape (3,2) squeeze会将维度大小为1的维度干掉
a = a.unsqueeze(2) # a.shape (3,2,1) unsqueeze(d)指定增加第d维，最小为第0维
```

- 转置

```python
a = torch.randn((3,2))
a = a.T # a.shape (2,3) 最好只用于2维，高于二维目前会warning，在未来版本中会报错
a = a.transpose(0,1) # 交换第0和第1维，可以通过a.transpose_(0,1)直接改变a变量而不用赋值(在pytorch中很多操作都可以通过加入一个_来实现原地修改)，在2-D的tensor下，等价于a.T，在高维下就是交换两个指定的维度
```

- 交换维度

```python
"""
可以感觉transpose方法显得有些鸡肋，所以使用permute会更为方便
"""
a = torch.arange(12).reshape(2,3,2)
a = a.permute(1,0,2) 
# 交换了第0和第1维，permute需要把所有维度列出来，其中的参数代表的就是原来的维度，重新排列就可以同时交换多维
# 上面的a.permute(1,0,2)可以等价于a.transpose(0,1)
```

## 四、梯度计算(自动微分)

> 可以说，torch很重要的一点就是可以自动微分了，这也是后续神经网络进行反向传播更新参数的关键所在

举一个简单的例子：

求$y=\sum_{i=0}^{2}x^{2}_{i}$对$x_i$在$x_0=2,x_1=3,x_2=4$点的微分$\frac{\partial y}{\partial x_{i}}|_{x_0=2,x_1=3,x_2=4}$

```python
"""
x [2.,3.,4.]
requires_grad 允许自动微分
dtype torch.float32 指定为浮点型
Only Tensors of floating point and complex dtype can require gradients
只有浮点型和复数类型可以允许求梯度
"""
x = torch.arange(2,5,dtype=torch.float32,requires_grad=True)
y = x.pow(2).sum() # 可以用y.grad_fn查看反向传播的函数
y.backward() # 反向传播，求梯度
x.grad # [4.,6.,8.]
```

## 五、使用Pytorch搭建神经网络并训练

### 1. 使用Dataset构建数据集

```python
from torch.utils.data import Dataset

class myDataset(Dataset):
	super(myDataset,self).__init__()
	def __init__(self):
        # 读取数据X_data, y
		self.X_data = X_data
        self.y = y
    
    def __getitem__(self,idx):
        return X_data[idx],y[idx]
    
    def __len__(self):
        return len(self.y)
```

### 2. 使用nn.Module搭建神经网络结构

```python
import torch.nn as nn
import torch

class myNetwork(nn.Module):
    def __init__(self):
        super(myNetwork,self).__init__()
        # 这里使用nn.Sequential只是定义网络的一种方式，也可以单独拆开每个层，分别定义
        self.fc = nn.Sequential(  
            nn.Linear(3,6),
            nn.Sigmoid(),
            nn.Linear(6,1)
        )
    
    def forward(self,x):
        x = self.fc(x)

        return x
```

### 3. 训练模型

```python
from torch.utils.data import DataLoader,random_split
import torch.nn as nn
import torch

from mydataset import myDataset
from mynetwork import myNetwork

#定义超参数
n_epochs = 60 #迭代次数,每个epoch会对整个训练集遍历一遍
batch_size = 16 #一次加载的数据量，对一个epoch中的样本数的拆分
learning_rate = 0.001 #学习率，或者说步长

#加载数据
train_data = myDataset(is_train=True)
test_dataset = myDataset(is_train=False)
#按8:2划分训练集和验证集
train_size = int(len(train_data)*0.8)
validate_size = len(train_data) - train_size
train_dataset,validate_dataset = random_split(train_data,[train_size,validate_size])
#使用DataLoader加载数据集，转换为迭代器
train_dataloader = DataLoader(train_dataset,batch_size=batch_size,shuffle=True)
validate_dataloader = DataLoader(validate_dataset,batch_size=batch_size) #用于训练中验证模型效果，进而可以动态调整超参数，控制训练
test_dataloader = DataLoader(test_dataset,batch_size=batch_size)

#设置设备
device = "cuda" if torch.cuda.is_available() else "cpu"

#加载模型
model = myNetWork()
model.to(device)

#设置优化器
optimizer = torch.optim.SGD(model.parameters(),lr=learning_rate)

#设置损失函数
criterion = nn.MSEloss()

#训练模型
for epoch_idx in range(n_epochs):
    model.train()
    train_loss = 0.0
    for batch_idx,(X,y) in enumerate(train_dataloader):
        optimizer.zero_grad()
        X,y = X.to(device),y.to(device)
        pred = model(y)
        loss = criterion(pred,y)
        train_loss += loss.item()
        loss.backward()
        optimizer.step()
 	
    #验证模型
    model.eval()
    validate_loss = 0.0
    with torch.no_grad(): #不计算梯度，加快运算速度
        for batch_idx,(X,y) in enumerate(validate_dataloader):
            X,y = X.to(device),y.to(device)
            pred = model(y)
            loss = criterion(pred,y)
            validate_loss += loss.item()
    
    
    print(f"[epoch:{epoch_idx+1}/{epochs}] || train_loss:{train_loss:.2f} || validate_loss:{validate_loss:.2f}")
        
```

## 六、模型保存

- 只保存模型参数

```python
#保存
torch.save(the_model.state_dict(), PATH)
#读取
the_model = TheModelClass(*args, **kwargs)
the_model.load_state_dict(torch.load(PATH))
```

- 保存整个模型

```python
#保存
torch.save(the_model, PATH)
#读取
the_model = torch.load(PATH)
```