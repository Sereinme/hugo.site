# 线性反馈移位寄存器🛅


<!-- Emotive Ballad - Guthrie Govan -->
{{< music server="netease" type="song" id="1416264" autoplay="true" >}}

{{< admonition type=note title="线性反馈移位寄存器" >}}
Linear Feedback Shift Register, LFSR, 作为线性驱动部分来生成伪随机码，是数学意义上的最优解，自相关函数除主峰外具有最大平坦。
{{< /admonition >}}



## LFSR引入

若干个寄存器排列成一行，每个寄存器存储着一个二进制数。移位寄存器每次将最右端(末端)的数字输出，然后整体向右移动移位。同时具有一个反馈函数，以寄存器中已有的某些序列作为反馈函数的输入，将反馈函数的输出填充到移位寄存器的最左端，这样的，拥有反馈函数的移位寄存器称为反馈移位寄存器。

## 反馈函数

<a id="eq1"></a>
LFSR的反馈函数就是简单地对移位寄存器中的某些位进行异或，并将异或的结果填充到LFSR的最左端。对于LFSR中每一位的数据，可以参与异或，也可以不参与异或。其中，我们把参与异或的位称为抽头。


$$
b_{n+1} = c_1b_1 \oplus c_2b_2 \oplus \ldots \oplus c_nb_n \tag{1}
$$


## 级数

LFSR 的级数就是其中寄存器的个数，一个 $n$ 级的 LFSR 最多可以存储 $2^n-1$ 种状态，因为全零的状态反馈值始终是全零。至于为什么最多能够遍历 $2^n-1$ 种状态，将在之后介绍。一个 $n$ 级的 LFSR 的最大周期就是 $2^n-1$ ，我们将周期为 $2^n-1$ 的 LFSR 所生成的序列称为m 序列。


## 特征多项式

由 <a href="#eq1">(1)</a> 中的式子可以得到一个 LFSR 的递推关系式，则这个递推关系可以对应这一个特征多项式

$$
f(x) = c_nx^n + c_{n-1} + \ldots + c_2x^2 + c_1x + 1\tag{2}
$$

这里的 $c_i$ 与之前的定义相同，表示是否抽头。最终得到的特征多项式只保留抽头位次项，并加一。可以理解为一个 LFSR 的不同特征多项式对应着不同的反馈函数。

## 本原多项式

首先给出一个结论

{{< admonition type=info >}}
$n$ 级 LFSR 能够产生 $m$ 序列的充要条件为其特征多项式为本原多项式
{{< /admonition >}}

接下来以此引入**不可约多项式**和**本原多项式**。

{{< admonition type=note title="不可约多项式" >}}
Irreducible Polynomial. 一个多项式在指定域上不能分解，则称为不可约多项式，即不能等于次数更小的多项式的乘积。
{{< /admonition >}}

LFSR 中采用的有限域为 $GF(2^n)$ ，指多项式的系数要小于 2，即为 0 或 1，多项式的最高次数不能够大于或等于 n。如 $GF(2^3)$ 中的多项式为 $0, 1, x, x+1$ 。

{{< admonition typte=note title="本原多项式" >}}
Primitive Polynomial. 本原多项式本身就是一种不可约多项式，但是需要满足以下三个条件：对于 $n$ 次的本原多项式 $F(x)$。
{{< /admonition >}}

   1. $F(x)$ 是不可约的，不能再分解多项式
   2. $F(x)$ 可整除 $x^p+1$ ，其中 $p = 2^n-1$
   3. $F(x)$ 不能整除 $x^q+1$ ，其中 $q<p$ 

由本原多项式可以得到生成 $m$ 序列的反馈函数，如 $n=10$ 时本原多项式为

{{< math >}}
$$
F(x) = x^{10} + x^3 + 1\tag{3}
$$
{{< /math >}}

则反馈函数就表示为 `[0,0,1,0,0,0,0,0,0,1]` ，意味着第 3 位寄存器和第 10 位寄存器中的值做异或得到的输出赋值给最左端的寄存器，实现移位反馈。

{{< admonition type=tip >}}
MATLAB 中 `primpoly` 函数可以查看任意次数的本原多项式
{{< /admonition >}}

## $m$ 序列的性质

$m$ 序列实际上就是我们称呼的伪随机码或者扩频码。

### 均衡性


在 $m$ 序列的一个周期中，0 和 1 的数目基本相等，1 的个数会比 0 多一个。

### 移位相加性


一个 $m$ 序列 $m_1$ 与其经过任意延迟移位产生的另一序列 $m_2$ 模 2 相加(异或)，得到的仍是 $m_1$ 的某次延迟移位序列 $m_3$。

### 自相关性


由于 1 的个数始终比 0 多一个，因此在做自相关的过程中，始终会得到一个 -1 的值，这与完全的随机信号有着很大的区别。

{{< math >}}
$$
R(\tau) = \left\{
\begin{align*}
    &1, &\tau = 0\\
    &-\frac{1}{N}, &\mathrm{else}
\end{align*}\right.\tag{4}
$$
{{< /math >}}

### 游程分布


$m$ 序列中取值相同的相继元素合称为一个“游程”，游程中元素的个数称为游程长度。n 位 LFSR 生成的 $m$ 序列中，总共由 $2^{n-1}$ 个游程，其中长度为 1 的游程占总游程数的 1/2，长度为 2 游程占总游程数的 1/4，长度为 k 的游程占总游程数的 $2^{-k}$ ，且长度为 k 的游程中，连 0 和连 1 的游程数各占一半。游程分布具有很好的均衡性。

如序列 `1000010010110011111000110111010` 中，游程总数为 $2^{5-1}=16$ ，各个长度的游程分布如下：

- 长度为 1 的游程数目为 8，其中 4 个 1 游程和 4 个 0 游程
- 长度为 2 的游程数目为 4，其中 2 个 11 游程和 2 个 00 游程
- 长度为 3 的游程数目为 2，其中 1 个 111 游程和 1 个 000 游程
- 长度为 4 的连 0 游程数目为 1
- 长度为 5 的连 1 游程数目为 1


## 附录

由于时间原因，之前有关本原多项式的生成和证明有所省略，未来有时间可以在其他文献中进行查阅

- [Generation of primitive binary polynomial](./generation.pdf)
- [LFSR AND PRIMITIVE POLYNOMIAL](./lfsr.pdf)