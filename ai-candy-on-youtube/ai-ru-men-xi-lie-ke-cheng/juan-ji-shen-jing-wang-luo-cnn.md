# 卷积神经网络（CNN）

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

<figure><img src="../../.gitbook/assets/CNN-3.gif" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/CNN-2.gif" alt=""><figcaption></figcaption></figure>
