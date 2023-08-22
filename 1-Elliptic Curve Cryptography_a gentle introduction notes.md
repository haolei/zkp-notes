# Elliptic Curve Cryptography: a gentle introduction 注释及推导
原文 [《Elliptic Curve Cryptography: a gentle introduction》](https://andrea.corbellini.name/2015/05/17/elliptic-curve-cryptography-a-gentle-introduction/).
attachment [pdf](pdf/elliptic-curve-cryptography-a-gentle-introduction.pdf)


**椭圆曲线定义**
$$\{ (x, y) \in \mathbb{R}^2\ |\ y^2 = x^3 + ax + b,\ 4 a^3 + 27 b^2 \ne 0 \}\ \cup\ \{ 0 \}$$

*椭圆曲线群性质*
* the elements of the group are the points of an elliptic curve;
* the identity element is the point at infinity 0;
* the inverse of a point $P$ is the one symmetric about the $x$-axis;
* addition is given by the following rule: given three aligned, non-zero points $P$, $Q$ and $R$, their sum is $P + Q + R = 0$

## [Geometric addition](https://andrea.corbellini.name/2015/05/17/elliptic-curve-cryptography-a-gentle-introduction/#geometric-addition)
所代表几何含义：$P$, $Q$, $R$在同一直线

$$P + Q = -R$$

![P + Q = -R](https://andrea.corbellini.name/images/point-addition.png)

## [Algebraic addition](https://andrea.corbellini.name/2015/05/17/elliptic-curve-cryptography-a-gentle-introduction/#algebraic-addition)
**$P$ ，$Q$ 不相等时：**

if $P$ and $Q$ are distinct($x_P \neq x_Q$) the line through them has **slope**
$$m = \frac{y_P - y_Q}{x_P - x_Q}$$

The intersection of this line with the elliptic curve is a third point $R = (x_R, y_R)$

$$\begin{align*}
    x_R & = m^2 - x_P - x_Q & (1^*)\\
    y_R & = y_P + m(x_R - x_P)
\end{align*}$$

*推导（$1^*$）*
$$
(m(x_R-)
$$

**$P$ ，$Q$ 相等时：**


## Reference

 1. [椭圆曲线上的相同点相加与相异点相加的推导计算](https://blog.csdn.net/guyongqiangx/article/details/121793398)