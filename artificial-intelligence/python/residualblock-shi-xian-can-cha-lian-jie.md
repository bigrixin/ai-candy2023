# ResidualBlock 实现残差连接

ResidualBlock 的具体作用:

* 缓解梯度消失：通过直接将输入信息传递到输出，残差连接为梯度提供了额外的传播路径，使网络能够更有效地学习。&#x20;
* 促进特征复用：残差块允许网络保留输入中的重要特征，同时学习增量变化，有助于保留低级特征。
* &#x20;提高训练稳定性：实验表明，带有残差连接的网络通常比没有残差连接的网络更容易训练，并且在相同深度下表现更好

{% code fullWidth="true" %}
```
import torch
import torch.nn as nn

# 你的ResidualBlock实现
class ResidualBlock(nn.Module):
    def __init__(self, in_channels, out_channels, stride=1):
        super(ResidualBlock, self).__init__()
        self.conv1 = nn.Conv2d(in_channels, out_channels, kernel_size=3, stride=stride, padding=1, bias=False)
        self.bn1 = nn.BatchNorm2d(out_channels)
        self.relu = nn.ReLU(inplace=True)
        self.conv2 = nn.Conv2d(out_channels, out_channels, kernel_size=3, stride=1, padding=1, bias=False)
        self.bn2 = nn.BatchNorm2d(out_channels)

        # Handle cases of dimension mismatch
        self.shortcut = nn.Sequential()
        if stride != 1 or in_channels != out_channels:
            self.shortcut = nn.Sequential(
                nn.Conv2d(in_channels, out_channels, kernel_size=1, stride=stride, bias=False),
                nn.BatchNorm2d(out_channels)
            )

    def forward(self, x):
        out = self.relu(self.bn1(self.conv1(x)))
        out = self.bn2(self.conv2(out))
        out += self.shortcut(x)
        out = self.relu(out)
        return out

# 测试用例
def test_residual_block():
    # 测试用输入 (batch_size, channels, height, width)
    x = torch.randn(1, 64, 32, 32)
    
    # 测试1: 通道数不变，步长为1
    block1 = ResidualBlock(64, 64, stride=1)
    out1 = block1(x)
    print(f"测试1: 输入尺寸={x.shape}, 输出尺寸={out1.shape}")
    
    # 测试2: 通道数增加，步长为1
    block2 = ResidualBlock(64, 128, stride=1)
    out2 = block2(x)
    print(f"测试2: 输入尺寸={x.shape}, 输出尺寸={out2.shape}")
    
    # 测试3: 通道数不变，步长为2 (空间尺寸减半)
    block3 = ResidualBlock(64, 64, stride=2)
    out3 = block3(x)
    print(f"测试3: 输入尺寸={x.shape}, 输出尺寸={out3.shape}")
    
    # 测试4: 通道数增加，步长为2 (空间尺寸减半)
    block4 = ResidualBlock(64, 128, stride=2)
    out4 = block4(x)
    print(f"测试4: 输入尺寸={x.shape}, 输出尺寸={out4.shape}")

test_residual_block()
```
{% endcode %}

测试1: 输入尺寸=torch.Size(\[1, 64, 32, 32]), 输出尺寸=torch.Size(\[1, 64, 32, 32])\
测试2: 输入尺寸=torch.Size(\[1, 64, 32, 32]), 输出尺寸=torch.Size(\[1, 128, 32, 32])\
测试3: 输入尺寸=torch.Size(\[1, 64, 32, 32]), 输出尺寸=torch.Size(\[1, 64, 16, 16])\
测试4: 输入尺寸=torch.Size(\[1, 64, 32, 32]), 输出尺寸=torch.Size(\[1, 128, 16, 16])
