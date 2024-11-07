# 平稳时间序列 Part 1



## 时间序列的数字特征

时间序列指的是同一个体在不同时点上的数据。对于离散时间$\{1，2，…，T\}$，记随机变量y 的相应观测值为$\{y_1，y_2，…，y_T\}$，并假设其为（严格）平稳过程。因此，该时间序列的期望、方差、 自协方差、自相关系数等数字特征均不随时间推移而改变。



### k 阶自协方差

$\gamma_{k}\equiv\mathrm{Cov}( y_{t} ,y_{t+k} ) =\mathrm{E}\left[ ( y_{t} -\mu ) ( y_{t+k} -\mu ) \right]$。

它反映的是同一变量$y$相隔k期之间的自相关（受之前数据点影响）程度。对$\gamma_k$的估计值$\hat{\gamma}_k$为样本自协方差。



### k 阶自相关系数

$\begin{equation}
    \rho_k\equiv\mathrm{Corr}( y_t ,y_{t+k} )\equiv\frac{\mathrm{Cov}( y_t ,y_{t+k} )}{\mathrm{Var}( y_t )}
\end{equation}$

它将自协方差标准化为介于[-1，1]之间的量。显然，对于严格平稳过程，$\rho_k$不依赖于时间，而仅仅是滞后阶数k的函数，故被称为“自相关函数”（Autocorrelation Function，简记ACF）。



### k 阶偏自相关系数

$\rho_k^*\equiv\mathrm{Corr}(y_t,y_{t+k}\mid y_{t+1},\cdots,y_{t+k-1})$

$\rho_k$不依赖于时间，而仅仅是滞后阶数k的函数，故被称为“偏自相关函数”（Partially Autocorrelation Function，简记PACF）。

ACF 和 PACF 用于检验时间序列平稳性。ACF 考虑的是时间序列与其滞后值的整体相关性，而 PACF 则是剔除中间滞后值影响后的纯相关性。



### 平稳性检验

截尾：在大于某阶(k)后快速趋于0为k阶截尾，可简单理解为从某阶之后直接就变为0。

拖尾：始终有非零取值，不会在大于某阶后就快速趋近于0（而是在0附近波动），可简单理解为无论如何都不会为0，而是在某阶之后在0附近随机变化。

![左边截尾，右边拖尾](./20_1.png)

| 模型       | ACF 自相关图 | PACF 偏自相关图 |
| ---------- | ------------ | --------------- |
| AR(p)      | 拖尾         | p阶截尾         |
| MA(q)      | q阶截尾      | 拖尾            |
| ARMA(p,q)  | 拖尾         | 拖尾            |
| 模型不适合 | 截尾         | 截尾            |



## 自回归模型（AR）

从客户角度仅关心某变量（比如股价）的未来值，可用该变量的过去值来预测其未来值（因为时间序列一般存在自相关）。这种模型称为“单变量时间序列”(univariate time series)。此时可不必理会因果关系，只考虑相关关系即可。

一阶自回归 AR(1)：$y_{t}=\beta_{0}+\beta_{1}y_{t-1}+\varepsilon_{t}\quad( t=2 ,\cdots,T )$

$\varepsilon_t$为白噪声，即一个不存在自相关的序列，满足：

- $E(\varepsilon_t)=0$
- $\text{Var} \left( \varepsilon_{t} \right) =\sigma_{\varepsilon}^{2}$
- $\text{Cov} \left( \varepsilon_{t} ,\varepsilon_{s} \right) =0\quad t\neq s$

p 阶自回归 AR(p)：$y_{t}=\beta_{0}+\beta_{1}y_{t-1}+\cdots+\beta_{p}y_{t-p}+\varepsilon_{t}$

通常滞后期 p 是未知的，所以实践中需要估计 $\hat{p}$

1. 由小到大的序贯 t 准则。设一个最大滞后期 $p_{\max}$，然后令 $\hat{p}=p_\max$进行估计，并对最后一个滞后期系数的显著性进行 t 检验。如果接受该系数为 0，则令 $\hat{p}=p_{\max}-1$，重新进行估计，再对（新的）最后一个滞后期的系数进行t检验，如果显著，则停止；否则，令$\hat{p}=p_{\max}-2$；依此类推。
2. 信息准则：选择$\hat{p}$使得 AIC 或者 BIC 最小化，通常也要设定最大滞后期。

$\text{AIC} = 2k - 2\ln(L)$其中 (k) 是模型参数的数量，(L) 是模型的最大似然估计值。

$\text{BIC} = \ln(n)k - 2\ln(L)$其中 (n) 是样本大小，(k) 是模型参数的数量，(L) 是模型的最大似然估计值。

AIC 对模型复杂性的惩罚较轻，可能选择更复杂的模型；BIC 对模型复杂性的惩罚较重，尤其在样本量较大时，更倾向于选择简单模型。



## 移动平均模型（MA）

MA(1)：$y_t = \mu + \varepsilon_t + \theta \varepsilon_{t-1} $

- $\mu$是序列的平均值。
- $ \varepsilon_t$是白噪声项，通常假定为独立同分布的随机变量，均值为零，方差为 $\sigma^2$
- $\theta$ 是模型参数，衡量前一个时间步的随机误差对当前值的影响。
- $\varepsilon_{t-1}$ 是前一个时间步的随机误差。

MA(q)：$y_{t}=\mu+\varepsilon_{t}+\theta_{1}\varepsilon_{t-1}+\theta_{2}\varepsilon_{t-2}+\cdots+\theta_{q}\varepsilon_{t-q}$



## ARMA

ARMA 就是将 AR(p) 和 MA(q) 结合起来，得到 ARMA(p, q)

$y_{t}=\beta_{0}+\beta_{1}y_{t-1}+\cdots+\beta_{p}y_{t-p}+\varepsilon_{t}+\theta_{1}\varepsilon_{t-1}+\cdots+\theta_{q}\varepsilon_{t-q}$



### Stata 代码

```stata
tsset year
```

告诉 Stata 数据集是时间序列数据，并指定 `year` 变量作为时间变量设置数据的时间排序。此后就可以进行差分等操作。

```stata
 g d_logpe = d.logpe
```

`d.<变量名>` 将指定变量做一阶差分，存储到新的变量里。

```stata
corrgram d_logpe, lags(10)

ac d_logpe, lags(10) // 绘制 1-10 阶 ACF 图（自相关图）
pac d_logpe, lags(10) // 绘制 1-10 阶 ACF 图（偏自相关图）
```

计算差分后数据从第1阶到第10阶的自相关与偏自相关系数，找到 p 最小的阶数，估计 AR(4) 和 MA(4) 模型

``` stata
arima d_logpe, ar(1/4) nolog // 估计 AR(4) 模型

estat ic // 计算 AIC 和 BIC
```

ar(1/4) 表示包含了从滞后1期到滞后4期的自回归项，即模型中包含 t-1, t-2, t-3, t-4 这四个滞后期的值作为解释变量，nolog 表示不显示迭代日志。

```stata
predict e1, res // 计算残差存储到 e1
corrgram e1, lags(10)
```

如果 p 值全部都很大，就可以接受残差项无自相关的原假设。

同样步骤应用到 MA 模型

```stata
arima d_logpe, ma(1/4) nolog

estat ic

predict e2, res
corrgram e2, lags(10)
```



 
