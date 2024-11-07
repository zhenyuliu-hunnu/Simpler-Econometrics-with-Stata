# 平稳时间序列 Part 2



## 误差修正模型

考虑最简单的 AR(1) 模型：$y_{t}=\beta_{0}+\beta_{1}y_{t-1}+\varepsilon_{t}$

两边求期望：$E\left( y_{t} \right) =E\left( \beta_{0} +\beta_{1} y_{t-1}+\varepsilon_{t} \right) =\beta_{0} +\beta_{1} E\left( y_{t-1} \right) +0$

并令长期均衡值：$y^{\ast}=E\left( y_{t} \right) =E\left( y_{t-1} \right)$

得：$y^{*} =\frac{\beta_{0}}{1 -\beta_{1}}$

重新代入方程，并在两边同时减去$y_{t-1}$

$$\Delta y_{t}=\underbrace{\left(\begin{array}{c}{1-\beta_{1}}\\\end{array}\right)\left(\begin{array}{c}{y^{*}-y_{t-1}}\\\end{array}\right)}_{\text{error correction}}+\varepsilon_{t}$$

即为 AR(1) 的误差修正模型，将$\Delta y_t$表达为对长期均衡（即$y^{\ast}-y_{t-1}$ ）偏离的误差修正。



## MA(∞) 与滞后算子

**MA(∞) 模型**: 自相关系数永不为 0 的移动平均模型，表达式为： $$ y_{t}=\mu+\sum_{j=0}^{\infty}\theta_{j}\varepsilon_{t-j} $$ 其中 $\theta_{0}=1$。

> 如果一个时间序列的自相关系数永远不为 0，这意味着该序列存在长期依赖性，即当前值受到过去很长时间内冲击的影响。

**脉冲响应函数 (IRF)** 是用于刻画时间序列模型中某个变量对另一个变量冲击的动态反应的工具。它衡量的是当某个变量在某一期受到一个单位冲击时（其他变量和其他期的冲击均不变），对另一个变量在未来不同时期的影响。

**累积脉冲响应函数 (CIRF)：** 考察的是经过 k 期后的累积效应。$C I R F(k)=\sum_{j=0}^{k}\frac{\partial y_{\varepsilon+j}}{\partial\varepsilon_{\varepsilon}}$ 



