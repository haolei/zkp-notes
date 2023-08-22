# Elliptic Curve Cryptography: finite fields and discrete logarithms 注释

原文 [《Elliptic Curve Cryptography: finite fields and discrete logarithms》](https://andrea.corbellini.name/2015/05/23/elliptic-curve-cryptography-finite-fields-and-discrete-logarithms/).
attachment [pdf](pdf/elliptic-curve-cryptography-finite-fields-and-discrete-logarithms.pdf)

## The order of an elliptic curve group  椭圆曲线的阶
Firstly, let’s say that the number of points in a group is called the **order of the group**.
快速算法 [Schoof’s algorithm](https://en.wikipedia.org/wiki/Schoof%27s_algorithm)


## cyclic subgroup 循环子群
$$nP + mP = \underbrace{P + \cdots + P}_{n\ \text{times}} + \underbrace{P + \cdots + P}_{m\ \text{times}} = (n + m)P$$
the set of the multiples of is a **cyclic subgroup** of the group formed by the elliptic curve.

The multiples of $P=(3,5)$ are just five distinct points $(0,P,2P,3P,4P)$ and they are repeating cyclically. It's easy to spot the similarity between scalar multiplication on elliptic curves and addition in modular arithmetic.
![The multiples of P=(3,5) are just five distinct points (0,P,2P,3P,4P) and they are repeating cyclically. It's easy to spot the similarity between scalar multiplication on elliptic curves and addition in modular arithmetic.](https://andrea.corbellini.name/images/cyclic-subgroup.png)

A “subgroup” is a group which is a subset of another group. A “cyclic subgroup” is a subgroup which elements are repeating cyclically, like we have shown in the previous example. The point $P$ is called **generator** or **base point** of the cyclic subgroup.

**P被称为base point**
## Subgroup order 子群的阶

* the order of $P$ is the smallest positive integer $n$ such that $nP = 0$.
* The order of $P$ is linked to the order of the elliptic curve by **Lagrange’s theorem**, which states that the order of a subgroup is a divisor of the order of the parent group. In other words, if an elliptic curve contains $N$ points and one of its **subgroups contains $n$ points**, **then $n$ is a divisor of $N$**.
* 上图中子群的阶$n=5$
* $P$被称为base point

寻找子群的方法：
* 计算椭圆曲线的阶$N$
* 找到$N$所有的除数$n$
* 对于每一个除数$n$,计算$nP$
* 找到最小的$n$,令$nP=0$，就是子群的阶

## Finding a base point
目的是找到合适的base point
需要尽可能大的子群的阶
整体思路为：选择一个看起来足够好阶，然后计算base point

we need to introduce one more term. Lagrange’s theorem implies that the number $h = N / n$ is always an integer (because $n$ is a divisor of $N$). The number $h$ has a name: it’s the **cofactor of the subgroup**.

由于$NP = 0$，又$h = N / n$ 因此 $N = nh$
$$(nh)P = 0$$ 从而
$$n(hP) = 0$$
这里定义
$$G = hP$$ 产生了**阶为$n$的子群。** 非常重要，因为  **$nG=0$** 符合循环子群的特性。 
具体算法如下：
* 计算椭圆曲线的阶$N$
* 选择阶为n的子群，其中n为素数
* 计算协因子$h = N / n$
* 选择随机数$P$
* 计算$G = hP$
* 如果$G$等于0，回到第四部，重新选择随机数，否则就得到了$h,n$


# Elliptic Curve Cryptography: ECDH and ECDSA
原文 [《Elliptic Curve Cryptography: ECDH and ECDSA》](https://andrea.corbellini.name/2015/05/30/elliptic-curve-cryptography-ecdh-and-ecdsa/).
attachment [pdf](pdf/elliptic-curve-cryptography-ecdh-and-ecdsa.pdf)
## Domain parameters
Our elliptic curve algorithms will work in a cyclic subgroup of an elliptic curve over a finite field. Therefore, our algorithms will need the following parameters:

* The **prime** $p$ that specifies the size of the finite field.
* The **coefficients** **$a$** and **$b$** of the elliptic curve equation.
* The **base point** $G$ that generates our subgroup.
* The **order** **$n$** of the subgroup.
* The cofactor **$h$** of the subgroup.

In conclusion, the **domain parameters **for our algorithms are the **sextuple** $(p, a, b, G, n, h)$.

以上domain完整定**一个有限域椭圆曲线的循环子群**

$$\begin{array}{rcl}
  \left\{(x, y) \in (\mathbb{F}_p)^2 \right. & \left. | \right. & \left. y^2 \equiv x^3 + ax + b \pmod{p}, \right. \\
  & & \left. 4a^3 + 27b^2 \not\equiv 0 \pmod{p}\right\}\ \cup\ \left\{0\right\}
\end{array}$$

## Elliptic Curve Cryptography
* The **private key** is a random integer $d$ chosen from $\{1, \dots, n - 1\}$ (where $n$ is the order of the subgroup). 随机取一个d作为私钥
* The **public key** is the point $H$ (where $H = dG$ is the base point of the subgroup). 公钥通过基点生成

If we know $d$ and $G$ (along with the other domain parameters), finding $H$ is “easy”. But if we know 
$H$ and $G$, finding the private key $d$ is “hard”, because it requires us to solve the discrete logarithm problem. 

**回归到离散log问题，想要求d是很困难的，也是公私钥机制的本质**

## Encryption with ECDH

ECDH is a variant of the Diffie-Hellman algorithm for elliptic curves. It is actually a **key-agreement protocol**, more than an encryption algorithm

ECDH解决的问题：共享私钥

* Alice and Bob分别生成他们自己的公私钥： $H_A = d_AG$, $H_B = d_BG$
* Alice 和 Bob交换自己的公钥 $H_A$ 和 $H_B$
* $S = d_A H_B = d_A (d_B G) = d_B (d_A G) = d_B H_A$

共享私钥即为 $S$

## Playing with ECDH
[ecdhe.py](https://github.com/andreacorbellini/ecc/blob/master/scripts/ecdhe.py)


```
for the public key point (0x4034127647bb7fdab7f1526c7d10be8b28174e2bba35b06ffd8a26fc2c20134a, 0x9e773199edc1ea792b150270ea3317689286c9fe239dd5b9c5cfd9e81b4b632), how to represent the pair as one hex?
```
```
To represent an elliptic curve public key point (x, y) as a single hex value, you can concatenate the x and y values into a single byte string.

For example, with the secp256k1 public key point:

x = 0x4034127647bb7fdab7f1526c7d10be8b28174e2bba35b06ffd8a26fc2c20134a

y = 0x9e773199edc1ea792b150270ea3317689286c9fe239dd5b9c5cfd9e81b4b632

To combine these into a single hex string:

Convert each value to a 32-byte raw byte string:
x_bytes = 0x4034127647bb7fdab7f1526c7d10be8b28174e2bba35b06ffd8a26fc2c20134a

y_bytes = 0x9e773199edc1ea792b150270ea3317689286c9fe239dd5b9c5cfd9e81b4b632

Concatenate the byte strings together:
point_bytes = x_bytes + y_bytes

Convert the resulting 64-byte string to hex:
point_hex = point_bytes.hex()

The final single hex string representing the point is:

0x4034127647bb7fdab7f1526c7d10be8b28174e2bba35b06ffd8a26fc2c20134a9e773199edc1ea792b150270ea3317689286c9fe239dd5b9c5cfd9e81b4b632

So in summary, concatenate the raw byte values of x and y, and convert the result to hex to get a single hex string for the public key point.
```

## Signing with ECDSA
Alice wants to **sign** a message with her private key ($d_A$), and Bob wants to **validate** the signature using Alice’s public key ($H_A$)

The truncated hash is an integer and will be denoted as $z$

* 随机选正数 $k$ in {$1,...,n-1$} ($n$是子群的阶)
* 计算点$P=kG$
* 计算 $r = x_P \bmod n$
* 如果 $r=0$ 重新计算$r$
* 计算 $s = k^{-1} (z + rd_A) \bmod{n}$
* 如果 $s = 0$， 重新选择$k$

## Verifying signatures
* Calculate the integer $u_1 = s^{-1} z \bmod{n}$.
* Calculate the integer $u_2 = s^{-1} r \bmod{n}$.
* Calculate the point $P = u_1 G + u_2 H_A$.

The signature is valid only if $r = x_P \bmod{n}$

## 证明

$$\begin{align*}
  P & = u_1 G + u_2 H_A \\
    & = u_1 G + u_2 d_A G \\
    & = (u_1 + u_2 d_A) G
\end{align*}$$
带入$u1,u2$
$$\begin{align*}
  P & = (u_1 + u_2 d_A) G \\
    & = (s^{-1} z + s^{-1} r d_A) G \\
    & = s^{-1} (z + r d_A) G
\end{align*}$$

由于 $s = k^{-1} (z + rd_A) \bmod{n}$, 因此 $k = s^{-1} (z + rd_A) \bmod{n}$

从而
$$\begin{align*}
  P & = s^{-1} (z + r d_A) G \\
    & = k G
\end{align*}$$
## Playing with ECDSA
[ecdsa.py](https://github.com/andreacorbellini/ecc/blob/master/scripts/ecdsa.py)

## The importance of k
When generating ECDSA signatures, it is important to keep the secret $k$ really secret. If we used the same $k$ for all signatures, or if our random number generator were somewhat predictable, **an attacker would be able to find out the private key!**