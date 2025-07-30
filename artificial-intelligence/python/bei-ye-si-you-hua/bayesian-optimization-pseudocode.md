# Bayesian Optimization pseudocode



{% code fullWidth="true" %}
```
// 贝叶斯优化算法伪代码
算法：贝叶斯优化(Bayesian Optimization)
输入：目标函数 f(x)，初始样本集 D = {(x₁, f(x₁)), ..., (xₙ, f(xₙ))}，
      迭代次数 T，采集函数 α(x; D)，概率模型参数先验 p(w)
输出：最优解 x*

1: for t = 1 to T do
2:     根据贝叶斯公式计算参数后验:
         p(w|D) ∝ p(D|w)p(w)  // 其中p(D|w)是似然函数
3:     使用后验参数 p(w|D) 训练概率模型 M（如高斯过程）
4:     求解下一个采样点 xₜ₊₁ = argmax_x α(x; D, M)
5:     计算目标函数值 yₜ₊₁ = f(xₜ₊₁)
6:     更新样本集 D = D ∪ {(xₜ₊₁, yₜ₊₁)}
7: end for
8: 返回历史最优解 x* = argmin_{x∈D} f(x)
```
{% endcode %}

{% code fullWidth="true" %}
```
// 采集函数的数学表达
1. 期望改进(Expected Improvement, EI):
   α_EI(x; D) = E[max(f(x) - f(x⁺), 0)]
   其中 f(x⁺) = min{f(x₁), f(x₂), ..., f(xₙ)}
   
2. 置信上限(Upper Confidence Bound, UCB):
   α_UCB(x; D) = μ(x) + κ·σ(x)
   其中 κ 控制探索与利用的平衡
   
3. 概率改进(Probability of Improvement, PI):
   α_PI(x; D) = Φ((f(x⁺) - μ(x))/σ(x))
   其中 Φ 是标准正态分布的累积分布函数
```
{% endcode %}

{% code fullWidth="true" %}
```
// 贝叶斯推断核心步骤
p(w|D) = p(D|w)p(w)/p(D)
其中:
- p(w) 是参数 w 的先验分布
- p(D|w) 是给定参数 w 下数据 D 的似然函数
- p(D) 是数据 D 的边缘似然，作为归一化常数
- p(w|D) 是参数 w 的后验分布

// 高斯过程回归核心公式
给定训练数据 D = {(x₁, y₁), ..., (xₙ, yₙ)}
预测点 x* 的分布为:
μ(x*) = K(x*, X)[K(X, X) + σ²I]⁻¹y
σ²(x*) = K(x*, x*) - K(x*, X)[K(X, X) + σ²I]⁻¹K(X, x*)
其中 K 是核函数，X 是训练输入，y 是训练输出
```
{% endcode %}
