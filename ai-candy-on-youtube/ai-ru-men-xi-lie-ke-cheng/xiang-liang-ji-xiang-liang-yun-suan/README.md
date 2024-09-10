# 0️ 向量及向量运算

向量的基本运算：

* 相加（向量之和），向量相应坐标相加产生的新的向量为向量之和。

```
 两个向量之和可以看成是： 向量A(0->a) + 向量B(a->c) = 向量C(0->c)
    第一个向量的开始到尾 向量A(0->a)
    第二个向量的开始到尾 向量B(a->c)
    向量和为：第一向量开始到第二向量尾  新方向 向量C(0->c)
```

* 相减（向量之差），向量相应坐标相减产生的新向量为向量之差，由减数指向被减数。

```
 两个向量之差可以看成是： 向量A(0->a) - 向量B(0->b) = 向量C(b->a)
    第一个向量的开始到尾 向量A(0->a)
    第二个向量的开始到尾  向量B(0->b)
    向量差为：第二向量尾到第一向量尾 向量C(0->c)
```

1. 标量乘法（scalar），向量的各坐标乘以相应的标量(scalar)后产生的新向量，方向不变。
2. 长度(magnitude)，各坐标平方后求和再开平方所得的值即为向量的长度。
3. 方向(direction)，可以被同一方向上的单位向量表示，寻找单位向量的过程称为标准化。
4. 点积(dot product)，两个向量相对应坐标的乘积和，为一个数值。
5. 平行向量，一个向量可以由另一个向量进行标量乘法得到，则互为平行向量。
6. 正交向量，两个向量的点积为0则互为正交向量。
7.

<mark style="color:purple;">**向量的点乘**</mark>

也叫向量的点积、内积、数量积，对两个向量执行点乘运算，(对这两个向量对应位一一相乘之后求和的操作)， 点乘的结果是一个标量。 向量a和向量b 的点乘公式：要求一维向量a和向量b的行列数相同。

<figure><img src="../../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

**点乘几何意义**：可以用来表征或计算两个向量之间的夹角，以及在b向量在a向量方向上的投影，有公式：

<figure><img src="../../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

向量的点乘:a \* b 公式：a \* b = |a| \* |b| \* cosθ 点乘又叫向量的内积、数量积，是一个向量和它在另一个向量上的投影的长度的乘积；是标量。 点乘反映着两个向量的“相似度”，两个向量越“相似”，它们的点乘越大。

<figure><img src="../../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

根据这个公式就可以计算向量a和向量b之间的夹角。从而就可以进一步判断这两个向量是否是同一方向， 是否正交(也就是垂直)等方向关系，&#x20;

*   具体对应关系为：\
    a·b>0 方向基本相同，夹角在0°到90°之间&#x20;

    a·b=0 正交，相互垂直 &#x20;

    a·b<0 方向基本相反，夹角在90°到180°之间



<mark style="color:purple;">**向量的叉乘**</mark>

又叫向量积、外积、叉积，叉乘的运算结果是一个向量而不是一个标量。 并且两个向量的叉积与这两个向量组成的坐标平面垂直。

向量的叉乘： a ∧ b = |a| \* |b| \* sinθ&#x20;

向量积被定义为：&#x20;

模长：在这里θ表示两向量之间的夹角(共起点的前提下)（0° ≤ θ ≤ 180°），它位于这两个矢量所定义的平面上。&#x20;

方向：a向量与b向量的向量积的方向与这两个向量所在平面垂直，且遵守右手定则。（一个简单的确定满足“右手定则”的结果向量的方向的方法是这样 标系是满足右手定则的，当右手的四指从a以不超过180度的转角转向b时，竖起的大拇指指向是c的方向。c = a ∧ b）

<figure><img src="../../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

**叉乘几何意义**：在三维几何中，向量a和向量b的叉乘结果是一个向量，更为熟知的叫法是法向量，该向量垂直于a和b向量构成的平面。在3D图像学中，叉乘的概念非常有用，可以通过两个向量的叉乘，生成第三个垂直于a，b的法向量，从而构建X、Y、Z坐标系。如下图所示：

<figure><img src="../../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>





reference: \
[https://blog.csdn.net/qq\_27161673/article/details/53056999\
https://blog.csdn.net/dcrmg/article/details/52416832\
https://www.cnblogs.com/zhixingzhong/p/7512565.html](https://blog.csdn.net/qq\_27161673/article/details/53056999https:/blog.csdn.net/dcrmg/article/details/52416832https:/www.cnblogs.com/zhixingzhong/p/7512565.html)

\
