# 2️ 卷积神经网络（CNN）

<figure><img src="../../.gitbook/assets/CNN-1.gif" alt=""><figcaption></figcaption></figure>

{% code fullWidth="true" %}
```
import torch.nn as nn

Class Net (nn.Module):

    def __init__(self):
        super().__init__()
        self.conv1=nn.Conv2d(1,6,3)      ## 输入1通道，输出6通道，卷积核3*3的的一个卷积层
        self.conv2=nn.Conv2d(6,16,5)     ## 输入6通道，输出16通道，卷积核3*3的的一个卷积层
 
    def forward(self, x):
        x=F.max_pool2d(F.relu(self.conv1(x)),2)
        x=F.max_pool2d(F.relu(self.conv2(x)),2)
        return x
```
{% endcode %}



### 卷积过程：如下图，输入为5x5 蓝色矩阵， 卷积核（权重黄色矩阵）为3x3。                      将卷积核矩阵对应到输入矩阵左上角3x3的区域（感受野）。

<figure><img src="../../.gitbook/assets/CNN-3.gif" alt=""><figcaption></figcaption></figure>

> 将卷积核矩阵中的每个值分别与输入矩阵内的值相乘，再叠加，最后得出1个值，作为输出值。\
> 移动卷积核1步（步长），再按上述方法，点积求和，作为第二个值，以此类推。
>
> 步长为1，卷积后的矩阵边长尺寸：输入矩阵边长-卷积核边长+1   （5-3+1）

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption><p>卷积核</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/CNN-2.gif" alt=""><figcaption><p>点积求和</p></figcaption></figure>

### 池化层：降低维度（参数）
