# 🥇 Deep learning notes

逻辑回归：

1. 线性函数 𝑦^ = 𝑤𝑇𝑥 + 𝑏&#x20;

```
// Some code
𝑦^ = 𝑤𝑇𝑥 + 𝑏。

y^: 实际值 𝑦 的估计,等于 1 的一种可能性或者是机会.
wT: 特征权重
𝑥 ：N𝑥 维特征向量
```

&#x20; 2\. 激活函数：转换线性到非线性 （由于线性值可能超过1或小于1）\


<figure><img src="https://n.sinaimg.cn/spider2021224/658/w875h583/20210224/0b99-kkmphps7571844.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://www.gstatic.com/education/formulas2/472522532/en/sigmoid_function.svg" alt=""><figcaption></figcaption></figure>

ReLU 激活函数

<figure><img src="https://n.sinaimg.cn/spider2021224/138/w600h338/20210224/0781-kkmphps7572767.png" alt=""><figcaption></figcaption></figure>



```
Sigmoid 函数 𝜎(𝑧) = 1/(1+𝑒^−𝑧)

𝑧：表示 𝑤𝑇𝑥 + 𝑏 的值
```

&#x20;  3\. 损失函数 :  来衡量预测输出值和实际值有多接近\
\
&#x20;      Loss function: L(y^,y) = -𝑦log(𝑦^) − (1 − 𝑦)log(1 − 𝑦^)

&#x20;

