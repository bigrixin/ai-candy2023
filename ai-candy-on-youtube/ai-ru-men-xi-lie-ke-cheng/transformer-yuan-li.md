---
description: <<Attention Is All You Need>> paper study
---

# Transformer 原理

Trasnformer 可以理解为是基于自注意力机制self-attention的一个深度学习模型。\
Trasnformer 包含了两个主要组成：Encoder 和 Decoder。 (序列模型)

* Encoder里边有6个小编码器，每一个的小编码器的输入是前一个小编码器的输出。
* Decoders里边也有6个小解码器。每一个小解码器的输入，不光是它的前一个解码器的输出，还包括了整个编码部分的输出。

1. <mark style="color:purple;">**Encoder**</mark>

Encoder 的结构是一个**自注意力机制**加上一个**前馈神经网络**。

**自注意力机制 self-attention**：是自己和自己计算一遍注意力。 输入为一个向量，再乘以三个矩阵，得到三个新的向量。如下图：输入向量为X, 乘以 WQ，WK，WV矩阵，就会分别得到新的权重矩阵 Q，K，V。

(Q: Query, K: key, V: value。K 和 V长度相同)

<figure><img src="../../.gitbook/assets/self-attention-1.png" alt=""><figcaption></figcaption></figure>

**前馈神经网络（feedforward neural network，FNN）**: 包含了输入层 Input layer，隐藏层 Hidden layer，和输出层 Output layer。整个网络中无反馈，信号从输入层向输出层单向传播。前馈网络是一种静态非线性映射．通过简单非线性处理单元的复合映射，可获得复杂的非线性处理能力。

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

**Attention:** 将新向量内容分别与K1, K2, Kt 向量进来点积运算，得到相似度（值越大，相似度越高）。再除以根号dk。再将结果进行softmax运算，即：将分数标准化, 归一化运算。(得到大约0，小于1的数，总和为1的权重).

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

最后，将上述结果分别与V1，V2，Vt 向量进来点积运算，再将结果加起来，得到一个权重矩阵Z。\


<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption><p>计算图</p></figcaption></figure>



多头注意力机制，顾名思义，包含多个自注意力机制，然后将多个自注意力机制的输出进行拼接，最后通过全连接层得到输出。

输入X, 通过上述方法，得到多组Q/K/V权重矩阵，

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

然后按照上节描述的那样，得到多个Z。

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

对多个注意力机制的输出Z进行拼接, 就得到了self-attention层的输出。

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

最后, 再乘以一个矩阵，得到一个前馈神经网络层的输入。

**总结一下：** Encoder是对输入进行编码，使用的是自注意力机制+前馈神经网络的结构。\


2. <mark style="color:purple;">**Decoder**</mark>

**Decoder中使用的也是同样的结构。** 不同的地方在于，进行过自注意力机制后，将self-attention的输出再与Decoders模块的输出计算一遍注意力机制得分， 之后，再进入前馈神经网络模块。

解码器输出本来是一个浮点型的向量。如何实现将“机器学习”翻译成“machine learing”。

最后， 接上一个softmax变成一个线性层。 使用全连接神经网络，它将解码器产生的向量投影到一个更高维度的向量（logits）上。 之后的softmax层将这些分数转换为概率。选择概率最大的维度， 并对应地生成与之关联的单词作为此时间步的输出就是最终的输出啦！

但是，上述方法不含有顺序信息。**为了实现Transformer的顺序信息，**在每个输入词向量加上一个有顺序特征的向量（Positional Encoding)， 研究发现sin和cos函数能够很好的表达这种特征。

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption><p>The Transformer - model architecture</p></figcaption></figure>

***

Reference: \
[https://papers.nips.cc/paper\_files/paper/2017/file/3f5ee243547dee91fbd053c1c4a845aa-Paper.pdf](https://papers.nips.cc/paper\_files/paper/2017/file/3f5ee243547dee91fbd053c1c4a845aa-Paper.pdf)\
[https://mp.weixin.qq.com/s/slOlE8fe91jZBMYhAV9Z7A\
https://mp.weixin.qq.com/s?\_\_biz=Mzg3MjY1MzExMA==\&mid=2247514134\&idx=1\&sn=2efa12ff4b0c5211a2bdc13596cc44b9\&chksm=cfb6f9ebd0f6539bc5bbcdd305f27b47a3546100e444b43ac2a8e78c8e48a280aea55d61bcc2\&scene=132\&exptype=timeline\_recommend\_article\_extendread\_samebiz\&show\_related\_article=1\&subscene=0\&poc\_token=HC0s7mWjrZK5vW9esQvZFLfHgXa8Sqph7wWBf-Wb\
https://blog.csdn.net/keyue123/article/details/89209888](https://mp.weixin.qq.com/s/slOlE8fe91jZBMYhAV9Z7Ahttps:/mp.weixin.qq.com/s?\_\_biz=Mzg3MjY1MzExMA==\&mid=2247514134\&idx=1\&sn=2efa12ff4b0c5211a2bdc13596cc44b9\&chksm=cfb6f9ebd0f6539bc5bbcdd305f27b47a3546100e444b43ac2a8e78c8e48a280aea55d61bcc2\&scene=132\&exptype=timeline\_recommend\_article\_extendread\_samebiz\&show\_related\_article=1\&subscene=0\&poc\_token=HC0s7mWjrZK5vW9esQvZFLfHgXa8Sqph7wWBf-Wbhttps://blog.csdn.net/keyue123/article/details/89209888)
