# 激子与激发态的多体理论 —— 从二次量子化到 GW-BSE 与 TDDFT

## 目录

- [激子与激发态的多体理论 —— 从二次量子化到 GW-BSE 与 TDDFT](#激子与激发态的多体理论--从二次量子化到-gw-bse-与-tddft)
  - [目录](#目录)
  - [什么是激子](#什么是激子)
  - [单粒子表象](#单粒子表象)
  - [多粒子表象](#多粒子表象)
    - [一次量子化](#一次量子化)
    - [二次量子化](#二次量子化)
      - [产生与湮灭算符](#产生与湮灭算符)
        - [位置算符与动量算符](#位置算符与动量算符)
        - [一维谐振子](#一维谐振子)
        - [产生湮灭算符的性质及物理意义](#产生湮灭算符的性质及物理意义)
      - [占据数表象的矩阵表示](#占据数表象的矩阵表示)
      - [场算符](#场算符)
        - [场算符的性质及物理意义](#场算符的性质及物理意义)
  - [激子模型](#激子模型)
    - [相互作用的二次量子化](#相互作用的二次量子化)
    - [双带近似](#双带近似)
    - [双电子到电子-空穴对](#双电子到电子-空穴对)
    - [激子哈密顿量和激子态](#激子哈密顿量和激子态)
    - [Wannier 方程](#wannier-方程)
  - [GW + BSE](#gw--bse)
    - [GW 近似](#gw-近似)
      - [单粒子格林函数](#单粒子格林函数)
      - [Dyson 方程](#dyson-方程)
      - [双粒子格林函数](#双粒子格林函数)
      - [Hedin 方程组](#hedin-方程组)
        - [Schwinger 导数](#schwinger-导数)
        - [逆介电函数和顶点函数](#逆介电函数和顶点函数)
        - [库仑屏蔽和极化率](#库仑屏蔽和极化率)
        - [封闭循环与 $G\_0W\_0$](#封闭循环与-g_0w_0)
    - [Bethe–Salpeter 方程](#bethesalpeter-方程)
      - [BSE 哈密顿量](#bse-哈密顿量)
      - [BSE 激子波函数](#bse-激子波函数)
  - [含时密度矩阵形式](#含时密度矩阵形式)
    - [场算符展开](#场算符展开)
    - [密度矩阵的时间演化](#密度矩阵的时间演化)
  - [RT-TDDFT 与 LR-TDDFT](#rt-tddft-与-lr-tddft)
    - [RT-TDDFT](#rt-tddft)
      - [显式传播](#显式传播)
      - [半隐式传播](#半隐式传播)
      - [RT-TDDFT 的可观测量](#rt-tddft-的可观测量)
    - [LR-TDDFT](#lr-tddft)
      - [微扰密度矩阵演化](#微扰密度矩阵演化)
      - [微扰哈密顿量与 Hartree–xc 耦合核](#微扰哈密顿量与-hartreexc-耦合核)
      - [傅里叶变换](#傅里叶变换)
      - [响应理论](#响应理论)
      - [Casida 方程](#casida-方程)
      - [构建激子多体波函数基底](#构建激子多体波函数基底)
      - [类 FSSH 含时演化](#类-fssh-含时演化)
      - [多体非绝热耦合的单粒子表象](#多体非绝热耦合的单粒子表象)
  - [OT-SRSH 泛函（最优调控范围分离杂化泛函）](#ot-srsh-泛函最优调控范围分离杂化泛函)

## 什么是激子

当价带上的电子被激发到导带上时，会在价带上留下一个空穴；电子与空穴之间存在强烈的库仑相互作用，由此形成的束缚电子-空穴对被称作**激子（exciton）**。

如果电子-空穴对能够与晶格发生强相互作用，激子周围的晶格会发生畸变，这种畸变会产生一个势阱，反过来将激子捕获在某一位点上，从而形成**自陷激子态（self-trapped exciton, STE）**。

---

## 单粒子表象

由于激子是存在强烈相互作用的电子-空穴对，因此单粒子近似（平均场近似）不能描述电子与空穴之间的强相互作用。

> **说明：**
>
> - 当存在一个包含 $N$ 个相互作用粒子的系统时，描述它的完整波函数是 $N$ 个粒子坐标的函数，其复杂性随着粒子数指数增长，直接求解薛定谔方程几乎不可能。
> - 这时，可以仅考虑将每个粒子看作是在一个由其他所有粒子平均作用形成的、静态的有效势场中独立运动，这种近似被称为单粒子近似或平均场近似。
> - 因此，这种情况下，就能够将相互耦合的多体问题转化成较为简单的 $N$ 个单体问题（对应一个 $N$ 阶（$N\times N$）的 Slater 行列式）。
> - 但是其局限性在于，如果考虑多粒子体系的关联作用，单粒子表象就会失效，因为单粒子表象无法描述多粒子之间的关联相互作用。
> - 例如，我们关心粒子 1 和粒子 2 之间的耦合，但是在单粒子表象下，考虑粒子 1 的时候，其他所有粒子均被当作粒子 1 的平均势场；粒子 2 也是如此。因此，这种情况下无法考虑到粒子 1 和粒子 2 之间的瞬时相互作用。
> - 此外，单粒子表象无法处理粒子的产生和湮灭（比如电子-空穴对的产生），因为 Slater 行列式的阶数是保持不变的。

---

## 多粒子表象

> **说明：**
>
> - 描述激子问题需要涉及电子-空穴之间的相互作用，而单粒子近似则会把二者的相互作用忽略。在单粒子近似下，激子的激发能只能是 $E_c - E_v$，即价带顶和导带底之间的能量差。在这种情况下，电子和空穴之间的相互作用被完全忽略了。在一般的单粒子表象下，电子在其他电子形成的平均场中运动；如果某个电子的状态发生变化，其他电子由于单粒子表象下缺乏与之的动态关联，因此其他电子的波函数不会进行瞬时调整。
> - 因此对于需要关注的多粒子体系（例如激子或俄歇过程），需要引入多粒子表象。

但是在引入多粒子表象前，我们需要了解使用什么方法来处理多粒子表象下粒子数可变的情况。

---

### 一次量子化

一次量子化是我们最早接触的形式，例如在薛定谔方程中，单粒子哈密顿量可以表示成：

$$
h_i = \hat{T} + \hat{V} = -\frac{\hbar^2}{2m}\nabla^2 + V(\mathbf{r}) \tag{1}
$$

其中 $\hat{T}$ 为**动能算符**，$\hat{V}$ 为势能算符。

动能项中，拉普拉斯算符 $\nabla^2$ 描述的是波函数的曲率，弯曲得越强表明曲率越大，动量越大。

势能项 $V(\mathbf{r})$ 是一个势场，不论是否作用在波函数上，其他粒子导致的平均势场一直都存在。

因此，整个哈密顿量是一个待解的微分方程算符，需要整体作用在波函数上，才能得到其本征能量等性质。

此时，如果我们使用最简单的独立粒子近似（即粒子间无相互作用），那么 $N$ 个电子整体的哈密顿量为：

$$
H_p \approx \sum_{i=1}^N h_i \tag{2}
$$

最终得到的总能量等于每个电子能量的简单相加 $E = \sum \epsilon_i$。

如果考虑相互作用，此时 $N$ 个电子整体的哈密顿量为：

$$
H = \sum_{i=1}^N \left(-\frac{\hbar^2}{2m}\nabla_i^2 + V(\mathbf{r}_i)\right) + \frac{1}{2}\sum_{i \neq j} \frac{e^2}{|\mathbf{r}_i - \mathbf{r}_j|} \tag{3}
$$

其中最后一项为多体算符，表示第 $i$ 个电子和第 $j$ 个电子之间的相互作用。

虽然第一量子化能够写出最后一项的电子-电子相互作用项，但是很难求解，因为第 $i$ 个电子和第 $j$ 个电子的坐标在分母中，在 $N$ 个电子的情况下需要求解一个 $3N$ 维的偏微分方程。

如果考虑单电子（平均场）近似，那么这一项可以写作：

$$
V_{ee} = \sum_{i<j} \frac{e^2}{r_{ij}} \tag{4}
$$

在 DFT 中，对应的能量通常包含三部分：

$$
\left\langle \sum_{i<j} \frac{e^2}{r_{ij}} \right\rangle = E_{\text{Hartree}} + E_{\text{Exchange}} + E_{\text{Correlation}} \tag{5}
$$

第一项被称为 Hartree 项，包含了经典的静电排斥相互作用。后两项为交换-关联项。其中，**交换项**来源于泡利不相容原理：相同自旋的电子会因 Pauli 排斥而相互回避（"Pauli 空穴"），从而降低体系能量。**关联项**来源于不同自旋电子之间的瞬时库仑回避（"Coulomb 空穴"），它会让一个电子周围出现另一个电子的概率降低，进一步降低整体能量。

交换项和关联项均会导致额外的能量下降。其中，由于 Hartree–Fock 方法显式构造了 Slater 行列式，因此其计算的交换作用是精确的；主要难以准确描述的是关联项。

上述为一次量子化。由于需要构造巨大的 Slater 行列式，且无法处理粒子数变化的情况，因此需要进行二次量子化来处理这种情况。

---

### 二次量子化

在进入二次量子化之前，我们需要引入一些工具来描述二次量子化。

#### 产生与湮灭算符

由于我们需要研究激子，而激子不是不变的——它们可以通过吸收光子而产生，也能通过辐射（发光）或非辐射（声子）过程湮灭。产生和湮灭算符可以很好地描述这种动态的粒子数变化情况。

##### 位置算符与动量算符

在二次量子化的框架下，产生与湮灭算符并不是基础的算符，而是由位置算符和动量算符派生出来的。因此我们先来看位置算符和动量算符是怎么得来的。（以下过程可能和格里菲斯量子力学讲义中略有不同）

对于一个处于 $x$ 位置的**粒子**，其位置期望值应该是：

$$
\langle x \rangle = \int_{-\infty}^{\infty} x \, |\Psi(x, t)|^2 \, dx = \int_{-\infty}^{\infty} \Psi^*(x, t) \, \hat{x} \, \Psi(x, t) \, dx = \langle \Psi | \hat{x} | \Psi \rangle \tag{6}
$$

我们可以看出，一个"算符"是对其后函数做某种操作的指令。位置算符告诉你乘以 $x$，作用在波函数上，就能得到波函数的位置期望值。因此，我们将位置算符定义为 $\hat{x}$。

同样地，我们可能对粒子的运动速度感兴趣，对 $\langle x \rangle$ 关于时间 $t$ 求导可得：

$$
\frac{d\langle x \rangle}{dt} = \int x \frac{\partial}{\partial t} |\Psi|^2 dx = \int \left( \frac{\partial \Psi^*}{\partial t} x \Psi + \Psi^* x \frac{\partial \Psi}{\partial t} \right) dx \tag{7}
$$

> **注意：** 这里没有对 $x$ 求 $dt$ 是因为 $x$ 的积分上下限是负无穷到正无穷，$x$ 为积分变量而与 $t$ 无关，即 $\frac{\partial x}{\partial t} = 0$，因此求导仅对波函数求导。

使用含时薛定谔方程替换 $\frac{\partial \Psi}{\partial t}$，可以得到：

$$
i\hbar \frac{\partial \Psi}{\partial t} = -\frac{\hbar^2}{2m} \frac{\partial^2 \Psi}{\partial x^2} + V\Psi \tag{8}
$$

仅在左侧保留 $\frac{\partial \Psi}{\partial t}$ 可得：

$$
\frac{\partial \Psi}{\partial t} = \frac{1}{i\hbar} \left( -\frac{\hbar^2}{2m} \frac{\partial^2 \Psi}{\partial x^2} + V\Psi \right) \tag{9}
$$

其复共轭可以写作：

$$
\frac{\partial \Psi^*}{\partial t} = -\frac{1}{i\hbar} \left( -\frac{\hbar^2}{2m} \frac{\partial^2 \Psi^*}{\partial x^2} + V\Psi^* \right) \tag{10}
$$

将式（9） 和式（10） 代入式 (7')，可以消去势能项，仅保留动能项：

$$
\frac{d\langle x \rangle}{dt} = -\frac{1}{i\hbar} \frac{\hbar^2}{2m} \int \left( -\frac{\partial^2 \Psi^*}{\partial x^2} x \Psi + \Psi^* x \frac{\partial^2 \Psi}{\partial x^2} \right) dx \tag{11}
$$

提取其中公因子 $-\frac{i\hbar}{2m}$：

$$
\frac{d\langle x \rangle}{dt} = -\frac{i\hbar}{2m} \int \left( \frac{\partial^2 \Psi^*}{\partial x^2} x \Psi - \Psi^* x \frac{\partial^2 \Psi}{\partial x^2} \right) dx \tag{12}
$$

对其中 $\int \frac{\partial^2 \Psi^*}{\partial x^2} x \Psi \, dx$ 进行分部积分计算，就可以得到：

$$
\frac{d\langle x \rangle}{dt} = -\frac{i\hbar}{m} \int \Psi^* \frac{\partial \Psi}{\partial x} dx \tag{13}
$$

一般我们使用动量来描述粒子的速度，我们将其转化为动量：

$$
\langle p \rangle = m \frac{d\langle x \rangle}{dt} = -i\hbar \int \left( \Psi^* \frac{\partial \Psi}{\partial x} \right) dx \tag{14}
$$

上式又能写成：

$$
\langle p \rangle = \int_{-\infty}^{\infty} \Psi^*(x, t) \left( -i\hbar \frac{\partial}{\partial x} \right) \Psi(x, t) \, dx = \langle \Psi | \left( -i\hbar \frac{\partial}{\partial x} \right) | \Psi \rangle \tag{15}
$$

可以看出，括号内就是描述粒子动量的算符。它的意思是：对 $x$ 求导并将结果乘以 $-i\hbar$，作用在波函数上，就能得到波函数的动量期望值。因此，我们将动量算符定义为：

$$
\hat{p} = -i\hbar \frac{\partial}{\partial x}
$$

我们对位置算符和动量算符还有一个重要的推广，即二者的对易关系为 $[\hat{x}, \hat{p}_x] = i\hbar$，其中：

$$
[\hat{x}, \hat{p}_x] \Psi = (\hat{x}\hat{p}_x - \hat{p}_x\hat{x}) \Psi \tag{16}
$$

分别计算两项：

$$
\begin{aligned}
(\hat{x}\hat{p}_x - \hat{p}_x\hat{x}) \Psi &= \left( -i\hbar \, x \frac{\partial \Psi}{\partial x} \right) - \left( -i\hbar \Psi - i\hbar \, x \frac{\partial \Psi}{\partial x} \right) \\
&= i\hbar \Psi
\end{aligned} \tag{17}
$$

至此，我们完成了对位置算符和动量算符的推导。接下来我们需要在此基础上推导产生和湮灭算符，而其起源则是一维谐振子。

##### 一维谐振子

对于一个一维谐振子（例如弹簧上的小球）来说，它的经典能量是动能和势能之和（在经典力学中，哈密顿量为动能和势能的总和）：

$$
H = \frac{p^2}{2m} + \frac{1}{2}m\omega^2 x^2 \tag{18}
$$

其中，$p$ 为粒子动量，$m$ 为粒子质量，$x$ 为粒子位置，$\omega$ 为角频率。

在一次量子化中，我们把 $p$ 和 $x$ 变成算符 $\hat{p}$ 和 $\hat{x}$，满足 $[\hat{x}, \hat{p}] = i\hbar$。此时哈密顿算符可以写作：

$$
\hat{H} = \frac{\hat{p}^2}{2m} + \frac{1}{2}m\omega^2 \hat{x}^2 \tag{19}
$$

我们求解的思路是将哈密顿算符分解。如果是普通代数量 $u$ 和 $v$，比较容易看出 $u^2 + v^2 = (u - iv)(u + iv)$，但是由于 $\hat{p}$ 和 $\hat{x}$ 是算符，因此不能随意交换顺序。我们先引入约化后的过渡算符，去掉常数的干扰：

$$
\hat{X} = \sqrt{\frac{m\omega}{\hbar}} \, \hat{x}, \quad \hat{P} = \frac{1}{\sqrt{m\hbar\omega}} \, \hat{p} \tag{20}
$$

此时哈密顿量可以写成更简洁的形式：

$$
\hat{H} = \frac{\hbar\omega}{2} (\hat{P}^2 + \hat{X}^2) \tag{21}
$$

这时，我们可以借用平方和分解的思想，将 $u^2 + v^2$ 分解为 $(u - iv)(u + iv)$。表现在算符上可以写作：

$$
\hat{a}_- = \frac{1}{\sqrt{2}}(\hat{X} + i\hat{P}) = \sqrt{\frac{m\omega}{2\hbar}} \left( \hat{x} + \frac{i}{m\omega} \hat{p} \right) \tag{22}
$$

$$
\hat{a}_+ = \frac{1}{\sqrt{2}}(\hat{X} - i\hat{P}) = \sqrt{\frac{m\omega}{2\hbar}} \left( \hat{x} - \frac{i}{m\omega} \hat{p} \right) \tag{23}
$$

> **注意：**
>
> - 我们将两个算符定义为 $(\hat{X} + i\hat{P})$ 和 $(\hat{X} - i\hat{P})$。
> - 这二者是不能交换顺序的，不能写作 $(\hat{P} + i\hat{X})$，因为它们依然是非对易的。
> - 证明如下：
> - 考虑一个连在弹簧上的小球（质量 $m$，频率为 $\omega$）。
> - 根据牛顿定律：$F = ma \implies -kx = m \ddot{x}$。我们更喜欢用频率代替 $k$，即 $k = m\omega^2$。
> - 因此上式可以写作：$\ddot{x} + \omega^2 x = 0$。
> - 这个方程的通解是正弦或者余弦函数，粒子的位置随时间变化的解为：$x(t) = A \cos(\omega t)$。
> - 此处的 $A$ 为振幅。
> - 而动量则为 $p = mv = m\dot{x}$，需要对时间求导，即：
>
> $$\dot{x}(t) = \frac{d}{dt} [A \cos(\omega t)] = -A\omega \sin(\omega t) \tag{24}$$
>
> - 因此，动量表示为：
>
> $$p(t) = m \cdot (-A\omega \sin(\omega t)) = -m\omega A \sin(\omega t) \tag{25}$$
>
> - 如果我们将位置和动量画在同一个坐标系中，横轴是位置，纵轴是动量，我们可以得到：
>
> $$\frac{x(t)}{A} = \cos(\omega t) \tag{26}$$
>
> $$\frac{p(t)}{-m\omega A} = \sin(\omega t) \tag{27}$$
>
> - 它们的平方和为 1，即：
>
> $$\left( \frac{x}{A} \right)^2 + \left( \frac{p}{m\omega A} \right)^2 = 1 \tag{28}$$
>
> - 它代表了：在 $x$-$p$ 相平面上，速度最大的时候位置在 0 点；位置最大（$A$）的时候速度为 0。
> - 如果我们使用欧拉公式描述，即 $e^{-i\omega t} = \cos(\omega t) - i\sin(\omega t)$，那么实部对应位置，虚部对应动量，组合即 $\hat{X} + i\hat{P}$。它代表了：在复平面上，位置和动量有 90° 的相位差，即动量和位置是不同步的。
> - 如果我们将算符写成 $\hat{P} + i\hat{X}$（这里先借用一个性质：湮灭算符作用在基态上必须等于 0，即 $\hat{a} |0\rangle = 0$），那么作用在波函数上，我们就得到 $\left( -i\frac{d}{dX} + iX \right)\psi = 0$，对应的波函数为 $\psi \sim e^{+X^2/2}$，这是一个发散的函数，归一化积分为无穷大，物理上不存在。因此，我们定义的算符顺序必须是 $(\hat{X} + i\hat{P})$ 和 $(\hat{X} - i\hat{P})$。

##### 产生湮灭算符的性质及物理意义

其中，式（22） 定义为湮灭算符 $\hat{a}_-$，式（23） 定义为产生算符 $\hat{a}_+$。

接下来我们考虑它们的对易子 $[\hat{a}_-, \hat{a}_+]$：

$$
[\hat{a}_-, \hat{a}_+] = C^2 \left[ \left( \hat{x} + \frac{i}{m\omega}\hat{p} \right), \left( \hat{x} - \frac{i}{m\omega}\hat{p} \right) \right] \tag{29}
$$

其中，为简化计算令 $C = \sqrt{\frac{m\omega}{2\hbar}}$。

对方括号中每一项求对易，可得：

$$
[\hat{a}_-, \hat{a}_+] = C^2 \Big( [\hat{x}, \hat{x}] + \left[\hat{x}, -\frac{i}{m\omega}\hat{p}\right] + \left[\frac{i}{m\omega}\hat{p}, \hat{x}\right] + \left[\frac{i}{m\omega}\hat{p}, -\frac{i}{m\omega}\hat{p}\right] \Big) \tag{30}
$$

根据算符关系，算符与自身对易，即 $[\hat{x}, \hat{x}] = 0$，因此第一项和第四项为 0。第二项 $-\frac{i}{m\omega}[\hat{x}, \hat{p}]$ 由对易关系 $[\hat{x},\hat{p}]=i\hbar$ 得到 $\frac{\hbar}{m\omega}$，同理第三项也为 $\frac{\hbar}{m\omega}$。

因此，最终可以得到：

$$
[\hat{a}_-, \hat{a}_+] = \left( \frac{m\omega}{2\hbar} \right) \cdot \left( \frac{2\hbar}{m\omega} \right) = 1 \tag{31}
$$

因此，式（21） 的哈密顿量最终可以分解成：

$$
\hat{H} = \hbar\omega \left( \hat{a}_+ \hat{a}_- + \frac{1}{2} \right) \tag{32}
$$

注意到由于算符的次序问题，导致后面是 $+\frac{1}{2}$。利用式（31） 的对易关系 $[\hat{a}_-, \hat{a}_+] = 1$，更一般的情况可以写作：

$$
\hat{H} = \hbar\omega \left( \hat{a}_{\mp} \hat{a}_{\pm} \mp \frac{1}{2} \right) \tag{33}
$$

或者：

$$
\hat{H} = \frac{\hbar\omega}{2} (\hat{a}_+ \hat{a}_- + \hat{a}_- \hat{a}_+) \tag{34}
$$

式（33） 和式（34） 是等价的。

下面我们说明为什么式（22） 被叫做湮灭算符、式（23） 被叫做产生算符。

从薛定谔方程 $\hat H \psi = E \psi$ 出发，如果将上升算符 $\hat{a}_+$ 作用在波函数上：

$$
\hat{H} (\hat{a}_+ \psi) = \hat{a}_+ \hat{H} \psi + [\hat{H}, \hat{a}_+] \psi \tag{35}
$$

其中对易关系为 $[\hat{H}, \hat{a}_+] = \hbar\omega \, \hat{a}_+$，则：

$$
\hat{H} (\hat{a}_+ \psi) = (E + \hbar\omega)(\hat{a}_+ \psi) \tag{36}
$$

这说明，$\hat{a}_+$ 作用在波函数上后，新的波函数依然是 $\hat{H}$ 的本征态，本征能量为 $E + \hbar\omega$。这意味着，我们可以使用上升/下降算符（产生/湮灭算符）直接生成解。

然而如果对基态 $\psi_0$（能量 $\frac{1}{2}\hbar\omega$）作用湮灭算符：$\hat{a}_- \psi_0 = 0$，它代表波函数消失。这保证谐振子存在一个最低能量（基态）。

> **注意：** 我们用 $\hat{x}, \hat{p}$ 表示出了产生湮灭算符，反过来也可以用产生湮灭算符表示位置算符和动量算符：
>
> $$\hat{x} = \sqrt{\frac{\hbar}{2m\omega}} (\hat{a}_+ + \hat{a}_-) \tag{37}$$
>
> $$\hat{p} = i\sqrt{\frac{m\hbar\omega}{2}} (\hat{a}_+ - \hat{a}_-) \tag{38}$$
>
> 这时，$\hat{x}$ 代表着：测量粒子位置时，对粒子数状态进行了扰动，系统同时处于"少一个粒子"和"多一个粒子"的叠加态。同理动量算符也是如此。将位置算符作用在有 $n$ 个粒子的 $|n\rangle$ 态上：
>
> $$\begin{aligned}
\hat{x} |n\rangle &\propto (\hat{a}_- + \hat{a}_+) |n\rangle \\
&= \hat{a}_- |n\rangle + \hat{a}_+ |n\rangle \\
&= \sqrt{n} |n-1\rangle + \sqrt{n+1} |n+1\rangle
\end{aligned} \tag{39}$$
>
> 可以看到，位置算符作用在 $|n\rangle$ 态后，系统处在 $|n-1\rangle$ 和 $|n+1\rangle$ 的叠加态。

> **注意：**
>
> - 产生算符 $\hat{c}^\dagger$ 和湮灭算符 $\hat{c}$ 互为厄米共轭。（在二次量子化中，我们使用厄米共轭符号 $\dagger$ 表示产生算符，不加 $\dagger$ 的代表湮灭算符，作为一般的记号规则。）
> - 湮灭算符我们使用之前的无量纲化形式：
>
> $$\hat{c} = C \left( \hat{x} + \frac{i}{m\omega} \hat{p} \right) \tag{40}$$
>
> - 其中 $C = \sqrt{\frac{m\omega}{2\hbar}}$。
> - 对其取厄米共轭 $\dagger$：
>
> $$\hat{c}^\dagger = \left[ C \left( \hat{x} + \frac{i}{m\omega} \hat{p} \right) \right]^\dagger \tag{41}$$
>
> - 其中实数 $C$ 保持不变，即 $C^*=C$。又因为力学量算符均为厄米算符，故 $\hat{x}^\dagger = \hat{x}$、$\hat{p}^\dagger = \hat{p}$，中间仅 $i$ 变号，整理得：
>
> $$\hat{c}^\dagger = C \left( \hat{x} - \frac{i}{m\omega} \hat{p} \right) \tag{42}$$
>
> - 我们就得到了产生算符，证毕。
> - 因此，我们在后续约定使用 $\hat{c}^\dagger$ 代表产生算符，$\hat{c}$ 代表湮灭算符。

#### 占据数表象的矩阵表示

我们考虑一个包含三个态的系统，把它写作列向量：第一行定义为真空态，第二行定义为态 1，第三行定义为态 2，并作如下约定：

$$
|vac\rangle = \begin{pmatrix} 1 \\ 0 \\ 0 \end{pmatrix} \tag{43}
$$

它代表 100% 概率是空的，0% 在轨道（态）1，0% 在轨道（态）2。

$$
|1\rangle = \begin{pmatrix} 0 \\ 1 \\ 0 \end{pmatrix} \tag{44}
$$

它代表电子 100% 在第一个轨道（态）里。

$$
|2\rangle = \begin{pmatrix} 0 \\ 0 \\ 1 \end{pmatrix} \tag{45}
$$

它代表电子 100% 在第二个轨道（态）里。

产生算符的含义是将真空态变为某个态中有电子的情况。以态 1 为例，要把 $\begin{pmatrix} 1 \\ 0 \\ 0 \end{pmatrix}$ 变成 $\begin{pmatrix} 0 \\ 1 \\ 0 \end{pmatrix}$，那么态 1 的产生算符应该写作：

$$
\hat{c}_1^\dagger = \begin{pmatrix} 0 & 0 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 0 \end{pmatrix} \tag{46}
$$

将这个算符作用在真空态：

$$
\begin{pmatrix} 0 & 0 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 0 \end{pmatrix} \begin{pmatrix} 1 \\ 0 \\ 0 \end{pmatrix} = \begin{pmatrix} 0 \\ 1 \\ 0 \end{pmatrix} \tag{47}
$$

这时电子就产生在态 1 上。同理，态 2 的产生算符和湮灭算符也是如此，这里不再赘述。

如果要将电子从态 1 跳跃到态 2，我们可以写作：

$$
\hat{t}_{1\to2} = \hat{c}_2^\dagger \hat{c}_1 \tag{48}
$$

因为算符的作用顺序是从右向左：首先湮灭算符作用在态 1 消灭电子，随后产生算符在态 2 产生电子，它们整体可以写作跳跃算符 $\hat{t}_{1\to2}$。

#### 场算符

前面我们推导得到的哈密顿量（即式 34）是从非相互作用的谐振子得到的，也就是最理想的情况。然而实际更复杂的量子场是由无数个耦合在一起的谐振子组成的，我们并不清楚实际的产生湮灭算符是什么样子的。

现在，我们需要从一次量子化的形式转换为二次量子化。

在一次量子化中，希尔伯特空间内任意一个量子态 $|\psi\rangle$ 都可以用一组完备的基底 $\{|n\rangle\}$ 展开：

$$
|\psi\rangle = \sum_n C_n |n\rangle \tag{49}
$$

其中 $C_n$ 是投影系数。

> **注意：**
>
> - 如果我们使用哈密顿量作用在态 $|\Psi\rangle$ 上，得到的本征方程为：
>
> $$\hat{H} |\Psi\rangle = E |\Psi\rangle \tag{50}$$
>
> - 它代表了哈密顿算符 $\hat{H}$ 作用在波函数 $|\Psi\rangle$ 上，得到了一个能量值 $E$。表明 $|\Psi\rangle$ 是一个能量定态：使用哈密顿算符测量能量，会得到一个 100% 的确定值 $E$，且测量后系统状态不变。上式为能量的本征方程。
> - 类似地，如果有一个位置算符 $\hat{\vec{r}}$ 作用在状态 $|\Psi\rangle$ 上，得到的本征方程为：
>
> $$\hat{\vec{r}} |\Psi\rangle = \vec{R} |\Psi\rangle \tag{51}$$
>
> - 它表明 $|\Psi\rangle$ 是一个位置定态。如果测量位置，那么粒子 100% 处于位置 $\vec{R}$ 处。上式为位置的本征方程。
> - **重要：** 根据波函数的定义——处于状态 $|n\rangle$ 的粒子在位置 $\vec{r}$ 被发现的概率幅——可以将波函数 $\phi_n(\vec{r})$ 写作：
>
> $$\phi_n(\vec{r}) = \langle \vec{r} | n\rangle \tag{52}$$
>
> - 其中，只要 $\{|n\rangle\}$ 是完备的，就具备以下性质：
>
> $$\sum_n |n\rangle \langle n| = \hat{1} \tag{53}$$
>
> - 这个算符为单位算符，可以将任何态投影上去且不丢失任何信息。上式叫做**完备性关系**。

现在我们需要了解位置态。使用 $\{|n\rangle\}$ 将 $|\vec{r}\rangle$ 展开：

$$
|\vec{r}\rangle = \sum_n |n\rangle \langle n | \vec{r}\rangle \tag{54}
$$

其中，$\langle n | \vec{r}\rangle$ 是位置态在 $|n\rangle$ 基底下的展开系数。由波函数的定义 $\phi_n(\vec{r}) = \langle \vec{r} | n\rangle$，对它求复共轭可得：

$$
\langle n | \vec{r}\rangle = \langle \vec{r} | n\rangle^* = \phi_n^*(\vec{r}) \tag{55}
$$

因此位置态 $|\vec{r}\rangle$ 可以用能级态 $|n\rangle$ 表示：

$$
|\vec{r}\rangle = \sum_n \phi_n^*(\vec{r}) |n\rangle \tag{56}
$$

在二次量子化中，每个态都对应一个产生算符。根据之前的推导，产生算符 $\hat{c}_n^\dagger$ 作用在真空上，会创造出状态 $|n\rangle$：

$$
|n\rangle = \hat{c}_n^\dagger |0\rangle \tag{57}
$$

同样地，如果有一个产生算符 $\hat{\psi}^\dagger(\vec{r})$，作用在真空上，能够创造出位置在 $\vec{r}$ 的粒子：

$$
|\vec{r}\rangle = \hat{\psi}^\dagger(\vec{r}) |0\rangle \tag{58}
$$

我们将式（57） 和 (57) 代入式（56），就可以得到：

$$
\hat{\psi}^\dagger(\vec{r}) |0\rangle = \left( \sum_n \phi_n^*(\vec{r}) \hat{c}_n^\dagger \right) |0\rangle \tag{59}
$$

由于两个算符作用在真空态上得到相同的态，因此我们可以将两边的真空态去掉，就得到了：

$$
\hat{\psi}^\dagger(\vec{r}) = \sum_n \phi_n^*(\vec{r}) \hat{c}_n^\dagger \tag{60}
$$

上面这个算符我们将其定义为**场产生算符**。根据之前的推导，产生算符和湮灭算符互为厄米共轭，因此将上式整体取厄米共轭就能得到**场湮灭算符**：

$$
\hat{\psi}(\vec{r}) = \sum_n \phi_n(\vec{r}) \hat{c}_n \tag{61}
$$

场产生算符的物理意义是：在位置 $\vec{r}$ 处创造一个以 $\hat{c}_n^\dagger$ 为基底、贡献为 $\phi_n^*(\vec{r})$ 的粒子。同理，场湮灭算符则是消灭这样的粒子。

> **注意：**
>
> - 在式（60） 中，不要将左边的 $\hat{\psi}^\dagger(\vec{r})$ 跟波函数 $\psi$ 搞混，它是一个算符，是用波函数作为系数展开的。
> - 右边的 $\hat{c}_n^\dagger$ 物理含义是在第 $n$ 个量子态（轨道）里制造一个粒子。
> - $\phi_n^*(\vec{r})$ 代表了第 $n$ 个轨道在 $\vec{r}$ 处的权重，即 $\langle n | \vec{r}\rangle$。
> - 例如，在 $r = 0$ 处制造一个粒子，而现在有两个波：
>     - 波 1（$\hat{c}_1^\dagger$）：$\cos(x)$，在 $x=0$ 处值为 1。
>     - 波 2（$\hat{c}_2^\dagger$）：$\sin(x)$，在 $x=0$ 处值为 0。
> - 我们使用场产生算符：$\hat{\psi}^\dagger(0) = \sum_n \phi_n^*(0) \hat{c}_n^\dagger$
>     - 波 1：$\phi_1^*(0) = \cos(0) = 1$
>     - 波 2：$\phi_2^*(0) = \sin(0) = 0$
> - 最后：$\hat{\psi}^\dagger(0) = 1 \cdot \hat{c}_1^\dagger + 0 \cdot \hat{c}_2^\dagger$
> - 它代表了：如果要在 $r = 0$ 处创造一个粒子，余弦波占比是 1，而正弦波则不贡献。

##### 场算符的性质及物理意义

**1. 反对易关系**

场算符（费米子）满足如下反对易关系（$\{A, B\} \equiv AB + BA$）：

$$
\{ \hat{\psi}(\vec{r}), \hat{\psi}^\dagger(\vec{r}') \} = \delta(\vec{r} - \vec{r}') \tag{62}
$$

它说明了：在同一个位置发生产生湮灭操作会"撞上"（出现 $\delta$ 函数贡献），如果位置不同则互不影响。

$$
\{ \hat{\psi}(\vec{r}), \hat{\psi}(\vec{r}') \} = 0 \tag{63}
$$

两个湮灭算符**反对易**——即 $\hat{\psi}(\vec{r})\hat{\psi}(\vec{r}') = -\hat{\psi}(\vec{r}')\hat{\psi}(\vec{r})$。它体现了费米子波函数对粒子交换的反对称性：交换两次湮灭操作的次序会带来一个负号，但物理观测量（密度、能量等）不变。

$$
\{ \hat{\psi}^\dagger(\vec{r}), \hat{\psi}^\dagger(\vec{r}') \} = 0 \tag{64}
$$

两个产生算符也满足反对易关系。特别地，取 $\vec r=\vec r'$ 可得 $[\hat\psi^\dagger(\vec r)]^2 = 0$，即不能在同一个位置产生两个相同自旋的电子——这正是**泡利不相容原理**的体现。

以下我们做式（62） 的推导：

$$
\{ \hat{\psi}(\vec{r}), \hat{\psi}^\dagger(\vec{r}') \} = \left\{ \sum_n \phi_n(\vec{r}) \hat{c}_n, \, \sum_m \phi_m^*(\vec{r}') \hat{c}_m^\dagger \right\} \tag{65}
$$

我们将求和符号提出：

$$
= \sum_{n,m} \phi_n(\vec{r}) \phi_m^*(\vec{r}') \{ \hat{c}_n, \hat{c}_m^\dagger \} \tag{66}
$$

其中我们有以下基本反对易关系（公理）：

$$
\{ \hat{c}_n, \hat{c}_m^\dagger \} = \delta_{nm} \tag{67}
$$

$$
\{ \hat{c}_n, \hat{c}_m \} = 0 \tag{68}
$$

因此，式（66） 末尾的反对易子变为 $\delta_{nm}$，消掉一个求和符号：

$$
\{ \hat{\psi}(\vec{r}), \hat{\psi}^\dagger(\vec{r}') \} = \sum_n \phi_n(\vec{r}) \phi_n^*(\vec{r}') \tag{69}
$$

由于基底 $\{|n\rangle\}$ 是完备的，因此：

$$
\sum_n \phi_n(\vec{r}) \phi_n^*(\vec{r}') = \delta(\vec{r} - \vec{r}') \tag{70}
$$

证毕。

**2. 电子密度算符**

我们在 DFT 中关心的电子密度，在一次量子化中可以写成 $\rho = |\psi|^2 = \psi^* \psi$。那么在二次量子化中，可以将其表示为：

$$
\hat{n}(\vec{r}) = \hat{\psi}^\dagger(\vec{r}) \hat{\psi}(\vec{r}) \tag{71}
$$

上式被称为**（局域）密度算符**或粒子数密度算符，其物理意义是 $\vec{r}$ 处的电子密度，与一次量子化相比只是变成了算符形式。

上式较为重要，且其产生湮灭算符顺序不能更换。对于费米子，由于泡利不相容原理，在位置 $\vec{r}$ 处（给定自旋下）只能存在一个粒子，因此使用粒子数算符只能得到两个结果：

- 原本在 $\vec{r}$ 处的态是 $|0\rangle$（没有粒子），湮灭算符先作用得到 0，说明这里粒子数为 0，无需还原。
- 原本在 $\vec{r}$ 处的态是 $|1\rangle$（存在一个粒子），湮灭算符先作用得到 $|0\rangle$，这时产生算符再作用上去，把态还原成 $|1\rangle$，最终知道这里粒子数为 1。

**3. 总粒子数算符**

我们将上式的电子密度在全空间积分，就能得到总的粒子数算符：

$$
\hat{N} = \int d\vec{r} \, \hat{\psi}^\dagger(\vec{r}) \hat{\psi}(\vec{r}) \tag{72}
$$

这个算符的本征值只能为非负整数。

**4. 场算符的非厄米性**

我们已经知道：

$$
\hat{\psi}^\dagger(\vec{r}) \neq \hat{\psi}(\vec{r}) \tag{73}
$$

因此，场算符不是厄米的，它本身不是一个可观测量。

---

## 激子模型

### 相互作用的二次量子化

上面我们所做的一切都是为了将式（3） 最后一项的双体相互作用表达成二次量子化形式。在求得了场算符以后，我们重新回到上面的双体相互作用项：

$$
\frac{1}{2}\sum_{i \neq j} \frac{e^2}{|\mathbf{r}_i - \mathbf{r}_j|} = \frac{1}{2}\sum_{i \neq j} V(\mathbf{r}_i - \mathbf{r}_j) \tag{74}
$$

我们将其写为关于 $V$ 的函数，$V$ 代表两体相互作用势函数，方便后续研究。

我们将 $V$ 从一次量子化（区分粒子编号）翻译成二次量子化（只看占据数）。对于任意算符 $\hat{O}$，我们都可以使用式（53） $\sum_n |n\rangle \langle n| = \hat{1}$ 插入完备性表示：

$$
\hat{O} = \sum_{m,n} |m\rangle\langle m| \hat{O}|n\rangle\langle n| \tag{75}
$$

其中 $|n\rangle$ 为初态，$\langle m|$ 为末态，$\langle m| \hat{O}|n\rangle$ 为算符 $\hat{O}$ 在该基底下的矩阵元。

因此，我们可以将相互作用势写作如下形式：

$$
\begin{aligned}
\hat{V} &= \hat{1} \cdot \hat{V} \cdot \hat{1} \\
&= \left( \sum_{k,l} |kl\rangle \langle kl| \right) \hat{V} \left( \sum_{m,n} |mn\rangle \langle mn| \right) \\
&= \sum_{k,l,m,n} |kl\rangle \, \langle kl | \hat{V} | mn \rangle \, \langle mn|
\end{aligned} \tag{76}
$$

其中 $\langle kl | \hat{V} | mn \rangle$ 是矩阵元 $V_{klmn}$，它描述了这个过程发生的概率幅。我们可以将其展开为：

$$
\langle kl | V | mn \rangle = \iint d\vec{r}_1 \, d\vec{r}_2 \, \phi_k^*(\vec{r}_1) \phi_l^*(\vec{r}_2) V(\vec{r}_1 - \vec{r}_2) \phi_m(\vec{r}_1) \phi_n(\vec{r}_2) \tag{77}
$$

最右侧的 $\phi_m \phi_n$ 为初态，$\phi_k^* \phi_l^*$ 为末态，中间为相互作用。上式的物理含义是：初态两个粒子的概率幅 × 相互作用强度 × 末态两个粒子的概率幅。其中，相互作用强度代表权重——如果两个粒子距离近，权重就大。

算符 $|kl\rangle \langle mn|$ 的物理意义是：如果系统处于 $m, n$ 态，就将状态改为 $k, l$，写成二次量子化的形式为：

$$
|kl\rangle \langle mn| = \hat{c}_k^\dagger \hat{c}_l^\dagger \hat{c}_n \hat{c}_m \tag{78}
$$

式（76） 代表了：$m, n$ 态上的粒子在发生相互作用以后，被湮灭算符 $\hat{c}_n \hat{c}_m$ 消灭掉，然后使用产生算符 $\hat{c}_k^\dagger \hat{c}_l^\dagger$ 在 $k, l$ 态上产生粒子。

> **注意：** 这里下标不能随意替换，需要保证和矩阵元一致。最外层对应粒子 1 从 $m$ 态到 $k$ 态的转变，内层代表粒子 2 从 $n$ 态到 $l$ 态的转变。

我们将式（74） 重新写成二次量子化的形式：

$$
H_{\text{int}} = \frac{1}{2} \sum_{k,l,m,n} \langle kl | \hat{V} | mn \rangle \, \hat{c}_k^\dagger \hat{c}_l^\dagger \hat{c}_n \hat{c}_m \tag{79}
$$

并且我们了解了中间 $\langle kl | \hat{V} | mn \rangle$ 可以用积分写出来。因此，整个双体相互作用可以写作：

$$
H_{\text{int}} = \frac{1}{2} \sum_{klmn} \left[ \iint d\vec{r} \, d\vec{r}' \, \phi_k^*(\vec{r}) \phi_l^*(\vec{r}') V(\vec{r}-\vec{r}') \phi_m(\vec{r}) \phi_n(\vec{r}') \right] \hat{c}_k^\dagger \hat{c}_l^\dagger \hat{c}_n \hat{c}_m \tag{80}
$$

我们将带有相同空间变量的项分组放在一起：

$$
H_{\text{int}} = \frac{1}{2} \iint d\vec{r} \, d\vec{r}' \, V(\vec{r}-\vec{r}') \left[ \sum_k \phi_k^*(\vec{r}) \hat{c}_k^\dagger \right] \left[ \sum_l \phi_l^*(\vec{r}') \hat{c}_l^\dagger \right] \left[ \sum_n \phi_n(\vec{r}') \hat{c}_n \right] \left[ \sum_m \phi_m(\vec{r}) \hat{c}_m \right] \tag{81}
$$

根据之前推导的场算符的定义（式（60） 和式 61），我们可以对应上面方括号内的四项：

$$
\begin{aligned}
\sum_k \phi_k^*(\vec{r}) \hat{c}_k^\dagger &\longrightarrow \hat{\psi}^\dagger(\vec{r}) \\
\sum_l \phi_l^*(\vec{r}') \hat{c}_l^\dagger &\longrightarrow \hat{\psi}^\dagger(\vec{r}') \\
\sum_n \phi_n(\vec{r}') \hat{c}_n &\longrightarrow \hat{\psi}(\vec{r}') \\
\sum_m \phi_m(\vec{r}) \hat{c}_m &\longrightarrow \hat{\psi}(\vec{r})
\end{aligned} \tag{82}
$$

我们就得到了最终的二次量子化表述的双体相互作用公式：

$$
H_{\text{int}} = \frac{1}{2} \iint d\vec{r} \, d\vec{r}' \, \hat{\psi}^\dagger(\vec{r}) \hat{\psi}^\dagger(\vec{r}') V(\vec{r}-\vec{r}') \hat{\psi}(\vec{r}') \hat{\psi}(\vec{r}) \tag{83}
$$

或者，为了保证 $\vec{r}'$ 处和 $\vec{r}$ 处的算符是配对的，可以写作：

$$
H_{\text{int}} = \frac{1}{2} \iint d\vec{r} \, d\vec{r}' \, V(\vec{r}-\vec{r}') \, \hat{\psi}^\dagger(\vec{r}) \hat{\psi}^\dagger(\vec{r}') \hat{\psi}(\vec{r}') \hat{\psi}(\vec{r}) \tag{84}
$$

### 双带近似

在实际的晶体中存在大量能级，但是为了研究激子，我们仅关注激子涉及的轨道，例如包含价带顶和导带底的基态激子。因此我们先考虑最简单的情况——价带顶的一个电子被激发到导带底（单激子态）。

场算符的完整定义是所有能级态的叠加：

$$
\hat{\psi}(\vec{r}) = \sum_{n, \vec{k}} \phi_{n,\vec{k}}(\vec{r}) \, \hat{c}_{n,\vec{k}} \tag{85}
$$

其中 $n$ 是能带索引，$\vec{k}$ 是波矢。$\phi_{n,\vec{k}}(\vec{r})$ 表示：处于第 $n$ 个能带、动量为 $\vec{k}$ 的那个电子，在位置 $\vec{r}$ 处的形状（权重）。在 DFT 计算中，它就是 Kohn–Sham (KS) 轨道。

这里我们使用双带近似——仅保留价带 $n=v$ 和导带 $n=c$，将其限制在这两个带上，且仅包含单个 $\vec k$ 点：

$$
\hat{\psi}(\vec{r}) \approx \sum_{\vec{k}} \phi_{c,\vec{k}}(\vec{r}) \hat{c}_{c,\vec{k}} + \sum_{\vec{k}} \phi_{v,\vec{k}}(\vec{r}) \hat{c}_{v,\vec{k}} \equiv \hat{\psi}_c + \hat{\psi}_v \tag{86}
$$

将式（86） 代入式（83），可以得到仅包含价带、导带的两体相互作用：

$$
H_{\text{int}} = \frac{1}{2} \iint d\vec{r} \, d\vec{r}' \, (\hat{\psi}_c^\dagger + \hat{\psi}_v^\dagger)(\vec{r}) \cdot (\hat{\psi}_c^\dagger + \hat{\psi}_v^\dagger)(\vec{r}') \cdot V \cdot (\hat{\psi}_c + \hat{\psi}_v)(\vec{r}') \cdot (\hat{\psi}_c + \hat{\psi}_v)(\vec{r}) \tag{87}
$$

上式展开后总共 16 项，代表了仅包含价带、导带所有可能的过程。

> **注意：** 此时我们已经认为价带顶有一个电子被激发到导带底，已经形成了激子。我们现在要做的是研究已经形成的电子-空穴对之间的相互作用。

然而，这 16 项中并非所有项都有意义。例如：

1. $\hat{\psi}_c^\dagger \hat{\psi}_c^\dagger \hat{\psi}_c \hat{\psi}_c$：全是导带电子之间的相互作用，但我们考虑的单激子态此时导带仅有 1 个电子，双湮灭作用得 0，因此这一项在单激子态等于 0。
2. $\hat{\psi}_v^\dagger \hat{\psi}_v^\dagger \hat{\psi}_v \hat{\psi}_v$：代表激发完一个电子以后，价带中 $N-1$ 个价带电子两两之间的相互作用——这是价带背景的自相互作用，通常被吸收进基态能量或单粒子项中，不直接影响激子的结合能。
3. $\hat{\psi}_c^\dagger \hat{\psi}_c^\dagger \hat{\psi}_v \hat{\psi}_v$：同时消灭两个价带电子，产生两个导带电子（双激发），不属于单激子过程。

因此，我们需要的是在 $\vec{r}$ 处理导带电子（$\hat{\psi}_c$ 与 $\hat{\psi}_c^\dagger$）以及在 $\vec{r}'$ 处理价带电子（$\hat{\psi}_v$ 与 $\hat{\psi}_v^\dagger$）的项。由于 $\vec{r}$ 与 $\vec{r}'$ 是等价的（积分对称性），可抵消掉前面的 $1/2$ 系数，因此保留的项仅为：

$$
\hat{\psi}_c^\dagger(\vec{r}) \hat{\psi}_v^\dagger(\vec{r}') V(\vec{r}-\vec{r}') \hat{\psi}_v(\vec{r}') \hat{\psi}_c(\vec{r}) \tag{88}
$$

算符从右向左作用，其含义依次是：

1. $\hat{\psi}_c(\vec{r})$：在 $\vec{r}$ 处消灭那个唯一的导带电子。
2. $\hat{\psi}_v(\vec{r}')$：在 $\vec{r}'$ 处消灭一个价带背景电子。
3. $V(\vec{r}-\vec{r}')$：计算这两个电子之间的库仑相互作用。
4. $\hat{\psi}_v^\dagger(\vec{r}')$：把那个价带电子重新放回 $\vec{r}'$。
5. $\hat{\psi}_c^\dagger(\vec{r})$：把导带电子重新放回 $\vec{r}$。

### 双电子到电子-空穴对

在上面的式（88） 中，我们描述的全是电子和电子之间的相互作用。然而，空穴本质就是电子的缺失，因此我们可以改用空穴产生/湮灭场算符代替价带电子算符。对应关系为：消灭价带电子 $\hat{\psi}_v$ 等于产生空穴 $\hat{\psi}_h^\dagger$；产生价带电子 $\hat{\psi}_v^\dagger$ 等于消灭空穴 $\hat{\psi}_h$。直接替换可得：

$$
H_{\text{int}} = \iint d\vec r \, d\vec r' \, V(\vec r-\vec r') \cdot \hat{\psi}_c^\dagger(\vec{r}) \hat{\psi}_h(\vec{r}') \hat{\psi}_h^\dagger(\vec{r}') \hat{\psi}_c(\vec{r}) \tag{89}
$$

但是目前的顺序并不是"左产生右湮灭"的标准正规序，因此我们需要交换算符顺序。由于是费米子，交换顺序要带一个负号。根据反对易关系 $\{ \hat{\psi}_h(\vec{r}'), \hat{\psi}_h^\dagger(\vec{r}') \} = \delta(\vec{r}'-\vec{r}')$ 可得：

$$
\hat{\psi}_h(\vec{r}') \hat{\psi}_h^\dagger(\vec{r}') = \delta(\vec{r}' - \vec{r}') - \hat{\psi}_h^\dagger(\vec{r}') \hat{\psi}_h(\vec{r}') \tag{90}
$$

因为算符坐标都是 $\vec{r}'$，所以 $\delta(\vec{r}' - \vec{r}') = \delta(\mathbf 0)$，这代表无穷大的局域贡献，需要通过正则化吸收到单粒子能量中。具体地，把这一项代入式（89） 的积分可得：

$$
H_{\delta} = \int d\vec{r} \, \hat{\psi}_c^\dagger(\vec{r}) \left[ \int d\vec{r}' \, \rho_{\text{valence}}(\vec{r}') V(\vec{r}-\vec{r}') \right] \hat{\psi}_c(\vec{r}) \tag{91}
$$

其中 $\hat{\psi}_c^\dagger(\vec{r}) \hat{\psi}_c(\vec{r})$ 是导带电子的密度算符，中间积分描述了导带电子与所有价带电子之间的库仑相互作用。因此整体的物理意义是：导带的那个电子与所有价带背景电子的库仑作用。我们可以将这一项并入到单粒子哈密顿量的本征值中去，双体相互作用部分仅计算激子相对于基态的能量变化。

因此，我们最终得到了：

$$
H_{\text{exc}} = -\iint d\vec r \, d\vec r' \, \hat{\psi}_c^\dagger(\vec{r}) \hat{\psi}_h^\dagger(\vec{r}') V(\vec{r}-\vec{r}') \hat{\psi}_h(\vec{r}') \hat{\psi}_c(\vec{r}) \tag{92}
$$

上式中，本来应该是 $V$ 的电子斥力，由于交换算符带来变号，变成了**电子-空穴吸引相互作用**，这就是激子能够结合在一起的原因。

> **注意：**
>
> - 哈密顿量 $H_{\text{exc}}$ 中仅显式包含双体相互作用项，但在数值计算上已经隐含了背景电荷的修正。例如计算带隙得到 1.5 eV，这个数值已经扣除了背景电荷、核作用等。
> - $H_{\text{exc}}$ 在激子理论中等于**电子-空穴的有效双体相互作用**，是已经包含了所有背景电子与离子影响后的、被屏蔽的双体相互作用。当我们选择价带、导带的那一刻，它已经包含了所有价带电子彼此的排斥、激子与离子背景的平衡效果——这是一个我们经过重整化后的"真空能级"。

### 激子哈密顿量和激子态

我们前面推导出来的只是相互作用势能项。现在我们写出激子的整体哈密顿量，同时包含动能项和势能项：

$$
\begin{aligned}
H_{\text{total}} = & \int d\vec{r}_e \, \hat{\psi}_c^\dagger(\vec{r}_e) \left[ -\frac{\hbar^2 \nabla_e^2}{2m_e^*} + E_g \right] \hat{\psi}_c(\vec{r}_e) \\
& + \int d\vec{r}_h \, \hat{\psi}_h^\dagger(\vec{r}_h) \left[ -\frac{\hbar^2 \nabla_h^2}{2m_h^*} \right] \hat{\psi}_h(\vec{r}_h) \\
& - \iint d\vec{r}_e \, d\vec{r}_h \, \hat{\psi}_c^\dagger(\vec{r}_e) \hat{\psi}_h^\dagger(\vec{r}_h) V(\vec{r}_e-\vec{r}_h) \hat{\psi}_h(\vec{r}_h) \hat{\psi}_c(\vec{r}_e)
\end{aligned} \tag{93}
$$

第一项为电子动能项，其中 $m_e^*$ 为**电子有效质量**；第二项为空穴动能项，其中 $m_h^*$ 为**空穴有效质量**；第三项为我们以上推导出的电子-空穴吸引相互作用能。在电子动能项中还包含一项带隙值 $E_g$，这是电子跃迁到导带必须跨过的能量。

我们现在得到了激子哈密顿量，需要将其作用在激子波函数上。激子波函数我们用如下形式表示：

$$
| \Psi_{\text{exc}} \rangle = \sum_{\vec{r}_e, \vec{r}_h} \Phi(\vec{r}_e, \vec{r}_h) \, \hat{\psi}_c^\dagger(\vec{r}_e) \hat{\psi}_h^\dagger(\vec{r}_h) | G \rangle \tag{94}
$$

从右往左看：$| G \rangle$ 是基态（价带满电子、导带全空）；$\hat{\psi}_c^\dagger(\vec{r}_e) \hat{\psi}_h^\dagger(\vec{r}_h)$ 在 $\vec{r}_h$ 位置创造一个价带空穴，并在 $\vec{r}_e$ 位置创造一个导带电子；$\Phi(\vec{r}_e, \vec{r}_h)$ 是电子出现在 $\vec{r}_e$ 且空穴出现在 $\vec{r}_h$ 的概率幅（在均匀晶体中仅与相对距离 $\vec{r}_e - \vec{r}_h$ 有关）。

> **说明：** 激子波函数的完整图像可以理解为一个弥散在整个晶体中的氢原子（电子-空穴束缚态）。

### Wannier 方程

我们将上面得到的激子哈密顿量作用在激子态上。由于项数较多且每一项较复杂，我们将每一项分开看。

**(1) 电子动能项作用：**

$$
\hat{T}_e |\Psi_{\text{exc}}\rangle = \sum_{\vec{r}_e, \vec{r}_h} \Phi(\vec{r}_e, \vec{r}_h) \int d\vec{r} \, \hat{\psi}_c^\dagger(\vec{r}) \left[ \hat{O}_e(\vec{r}) \right] \hat{\psi}_c(\vec{r}) \hat{\psi}_c^\dagger(\vec{r}_e) \hat{\psi}_h^\dagger(\vec{r}_h) |G\rangle \tag{95}
$$

为了简化，记 $\hat{O}_e = -\frac{\hbar^2 \nabla^2}{2m_e^*} + E_g$。我们主要关注的是算符部分如何相互作用：

- 在电子动能哈密顿量 $\hat{T}_e$ 中，最右侧的电子湮灭算符 $\hat{\psi}_c(\vec{r})$ 会越过数 $\Phi(\vec{r}_e, \vec{r}_h)$，因为它是一个复数（波函数的幅值），算符可以穿透它。因此求和与积分均可往前提。
- 后面三个算符作用在基态上：

$$
\hat{\psi}_c(\vec{r}) \hat{\psi}_c^\dagger(\vec{r}_e) \hat{\psi}_h^\dagger(\vec{r}_h) |G\rangle \tag{96}
$$

前两个都是导带相关的算符，第三个是价带（空穴）产生算符，因此前两个算符可以整体与第三个算符互换（它们之间反对易子为零，互不干扰），但需注意每次互换带来的负号。将 $\hat{\psi}_h^\dagger(\vec{r}_h)$ 向前提，可得：

$$
\dots \hat{\psi}_h^\dagger(\vec{r}_h) \, \hat{\psi}_c^\dagger(\vec{r}_e) \, \hat{\psi}_c(\vec{r}) \, |G\rangle \tag{97}
$$

> **注意：** 这里交换算符位置时，因为反对易关系需要相应地加负号；最终偶数次交换符号回到正号。

依旧看电子湮灭算符 $\hat{\psi}_c(\vec{r})$，它和紧跟在后面的 $\hat{\psi}_c^\dagger(\vec{r}_e)$ 满足反对易关系（式 62）：$\{ \hat{\psi}_c(\vec{r}), \hat{\psi}_c^\dagger(\vec{r}_e) \} = \delta(\vec{r} - \vec{r}_e)$。可得：

$$
\hat{\psi}_c(\vec{r}) \hat{\psi}_c^\dagger(\vec{r}_e) = \delta(\vec{r} - \vec{r}_e) - \hat{\psi}_c^\dagger(\vec{r}_e) \hat{\psi}_c(\vec{r}) \tag{98}
$$

这个式子作用在 $|G\rangle$ 上：

- 第一项 $\delta(\vec{r} - \vec{r}_e) |G\rangle$ 保留。
- 第二项 $-\hat{\psi}_c^\dagger(\vec{r}_e) \hat{\psi}_c(\vec{r}) |G\rangle$ 中，湮灭算符先作用在基态上，但基态的导带本身就没有电子，因此这项直接等于 0。

整理可得：

$$
\hat{T}_e |\Psi_{\text{exc}}\rangle = \sum_{\vec{r}_e, \vec{r}_h} \Phi(\vec{r}_e, \vec{r}_h) \int d\vec{r} \, \hat{\psi}_c^\dagger(\vec{r}) \left[ \hat{O}_e(\vec{r}) \right] \delta(\vec{r} - \vec{r}_e) \, \hat{\psi}_h^\dagger(\vec{r}_h) |G\rangle \tag{99}
$$

> **注意：** 这里微分算符 $\hat{O}_e(\vec{r})$ 作用在 $\delta$ 函数上，对 $\delta$ 函数求导。根据 $\delta$ 函数的性质：
>
> $$\int d\vec{r} \, f(\vec{r}) \, \nabla^2 \delta(\vec{r}-\vec{r}_e) = \nabla^2 f(\vec{r}_e) \tag{100}$$
>
> 积分 $\int d\vec{r}$ 和 $\delta(\vec{r} - \vec{r}_e)$ 相抵消——所有 $\vec{r}$ 变成 $\vec{r}_e$：$\hat{\psi}_c^\dagger(\vec{r})$ 变成 $\hat{\psi}_c^\dagger(\vec{r}_e)$，微分算符 $\hat{O}_e(\vec{r})$ 变成 $\hat{O}_e(\vec{r}_e)$；并且因为 $\Phi$ 是关于 $\vec{r}_e$ 的函数，这个微分算符现在直接对 $\Phi$ 里的 $\vec{r}_e$ 求导。

整理可得：

$$
\hat{T}_e |\Psi_{\text{exc}}\rangle = \sum_{\vec{r}_e, \vec{r}_h} \left[ \left( -\frac{\hbar^2 \nabla_{\vec{r}_e}^2}{2m_e^*} + E_g \right) \Phi(\vec{r}_e, \vec{r}_h) \right] \hat{\psi}_c^\dagger(\vec{r}_e) \hat{\psi}_h^\dagger(\vec{r}_h) |G\rangle \tag{101}
$$

**(2) 空穴动能项作用：**

类似电子动能项，这里不再做详细推导。同样地，所有的 $\vec{r}$ 都变成了 $\vec{r}_h$，可得：

$$
\hat{T}_h |\Psi_{\text{exc}}\rangle = \sum_{\vec{r}_e, \vec{r}_h} \left[ \left( -\frac{\hbar^2 \nabla_{\vec{r}_h}^2}{2m_h^*} \right) \Phi(\vec{r}_e, \vec{r}_h) \right] \hat{\psi}_c^\dagger(\vec{r}_e) \hat{\psi}_h^\dagger(\vec{r}_h) |G\rangle \tag{102}
$$

**(3) 相互作用项作用：**

将系数和积分全部提到前面：

$$
\hat{V} |\Psi_{\text{exc}}\rangle = -\sum_{\vec{r}_e, \vec{r}_h} \Phi(\vec{r}_e, \vec{r}_h) \iint d\vec{r} \, d\vec{r}' \, V(\vec{r}-\vec{r}') \times \left[ \hat{\psi}_c^\dagger(\vec{r}) \hat{\psi}_h^\dagger(\vec{r}') \hat{\psi}_h(\vec{r}') \hat{\psi}_c(\vec{r}) \right] \left[ \hat{\psi}_c^\dagger(\vec{r}_e) \hat{\psi}_h^\dagger(\vec{r}_h) |G\rangle \right] \tag{103}
$$

我们只关注两组算符之间的乘积：

$$
\hat{\psi}_c^\dagger(\vec{r}) \hat{\psi}_h^\dagger(\vec{r}') \, \hat{\psi}_h(\vec{r}') \, \hat{\psi}_c(\vec{r}) \cdot \hat{\psi}_c^\dagger(\vec{r}_e) \hat{\psi}_h^\dagger(\vec{r}_h) |G\rangle \tag{104}
$$

其中电子和空穴湮灭算符分别和右边的对应产生算符运用反对易关系：与"反对易子" $\delta$ 函数贡献结合，另一项中湮灭算符先作用于基态而得 0。因此式（104） 可以写作：

$$
\hat{\psi}_c^\dagger(\vec{r}) \hat{\psi}_h^\dagger(\vec{r}') \cdot \delta(\vec{r} - \vec{r}_e) \cdot \delta(\vec{r}' - \vec{r}_h) \cdot |G\rangle \tag{105}
$$

把原本的式子拼回去：

$$
\hat{V} |\Psi_{\text{exc}}\rangle = -\sum_{\vec{r}_e, \vec{r}_h} \Phi(\vec{r}_e, \vec{r}_h) \iint d\vec{r} \, d\vec{r}' \, V(\vec{r}-\vec{r}') \left[ \hat{\psi}_c^\dagger(\vec{r}) \hat{\psi}_h^\dagger(\vec{r}') \delta(\vec{r} - \vec{r}_e) \delta(\vec{r}' - \vec{r}_h) |G\rangle \right] \tag{106}
$$

双重积分被两个 $\delta$ 函数分别抵消——$\delta$ 函数强制让 $d\vec{r}$ 积分塌缩到 $\vec{r} = \vec{r}_e$，同理 $d\vec{r}'$ 塌缩到 $\vec{r}' = \vec{r}_h$。最后整理可得：

$$
\hat{V} |\Psi_{\text{exc}}\rangle = \sum_{\vec{r}_e, \vec{r}_h} \left[ -V(\vec{r}_e - \vec{r}_h) \Phi(\vec{r}_e, \vec{r}_h) \right] \hat{\psi}_c^\dagger(\vec{r}_e) \hat{\psi}_h^\dagger(\vec{r}_h) |G\rangle \tag{107}
$$

**(4) 合并得本征方程**

最后，我们将上面所有项加起来，让它等于激子总能量 $E |\Psi_{\text{exc}}\rangle$：

$$
\sum_{\vec{r}_e, \vec{r}_h} \left\{ \left[ -\frac{\hbar^2 \nabla_{\vec{r}_e}^2}{2m_e^*} + E_g -\frac{\hbar^2 \nabla_{\vec{r}_h}^2}{2m_h^*} - V(\vec{r}_e - \vec{r}_h) \right] \Phi(\vec{r}_e, \vec{r}_h) \right\} \hat{\psi}_c^\dagger(\vec{r}_e) \hat{\psi}_h^\dagger(\vec{r}_h) |G\rangle = \sum_{\vec{r}_e, \vec{r}_h} \left\{ E \cdot \Phi(\vec{r}_e, \vec{r}_h) \right\} \hat{\psi}_c^\dagger(\vec{r}_e) \hat{\psi}_h^\dagger(\vec{r}_h) |G\rangle \tag{108}
$$

由于左右两边的基底向量相同且对所有项均成立，大括号 $\{\dots\}$ 内的内容必须相等。于是我们仅保留系数方程：

$$
\left[ -\frac{\hbar^2 \nabla_{e}^2}{2m_e^*} - \frac{\hbar^2 \nabla_{h}^2}{2m_h^*} - V(\vec{r}_e - \vec{r}_h) \right] \Phi(\vec{r}_e, \vec{r}_h) = (E - E_g) \Phi(\vec{r}_e, \vec{r}_h) \tag{109}
$$

上式被称为 **Wannier 激子方程**。

我们将 Wannier 方程和氢原子薛定谔方程比较一下：

$$
\left[ -\frac{\hbar^2 \nabla_e^2}{2m_0} - \frac{\hbar^2 \nabla_p^2}{2M_p} - \frac{e^2}{4\pi\epsilon_0 |\vec{r}_e - \vec{r}_p|} \right] \Psi(\vec{r}_e, \vec{r}_p) = E_{\text{total}} \Psi(\vec{r}_e, \vec{r}_p) \tag{110}
$$

上式为氢原子方程。可以看出，Wannier 方程实际上描述的是晶体中的"氢原子模型"，二者的数学形式完全一致。

---

## GW + BSE

我们上面推导的 Wannier 方程实际上包含了三个关键的近似，导致它适用的是大半径激子，即 **Wannier–Mott 激子**（或直接称为 Wannier 激子）：

1. **有效质量近似**——假设能带的带边附近是完美的抛物线：

    $$E_{c\mathbf{k}} - E_{v\mathbf{k}} \approx E_g + \frac{\hbar^2 k^2}{2\mu^*} \tag{111}$$

    我们将复杂的能带信息简化成了有效质量。

2. **介质屏蔽近似**——相互作用是被极度简化的：

    $$W \approx \frac{1}{\epsilon_r} \frac{e^2}{|\mathbf{r}_e - \mathbf{r}_h|} \tag{112}$$

    在这里我们忽略了微观原子结构，把背景当作连续介质。

3. **忽略交换项**——因为 Wannier 激子有较大的半径（约 $10^1\sim10^2$ Å），电子和空穴的短程交换作用很弱，因此在 Wannier 方程中通常忽略这一项。这会导致使用 Wannier 方程计算的单重态/三重态劈裂不准确。

对于一般情形，钙钛矿等半导体中的激子大都是大半径激子，使用 Wannier 方程已经足够好。但是如果要最精确计算，必须使用 **GW + BSE**。

---

### GW 近似

我们前面推导的 Wannier 方程是基于多粒子表象的，而 GW 近似处理的是单粒子。它的目的是把 DFT 里假想的 Kohn–Sham 轨道修正为"真实可观测的准粒子能量"。换句话说，GW 是用来"把电子当成真正带着自能的粒子"，而不是 DFT 里的平均场电子。

为了替换掉 DFT 中那个近似的 $V_{xc}$，我们需要得到一个非局域的、含时的自能算符 $\Sigma$。

#### 单粒子格林函数

为了能够解出包含自能 $\Sigma$ 的方程（Dyson 方程），我们首先需要建立"传播"和"散射"的概念。

我们定义单粒子格林函数：

$$
G(1,2) = -i \langle \Psi_0 | T [ \hat{\psi}(1) \hat{\psi}^\dagger(2) ] | \Psi_0 \rangle \tag{113}
$$

它的每一项含义如下：

- $1, 2$：代表时空坐标 $(\vec{r}_1, t_1)$ 和 $(\vec{r}_2, t_2)$。
- $\hat{\psi}^\dagger(2)$：产生算符，意思是"在 $t_2$ 时刻、$\vec{r}_2$ 位置创造一个电子"。
- $\hat{\psi}(1)$：湮灭算符，意思是"在 $t_1$ 时刻、$\vec{r}_1$ 位置消灭一个电子"。
- $T$：编时算符（time-ordering）。**它强制要求：时间晚的算符放在左边，时间早的放在右边**（对费米子算符每交换一次还要带一个负号）。
- $\langle \Psi_0 | \dots | \Psi_0 \rangle$：期望值，表示这个过程是在系统基态 $|\Psi_0\rangle$ 上发生的。
- $|G|^2$ 代表电子从 $2$ 运动到 $1$ 的概率密度。

格林函数包含两个物理过程，分别是电子传播和空穴传播：

1. **电子传播**（$t_1 > t_2$）：因为 $t_1$ 较晚，产生算符先作用，在 $\vec r_2$ 处加一个电子，然后电子运动，经过 $t_1 - t_2$ 时间后湮灭算符把电子拿走。这段过程描述了导带电子的运动。
2. **空穴传播**（$t_1 < t_2$）：因为 $T$ 算符的作用，优先让湮灭算符在 $\vec r_1$ 处拿走一个电子（创造空穴），然后空穴运动，经过 $t_2 - t_1$ 时间后产生算符把电子放回。这段过程描述了价带空穴的运动。

自由电子格林函数把时域和频域联系起来。我们先写出时域格林函数 $G_0(t)$：根据量子力学，如果一个粒子的能量是 $\epsilon_{\mathbf{k}}$，它的波函数随时间的演化遵循：

$$
\psi(t) \sim e^{-i \epsilon_{\mathbf{k}} t / \hbar} \tag{114}
$$

> **注意：** 为了方便，下面取原子单位制 $\hbar=1$。

而格林函数 $G_0(t)$ 描述的是：在 $t=0$ 时刻产生一个粒子，它在 $t$ 时刻还存在的概率幅。粒子必须是先产生后传播，因此它应该只在 $t>0$ 时存在。我们引入阶跃函数 $\theta(t)$，$G_0(t)$ 就可以写作：

$$
G_0(t) = -i \theta(t) e^{-i \epsilon_{\mathbf{k}} t} \tag{115}
$$

其中：

- $\theta(t)$ 在 $t>0$ 时为 1、在 $t<0$ 时为 0，保证因果律。
- $e^{-i \epsilon_{\mathbf{k}} t}$：相位随时间旋转，这一项代表粒子有能量 $\epsilon_{\mathbf{k}}$。
- 前面的 $-i$ 是因为薛定谔方程左边有一个 $i$，求解的时候到了分母上，就是 $-i$。

下面我们将格林函数转换到频域 $\omega$，对其做傅里叶变换：

$$
G_0(\omega) = \int_{-\infty}^{+\infty} dt \, e^{i\omega t} G_0(t) \tag{116}
$$

代入式（115） 可得：

$$
G_0(\omega) = -i \int_{0}^{+\infty} dt \, e^{i(\omega - \epsilon_{\mathbf{k}}) t} \tag{117}
$$

> **注意：** 因为 $\theta(t)$ 的存在，积分下限从 $-\infty$ 变成了 $0$。

这里出现数学问题：当 $t \to +\infty$ 时，指数函数是振荡且不衰减的，这个积分严格不收敛。因此我们引入一个假设——粒子不是永生的，会随时间极其缓慢地消失。这样在指数上加入一个无穷小的衰减因子 $-\eta t$（其中 $\eta \to 0^+$），上式变成：

$$
G_0(\omega) = -i \int_{0}^{+\infty} dt \, e^{i(\omega - \epsilon_{\mathbf{k}} + i\eta) t} \tag{118}
$$

经过数学推导后，就得到了：

$$
G_0(\omega) = \frac{1}{\omega - \epsilon_{\mathbf{k}} + i\eta} \tag{119}$$

- $+i\eta$ 这一项在物理上代表的边界条件是：保证了粒子只能在 $t>0$ 传播。
- 当分母为 0 时（$\omega = \epsilon_{\mathbf{k}}$），即在 $\epsilon_{\mathbf{k}}$ 处出现一个 $\delta$ 函数式的尖峰——这是电子的本征能量。
- 从时域上看，$G_0(t)$ 的模长为 1（$\eta\to 0$ 极限下），因此粒子寿命无限。

> **说明：**
>
> - 在了解 $G(t)$ 后，我们也要了解它的逆 $G^{-1}$。
> - 格林函数在数学上的定义是某个微分算符的逆，因此 $G^{-1}$ 本质上就是薛定谔方程里的微分算符。
> - 含时薛定谔方程可以写作：
>
> $$i \frac{\partial}{\partial t} \psi(t) = H \psi(t) \tag{120}$$
>
> - 代入格林函数并整理可得：
>
> $$\left( i \frac{\partial}{\partial t} - H \right) G(t) = \delta(t) \tag{121}$$
>
> - 在数学上，把微分算符作用在 $G$ 上会得到一个 $\delta$ 函数。
> - 把 $\left( i \frac{\partial}{\partial t} - H_0 \right)$ 记为 $\hat A$。
> - 那么 $\hat A G = I$，因此 $G$ 只能是 $\hat A$ 的逆。
> - 因此，$G^{-1}$ 等于 $\left( i \frac{\partial}{\partial t} - H_0 \right)$，也就是整体的微分算符。
> - 因此，$G(t)$ 描述粒子的运动状态（解），$G^{-1}$ 描述粒子的运动方程（动力学）。
> - 频域下的 $G^{-1}$ 为：
>
> $$G^{-1}(\mathbf{k}, \omega) = \omega - \epsilon_{\mathbf{k}} + i\eta \tag{122}$$
>
> - 在频域的表达下，它变成了简单的代数，如果再令 $G^{-1}=0$，那么就得到了本征关系：$\omega = \epsilon_{\mathbf{k}}$。

单粒子格林函数满足如下算符方程（后面我们称之为 Dyson 方程）：

$$
G^{-1}(1,2) = G_0^{-1}(1,2) - V_H(1)\delta(1,2) - \Sigma(1,2) \tag{123}
$$

上面我们已经了解了格林函数的逆相当于哈密顿（运动方程）算符。因此，可以写出上式各项的含义：

- $G^{-1}(1,2)$：相互作用系统格林函数的逆。
- $G_0^{-1}(1,2)$：无相互作用（含外势）系统的格林函数逆，完整展开为 $G_0^{-1}(1,2) = \left[i\hbar \frac{\partial}{\partial t_1} + \frac{\hbar^2 \nabla_1^2}{2m} - V_{ext}(1)\right] \delta(1,2)$。
- $V_H(1)$：经典平均静电势（Hartree 势）。
- $\delta(1,2) = \delta(\vec{r}_1-\vec{r}_2)\delta(t_1-t_2)$：时空 $\delta$ 函数。
- $\Sigma(1,2)$：自能函数，包含了所有超越平均场的多体效应（交换与关联）。

> **说明：**
>
> - 准粒子总有效哈密顿量算符可写作：
>
> $$G^{-1}(1,2) = \left[ i\hbar \frac{\partial}{\partial t_1} - H_{\text{total}}^{\text{eff}} \right] \delta(1,2) \tag{124}$$
>
> 上式代表了真实准粒子遵守的运动方程算符。若 $G^{-1} \psi = 0$，那就是真实准粒子的薛定谔方程。
>
> - 自由格林函数的逆：
>
> $$G_0^{-1}(1,2) = \left[ i\hbar \frac{\partial}{\partial t_1} + \frac{\hbar^2 \nabla_1^2}{2m} - V_{ext}(1) \right] \delta(1,2) \tag{125}$$
>
> 上式表示单电子在仅存在核作用下的运动方程算符，这部分没有任何电子之间的关联。
>
> - Hartree 项：
>
> $$V_H(1) \delta(1,2) = \int d3 \, v(1,3) \rho(3) = \int d\mathbf{r}_3 \frac{e^2 \rho(\mathbf{r}_3)}{|\mathbf{r}_1 - \mathbf{r}_3|} \, \delta(1,2) \tag{126}$$
>
> 上式表示电子在 $\mathbf{r}_1$ 感受到的 Hartree 势，来源是其他所有电子在 $\mathbf{r}_3$ 处的密度。它仅包含电子之间的经典排斥相互作用，不含量子的交换-关联作用。
>
> - 最后一项自能 $\Sigma(1,2)$ 没有显式的封闭表达式。它包含了所有经典平均场无法描述的量子效应。
> - 因此，经过全部加和我们可以写出准粒子波动方程：
>
> $$\left[ i\hbar \frac{\partial}{\partial t_1} + \frac{\hbar^2 \nabla_1^2}{2m} - V_{ext}(1) - V_H(1) \right] \psi(1) - \int d2 \, \Sigma(1,2) \psi(2) = 0 \tag{127}$$
>
> - 在上式中，我们对原算符方程两边同乘 $\psi(2)$ 并对 $2$ 积分。因为 $G^{-1}(1,2)$ 将状态从位置 $2$ 映射到位置 $1$，为了得到位置 $1$ 的状态，必须把位置 $2$ 的波函数作为输入。
> - 矩阵形式上，这相当于 $y_i = \sum_j A_{ij} x_j$：我们为了得到 $y_i$，输入必须是 $x_j$。
> - 式（127） 中除了最后一项，所有的 $\delta(1,2)$ 都把对 $2$ 的积分塌缩掉了；**只有最后一项不是对角矩阵**，因此积分必须保留。

最终，我们可以将全相互作用系统的逆格林函数 $G^{-1}(\mathbf{r}_1, t_1; \mathbf{r}_2, t_2)$ 写成如下形式：

$$
\begin{aligned}
G^{-1}(\mathbf{r}_1, t_1; \mathbf{r}_2, t_2) = & \underbrace{\delta(\mathbf{r}_1 - \mathbf{r}_2)\delta(t_1 - t_2)}_{\text{时空对角化因子}} \times \Bigg[ \underbrace{i\hbar \frac{\partial}{\partial t_1}}_{\text{含时演化}} + \underbrace{\frac{\hbar^2 \nabla_{\mathbf{r}_1}^2}{2m}}_{\text{动能}} - \underbrace{V_{ext}(\mathbf{r}_1)}_{\text{原子核势}} - \underbrace{\int d\mathbf{r}_3 \frac{e^2 \rho(\mathbf{r}_3, t_1)}{|\mathbf{r}_1 - \mathbf{r}_3|}}_{\text{Hartree 平均场}} \Bigg] \\
& - \underbrace{\Sigma(\mathbf{r}_1, t_1; \mathbf{r}_2, t_2)}_{\text{量子自能}}
\end{aligned} \tag{128}
$$

上式方括号内的项都是局域的，因为 $\delta(\mathbf{r}_1 - \mathbf{r}_2)\delta(t_1 - t_2)$ 的存在使仅有对角项起作用。最后一项为非局域项，我们将其写成矩阵形式：

$$
\boldsymbol{\Sigma}(\omega) = \begin{pmatrix}
\Sigma_{11}(\omega) & \Sigma_{12}(\omega) & \cdots \\
\Sigma_{21}(\omega) & \Sigma_{22}(\omega) & \cdots \\
\vdots & \vdots & \ddots
\end{pmatrix} \tag{129}
$$

其中，矩阵中每一项都是能量 $\omega$ 的函数。对角元和非对角元具有不同的含义：

**(1) 对角元——轨道自己对自己的影响**

$$
\Sigma_{ii} = \langle \phi_i | \hat{\Sigma} | \phi_i \rangle \tag{130}
$$

它包含实部和虚部：

- 实部 $\text{Re}[\Sigma_{ii}(\omega)]$ 代表能量的重整化——电子在轨道 $i$ 上时极化了周围的环境，导致自己的能量发生变化。因此这项修正了 DFT 计算出的能级：

    $$E_i^{QP} \approx \epsilon_i^{DFT} + \text{Re}[\Sigma_{ii}(E_i)] - V_{xc,ii} \tag{131}$$

    上式代表准粒子能量 = DFT 计算的能量 + GW 计算出的自能 − DFT 中近似的 $V_{xc}$（因为 DFT 计算已经包含了 $V_{xc,ii}$，要去掉以避免重复计数）。

- 虚部 $\text{Im}[\Sigma_{ii}(\omega)]$ 代表寿命的衰减——电子在轨道 $i$ 上会因为散射（电子-电子散射、电声耦合等）而跃迁到其他态，虚部越大寿命越短。

**(2) 非对角元——两个轨道之间的相互作用修正**

$$
\Sigma_{ij} = \langle \phi_i | \hat{\Sigma} | \phi_j \rangle \tag{132}
$$

在一般情况下，$H_{ij}$ 非对角元代表了电子从 $j$ 跳到 $i$ 的概率。但在自能中——正如我们上面讨论的——电子会自己极化周围的环境导致能量变化，电子跳跃时也伴随周围被极化的环境一起跳跃。因此这一项包含了电子从 $j$ 态出发、受到扰动到了 $i$ 态、然后到达的复杂过程。

同样地，这一项也包含实部和虚部：

- 实部 $\text{Re}[\Sigma_{ij}]$ 代表跃迁的耦合强度。
- 虚部 $\text{Im}[\Sigma_{ij}]$ 代表跳跃过程中受到的阻力，即发生散射的强度。

在绝大多数半导体中，非对角项通常被忽略（称为 $G_0W_0$），此时对角元占主导——我们认为 DFT 计算出的波函数已经足够好，不太需要非对角元修正。但在强关联体系中，这是不可忽略的。

#### Dyson 方程

我们将格林函数满足的算符方程 (116) 写成最简单的形式：

$$
G^{-1} = G_0^{-1} - \Sigma \tag{133}
$$

上面我们已经了解了它的物理含义：

> 真实运动方程 = 自由运动方程 − 修正项

上式两边同时从左边乘以自由格林函数 $G_0$ 可得：

$$
G_0 \cdot G^{-1} = G_0 \cdot (G_0^{-1} - \Sigma) \tag{134}
$$

其中 $G_0 \cdot G_0^{-1}$ 为单位矩阵 $I$，整理可得：

$$
G_0 G^{-1} = I - G_0 \Sigma \tag{135}
$$

等式两边右乘全格林函数 $G$，消掉左边的 $G^{-1}$：

$$
(G_0 G^{-1}) \cdot G = (I - G_0 \Sigma) \cdot G \tag{136}
$$

整理可得：

$$
G = G_0 + G_0 \Sigma G \tag{137}
$$

上式就是 **Dyson 方程**，也是我们前面提到的算符方程（式 123）的等价形式。等式两边都有 $G$，看起来是递归的。我们把 $G$ 代入到上式右边的 $G$，整理可得：

$$
G = G_0 + G_0 \Sigma G_0 + G_0 \Sigma G_0 \Sigma G \tag{138}
$$

如果一直代入，就会得到无限的迭代级数：

$$
G = G_0 + G_0 \Sigma G_0 + G_0 \Sigma G_0 \Sigma G_0 + G_0 \Sigma G_0 \Sigma G_0 \Sigma G_0 + \cdots \tag{139}
$$

在 Dyson 方程中，我们把 $G_0$ 叫做传播子。上式的物理含义是：

> 粒子从 $i$ 到 $j$ 的总传播概率幅等于所有路径之和，包含粒子所有可能散射的路径叠加：
>
> - 第一项 $G_0$ 代表粒子没有发生散射，直接从 $i$ 到 $j$；
> - 第二项 $G_0 \Sigma G_0$ 代表粒子传播 $G_0$，散射了一次 $\Sigma$，然后又传播 $G_0$，到了 $j$；
> - 第三项则是散射两次……

我们切换到频域空间来看式（138） 的最简单情形（动量守恒下 $\Sigma$ 在 $\mathbf k$ 表象对角）：

$$
G(\mathbf k, \omega) = G_0(\mathbf k, \omega) + G_0(\mathbf k, \omega) \Sigma(\mathbf k, \omega) G(\mathbf k, \omega) \tag{140}
$$

将含有 $G$ 的项移到一边并代入 $G_0 = \frac{1}{\omega - \epsilon_k}$，可得：

$$
G(\mathbf{k}, \omega) = \frac{\frac{1}{\omega - \epsilon_k}}{1 - \frac{\Sigma}{\omega - \epsilon_k}} = \frac{1}{\omega - \epsilon_k - \Sigma(\mathbf{k}, \omega)} \tag{141}
$$

可以看到，原本在 $\epsilon_k$ 的 DFT 能级现在移动到了 $\epsilon_k + \Sigma$ 的 GW 能级。它对应了能带图中的 $y$ 轴；$\mathbf k$ 代表了 $\mathbf k$ 空间中不同的 $\mathbf k$ 点。

当 $\omega = \epsilon_{\mathbf{k}} + \text{Re}\,\Sigma(\mathbf{k}, \omega)$ 时，分母实部为 0，$|G(\mathbf{k}, \omega)|$ 出现峰值，它代表了这里出现了一条能带。这个时刻的 $\omega$ 值就是准粒子能量。

#### 双粒子格林函数

我们需要算格林函数 $G$，因为它包含了能带信息。但是仅仅从 $G$ 的定义来看，我们无法直接解出来具体的能带结构，需要找到一个 $G$ 满足的方程，然后解这个方程。

$$
G(1,2) = -i \langle \Psi_0 | T \, \hat\psi(1) \hat\psi^\dagger(2) | \Psi_0 \rangle \tag{142}
$$

上式是格林函数的定义。我们了解算符的定义，但不清楚 $|\Psi_0\rangle$ 的具体形式——因为我们面对的是一个真实多体系统。

我们要找的能带本征能量，本质上是微分方程的特征值（对应时间导数 $i \frac{\partial}{\partial t}$）。因此，我们需要对格林函数关于时间求导；但格林函数中的 $T$ 算符需要先写成数学上的阶跃函数形式才能求导：

$$
T \hat\psi(1) \hat\psi^\dagger(2) = \theta(t_1 - t_2) \hat\psi(1) \hat\psi^\dagger(2) - \theta(t_2 - t_1) \hat\psi^\dagger(2) \hat\psi(1) \tag{143}
$$

前面同样提到过，当 $t_1 > t_2$ 时仅保留右侧第一项，反之保留第二项；负号来自费米子算符交换。

因此，格林函数 $G$ 可以写作：

$$
G(1,2) = -i \left[ \theta(t_1 - t_2) \langle \hat\psi(1) \hat\psi^\dagger(2) \rangle - \theta(t_2 - t_1) \langle \hat\psi^\dagger(2) \hat\psi(1) \rangle \right] \tag{144}
$$

接下来对时间 $t_1$ 求导。求导会同时作用在 $\theta$ 函数和 $\hat\psi(1)$ 算符上。我们将对阶跃函数求导得到的部分称为**边界项**，将对 $\hat\psi(1)$ 求导得到的部分称为**动力学项**：

$$
i \frac{\partial G}{\partial t_1} = i \left( \frac{\partial G}{\partial t_1} \right)_{\theta} + i \left( \frac{\partial G}{\partial t_1} \right)_{\psi} \tag{145}
$$

**(1) 边界项：** 对 $\theta$ 函数的求导是 $\delta$ 函数，即 $\frac{\partial}{\partial t_1} \theta(t_1 - t_2) = \delta(t_1 - t_2)$。因此：

$$
i \left( \frac{\partial G}{\partial t_1} \right)_{\theta} = \delta(t_1 - t_2) \langle \hat\psi(1) \hat\psi^\dagger(2) + \hat\psi^\dagger(2) \hat\psi(1) \rangle \tag{146}
$$

利用反对易关系 $\{\hat\psi(\mathbf{r}_1), \hat\psi^\dagger(\mathbf{r}_2)\} = \delta(\mathbf{r}_1 - \mathbf{r}_2)$，边界项直接变成：

$$
i \left( \frac{\partial G}{\partial t_1} \right)_{\theta} = \delta(t_1 - t_2) \delta(\mathbf{r}_1 - \mathbf{r}_2) = \delta(1,2) \tag{147}
$$

**(2) 动力学项：**

$$
i \left( \frac{\partial G}{\partial t_1} \right)_{\psi} = -i \left\langle T \left( i \frac{\partial \hat\psi(1)}{\partial t_1} \right) \hat\psi^\dagger(2) \right\rangle \tag{148}
$$

我们直接得到了海森堡运动方程的形式：

$$
i\hbar \frac{d \hat{A}(t)}{dt} = [\hat{A}(t), \hat{H}] \tag{149}
$$

> **说明：** 算符的时间演化属于海森堡绘景，必须遵守海森堡运动方程。

因此可以写作：

$$
i \left( \frac{\partial G}{\partial t_1} \right)_{\psi} = -i \langle T [\hat\psi(1), \hat H] \hat\psi^\dagger(2) \rangle \tag{150}
$$

为了计算对易子，我们将哈密顿量拆分为动能和相互作用两部分：$\hat H = \hat H_0 + \hat H_{\text{int}}$，其中 $\hat H_0$ 包含动能和外势能，$\hat H_{\text{int}}$ 包含电子间的相互作用能。

**动能部分：**

$$
\left[\hat\psi(\mathbf{r}_1), \int d\mathbf{x} \, \hat\psi^\dagger(\mathbf{x}) h_0(\mathbf{x}) \hat\psi(\mathbf{x})\right] \tag{151}
$$

利用对易公式 $[A, BC] = \{A, B\}C - B\{A, C\}$：

$$
[\hat\psi, \hat H_0] = \int d\mathbf{x} \left[ \{ \hat\psi(\mathbf{r}_1), \hat\psi^\dagger(\mathbf{x}) \} h_0(\mathbf{x}) \hat\psi(\mathbf{x}) - \hat\psi^\dagger(\mathbf{x}) h_0(\mathbf{x}) \{ \hat\psi(\mathbf{r}_1), \hat\psi(\mathbf{x}) \} \right] \tag{152}$$

其中，右侧第二项两个湮灭算符的反对易子为 0；第一项产生湮灭算符的反对易子为 $\delta(\mathbf r_1 - \mathbf x)$，与积分符号抵消：

$$
[\hat\psi, \hat H_0] = h_0(\mathbf{r}_1) \hat\psi(\mathbf{r}_1) = h_0(1) \hat\psi(1) \tag{153}
$$

**相互作用部分：**

$$
\hat H_{\text{int}} = \frac{1}{2} \int d\mathbf{x} \, d\mathbf{y} \, v(\mathbf{x},\mathbf{y}) \hat\psi^\dagger(\mathbf{x}) \hat\psi^\dagger(\mathbf{y}) \hat\psi(\mathbf{y}) \hat\psi(\mathbf{x}) \tag{154}
$$

需要计算对易子 $[\hat\psi(\mathbf{r}_1), \hat\psi^\dagger(\mathbf{x}) \hat\psi^\dagger(\mathbf{y}) \hat\psi(\mathbf{y}) \hat\psi(\mathbf{x})]$。看起来繁琐，但产生算符之间反对易子为 0，产生-湮灭反对易子是 $\delta$ 函数，所以最终化简得到：

$$
[\hat\psi(1), \hat H_{\text{int}}] = \int d\mathbf{x} \, v(\mathbf{r}_1, \mathbf{x}) \hat\psi^\dagger(\mathbf{x}) \hat\psi(\mathbf{x}) \hat\psi(\mathbf{r}_1) = \int d3 \, v(1,3) \hat\psi^\dagger(3) \hat\psi(3) \hat\psi(1) \tag{155}
$$

这里 $3$ 代表空间中任何一个位置，由于积分符号，效果上是把所有位置 $3$ 处的电子贡献加起来。

把两部分合并整理：

$$
\left[ i \frac{\partial}{\partial t_1} - h_0(1) \right] G(1,2) = \delta(1,2) - i \int d3 \, v(1,3) \langle T \hat\psi^\dagger(3)\hat\psi(3)\hat\psi(1) \hat\psi^\dagger(2) \rangle \tag{156}
$$

我们可以看到上式右侧出现了 $\hat\psi^\dagger \hat\psi \hat\psi \hat\psi^\dagger$（四个场算符），因此被称为**双粒子格林函数** $G_2$。

它说明了：如果我们要求单粒子 $G$，就要先求双粒子 $G_2$；要求 $G_2$，得先求 $G_3$……这变成了 BBGKY 等级，似乎陷入了死循环。

#### Hedin 方程组

##### Schwinger 导数

为了打破上式的死循环，Schwinger 引入了一个随时间变化的虚拟外场 $\phi$。经过数学推导，双粒子格林函数可以表示为：

$$
\langle T \hat\psi^\dagger(3)\hat\psi(3)\hat\psi(1)\hat\psi^\dagger(2) \rangle = i \frac{\delta G(1,2)}{\delta \phi(3)} + G(1,2) \langle \rho(3) \rangle \tag{157}
$$

代回到运动方程 (145)：

$$
\left[i\frac{\partial}{\partial t_1} - h_0(1) - V_H(1)\right] G(1,2) = \delta(1,2) + i \int d3 \, v(1,3) \frac{\delta G(1,2)}{\delta \phi(3)} \tag{158}
$$

其中多出来的 $V_H(1)$ 是做 Schwinger 替换后吸收掉式（157） 第二项得到的 Hartree 势。我们比较一下 Dyson 方程：

$$
\left[i\frac{\partial}{\partial t_1} - h_0(1) - V_H(1)\right] G(1,2) = \delta(1,2) + \int d3 \, \Sigma(1,3) G(3,2) \tag{159}
$$

我们就能得到 $\Sigma$ 的形式：

$$
\int d3 \, \Sigma(1,3) G(3,2) = i \int d3 \, v(1,3) \frac{\delta G(1,2)}{\delta \phi(3)} \tag{160}
$$

上式中，$2$ 是起始点，$1$ 是终点，$3$ 是粒子发生散射的中间点。

这个方程的物理含义是：自能 $\Sigma$ 是电子通过库仑力 $v$ 和它自己引发的环境变化 $\frac{\delta G}{\delta \phi}$ 之间的耦合的结果——

- 左边 $G(3,2)$ 是传播子，代表电子从起点 $2$ 跑到中间点 $3$。$\Sigma(1,3)$ 则是电子在 $3$ 受到散射、到达位置 $1$。
- 右边 $v(1,3)$ 是位置 $1$ 和位置 $3$ 之间的库仑相互作用，$\frac{\delta G(1,2)}{\delta \phi(3)}$ 是电子从 $2$ 到 $1$ 的路途中，在位置 $3$ 发生散射的概率幅。

整体描述了：电子从 $2$ 到 $3$，经过 $3$ 时由于排斥周围电子在 $3$ 产生了一个局部带正电的环境；电子随后跑到 $1$，在 $1$ 受到刚刚 $3$ 形成的正电环境影响——受到 $v(1,3)$ 的库仑相互作用。两边描述的物理过程是一致的。

##### 逆介电函数和顶点函数

我们有了式（160），为了解出自能 $\Sigma$，需要使用链式法则处理 $\frac{\delta G}{\delta \phi}$：

$$
\frac{\delta G(1,2)}{\delta \phi(3)} = \int d4 \, \frac{\delta G(1,2)}{\delta V_{\text{tot}}(4)} \frac{\delta V_{\text{tot}}(4)}{\delta \phi(3)} \tag{161}
$$

其中 $V_{\text{tot}}$ 是包括外场扰动后的总有效势：$V_{\text{tot}} = V_{ext} + V_H + \phi$（关心变化时可写 $\delta V_{\text{tot}} = \delta\phi + \delta V_H$）。

这里出现了第四个位置 $4$，因为我们在位置 $3$ 处施加了一个虚拟外场 $\phi(3)$，这会导致不仅 $3$ 位置的电势改变，全空间其他所有位置的电势都会变化——因此我们要将所有受影响的位置 $4$ 加起来。

我们把 $\frac{\delta V_{\text{tot}}}{\delta \phi}$ 定义为逆介电函数 $\epsilon^{-1}$：

$$
\frac{\delta V_{\text{tot}}(4)}{\delta \phi(3)} = \epsilon^{-1}(4,3) \tag{162}
$$

它描述了外场 $\phi$ 如何引起总势场的变化（产生屏蔽效应）。

我们把式（160） 右侧库仑势 $v$ 与逆介电函数的卷积定义为 $W(1,4)$：

$$
W(1,4) = \int d3 \, v(1,3) \frac{\delta V_{\text{tot}}(4)}{\delta \phi(3)} \tag{163}
$$

这就是 GW 近似中的 $W$——**屏蔽库仑相互作用**。

我们再看链式法则得到的另一项。根据数学恒等式 $\frac{\delta G}{\delta V} = -G \frac{\delta G^{-1}}{\delta V} G$，我们定义：

$$
\Gamma(1,2;3) = -\frac{\delta G^{-1}(1,2)}{\delta V_{\text{tot}}(3)} \tag{164}
$$

因此可得：

$$
\frac{\delta G}{\delta V_{\text{tot}}} = G \, \Gamma \, G \tag{165}
$$

$\Gamma$ 我们称之为**顶点函数（vertex function）**。它的物理意义是：在 $3$ 处的势场变化对电子从 $1$ 到 $2$ 的逆传播有什么影响——即电子在 $3$ 点感受到外场时发生的"非平凡散射"。

我们将得到的项全部代回式（160），最后可得（紧凑算符记号下）：

$$
\Sigma G = i \, W G \, \Gamma G \tag{166}
$$

我们将顶点函数中的 $G^{-1}$ 替换成 Dyson 方程中的形式：

$$
\Gamma(1,2;3) = -\frac{\delta}{\delta V(3)} \left[ G_0^{-1}(1,2) - V(1)\delta(1,2) - \Sigma(1,2) \right] \tag{167}
$$

其中：

- 第一项：自由格林函数 $G_0$ 只包含动能和外势，与总势场无关，因此为 0。
- 第二项：

    $$-\frac{\delta [-V(1)\delta(1,2)]}{\delta V(3)} = \frac{\delta V(1)}{\delta V(3)} \delta(1,2) = \delta(1,3)\delta(1,2) \tag{168}$$

- 第三项含 $\Sigma$，需要进一步链式法则。

因此，整体可以写作：

$$
\Gamma(1,2;3) = \delta(1,2)\delta(1,3) + \frac{\delta \Sigma(1,2)}{\delta V(3)} \tag{169}
$$

对其中 $\frac{\delta \Sigma(1,2)}{\delta V(3)}$ 再使用链式法则引入 $G$（因为 $\Sigma$ 是 $G$ 的泛函，而 $G$ 依赖于 $V$）：

$$
\frac{\delta \Sigma(1,2)}{\delta V(3)} = \int d4 \, d5 \, \frac{\delta \Sigma(1,2)}{\delta G(4,5)} \frac{\delta G(4,5)}{\delta V(3)} \tag{170}
$$

其中 $\frac{\delta G(4,5)}{\delta V(3)}$ 这项根据前面的式（165） 可写为：

$$
\frac{\delta G(4,5)}{\delta V(3)} = \int d6 \, d7 \, G(4,6) \, \Gamma(6,7; 3) \, G(7,5) \tag{171}
$$

将其代回式（169），最终可以得到顶点方程：

$$
\Gamma(1,2;3) = \delta(1,2)\delta(1,3) + \int d4 \, d5 \, d6 \, d7 \, \frac{\delta \Sigma(1,2)}{\delta G(4,5)} G(4,6) G(7,5) \Gamma(6,7;3) \tag{172}
$$

##### 库仑屏蔽和极化率

根据式（162） 以及总有效势的定义，$\frac{\delta V(4)}{\delta \phi(3)}$ 可以写作：

$$
\frac{\delta V(4)}{\delta \phi(3)} = \frac{\delta \phi(4)}{\delta \phi(3)} + \frac{\delta V_H(4)}{\delta \phi(3)} \tag{173}
$$

其中 $\frac{\delta \phi(4)}{\delta \phi(3)} = \delta(4,3)$。Hartree 势的定义是：

$$
V_H(4) = \int dX \, v(4, X) \rho(X) \tag{174}
$$

它的导数可以写成：

$$
\frac{\delta V_H(4)}{\delta \phi(3)} = \int dX \, v(4, X) \frac{\delta \rho(X)}{\delta \phi(3)} \tag{175}
$$

我们依然可以使用链式法则，将 $\frac{\delta \rho}{\delta \phi}$ 展开：

$$
\frac{\delta \rho(X)}{\delta \phi(3)} = \int dY \, \frac{\delta \rho(X)}{\delta V(Y)} \frac{\delta V(Y)}{\delta \phi(3)} \tag{176}
$$

其中根据极化率（不可约极化）的定义 $P(X, Y) = \frac{\delta \rho(X)}{\delta V(Y)}$，上式可以写作：

$$
\frac{\delta \rho(X)}{\delta \phi(3)} = \int dY \, P(X, Y) \frac{\delta V(Y)}{\delta \phi(3)} \tag{177}
$$

因此式（163） 右侧第二项变成：

$$
\frac{\delta V_H(4)}{\delta \phi(3)} = \int dX \, dY \, v(4, X) P(X, Y) \frac{\delta V(Y)}{\delta \phi(3)} \tag{178}
$$

将上式代回 (153)，那么 $W$ 可以写作：

$$
W(1,4) = \int d3 \, v(1,3) \left[ \delta(4,3) + \int dX \, dY \, v(4, X) P(X, Y) \frac{\delta V(Y)}{\delta \phi(3)} \right] \tag{179}
$$

- 第一项为**裸库仑相互作用**，代表两个电子之间的纯库仑相互作用：

    $$\int d3 \, v(1,3) \delta(4,3) = v(1,4) \tag{180}$$

- 第二项为**屏蔽修正**：

    $$\int dX \, dY \, v(4, X) P(X, Y) \left[ \int d3 \, v(1,3) \frac{\delta V(Y)}{\delta \phi(3)} \right] \tag{181}$$

    屏蔽修正代表了：电子 A 排斥了周围的电子并"吸引"空穴，导致电子 A 周围形成了一圈带正电的极化云；电子 B 原本仅感受到 A 的排斥作用，但因为正极化云的存在，被抵消了一部分——这就是屏蔽。

最终可以得到 $W$ 的 Dyson 形式：

$$
W(1,2) = v(1,2) + \int d3 \, d4 \, v(1,3) \, P(3,4) \, W(4,2) \tag{182}$$

如果我们希望用格林函数 $G$ 把极化率 $P$ 表达出来，就能把微观量（$G$）和宏观响应（$P$）联系起来。

在前面（式 71），我们推导出过密度算符 $\hat\rho(1) = \hat\psi^\dagger(1) \hat\psi(1)$。根据格林函数的定义 $G(1,2) = -i \langle T \hat\psi(1)\hat\psi^\dagger(2) \rangle$，如果 $2$ 趋近于 $1$（取时间无穷小正向极限 $1^+$），那么粒子数密度可以用格林函数写出：

$$
\rho(1) = \langle \hat\psi^\dagger(1) \hat\psi(1) \rangle = -i G(1, 1^+) \tag{183}
$$

这样就能根据极化率的定义写出格林函数的表达：

$$
P(1,2) = \frac{\delta \rho(1)}{\delta V(2)} = -i \frac{\delta G(1, 1^+)}{\delta V(2)} \tag{184}
$$

由式（165） $\frac{\delta G}{\delta V} = G\Gamma G$，直接代回式（184），可以写出极化率：

$$
P(1,2) = -i \int d3 \, d4 \, G(1, 3) G(4, 1^+) \Gamma(3, 4; 2) \tag{185}
$$

##### 封闭循环与 $G_0W_0$

根据上面的推导，我们得到了五个相互依赖的方程——**Hedin 方程组**。重新写出并看一下它们的物理意义：

**(1) 极化率方程：**

$$
P(1,2) = -i \int d3 \, d4 \, G(1,3) \, G(4,1^+) \, \Gamma(3,4;2) \tag{186}
$$

它描述了介质对外场的响应能力。尽管 $G \times G$ 看起来仅描述了一对粒子，但格林函数本身已经包括了所有电子能带和所有量子态信息的加和，因此 $P$ 表示的是整个电子海的集体响应。顶点 $\Gamma$ 描述了这一产生过程中的相互作用修正。

**(2) 屏蔽相互作用方程：**

$$
W(1,2) = v(1,2) + \int d3 \, d4 \, v(1,3) \, P(3,4) \, W(4,2) \tag{187}
$$

它描述了：裸库仑力 $v$ 通过被介质极化云 $P$ 反复散射，最终变成屏蔽相互作用 $W$。它表明粒子产生的极化场会导致库仑力变弱。

**(3) 自能方程：**

$$
\Sigma(1,2) = i \int d3 \, d4 \, G(1,3) \, W(1^+,4) \, \Gamma(3,2;4) \tag{188}
$$

它描述了电子传播过程中：通过与自身形成的极化场耦合所产生的能量修正，以及与所有其他电子发生非经典、非局域的纠缠。中间的耦合强度由 $\Gamma$ 决定。

**(4) 顶点方程：**

$$
\Gamma(1,2;3) = \delta(1,2)\delta(1,3) + \int d4 \, d5 \, d6 \, d7 \, \frac{\delta \Sigma(1,2)}{\delta G(4,5)} G(4,6) G(7,5) \Gamma(6,7;3) \tag{189}
$$

它描述了粒子间相互作用的顶角修正。真实的电子不仅仅是一个点电荷，它周围笼罩着一团复杂的极化云，使得它与外界场的相互作用不再是简单的点接触（$\delta$ 函数），而是复杂的散射过程。

**(5) 格林函数 Dyson 方程：**

$$
G(1,2) = G_0(1,2) + \int d3 \, d4 \, G_0(1,3) \, \Sigma(3,4) \, G(4,2) \tag{190}
$$

它把所有电子间的相互作用全部封装到 $\Sigma$ 中，加到自由电子 $G_0$ 上，从而得到相互作用系统下真实的传播子 $G$。

上面五个方程构成一个封闭的自洽循环：

如果我们知道初始的 $G$ 和 $\Gamma$（假设 $\Gamma = \delta\delta$），我们就能算出介质的极化 $P$；极化又能算出屏蔽相互作用 $W$（相互作用不是常数，而是由介质状态决定的）；我们就能根据 $W, G, \Gamma$ 算出自能 $\Sigma$（如果不知道电子在什么样的介质中运动，就不知道它会被修正多少）；这时根据新的自能又能算出新的顶点修正 $\Gamma$（当电子略微改变运动方式时，环境立刻重新组织响应它）；自能一旦改变，电子的传播 $G$ 就必须改变——回到循环开始。

上面的方程组意味着：电子不是在一个固定环境中运动，而是在一个被它自己不断重塑的环境中运动。构成循环的原因是：电子的传播、介质的响应和相互作用的有效形式并非独立物理量，而是由同一批电子共同决定的集体结果；任何一个量的改变都会通过其他量反馈回来，迫使它必须以循环、自洽的方式求解。

那么我们能不能像 DFT 那样直接迭代求解 GW 方程组？

回到 $\Sigma(\omega)$ 中，前面推导过 $\Sigma(\omega)$ 是存在虚部的，它代表了粒子的寿命（能量-时间不确定关系）：

$$
\Delta E \cdot \Delta t \sim \hbar \tag{191}
$$

这意味着 GW 里的电子不再是一个锐利的粒子，而是一个有宽度的波包。GW 近似中电子一定会因为散射而到达别的状态，因此 $\Delta t$ 是有限的，$\Delta E$ 必须是非零的，这会导致粒子在能谱中出现能量展宽。但是 DFT 里电子寿命是无限的，因此能带图每条线都是无限细的。当我们把有宽度的波包粒子代回去重新计算新的 $W$，得到的屏蔽效应会变弱、相互作用变强，粒子的寿命就会更短，波包会更展宽，粒子更模糊。因此迭代后的能带不再清晰，而是糊成一团。

更重要的是，GW 不是变分理论。我们使用自洽迭代求解 KS 方程的时候，理论基础是变分原理：存在一个明确的泛函，其真实解是该泛函的极值（通常是最小值）。任意试探波函数的能量期望值一定大于等于基态能量，即 $E[n] \geq E[n_0]$，唯一变量是电子密度。但 GW 的方程不是最小化某个物理量的结果，而仅仅是满足某些自洽关系，因此迭代不能保证收敛到物理解。

为了打开这组方程组的循环，我们直接忽略顶点修正，强行让 $\Gamma(1,2;3) = \delta(1,2)\delta(1,3)$（即纯粹的点接触），代回后所有的 $\Gamma$ 积分都化简。这时，极化率变成了：

$$
P(1,2) = -i \, G(1,2) \, G(2,1) \tag{192}
$$

> **注意：** 这里通过 $\Gamma$ 的两个 $\delta$ 强制 $3 = 4 = 2$，得到的 $\delta$ 函数消掉了两个积分变量。

此时极化率变成了电子和空穴的简单乘积。自能方程通过同样的化简变成：

$$
\Sigma(1,2) = i \, G(1,2) \, W(1^+, 2) \tag{193}
$$

这就是为什么它叫做 **GW 近似**——电子自能只取决于传播 $G$ 与屏蔽相互作用 $W$ 的乘积。屏蔽方程由于极化率的简化而被简化，但形式没有发生改变。

> 之所以叫**准粒子近似**：这里的电子是有寿命的，最终会散射消失；并且它不仅是一个电子，还包括了它周围被它极化的整体电子云。

虽然我们现在知道这组方程怎么求解，但是 GW 的第一步是构建无相互作用格林函数 $G_0$，即：

$$
G_0(\mathbf{r}, \mathbf{r}', \omega) = \sum_{n\mathbf{k}} \frac{\phi_{n\mathbf{k}}(\mathbf{r}) \phi_{n\mathbf{k}}^*(\mathbf{r}')}{\omega - \epsilon_{n\mathbf{k}} \pm i\eta} \tag{194}
$$

> **注意：**
>
> - 这是实空间表象下的格林函数，而我们前面推导的式（119） 则是本征态表象下的。我们可以使用完备性关系：
>
> $$\hat{1} = \sum_{n\mathbf{k}} |n\mathbf{k}\rangle \langle n\mathbf{k}| \tag{195}$$
>
> - 将格林函数转换为：
>
> $$\begin{aligned}
G(\mathbf{r}, \mathbf{r}') &= \langle \mathbf{r} | \hat{G} | \mathbf{r}' \rangle \\
&= \langle \mathbf{r} | \left( \sum_{n\mathbf{k}} |n\mathbf{k}\rangle \langle n\mathbf{k}| \right) \hat{G} \left( \sum_{m\mathbf{q}} |m\mathbf{q}\rangle \langle m\mathbf{q}| \right) | \mathbf{r}' \rangle
\end{aligned} \tag{196}$$
>
> - 因为 $\hat{G}$ 在本征基底下是对角的（$\langle n\mathbf{k} | \hat{G} | m\mathbf{q} \rangle = G_{n\mathbf{k}} \delta_{nm}\delta_{\mathbf{kq}}$），因此双重求和退化为单重求和：
>
> $$\begin{aligned}
G(\mathbf{r}, \mathbf{r}') &= \sum_{n\mathbf{k}} \underbrace{\langle \mathbf{r} | n\mathbf{k}\rangle}_{\phi_{n\mathbf{k}}(\mathbf{r})} \cdot \underbrace{\langle n\mathbf{k} | \hat{G} | n\mathbf{k} \rangle}_{\frac{1}{\omega - \epsilon_{n\mathbf{k}}}} \cdot \underbrace{\langle n\mathbf{k} | \mathbf{r}' \rangle}_{\phi_{n\mathbf{k}}^*(\mathbf{r}')} \\
&= \sum_{n\mathbf{k}} \frac{\phi_{n\mathbf{k}}(\mathbf{r}) \phi_{n\mathbf{k}}^*(\mathbf{r}')}{\omega - \epsilon_{n\mathbf{k}}}
\end{aligned} \tag{197}$$

对于 $G_0$ 来说，我们需要上式中的波函数和能级。DFT 的能量虽然不准确，但波函数通常是可靠的。因此 GW 近似不再去修正波函数，而是去修正能量。

我们现在要求解准粒子哈密顿量 $H_{QP}$，已有 DFT 的哈密顿量 $H_{DFT}$：

$$
H_{QP} = T + V_{ext} + V_H + \boldsymbol{\Sigma} \tag{198}
$$

$$
H_{DFT} = T + V_{ext} + V_H + \boldsymbol{V_{xc}} \tag{199}
$$

因此我们需要解出来：

$$
H_{QP} = H_{DFT} + (\boldsymbol{\Sigma} - \boldsymbol{V_{xc}}) \tag{200}
$$

虽然 $\Sigma$ 是非对角的，但 $\boldsymbol V_{xc}$ 在 KS 轨道基底下同样含有量级与之相当的非对角元，因此微扰项 $(\Sigma - V_{xc})$ 的非对角元会大量抵消，仅保留对角占优结构。因此我们仅计算对角元：

$$
E_{n} \approx \epsilon_{n}^{DFT} + Z_n \langle \phi_n | \Sigma(\epsilon_n) - V_{xc} | \phi_n \rangle \tag{201}
$$

上式中 $\epsilon_{n}^{DFT}$ 是 DFT 计算的本征值；$\Sigma(\epsilon_n) - V_{xc}$ 是用自能替换掉 DFT 中近似的交换关联势；$Z_n$ 是**准粒子重整化因子**（$Z_n = \left(1 - \partial \Sigma / \partial \omega \big|_{\omega=\epsilon_n}\right)^{-1}$），代表准粒子峰的相干谱权重。$Z_n$ 介于 0 和 1 之间——典型半导体中为 $0.7\sim0.8$，剩下的权重转移到非相干"卫星峰"上。

因此，在得到 DFT 计算的 $\epsilon_0$ 和 $\phi_0$ 后，构建格林函数 $G_0$ 和屏蔽 $W_0$，算出自能 $\Sigma = i G_0 W_0$，进而得到准粒子能量：

$$
E_{QP} = \epsilon_0 + Z \langle \Sigma - V_{xc} \rangle \tag{202}
$$

这时直接输出准粒子能量 $E_{QP}$，不再进行循环——此时得到的就是 $G_0 W_0$ 近似。

---

### Bethe–Salpeter 方程

#### BSE 哈密顿量

尽管我们在 GW 过程中涉及了"电子-空穴对"型的极化率 $P = -i GG$，但 GW 关心的本质上仍是单个电子——极化率的作用只是改变了单个电子感受到的有效势，我们关心的也只是单个电子被屏蔽后的样子。

因此，我们需要使用 **Bethe–Salpeter 方程（BSE）** 去解电子-空穴对的薛定谔方程，才能得到电子和空穴是如何相互作用、形成束缚激子的。我们的目的很明确：得到双粒子格林函数。

为了得到双粒子格林函数，我们需要解以下方程：

$$
L(1,2;1',2') = \frac{\delta G(1,1')}{\delta U(2',2)} \tag{203}
$$

其中 $U(2', 2)$ 是外加势场（非局域），它代表在 $2$ 消灭一个粒子、在 $2'$ 产生一个粒子。$\delta U$ 代表这只是一个微扰，$\delta G$ 是电子传播受到的相应变化。

物理上：在 $(2', 2)$ 对系统施加一个微扰，那么在 $(1, 1')$ 的电子传播状态会发生什么变化。左侧的 $L$ 本质上是电子-空穴对的传播子。

> **说明：** 为什么对电子传播施加一个微扰就变成了双粒子的传播子？我们做以下推导。
>
> - 在量子力学中，系统的状态随时间演化由时间演化算符 $S$ 控制：
>
> $$S = \exp\left( -i \int H(t) \, dt \right) \tag{204}$$
>
> - 我们在哈密顿量 $H$ 里施加一个外场 $U(\mathbf{r}, t)$，它会和电子密度 $\hat{\rho} = \hat{\psi}^\dagger \hat{\psi}$ 耦合：
>
> $$H_{\text{total}} = H_0 + \int U(\mathbf{r}, t) \, \hat{\psi}^\dagger(\mathbf{r}) \hat{\psi}(\mathbf{r}) \, d\mathbf{r} \tag{205}$$
>
> - 因此，时间演化算符变为：
>
> $$S[U] = \exp\left( -i \int (H_0 + U \cdot \hat{\psi}^\dagger \hat{\psi}) \, dt \right) \tag{206}$$
>
> - 如果我们对 $U$ 求导，那么 $U$ 的系数（$\hat{\psi}^\dagger \hat{\psi}$）会出现在前面：
>
> $$\frac{\delta S}{\delta U} = \left( -i \hat{\psi}^\dagger \hat{\psi} \right) \times \exp(\dots) \tag{207}$$
>
> - 但我们要求 $\frac{\delta G(1,1')}{\delta U(2',2)}$，需要使用链式法则。在此之前，要写出格林函数包含时间演化算符 $S$ 的完整定义：
>
> $$G(1, 1') = -i \frac{ \langle \Phi_0 | T [ S \, \hat{\psi}(1) \hat{\psi}^\dagger(1') ] | \Phi_0 \rangle }{ \langle \Phi_0 | S | \Phi_0 \rangle } \tag{208}$$
>
> - 因此可以写出：
>
> $$\frac{\delta G}{\delta U} = \frac{-i \langle T \, S \, \hat{\psi}(1) \hat{\psi}^\dagger(1') \hat{\psi}^\dagger(2) \hat{\psi}(2') \rangle}{\langle S \rangle} - G(1, 1') \cdot \frac{-i \langle T \, S \, \hat{\psi}^\dagger(2) \hat{\psi}(2') \rangle}{\langle S \rangle} \tag{209}$$
>
> - 第一项就是双粒子格林函数 $G_2(1, 2'; 1', 2)$，它包含了四个场算符。
> - 第二项是单粒子格林函数乘以系统平均密度，即 $G(1, 1') \times \langle \rho(2',2) \rangle$。它代表了非关联部分，因为这是两个独立事件的乘积，二者没有发生任何相互作用。
> - 如果我们希望知道外场 $U$ 对电子传播 $G$ 的纯关联贡献，必须减去这个非关联部分——剩下的才是关联 $L$：
>
> $$L(1,2;1',2') = \frac{\delta G(1,1')}{\delta U(2',2)} \tag{210}$$

我们现在得到了双粒子的传播子定义，但是我们需要找到双粒子传播子的动力学方程，即 $L$ 满足什么动力学规律。

式（210） 是 $L$ 的定义式，而动力学传播要从这里出发再推导。我们前面知道，格林函数和它的逆满足：

$$
G \, G^{-1} = I \tag{211}
$$

我们对上式两边对 $U$ 求导，然后右乘 $G$ 并移项可得：

$$
\frac{\delta G}{\delta U} G^{-1} + G \frac{\delta G^{-1}}{\delta U} = 0 \quad \Longrightarrow \quad \frac{\delta G}{\delta U} = -G \left( \frac{\delta G^{-1}}{\delta U} \right) G \tag{212}
$$

它代表了：粒子先传播 $G$，然后受到一个微扰的变化 $\delta G^{-1}$，再继续传播 $G$ 的过程。

> **注意：** 这里我们需要计算的是扰动让 $G$ 改变了多少，使用现在的基准来预测扰动后 $G$ 的变化 $\delta G$。这类似于一阶微扰：
>
> $$\delta \psi_n = \sum_{m \neq n} \frac{\langle m | V | n \rangle}{E_n - E_m} \psi_m \tag{213}$$
>
> 我们求一阶微扰时使用的是没有微扰时候的能级。上式类似于式（138） 中的第二项，仅保留了一阶微扰。

我们现在需要得到 $\frac{\delta G^{-1}}{\delta U}$ 这一项。回顾式（125） Dyson 方程，$G^{-1}$ 包含无相互作用部分加上电子之间的 Hartree 项 $V_H$ 和自能项 $\Sigma$。引入外场 $U$ 后：

$$
\begin{aligned}
G^{-1} &= (i\partial_t - T - V_{ext} - U) - (V_H + \Sigma) \\
&= G_{0,U}^{-1} - (V_H + \Sigma)
\end{aligned} \tag{214}
$$

然后我们对 $U$ 求导：

$$
\frac{\delta G^{-1}}{\delta U} = -\frac{\delta U}{\delta U} - \frac{\delta V_H}{\delta U} - \frac{\delta \Sigma}{\delta U} \tag{215}
$$

其中 $-\frac{\delta U}{\delta U} = -\mathbb{I}$，而 $V_H$ 和 $\Sigma$ 都是通过 $G$ 间接依赖于 $U$ 的，因此可以写作：

$$
\begin{aligned}
\frac{\delta G^{-1}}{\delta U} &= -\mathbb{I} - \left( \frac{\delta V_H}{\delta G} \frac{\delta G}{\delta U} + \frac{\delta \Sigma}{\delta G} \frac{\delta G}{\delta U} \right) \\
&= -\mathbb{I} - \underbrace{\left( \frac{\delta V_H}{\delta G} + \frac{\delta \Sigma}{\delta G} \right)}_{K} \underbrace{\frac{\delta G}{\delta U}}_{L}
\end{aligned} \tag{216}
$$

我们把括号中前面那项定义为**相互作用核** $K$。重新代回式（212）：

$$
L = \frac{\delta G}{\delta U} = -G \left[ -\mathbb{I} - K \cdot L \right] G \tag{217}
$$

其中：

- 第一项 $-G(-\mathbb{I})G = GG$，我们把它叫做**无相互作用电子-空穴传播子** $L_0$。
- 第二项 $-G(-K \cdot L)G = GGKL$，写作 $L_0 K L$。

最终，双粒子传播子动力学方程为：

$$
L = L_0 + L_0 \, K \, L \tag{218}
$$

我们发现双粒子传播子方程很像单粒子 Dyson 方程（式 138），其中：

$$
L_0(1, 2; 1', 2') = G(1, 1') \times G(2', 2) \tag{219}
$$

因此式（218） 也可以做迭代展开：

$$
L = L_0 + L_0 K L_0 + L_0 K L_0 K L_0 + L_0 K L_0 K L_0 K L_0 + \cdots \tag{220}
$$

它表明：双粒子的传播是两个分别与环境有无穷次耦合的粒子传播 $G$，再加上二者之间无穷次相互作用 $K$ 的卷积。

为了把 $K$ 的形式写出来，我们结合它的定义：

$$
K = \frac{\delta V_{\text{tot}}}{\delta G} = \frac{\delta V_H}{\delta G} + \frac{\delta \Sigma}{\delta G} \tag{221}
$$

第一项 $\frac{\delta V_H}{\delta G}$ 源自 Hartree 势的变分。习惯上在 BSE 中把它称为**交换核** $K^x$。这里需要小心术语：这个"交换"指的是 BSE 中的两种相互作用核之一（直接 vs. 交换），来源是 Hartree 势的变分，对应虚光子在电子-空穴对中的瞬时复合-重生过程；这跟 Hartree–Fock 中源于泡利不相容原理的"交换作用"是不同的概念。

> **说明：**
>
> - 根据 Hartree 势定义：
>
> $$V_H(1) = \int v(1, 2) \rho(2) \, d2 = -i \int v(1, 2) G(2, 2^+) \, d2 \tag{222}$$
>
> 即在点 $1$ 测得的所有其他位置 $2$ 处的电荷 $\rho(2)$ 对 $1$ 产生的库仑力的总和。
>
> - 对其求导得到相互作用核 $K^x$：
>
> $$K^x(1, 2; 3, 4) = \frac{\delta V_H(1)}{\delta G(3, 4)} \delta(1,2) \tag{223}$$
>
> 在上式中，位置 $1, 2$ 已经被作为观测点，因此需要使用 $3, 4$ 作为扰动源来求导。这个扰动对应于在 $3$ 处注入一个电子、在 $4$ 处注入一个空穴。
>
> - 代入 Hartree 势的表达式：
>
> $$\begin{aligned}
K^x &= -i \int v(1, 2) \frac{\delta G(2, 2^+)}{\delta G(3, 4)} \, d2 \\
&= -i \int v(1, 2) \delta(2, 3) \delta(2, 4) \, d2 \\
&= -i \, v(1, 3) \, \delta(3, 4)
\end{aligned} \tag{224}$$
>
> - 上式包含 $\delta(3, 4)$，即只有当我们在同一个地方放一个电子-空穴对时才起作用。它说明：只有当我们注入的电子-空穴复合时，才会有一个非零的响应场 $v$ 发射出去，产生交换核作用。复合产生的能量通过 $v$ 扩散到位置 $1$。
> - **量子场论中，没有超距作用，所有的力都是通过交换粒子来实现的。** 因此可以认为这里的电子-空穴复合产生的是虚光子；$v$ 本质上可以认为是虚光子的传播子。"虚"是指这种光子仅参与中间过程的能量传递。因此 $v(1, 3)$ 描述电子-空穴对复合产生的虚光子从位置 $3$ 传播到位置 $1$。
> - 我们可以将相互作用核写作 $K^x(1, 2; 3, 4)$，它代表在 $3, 4$ 处施加扰动后，$1, 2$ 处的响应。这样的矩阵是一个四维张量，为了方便计算，我们把输入端合并、输出端合并，写作二维矩阵：
>
> $$K_{IJ} = \begin{pmatrix} K_{11} & K_{12} & \dots \\ K_{21} & K_{22} & \dots \\ \dots & \dots & \dots \end{pmatrix} \tag{225}$$
>
> 输入复合指标 $J$ 代表初始电子-空穴对状态 $(v', c')$，输出指标 $I$ 代表最终电子-空穴对状态 $(v, c)$。一个激子由一个价带空穴 $v$ 和一个导带电子 $c$ 组成。
>
> - 对其在跃迁基底下积分：
>
> $$\langle vc | K | v'c' \rangle = \int d1 \, d2 \, d3 \, d4 \, \Psi^*_{vc}(1, 2) \, K(1, 2; 3, 4) \, \Psi_{v'c'}(3, 4) \tag{226}$$
>
> 其中双粒子波函数 $\Psi_{vc}(1, 2) = \phi_c(1) \phi^*_v(2)$（空穴在 $2$，电子在 $1$）。
>
> - 因此对 $K^x$ 可以写成：
>
> $$\langle vc | K^x | v'c' \rangle = 2 \int d\mathbf{r} \, d\mathbf{r}' \, \phi_c^*(\mathbf{r}) \phi_v(\mathbf{r}) \, v(\mathbf{r} - \mathbf{r}') \, \phi_{c'}(\mathbf{r}') \phi_{v'}^*(\mathbf{r}') \tag{227}$$
>
> （前置因子 2 来源于自旋单态求和。）
>
> - 上式的形式很像跃迁偶极矩 $\langle c | \mathbf{r} | v \rangle$。
> - 我们将左边的 $\phi_c^*(\mathbf{r}) \phi_v(\mathbf{r})$ 叫做左偶极、右边的 $\phi_{c'}(\mathbf{r}') \phi_{v'}^*(\mathbf{r}')$ 叫做右偶极。它的物理本质不再是带电粒子之间的静电相互作用，而是两个振荡偶极子之间通过电磁场进行的能量交换。我们把这种效应叫做"天线效应"，因为它具有长程性质。
> - 这种交换作用所产生的效果是排斥效应，因为偶极振荡产生的感应极化场会阻止电子-空穴复合过程。

式（221） 的第二项 $\frac{\delta \Sigma}{\delta G}$ 是激子结合能的来源。根据 GW 近似，电子自能为 $\Sigma(1, 2) = i G(1, 2) W(1, 2)$，因此这一项可以写作：

$$
\frac{\delta \Sigma(1, 2)}{\delta G(3, 4)} = \frac{\delta [i \, G(1, 2) W(1, 2)]}{\delta G(3, 4)} \tag{228}
$$

根据求导法则 $(uv)' = u'v + uv'$，可以写出两项：

$$
\frac{\delta [i \, G(1, 2) W(1, 2)]}{\delta G(3, 4)} = i \frac{\delta G(1, 2)}{\delta G(3, 4)} W(1, 2) + i \, G(1, 2) \frac{\delta W(1, 2)}{\delta G(3, 4)} \tag{229}
$$

- 右边第一项根据 $\delta$ 函数性质为：

    $$\frac{\delta G(1, 2)}{\delta G(3, 4)} W(1, 2) = \delta(1, 3) \delta(2, 4) W(1, 2) \tag{230}$$

- 右边第二项 $\frac{\delta W}{\delta G}$ 我们设为 0——因为 $W$ 取决于介电函数 $\epsilon^{-1}$，而 $\epsilon^{-1}$ 取决于极化率 $P$，$P \sim GG$；这一项描述了"激子的形成反过来改变了晶体的屏蔽性质"。在 BSE 中我们假设激子导致的扰动不会改变整个材料的介电常数。

因此第二项**直接相互作用核**为：

$$
K^d(1, 2; 3, 4) = -i \, W(1, 2) \, \delta(1, 3) \delta(2, 4) \tag{231}
$$

这一项代表电子-空穴对之间的吸引作用：因为 $\delta(1, 3)$ 和 $\delta(2, 4)$ 分别"标记"了电子和空穴的位置，$W(1, 2)$ 代表屏蔽后的库仑相互作用（形式如 $\frac{1}{\epsilon r}$）。静电势能 $U \propto -|W|$ 因电子-空穴异号而为负——表示吸引势，降低体系能量。这一项就是我们常说的**激子结合能**的来源。

我们写出这一项在跃迁基组下的表达：

$$
\langle vc | K^d | v'c' \rangle = -\int d\mathbf{r} \, d\mathbf{r}' \left[ \phi_c^*(\mathbf{r}) \phi_{c'}(\mathbf{r}) \right] W(\mathbf{r}, \mathbf{r}') \left[ \phi_v(\mathbf{r}') \phi_{v'}^*(\mathbf{r}') \right] \tag{232}
$$

其中 $\phi_c^*(\mathbf{r}) \phi_{c'}(\mathbf{r})$ 为导带电子在 $\mathbf r$ 处的密度重叠，$\phi_v(\mathbf{r}') \phi_{v'}^*(\mathbf{r}')$ 为价带电子（空穴）在 $\mathbf r'$ 处的密度重叠。当 $c = c'$ 且 $v = v'$ 时（对角项）最大，代表静态的电子-空穴吸引。

最后，我们写出总的相互作用核：

$$
\begin{aligned}
\langle vc | K | v'c' \rangle =& \, 2 \int d\mathbf{r} \, d\mathbf{r}' \, \phi_c^*(\mathbf{r}) \phi_v(\mathbf{r}) \, v(\mathbf{r}-\mathbf{r}') \, \phi_{c'}(\mathbf{r}') \phi_{v'}^*(\mathbf{r}') \\
&- \int d\mathbf{r} \, d\mathbf{r}' \, \phi_c^*(\mathbf{r}) \phi_{c'}(\mathbf{r}) \, W(\mathbf{r}, \mathbf{r}') \, \phi_v(\mathbf{r}') \phi_{v'}^*(\mathbf{r}')
\end{aligned} \tag{233}
$$

- 第一项**交换作用**（$K^x$）代表激子-激子型的远程偶极耦合，给出排斥；
- 第二项**直接作用**（$K^d$）代表激子内电子-空穴的屏蔽库仑吸引，给出结合能。

要写出整体的 BSE 哈密顿量，价带电子跃迁到导带还需要跨越一个准粒子带隙：

$$
H_{\text{diag}} = (\epsilon_c - \epsilon_v) \delta_{vv'} \delta_{cc'} \tag{234}
$$

因此，完整的 BSE 哈密顿量 $H^{BSE}$ 可以写作：

$$
H_{(vc), (v'c')}^{BSE} = \underbrace{(\epsilon_c - \epsilon_v) \delta_{vv'} \delta_{cc'}}_{\text{准粒子能隙}} + \underbrace{2 \langle vc | v | v'c' \rangle}_{\text{交换排斥}} \underbrace{- \langle cc' | W | vv' \rangle}_{\text{直接吸引}} \tag{235}
$$

#### BSE 激子波函数

我们现在有了 BSE 的哈密顿量，要解出激子能量还需要激子态的波函数。

激子态 $|S\rangle$ 本质是在基态 $|0\rangle$ 上激发一个电子-空穴对：

$$
|S\rangle = \sum_{v, c} A_{vc} \, \hat{a}_c^\dagger \hat{a}_v \, |0\rangle \tag{236}
$$

其中 $\hat a_c^\dagger$ 是导带轨道 $c$ 上的电子产生算符，$\hat a_v$ 是价带轨道 $v$ 上的电子湮灭算符（即等效地在价带中创造空穴）。

为了解出 $A_{vc}$，我们引入多体哈密顿量本征方程：

$$
\hat{H} |S\rangle = (E_0 + \Omega_S) |S\rangle \tag{237}
$$

其中 $\hat{H}$ 是二次量子化的总哈密顿量算符，$E_0$ 是基态能量，$\Omega_S$ 是激子激发能量（小于带隙），即 $\Omega_S = E_{\text{gap}} - E_b$（能隙减去激子结合能）。

> **说明：** 在二次量子化中，总能量算符的标准定义是把单粒子哈密顿量密度积分：
>
> $$\hat{H}_0 = \int d\mathbf{r} \, \hat{\Psi}^\dagger(\mathbf{r}) \, \hat{h}(\mathbf{r}) \, \hat{\Psi}(\mathbf{r}) \tag{238}$$
>
> - 代入场算符的定义式（60）（61）：
>
> $$\hat{H}_0 = \int d\mathbf{r} \left[ \sum_j \phi_j^*(\mathbf{r}) \hat{a}_j^\dagger \right] \hat{h}(\mathbf{r}) \left[ \sum_i \phi_i(\mathbf{r}) \hat{a}_i \right] \tag{239}$$
>
> - 把求和和算符 $\hat a^\dagger, \hat a$ 提到积分外（与空间坐标 $\mathbf r$ 无关）：
>
> $$\hat{H}_0 = \sum_{j, i} \hat{a}_j^\dagger \hat{a}_i \int d\mathbf{r} \, \phi_j^*(\mathbf{r}) \, \hat{h}(\mathbf{r}) \, \phi_i(\mathbf{r}) \tag{240}$$
>
> - 因为 $\phi_i$ 是本征态，所以 $\hat h \phi_i = \epsilon_i \phi_i$，代入可得：
>
> $$\hat{H}_0 = \sum_i \epsilon_i \, \hat{a}_i^\dagger \hat{a}_i \tag{241}$$

> **说明：** 我们一般说本征值方程，其形式为：
>
> $$\mathbf{A} \mathbf x = \lambda \mathbf x \tag{242}$$
>
> - 举个例子，假设算符 $\mathbf A$ 可以是：
>
> $$\mathbf{A} = \begin{pmatrix} 2 & 1 \\ 1 & 2 \end{pmatrix} \tag{243}$$
>
> - 现在有一个向量 $\mathbf{x} = \begin{pmatrix} 1 \\ 1 \end{pmatrix}$，验证 $\mathbf x$ 是不是 $\mathbf A$ 的本征向量——即 $\mathbf x$ 在算符 $\mathbf A$ 的作用下保持方向不变、只被整体放大或缩小。
>
> $$\mathbf{A}\mathbf{x} = \begin{pmatrix} 2 & 1 \\ 1 & 2 \end{pmatrix} \begin{pmatrix} 1 \\ 1 \end{pmatrix} = \begin{pmatrix} 3 \\ 3 \end{pmatrix} \tag{244}$$
>
> - 输出结果是原向量 $\mathbf x$ 的三倍，即：
>
> $$\lambda \mathbf{x} = 3 \times \begin{pmatrix} 1 \\ 1 \end{pmatrix} = \begin{pmatrix} 3 \\ 3 \end{pmatrix} \tag{245}$$
>
> - 因此式（242） 成立。它说明：在经过一个物理过程后，输出仅是输入的一个缩放，其方向（波函数）没有发生改变。

为了解出 $A_{vc}$，我们用一个探测态 $\langle 0 | \hat{a}_{v'}^\dagger \hat{a}_{c'}$ 左乘式（237）：

$$
\langle 0 | \hat{a}_{v'}^\dagger \hat{a}_{c'} \, \hat{H} \, |S\rangle = (E_0 + \Omega_S) \, \langle 0 | \hat{a}_{v'}^\dagger \hat{a}_{c'} \, |S\rangle \tag{246}
$$

将 $|S\rangle$ 的展开式代进去：

$$
\sum_{vc} \langle 0 | \hat{a}_{v'}^\dagger \hat{a}_{c'} \, \hat{H} \, \hat{a}_c^\dagger \hat{a}_v | 0\rangle A_{vc} = (E_0 + \Omega_S) \sum_{vc} \langle 0 | \hat{a}_{v'}^\dagger \hat{a}_{c'} \, \hat{a}_c^\dagger \hat{a}_v | 0\rangle A_{vc} \tag{247}
$$

其中，后面的 $\langle 0 | \hat{a}_{v'}^\dagger \hat{a}_{c'} \, \hat{a}_c^\dagger \hat{a}_v | 0\rangle = \delta_{vv'}\delta_{cc'}$。

并且我们在前面也推导出：

$$
K = K^x + K^d \tag{248}
$$

即相互作用核是交换核和直接核之和。BSE 哈密顿量再加入动能项（准粒子带隙）后，可写作：

$$
H_{(vc), (v'c')}^{BSE} = (\epsilon_c(\mathbf{k}) - \epsilon_v(\mathbf{k})) \delta_{vv'} \delta_{cc'} + 2 \langle cv' | v | vc' \rangle - \langle cc' | W | vv' \rangle \tag{249}
$$

第一项是"动能"（准粒子色散）项：$\epsilon_c(\mathbf{k}) - \epsilon_v(\mathbf{k})$ 已经是 GW 修正过的准粒子能差——既包含了 $E_g$（带边能差），又包含了对 $\mathbf k$ 的色散（动能修正）。有时为了方便简写为 $(\epsilon_c - \epsilon_v)$。

因此，重新代入式（247），可得：

$$
\sum_{vc} \left[ (\epsilon_c - \epsilon_v)\delta_{vv'}\delta_{cc'} + \langle v'c'|K|vc\rangle \right] A_{vc} = \Omega_S \, A_{v'c'} \tag{250}
$$

这就是 **BSE 本征方程**。

现在上面式子变成了与式（242） 一致的本征方程形式 $H \cdot \vec{A} = \Omega \cdot \vec{A}$。其中，左边的 $A_{vc}$ 是输入（所有可能的跃迁对混合在一起），右边是特定的跃迁对 $A_{v'c'}$。在数值计算中，调用线性代数库（如 LAPACK）找到满足上式的 $\Omega_S$ 和 $A_{v'c'}$。

接下来我们可以求出波函数的形式，写成基组展开：

$$
|\Psi^S\rangle = \sum_{v, c, \mathbf{k}} A_{vc\mathbf{k}}^S \, |v\mathbf{k} \to c\mathbf{k}\rangle \tag{251}
$$

其中 $|v\mathbf{k} \to c\mathbf{k}\rangle$ 是单次跃迁，代表在某个动量点 $\mathbf k$ 把电子从价带激发到导带；$A_{vc\mathbf{k}}^S$ 代表某个特定跃迁的贡献——它的平方即为该激子在 $v\mathbf{k} \to c\mathbf{k}$ 通道的占比；然后对所有 $\mathbf k$ 点和所有能带求和。

也可以把波函数写成实空间的坐标形式：

$$
\Psi^S(\mathbf{r}_e, \mathbf{r}_h) = \sum_{v, c, \mathbf{k}} A_{vc\mathbf{k}}^S \, \phi_{c\mathbf{k}}(\mathbf{r}_e) \, \phi_{v\mathbf{k}}^*(\mathbf{r}_h) \tag{252}
$$

它表明：激子在实空间的形状是由多个导带电子波函数 $\phi_{c\mathbf{k}}(\mathbf{r}_e)$ 和价带空穴波函数 $\phi_{v\mathbf{k}}^*(\mathbf{r}_h)$ 的乘积，按照 $A_{vc\mathbf{k}}^S$ 权重系数比例混合而成。

---

## 含时密度矩阵形式

### 场算符展开

在前面的式（71） 中，我们用场算符表达了电子密度算符。现在考虑自旋 $\sigma$，密度算符可以写作：

$$
\hat{\rho}(\mathbf{r}) = \sum_\sigma \hat{\psi}_\sigma^\dagger(\mathbf{r}) \, \hat{\psi}_\sigma(\mathbf{r}) \tag{253}
$$

现在我们使用正交归一的、定态 KS 轨道 $\{\phi_{p\sigma}(\mathbf{r})\}$ 作为基底，将场算符展开：

$$
\hat{\psi}_\sigma(\mathbf{r}) = \sum_p \phi_{p\sigma}(\mathbf{r}) \, \hat{a}_{p\sigma} \tag{254}
$$

上式是湮灭场算符，取厄米共轭得到产生场算符：

$$
\hat{\psi}_\sigma^\dagger(\mathbf{r}) = \sum_q \phi_{q\sigma}^*(\mathbf{r}) \, \hat{a}_{q\sigma}^\dagger \tag{255}
$$

其中：

- $p, q$ 是 KS 轨道指标，**遍历所有 KS 轨道（占据态和非占据态均包括在内）**；
- $\hat{a}_{p\sigma}$ 和 $\hat{a}_{q\sigma}^\dagger$ 分别是作用在第 $p$、$q$ 个 KS 轨道上的湮灭算符和产生算符；
- $\phi_{q\sigma}^*(\mathbf{r})$ 和 $\phi_{p\sigma}(\mathbf{r})$ 是标量函数（空间坐标确定时是确定的复数）。

我们将用 KS 轨道表达的场算符代入式（253），可得：

$$
\hat{\rho}(\mathbf{r}) = \sum_\sigma \left( \sum_q \phi_{q\sigma}^*(\mathbf{r}) \hat{a}_{q\sigma}^\dagger \right) \left( \sum_p \phi_{p\sigma}(\mathbf{r}) \hat{a}_{p\sigma} \right) \tag{256}
$$

将求和符号合并整理：

$$
\hat{\rho}(\mathbf{r}) = \sum_{p,q,\sigma} \phi_{q\sigma}^*(\mathbf{r}) \phi_{p\sigma}(\mathbf{r}) \, \hat{a}_{q\sigma}^\dagger \hat{a}_{p\sigma} \tag{257}
$$

如果要在某个量子态 $|\Phi(t)\rangle$ 下获得可观测电子密度 $\rho(\mathbf{r}, t)$，需要取期望值：

$$
\rho(\mathbf{r}, t) = \langle \Phi(t) | \hat{\rho}(\mathbf{r}) | \Phi(t) \rangle = \sum_{p,q,\sigma} \phi_{q\sigma}^*(\mathbf{r}) \phi_{p\sigma}(\mathbf{r}) \langle \Phi(t) | \hat{a}_{q\sigma}^\dagger \hat{a}_{p\sigma} | \Phi(t) \rangle \tag{258}
$$

对于左右矢而言，产生/湮灭算符会改变粒子数，因此不能提出左右矢；而两个标量函数则与态空间无关。需要注意：这里的左右矢是 Fock（粒子数占据）空间的态，并非位置表象的态：

$$
| \Phi \rangle = | n_1, n_2, n_3, \dots, n_i, \dots \rangle \tag{259}
$$

$n_i$ 表示第 $i$ 个 KS 轨道上的电子占据数（对费米子为 0 或 1）。

我们把期望值 $\langle \Phi(t) | \hat{a}_{q\sigma}^\dagger \hat{a}_{p\sigma} | \Phi(t) \rangle$ 定义为**单粒子约化密度矩阵**在 KS 轨道基底下的矩阵元，记作 $P_{pq\sigma}(t)$：

$$
P_{pq\sigma}(t) \equiv \langle \Phi(t) | \hat{a}_{q\sigma}^\dagger \hat{a}_{p\sigma} | \Phi(t) \rangle \tag{260}
$$

在上式中：

- $\phi_{q\sigma}^*(\mathbf{r}) \phi_{p\sigma}(\mathbf{r})$ 表达了轨道在空间中的重叠与干涉；
- $P_{pq\sigma}(t)$ 表示了相应轨道乘积 $\phi_{q\sigma}^*(\mathbf{r}) \phi_{p\sigma}(\mathbf{r})$ 在 $t$ 时刻的权重。

我们将空间分布属性和时间演化解耦——所有的含时演化都包含在 $P_{pq\sigma}(t)$ 中。最后，电子密度可以写作：

$$
\rho(\mathbf{r}, t) = \sum_{p,q,\sigma} P_{pq\sigma}(t) \, \phi_{q\sigma}^*(\mathbf{r}) \phi_{p\sigma}(\mathbf{r}) \tag{261}
$$

---

### 密度矩阵的时间演化

首先回顾一下密度矩阵。在 FSSH 的推导中曾对密度矩阵下过定义：对于双态系统 $|\psi\rangle = c_1|1\rangle + c_2|2\rangle$，密度矩阵的矩阵元为 $\rho_{kj} = c_k c_j^*$，整体的密度矩阵为：

$$
\rho = \begin{pmatrix} \rho_{11} & \rho_{12} \\ \rho_{21} & \rho_{22} \end{pmatrix} = \begin{pmatrix} |c_1|^2 & c_1 c_2^* \\ c_2 c_1^* & |c_2|^2 \end{pmatrix} \tag{262}
$$

现在我们需要一个更一般的定义。从现在开始我们做如下约定：

- $\rho(\mathbf{r})$ 表示空间坐标上的真实电子密度；
- $\hat{\rho}$ 表示密度算符；
- $\mathbf{P}$ 表示单粒子约化密度矩阵。

那么更一般的密度算符 $\hat{\rho}$ 可以写作：

$$
\hat{\rho} = \sum_i p_i \, |\Psi_i\rangle\langle\Psi_i| \tag{263}
$$

我们可以通过这个更一般的定义式推导出式（262）。

> **说明：**
>
> - 假设存在一个混合态系综：$|\Psi\rangle$ 以概率 $p_A$ 处于态 $|\Psi_A\rangle$，以概率 $p_B$ 处于态 $|\Psi_B\rangle$。
> - 此时密度算符可以写作：
>
> $$\hat{\rho} = p_A |\Psi_A\rangle\langle\Psi_A| + p_B |\Psi_B\rangle\langle\Psi_B| = \sum_k p_k |\Psi_k\rangle\langle\Psi_k| \tag{264}$$
>
> - 然后我们把每个态 $|\Psi_k\rangle$ 用一组正交归一的基底 $\{|1\rangle, |2\rangle, \dots\}$ 展开：
>
> $$|\Psi_k\rangle = c_{k1} |1\rangle + c_{k2} |2\rangle = \sum_i c_{ki} |i\rangle \tag{265}$$
>
> - 在量子力学中，任何一个算符 $\hat{A}$ 在基底 $\{|i\rangle\}$ 下的矩阵元定义为 $A_{ij} = \langle i | \hat{A} | j \rangle$，其中 $i$ 是行指标、$j$ 是列指标。因此密度算符的矩阵元可以写作：
>
> $$\rho_{ij} = \langle i | \hat{\rho} | j \rangle \tag{266}$$
>
> - 代入密度算符的定义式（263），可以得到：
>
> $$\rho_{ij} = \sum_k p_k \langle i | \Psi_k\rangle \langle \Psi_k | j \rangle \tag{267}$$
>
> - 其中 $\langle i | \Psi_k\rangle = c_{ki}$ 是展开系数；对 $\langle \Psi_k | j \rangle$ 求复共轭得到 $c_{kj}^*$。因此密度矩阵元为：
>
> $$\rho_{ij} = \sum_k p_k \, c_{ki} \, c_{kj}^* \tag{268}$$
>
> - 排成矩阵形式（以 2×2 情形为例）：
>
> $$\rho = \begin{pmatrix} \rho_{11} & \rho_{12} \\ \rho_{21} & \rho_{22} \end{pmatrix} = \begin{pmatrix} \sum_k p_k \, c_{k1} c_{k1}^* & \sum_k p_k \, c_{k1} c_{k2}^* \\ \sum_k p_k \, c_{k2} c_{k1}^* & \sum_k p_k \, c_{k2} c_{k2}^* \end{pmatrix} \tag{269}$$
>
> - 这是完整的混合态密度矩阵（同时包含量子叠加和经典混合）。
>
> - **纯态特例：** 如果系统是纯态，即只有单个 $k$（$p_1 = 1$、其余为 0），求和消失，演化为以下简单形式：
>
> $$\rho = \begin{pmatrix} c_1 c_1^* & c_1 c_2^* \\ c_2 c_1^* & c_2 c_2^* \end{pmatrix} = \begin{pmatrix} |c_1|^2 & c_1 c_2^* \\ c_2 c_1^* & |c_2|^2 \end{pmatrix} \tag{270}$$
>
> - **注意：** 退相干并不代表系统塌缩到了纯态，而只是指在密度矩阵中非对角元降低到 0。
>
> - **例子：** 假设存在一个叠加态：
>
> $$|\Psi\rangle = \frac{1}{\sqrt{2}}|0\rangle + \frac{1}{\sqrt{2}}|1\rangle \tag{271}$$
>
> - 根据式（270） 可以直接写出它的密度矩阵：
>
> $$\rho = \begin{pmatrix} 0.5 & 0.5 \\ 0.5 & 0.5 \end{pmatrix} \tag{272}$$
>
> - 非对角元 0.5 表明两个态之间存在显著的量子相干性。
>
> - 我们使用 $\mathrm{Tr}(\rho^2)$ 来检验一个态是否是纯态——纯态 $\mathrm{Tr}(\rho^2) = 1$，混合态 $\mathrm{Tr}(\rho^2) < 1$。验证当前叠加态：
>
> $$\rho^2 = \begin{pmatrix} 0.5 & 0.5 \\ 0.5 & 0.5 \end{pmatrix} \begin{pmatrix} 0.5 & 0.5 \\ 0.5 & 0.5 \end{pmatrix} = \begin{pmatrix} 0.5 & 0.5 \\ 0.5 & 0.5 \end{pmatrix} \tag{273}$$
>
> $$\mathrm{Tr}(\rho^2) = 0.5 + 0.5 = 1 \tag{274}$$
>
> - 退相干后，两个态之间的相干性消失（非对角元为 0），此时：
>
> $$\rho_{\text{mix}} = \begin{pmatrix} 0.5 & 0 \\ 0 & 0.5 \end{pmatrix} \tag{275}$$
>
> - 此时的纯度为：
>
> $$\rho_{\text{mix}}^2 = \begin{pmatrix} 0.5 & 0 \\ 0 & 0.5 \end{pmatrix} \begin{pmatrix} 0.5 & 0 \\ 0 & 0.5 \end{pmatrix} = \begin{pmatrix} 0.25 & 0 \\ 0 & 0.25 \end{pmatrix} \tag{276}$$
>
> $$\mathrm{Tr}(\rho_{\text{mix}}^2) = 0.25 + 0.25 = 0.5 \tag{277}$$
>
> - 此时已变为混合态——失去了量子相干性，只保留了"50% 概率在态 $|0\rangle$、50% 概率在态 $|1\rangle$"的经典概率。
>
> - 如果对混合态进行一次测量并测到了态 $|0\rangle$，那么密度矩阵塌缩为：
>
> $$\rho_{\text{post}} = \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix} \tag{278}$$
>
> - 再检验纯度：
>
> $$\mathrm{Tr}(\rho_{\text{post}}^2) = 1 + 0 = 1 \tag{279}$$
>
> - 因为观测的"投影"作用，态变成了 100% 确定在态 $|0\rangle$ 上的纯态。

回到含时演化推导。假设一个纯态体系的含时薛定谔方程为（原子单位制 $\hbar = 1$）：

$$
i \frac{\partial |\Psi(t)\rangle}{\partial t} = \hat{H} |\Psi(t)\rangle \tag{280}
$$

对应的左矢方程为：

$$
-i \frac{\partial \langle\Psi(t)|}{\partial t} = \langle\Psi(t)| \hat{H} \tag{281}
$$

前面得到了密度算符（纯态下）$\hat{\mathbf P} = |\Psi\rangle\langle\Psi|$。现在对其求时间导数（链式法则）：

$$
i \frac{\partial \hat{\mathbf{P}}}{\partial t} = i \left( \frac{\partial |\Psi\rangle}{\partial t} \langle\Psi| + |\Psi\rangle \frac{\partial \langle\Psi|}{\partial t} \right) \tag{282}
$$

然后代入式（280） 和 (267)：

$$
i \frac{\partial \hat{\mathbf{P}}}{\partial t} = (\hat{H} |\Psi\rangle) \langle\Psi| - |\Psi\rangle (\langle\Psi| \hat{H}) = \hat{H} (|\Psi\rangle \langle\Psi|) - (|\Psi\rangle \langle\Psi|) \hat{H} = \hat{H}\hat{\mathbf{P}} - \hat{\mathbf{P}}\hat{H} \tag{283}
$$

上式就是密度算符与哈密顿量的对易关系。引入对易子 $[\hat{H}, \hat{\mathbf{P}}] \equiv \hat{H}\hat{\mathbf{P}} - \hat{\mathbf{P}}\hat{H}$，因此可以写作：

$$
i \frac{\partial \mathbf{P}(t)}{\partial t} = [\mathbf{H}(t), \mathbf{P}(t)] \tag{284}
$$

这就是 **Liouville–von Neumann 方程**——它是我们之后 Casida 方程推导的出发点。
如果我们直接求解这个 LvN 方程，就是及其昂贵的实时含时密度泛函理论（Real-Time TDDFT, RT-TDDFT）。
因为这是一个及其复杂的非线性耦合偏微分方程，无法求出解析解，只能一步步在时间域进行昂贵的数值积分。
（虽然这一大章叫 Casida 方程，然而并没有出现 Casida 方程，但是我也不知道叫什么好了）

---

## RT-TDDFT 与 LR-TDDFT

（按理来说这两个应该是大标题，比 Casida 要大，但是不知道放在哪好了）

在上式（284）中，我们可以将哈密顿量替换成 Kohn-Sham (KS) 单粒子哈密顿量：
$$i \frac{\partial \mathbf{P}(t)}{\partial t} = [\mathbf{H}_{KS}(t), \mathbf{P}(t)] \tag{285}$$
其中，KS 哈密顿量为：
$$\mathbf{H}_{KS}(t) = -\frac{1}{2}\nabla^2 + v_{ext}(\mathbf{r}, t) + v_H[\rho](\mathbf{r}, t) + v_{xc}[\rho](\mathbf{r}, t) \tag{286}$$

在量子力学中，哈密顿量 $\mathbf{H}$ 是固定的，$\mathbf{P}$ 随着 $\mathbf{H}$ 演化。
但是我们发现，如果将哈密顿量替换为 $H_{KS}$，$H_{KS}$ 本身包含了 Hartree 势和 XC 势，这导致了 $\mathbf{H}_{KS}(t)$ 本身就是密度矩阵 $\mathbf{P}(t)$ 的函数。因此，我们写出完整的 LvN 方程：
$$i \frac{\partial \mathbf{P}(t)}{\partial t} = [\mathbf{H}_{KS}[\mathbf{P}(t)], \mathbf{P}(t)] \tag{287}$$
可以看出，它自身隐式地依赖于当前的密度矩阵 $\mathbf{P}(t)$。

### RT-TDDFT

如果我们硬要去求这个方程，那也可以，这时我们走的是 RT-TDDFT。
由于方程是非线性的且含时，我们不能无法求出解析解。因此必须将时间切分成无数个极小的时间步长 $\Delta t$。
根据量子动力学基本假设，从 $t$ 时刻到 $t+\Delta t$ 时刻的演化，由一个幺正的时间演化算符 $\mathbf{U}(t+\Delta t, t)$ 驱动：
$$\mathbf{P}(t+\Delta t) = \mathbf{U}(t+\Delta t, t) \mathbf{P}(t) \mathbf{U}^\dagger(t+\Delta t, t) \tag{288}$$
在 KS 轨道表象下，可以写成：
$$|\phi_i(t+\Delta t)\rangle = \mathbf{U}(t+\Delta t, t) |\phi_i(t)\rangle \tag{289}$$

> **说明：**
>
> - 一个孤立的量子系统中，状态矢量 $|\Psi(t)\rangle$ 随时间的演化是确定性的，并且严格服从含时薛定谔方程：
>
> $$i\hbar \frac{\partial}{\partial t}|\Psi(t)\rangle = \hat{H}(t) |\Psi(t)\rangle \tag{290}$$
>
> - 这是一个一阶的线性微分方程，因此可以将某一时刻的解写成初始时刻的解乘以一个算符。我们将这个算符定义成时间演化算符 $\mathbf{U}$：
>
> $$|\Psi(t+\Delta t)\rangle = \mathbf{U}(t+\Delta t, t) |\Psi(t)\rangle \tag{291}$$
>
> - 我们将其代入薛定谔方程：
>
> $$\frac{\partial}{\partial \tau} \Big[ \mathbf{U}(\tau, t) |\Psi(t)\rangle \Big] = -\frac{i}{\hbar} \hat{H}(\tau) \Big[ \mathbf{U}(\tau, t) |\Psi(t)\rangle \Big] \tag{292}$$
>
> - 其中，我们把 $\tau$ 定义为从 $t$ 连续演化到 $t+\Delta t$。
>
> - 右矢 $|\Psi(t)\rangle$ 代表的是系统在最初始相当于 $t = 0$ 的定态，它与当前的演化时间 $\tau$ 毫无关系（因此 $|\Psi(t)\rangle$ 是一个常数矢量），所以左边的偏导数直接作用在算符 $\mathbf{U}$ 上：
>
> $$\left[ \frac{\partial}{\partial \tau} \mathbf{U}(\tau, t) \right] |\Psi(t)\rangle = \left[ -\frac{i}{\hbar} \hat{H}(\tau) \mathbf{U}(\tau, t) \right] |\Psi(t)\rangle \tag{293}$$
>
> - 两边算符都作用在初始量子态上，说明两边算符严格相等：
>
> $$\frac{\partial}{\partial \tau} \mathbf{U}(\tau, t) = -\frac{i}{\hbar} \hat{H}(\tau) \mathbf{U}(\tau, t) \tag{294}$$
>
> - 同时，如果演化时间为 0，那么这时的状态保持在初始量子态上，因此：
>
> $$\mathbf{U}(t, t) = \mathbf{I} \quad (\text{单位算符}) \tag{295}$$
>
> - 由于算符不能直接除以 $\mathbf{U}$ ，必须从右边同时乘以它的逆算符 $\mathbf{U}^{-1}(\tau, t)$ ，因为算符的顺序不能随意更换，因此式（294）可以写作：
>
> $$\left( \frac{d\mathbf{U}(\tau, t)}{d\tau} \right) \mathbf{U}^{-1}(\tau, t) = -\frac{i}{\hbar} \hat{H} \mathbf{U}(\tau, t) \mathbf{U}^{-1}(\tau, t) \tag{296}$$
>
> - 我们把时间微元乘到右边，并且代入单位算符可得：
>
> $$\frac{1}{\mathbf{U}(\tau, t)} d\mathbf{U}(\tau, t) = -\frac{i}{\hbar} \hat{H} d\tau \tag{297}$$
>
> - 现在我们从初状态的 $\tau = t$ 到末状态的 $\tau = t + \Delta t$ 进行积分：
>
> $$\int_{\tau=t}^{\tau=t+\Delta t} \mathbf{U}^{-1}(\tau, t) d\mathbf{U}(\tau, t) = \int_{\tau=t}^{\tau=t+\Delta t} \left(-\frac{i}{\hbar} \hat{H}\right) d\tau \tag{298}$$
>
> - 我们可得到左边的式子为：
>
> $$\int_{t}^{t+\Delta t} \mathbf{U}^{-1}(\tau, t) d\mathbf{U}(\tau, t) = \Big[ \ln \mathbf{U}(\tau, t) \Big]_{t}^{t+\Delta t} = \ln \mathbf{U}(t+\Delta t, t) - \ln \mathbf{U}(t, t) \tag{299}$$
>
> - 因此：
>
> $$\ln \mathbf{U}(t+\Delta t, t) - \ln \mathbf{U}(t, t) = -\frac{i}{\hbar} \hat{H} \Delta t \tag{300}$$
>
> - 代入初始条件 $\mathbf{U}(t, t) = \mathbf{I}$，且单位矩阵的对数 $\ln \mathbf{I} = 0$，并且取以自然常数 $e$ 为底的指数：
>
> $$\mathbf{U}(t+\Delta t, t) = \exp\left( -\frac{i}{\hbar} \hat{H} \Delta t \right) \tag{301}$$
>

为了保证数学上的二阶精度和严格的保迹性（电荷守恒）与幺正性，演化算符通常用 $\Delta t$ 的时间中点表示：

$$\mathbf{U}(t+\Delta t, t) = \exp \left( -i \Delta t \cdot \mathbf{H}_{\text{KS}}\left(t + \frac{\Delta t}{2}\right) \right) \tag{302}$$

> **说明：**
>
> - KS 哈密顿量的完整表达式应该是：
> $$\mathbf{H}(t) = \mathbf{T} + \mathbf{V}_{\text{ion}} + \mathbf{V}_H[\rho(t)] + \mathbf{V}_{xc}[\rho(t)] + \mathbf{V}_{\text{ext}}(t) \tag{303}$$
>
> - 它包含了核势，hartree 势，交换关联等，而外场则显式是关联时间的函数。
>
> - 如果不含外场，则在 $t=0$ 时刻，哈密顿量可以写作：
>
> $$\mathbf{H}_0 = \mathbf{T} + \mathbf{V}_{\text{ion}} + \mathbf{V}_H[\rho^{(0)}] + \mathbf{V}_{xc}[\rho^{(0)}] \tag{304}$$

尽管 $V_H$ 和 $V_{xc}$ 是电子密度 $\rho(t)$ 的函数，且电子密度随时间变化，但是在绝对严格的纯电子系统（核固定不变）中，如果没有外场打破平衡，这些势会被冻结在基态，不会随时间发生任何变化。

在没有外场的情况下，在 $t=0$ 时刻，系统的哈密顿量可以写作如下形式：

$$\mathbf{H}_0 = \mathbf{T} + \mathbf{V}_{\text{ion}} + \mathbf{V}_H[\rho^{(0)}] + \mathbf{V}_{xc}[\rho^{(0)}] \tag{305}$$

此时基态的密度矩阵为 $\mathbf{P}^{(0)}$。

根据矩阵对角化性质，一个矩阵和它自己的本征态构建的密度矩阵，是严格对易的，即在 $t=0$ 时刻，二者满足：

$$[\mathbf{H}_0, \mathbf{P}^{(0)}] = 0 \tag{306}$$

> **说明：**
>
> - 我们在做完基态的自洽收敛后，得到了一个确定的 KS 哈密顿算符 $\hat{h}_{\text{KS}}$，本质是求解这个算符的本征值，即：
>
> $$\hat{h}_{\text{KS}} | \phi_i \rangle = \epsilon_i | \phi_i \rangle \tag{307}$$
>
> - 而 $\{ | \phi_i \rangle \}$ 是哈密顿算符的本征态（Kohn-Sham 轨道），它们构成了一组完整的、正交归一的基底，即满足 $\langle \phi_i | \phi_j \rangle = \delta_{ij}$。$\epsilon_i$ 是对应的轨道能量（本征值）。为了求解算符本征值，我们需要计算它的 $A_{ij} = \langle i | \hat{A} | j \rangle$，即：
>
> $$H_{ij} = \langle \phi_i | \hat{h}_{\text{KS}} | \phi_j \rangle \tag{308}$$
>
> - 我们把上式代入式（307），可得：
>
> $$H_{ij} = \langle \phi_i | \epsilon_j | \phi_j \rangle = \epsilon_j \langle \phi_i | \phi_j \rangle = \epsilon_j \delta_{ij} \tag{309}$$
>
> - 在它自己的本征轨道基底下，哈密顿量是一个完美的对角矩阵，对角线上是轨道能量，非对角元全是 0：
>
> $$\mathbf{H}_0 = \begin{pmatrix} \epsilon_1 & 0 & 0 & \dots \\ 0 & \epsilon_2 & 0 & \dots \\ 0 & 0 & \epsilon_3 & \dots \\ \vdots & \vdots & \vdots & \ddots \end{pmatrix} \tag{310}$$
>
> - 我们从上面式（263）可以得到，基态密度矩阵是由所有已占据的轨道组合而成（假设有 $N$ 个电子，占据了前 $N$ 个能量最低的轨道，不考虑自旋）：
>
> $$\hat{P}^{(0)} = \sum_{k=1}^{N} | \phi_k \rangle \langle \phi_k | \tag{311}$$
>
> - 同样，我们选择相同的完整的，正交归一的基底展开矩阵元：
>
> $$P_{ij} = \langle \phi_i | \hat{P}^{(0)} | \phi_j \rangle = \langle \phi_i | \left( \sum_{k=1}^{N} | \phi_k \rangle \langle \phi_k | \right) | \phi_j \rangle = \sum_{k=1}^{N} \langle \phi_i | \phi_k \rangle \langle \phi_k | \phi_j \rangle \tag{312}$$
>
> - 利用正交归一，可以得到：
>
> $$P_{ij} = \sum_{k=1}^{N} \delta_{ik} \delta_{kj} \tag{313}$$
>
> - 它代表着，如果 $i = j$ 且这个轨道是占据态的（$i \le N$），那么 $P_{ii} = 1$。如果 $i = j$ 但轨道属于未占据的虚轨道（$i > N$），那么 $P_{ii} = 0$。如果 $i \neq j$，由于 $\delta$ 函数，非对角元 $P_{ij} = 0$：
>
> $$\mathbf{P}^{(0)} = \begin{pmatrix} 1 & 0 & \dots & 0 & \dots \\ 0 & 1 & \dots & 0 & \dots \\ \vdots & \vdots & \ddots & \vdots & \ddots \\ 0 & 0 & \dots & 0 & \dots \end{pmatrix} \tag{314}$$
>
> - 接下来我们分别计算 $(HP)_{ij}$ 和 $(PH)_{ij}$：
>
> $$\left(\mathbf{H}_0 \mathbf{P}^{(0)}\right)_{ij} = \sum_{m} H_{im} P_{mj} = \sum_{m} (\epsilon_i \delta_{im}) (n_j \delta_{mj}) = \epsilon_i n_j \delta_{ij} \tag{315}$$
>
> - 其中， $n_j$ 代替前面的 0 或 1 占据数，只有 $i = j$ 时才不为 0。
>
> $$\left(\mathbf{P}^{(0)} \mathbf{H}_0\right)_{ij} = \sum_{m} P_{im} H_{mj} = \sum_{m} (n_i \delta_{im}) (\epsilon_j \delta_{mj}) = n_i \epsilon_j \delta_{ij} \tag{316}$$
>
> - 因此二者的值完全相同：
>
> $$[\mathbf{H}_0, \mathbf{P}^{(0)}] \equiv \mathbf{H}_0 \mathbf{P}^{(0)} - \mathbf{P}^{(0)} \mathbf{H}_0 = \mathbf{0} \tag{317}$$
>
> 证毕。

我们了解了二者是严格对易的，可以将式（306）代入 LvN 方程，即式（287）：

$$i \frac{\partial \mathbf{P}(t)}{\partial t} = [\mathbf{H}_0, \mathbf{P}^{(0)}] = 0 \tag{318}$$

密度矩阵的导数为 0，代表了在下一个时刻密度矩阵不会发生变化，即空间的电子密度不会发生变化，那么由它主导的 $\mathbf{V}_H[\rho]$ 和 $\mathbf{V}_{xc}[\rho]$ 将保持恒定。

这代表了，在没有外势 $\mathbf{V}_{\text{ext}}$ 打破的情况下，没有任何势会随时间改变。

但是，这并不意味着电子是完全静止的。在量子力学中，电子是弥散在全空间的概率波，当处于基态轨道时，虽然具有动能，但是它的概率密度是和时间无关的常数。这意味着虽然每时每刻电子都在动，但是整体的密度分布不会发生任何改变。

> **说明：**
>
> - 有一个例外是我们考虑原子核的运动，此时哪怕不加任何外场，哈密顿量也会随时间变化，而这时候并不是电子打破平衡，而是原子核。（非绝热动力学过程）
>
> - 此时原子核偏离平衡位置，不断振动，此时哈密顿量中的离子势变成了含时的项 $\mathbf{V}_{\text{ion}}(t)$，因为它开始随时间改变
>
> - $\mathbf{V}_{\text{ion}}(t)$ 的改变会导致 $[\mathbf{H}, \mathbf{P}]$ 不再是对易的，导致电子云 $\rho(t)$ 开始随着核位置改变而改变，最终导致 $V_H(t)$ 和 $V_{xc}(t)$ 也跟着含时变化。

我们现在重新考虑含时时候密度矩阵的时间演化过程。
首先我们需要构造一个 $t=0$ 时刻的密度矩阵，这时候需要求解定态 KS 方程，做一步基本的 SCF 得到单粒子轨道，并得到初始时刻的单粒子约化密度矩阵：

$$\mathbf{P}(0) = \sum_{i} | \phi_i(0) \rangle \langle \phi_i(0) | \tag{319}$$

接下来考虑 $t > 0$ 时刻，此时哈密顿量受到外场 $V_{\text{ext}}(t)$，即式（303）所描述，密度矩阵演化由 LvN 方程描述（$\hbar = 1$）：

$$\frac{d \mathbf{P}(t)}{d t} = -i [\mathbf{H}_{\text{KS}}[\mathbf{P}(t), t], \mathbf{P}(t)] \tag{320}$$

我们也可以通过式（288）写出密度矩阵随时间演化算符的传播：

$$\mathbf{P}(t+\Delta t) = \mathbf{U}(t+\Delta t, t) \mathbf{P}(t) \mathbf{U}^\dagger(t+\Delta t, t) \tag{321}$$

为了保证时间演化算符的幺正性和保迹性，我们一般将演化算符写作时间中点表示：

$$\mathbf{U}(t+\Delta t, t) = \exp \left( -i \Delta t \cdot \mathbf{H}_{\text{KS}}\left(t + \frac{\Delta t}{2}\right) \right) \tag{322}$$

但是这样的话，就会出现一个问题，如果要演化到 $t+\Delta t$，要先知道时间 $\frac{\Delta t}{2}$ 中点的 $\mathbf{H}_{\text{KS}}\left(t + \frac{\Delta t}{2}\right)$，而中点 $\mathbf{H}_{\text{KS}}\left(t + \frac{\Delta t}{2}\right)$ 又依赖尚未算出的 $\mathbf{P}(t+\Delta t)$，因此对时间中点哈密顿量的不同处理分为几条路径。

#### 显式传播

为了追求速度，我们直接认为在阿秒量级下，中点哈密顿量可以近似由当前时刻 $t$ 的哈密顿量或前几步的外推直接代替。

首先，计算当前哈密顿量，即式（303）：

$$\mathbf{H}(t) = \mathbf{T} + \mathbf{V}_{\text{ion}} + \mathbf{V}_H[\rho(t)] + \mathbf{V}_{xc}[\rho(t)] + \mathbf{V}_{\text{ext}}(t) \tag{323}$$

将时间演化算符泰勒展开到 2 阶：

$$\mathbf{U} \approx \mathbf{1} - i \Delta t \mathbf{H}_{\text{KS}}(t) - \frac{\Delta t^2}{2} \mathbf{H}_{\text{KS}}^2(t) \tag{324}$$

直接算后面的密度矩阵：

$$\mathbf{P}(t+\Delta t) = \mathbf{U} \mathbf{P}(t) \mathbf{U}^\dagger \tag{325}$$

这种方法比较便宜，但是稳定性较差，需要很小的 $\Delta t$。

#### 半隐式传播

半隐式传播又叫 Predictor–Corrector，首先进行 Predictor，对 $\mathbf{H}_{\text{KS}}\left(t + \frac{\Delta t}{2}\right)$ 近似，先对其进行不断演化，重构后重新用新的 $\mathbf{H}_{\text{KS}}\left(t + \frac{\Delta t}{2}\right)$ 进行迭代。

首先是预测步（Predictor），就让中点的哈密顿量等于当前时刻的值，即 $\mathbf{H}_{\text{KS}}\left(t + \frac{\Delta t}{2}\right) = \mathbf{H}_{\text{KS}}(t)$，临时演化到下一个密度的初猜值 $\mathbf{P}^{(0)}(t+\Delta t)$。

然后进入校正内循环（Corrector），用当前时刻和下一步的时刻密度，线性插值得到中点密度：

$$\mathbf{P}\left(t + \frac{\Delta t}{2}\right) = \frac{\mathbf{P}(t) + \mathbf{P}^{(m)}(t+\Delta t)}{2} \tag{326}$$

由中点密度重新计算中点哈密顿量 $\mathbf{H}_{\text{KS}}\left(t + \frac{\Delta t}{2}\right)$：

用更新滞后的哈密顿量重新计算更精确的 $\mathbf{P}^{(m)}(t+\Delta t)$，直到收敛（一般只需要 2-4 步）。

内层自洽结束，令 $\mathbf{P}(t+\Delta t) = \mathbf{P}^{(m+1)}(t+\Delta t)$，进行下一个阿秒时间点。

> **说明：**
>
> - 不论哪种传播方式，我们都是将时间演化算符泰勒展开，但是这样会导致演化算符不再是幺正的。如果我们仅保留一阶，那么演化算符模长平方为：$\mathbf{1} + \mathbf{H}^2 \Delta t^2 > \mathbf{1}$，这意味着每走一步，都会无中生有增加一点概率，最后会导致电子指数爆炸。因此一般也可以考虑 Crank–Nicolson 变换方法：
>
> $$e^{-i\mathbf H \Delta t} \approx (\mathbf{1} + i \mathbf H \Delta t / 2)^{-1} (\mathbf{1} - i\mathbf H \Delta t / 2) \tag{327}$$
>
> 在这里面，分子分母是互为共轭的，不论时间步长多大，算符的模严格为 1，幺正性守恒。但是代价是矩阵求逆复杂度很高，会导致计算资源消耗巨大。

#### RT-TDDFT 的可观测量

通过 RT-TDDFT，我们得到了 $\mathbf{P}(t)$，它包含了电子系统所有量子信息，**任何可观测量 $\hat{O}$ 的期望值都通过取迹得到**：

$$\langle O(t)\rangle = \text{Tr}[\hat{O} \mathbf{P}(t)] \tag{328}$$

> **说明：**
>
> - 对于纯态 $|\Psi\rangle$，某个物理量 $\hat{O}$ 的期望值可以写作：
>
> $$\langle O \rangle = \langle \Psi | \hat{O} | \Psi \rangle \tag{329}$$
>
> - 我们插入一个正交归一的完备基底：
>
> $$\sum_i |i\rangle \langle i| = \hat{I} \quad (\text{单位算符}) \tag{330}$$
>
> - 将单位算符代入式（329）：
>
> $$\langle O \rangle = \langle \Psi | \hat{O} \left( \sum_i |i\rangle \langle i| \right) | \Psi \rangle \tag{331}$$
>
> - 整理可得：
>
> $$\langle O \rangle = \sum_i \langle \Psi | \hat{O} | i\rangle \langle i | \Psi \rangle \tag{332}$$
>
> - 在上式中，$\langle i | \Psi \rangle$ 是一个复数，因此可以移动，移动后可以得到：
>
> $$\langle O \rangle = \sum_i \langle i | \Psi \rangle \langle \Psi | \hat{O} | i \rangle \tag{333}$$
>
> - 而 $|\Psi \rangle \langle \Psi|$ 是纯态的密度算符 $\hat{P}$，即：
>
> $$\langle O \rangle = \sum_i \langle i | \hat{P} \hat{O} | i \rangle \tag{334}$$
>
> - 在线性代数中，$\sum_i \langle i | \hat{M} | i \rangle$ 就是矩阵 $\hat{M}$ 对角线元素之和，也就是求迹（Trace）的严格定义（求迹可以调换顺序）：
>
> $$\langle O \rangle = \text{Tr}[\hat{P} \hat{O}] = \text{Tr}[\hat{O} \hat{P}] \tag{335}$$
>
> - 如果系统是混合态：
>
> $$\hat{P} = \sum_k w_k |\Psi_k\rangle \langle \Psi_k| \tag{336}$$
>
> - 测量 $\hat{O}$ 的总期望值，自然就是各个纯态期望值的经典概率加权平均：
>
> $$\langle O \rangle = \sum_k w_k \langle \Psi_k | \hat{O} | \Psi_k \rangle \tag{337}$$
>
> - 前面证明过纯态：$\langle \Psi_k | \hat{O} | \Psi_k \rangle = \text{Tr}[\hat{O} |\Psi_k\rangle \langle \Psi_k|]$，因此直接代入：
>
> $$\langle O \rangle = \sum_k w_k \text{Tr}\left[\hat{O} |\Psi_k\rangle \langle \Psi_k|\right] \tag{338}$$
>
> - 求迹是一个线性操作（可以把求和与常数提出来或放进去），把 $\sum_k w_k$ 放进迹运算里面：
>
> $$\langle O \rangle = \text{Tr}\left[ \hat{O} \left( \sum_k w_k |\Psi_k\rangle \langle \Psi_k| \right) \right] \tag{339}$$
>
> 方括号内就是混合态密度算符 $\hat{P}$：
>
> $$\langle O \rangle = \text{Tr}[\hat{O} \mathbf{P}(t)] \tag{340}$$

因此，RT-TDDFT 能算什么，等价于哪些算符 $\hat{O}$ 物理上有意义。

一、 电子结构性质（空间快照）

1. 含时电子密度 $\rho(\mathbf{r}, t)$

我们之前推导过，这也是密度矩阵最直接的输出：

$$\rho(\mathbf{r}, t) = \sum_{p, q, \sigma} P_{pq\sigma}(t)\,\phi_{q\sigma}^*(\mathbf{r})\,\phi_{p\sigma}(\mathbf{r}) \tag{341}$$

它可以直接可视化电子流动。例如电荷转移激发中“电子云从给体跑到受体”的实时动画，光激发后电子-空穴对的实空间分布，以及强场离化中电子被剥离原子的过程。

2. 含时电流密度 $\mathbf{j}(\mathbf{r}, t)$

电子运动产生电流（原子单位制）：

$$\mathbf{j}(\mathbf{r}, t) = \frac{1}{2i}\sum_i \bigl[\phi_i^*(\mathbf{r}, t)\nabla\phi_i(\mathbf{r}, t) - \phi_i(\mathbf{r}, t)\nabla\phi_i^*(\mathbf{r}, t)\bigr] \tag{342}$$

它可以研究光致电流，自旋流，磁化动力学，是计算非线性光学相应的核心量。

二、 光学性质

1. 含时偶极矩 $\boldsymbol{\mu}(t)$

$$\boldsymbol{\mu}(t) = -\int \mathbf{r}\,\rho(\mathbf{r}, t)\,d\mathbf{r} \tag{343}$$

它代表了 $t$ 时刻电子云在三维空间里的几何重心位置。

2. 吸收谱

通过施加 $\delta$-kick 极弱扰动 $\mathbf{V}_{\text{ext}}(t) = -\kappa\,\hat{e}\cdot\mathbf{r}\,\delta(t)$，追踪诱导偶极矩 $\delta\boldsymbol{\mu}(t) = \boldsymbol{\mu}(t) - \boldsymbol{\mu}(0)$，并做傅里叶变换：

$$\alpha_{\alpha\beta}(\omega) = \frac{1}{\kappa}\int_0^\infty \delta\mu_\alpha(t)\,e^{i\omega t}\,e^{-\eta t}\,dt \tag{344}$$

其中 $\eta$ 是控制谱线展宽的阻尼因子。由此得到光吸收截面：

$$\sigma_{\text{abs}}(\omega) = \frac{4\pi\omega}{c}\,\text{Im}\bigl[\bar{\alpha}(\omega)\bigr], \qquad \bar{\alpha} = \frac{1}{3}\text{Tr}\,\alpha \tag{345}$$

物理对应： 峰位 = 激发能 $\Omega_S$；峰高 $\propto$ 振子强度 $f_S$（与 LR-TDDFT 弱场极限一致）。

> **说明：**
>
> - 外场的形式如下：
>
> $$\mathbf{V}_{\text{ext}}(t) = -\kappa\,\hat{e}\cdot\mathbf{r}\,\delta(t) \tag{346}$$
>
> - $\delta(t)$（狄拉克 $\delta$ 函数）是一个在 $t=0$ 时刻无穷大、其他时刻全为 0，且积分面积为 1 的数学脉冲。$\delta$-kick 就是一个瞬时的扰动。
>
> - $\delta$ 函数的傅里叶变换是一个常数。这意味着，一个时间上无限短的 $\delta$ 脉冲，在频域上包含了所有从 0 到无穷大的频率，且各个频率的强度完全相等。所以，用 $\delta$-kick 相当于用“全频段白噪声”一次性均匀地“敲击”了体系的所有能级，瞬间激发了所有可能的跃迁。
>
> - 体系在 $t = 0$ 受到脉冲撞击后，外场瞬间消失。在 $t > 0$ 的时间里，电子云开始在惯性和内部库仑恢复力的作用下自由振荡。
>
> - 通过减去基态的静态偶极矩 $\boldsymbol{\mu}(0)$，只保留纯粹由外场诱导出来的信号 $\delta\boldsymbol{\mu}(t)$，这就是诱导偶极矩：
>
> $$\delta\boldsymbol{\mu}(t) = \boldsymbol{\mu}(t) - \boldsymbol{\mu}(0) \tag{347}$$
>
> - 把这个诱导偶极矩去做傅里叶变换：
>
> $$\alpha_{\alpha\beta}(\omega) = \frac{1}{\kappa}\int_0^\infty \delta\mu_\alpha(t)\,e^{i\omega t}\,e^{-\eta t}\,dt \tag{348}$$
>
> - $\frac{1}{\kappa}$ 是归一化系数，$e^{i\omega t}$ 是傅里叶变换核，用不同频率 $\omega$ 去跟电子的振荡信号 $\delta\mu(t)$ 对比。只有当扫描频率 $\omega$ 和电子自身的某一个共振频率完全吻合时，积分结果才会出现一个巨大的峰。$e^{-\eta t}$ 是人为加入的指数衰减，防止积分演化到无穷大，能让信号在模拟结束前平滑降到 0。
>
> - 通过提取吸收，可以得到不同的响应：
>
> $$\sigma_{\text{abs}}(\omega) = \frac{4\pi\omega}{c}\,\text{Im}\bigl[\bar{\alpha}(\omega)\bigr] \tag{349}$$
>
> - 对极化率张量的对角元求和再除以 3，得到的是各向同性平均：$\bar{\alpha} = \frac{1}{3}\text{Tr}\,\alpha$，这对应了无极化光吸收。
>
> - 在复数极化率中，实部代表的是色散（折射），虚部代表的是耗散（吸收）。虚部对应能量从光场真实地转移到了电子系统内部。

三、激发态投影

1. 多体激发态占据数 $n_S(t)$

将含时多体态投影到 LR-TDDFT 求解出的静态激发态 $|S\rangle$ 上：

$$n_S(t) = |\langle S|\Psi(t)\rangle|^2 \tag{350}$$

给出外场扰动后，体系处于每个激发态的概率。可直接用于模拟 Pump-Probe 实验中的激发态布居演化。

> **说明：**
>
> - 系统在极短的激光脉冲激发后，处于一个极其复杂的含时叠加态 $|\Psi(t)\rangle$。我们想知道的是在这个很复杂状态中，到底有多少成分属于某一个特定的静态激发态 $|S\rangle$（比如第一单重激发态 $S_1$）。
>
> - 对于没有外场情况下，体系哈密顿量为 $\hat{H}_0$，通过求解定态薛定谔方程，我们得到一组完备的正交归一的本征态集合 $\{ |S\rangle \}$，满足：$\langle S | S' \rangle = \delta_{SS'}$。$|0\rangle$ 是基态，$|1\rangle, |2\rangle, \dots, |S\rangle, \dots$ 是各个激发态。
>
> - 任意一个含时状态 $|\Psi(t)\rangle$ 都可以在这组完备的基底上展开：
>
> $$|\Psi(t)\rangle = \sum_{S'} C_{S'}(t) |S'\rangle \tag{351}$$
>
> - 复数系数 $C_{S'}(t)$ 就是在各个本征态上的概率幅。
>
> - 我们想要得到特定激发态 $|S\rangle$ 前面的那个系数 $C_S(t)$，左乘需要的激发态 $\langle S|$：
>
> $$\langle S | \Psi(t)\rangle = \langle S | \left( \sum_{S'} C_{S'}(t) |S'\rangle \right) = \sum_{S'} C_{S'}(t) \langle S | S'\rangle = C_S(t) \tag{352}$$
>
> - 我们利用正交归一得到了 $C_{S'}(t)$，而通过波恩统计解释，体系在 $t$ 时刻处于态 $|S\rangle$ 的概率（即多体激发态占据数 $n_S(t)$），等于该态概率幅的绝对值平方：
>
> $$n_S(t) = |C_S(t)|^2 = |\langle S|\Psi(t)\rangle|^2 \tag{353}$$
>
> - 上述推导在 FSSH 中有举例。

2. 单粒子轨道占据数 $n_p(t)$

投影到基态 KS 参考轨道 $\phi_p^{(0)}$（$p$ 为泛指，可为占据轨也可为虚轨；例如要看 LUMO 就取虚轨 $a$） 上：

$$n_{p}(t) = \sum_{i} |\langle \phi_{p}^{(0)}|\phi_{i}(t)\rangle|^2\,f_{i} \tag{354}$$

给出光激发后，哪些价带电子被打到了哪些导带轨道的直接图像。

> **说明：**
>
> - 如果我们不关心多体态，而是想看在这种激发下，有多少电子从基态轨道 $\phi_H^{(0)}$ 跃迁到了激发态轨道 $\phi_L^{(0)}$。
>
> - 而在这种情况下，单粒子轨道占据数 $n_p(t)$ 为：
>
> $$n_{p}(t) = \sum_{i} |\langle \phi_{p}^{(0)}|\phi_{i}(t)\rangle|^2\,f_{i} \tag{355}$$
>
> - 其中，$|\phi_p^{(0)}\rangle$ 是基底轨道，可以理解为某维度上的单位向量。$|\phi_i(t)\rangle$ 为含时演化轨道，它的空间是多维的，在 KS 轨道基底下可以写成多个 KS 轨道的叠加成的向量。$f_i$ 是初始的占据数，它代表了这个向量有多少个电子。
>
> - 因此，整个式子代表了，我们希望找到 $|\phi_i(t)\rangle$ 这个向量在单位向量 $|\phi_p^{(0)}\rangle$ 下的占据数，即某个单粒子轨道的占据数 $n_p(t)$。
>
> - 在 RT-TDDFT 中，演化的是无相互作用的 Kohn-Sham 体系，整个体系的状态由单粒子约化密度算符 $\hat{P}(t)$ 完全描述：
>
> $$\hat{P}(t) = \sum_{i} f_{i} |\phi_{i}(t)\rangle \langle \phi_{i}(t)| \tag{356}$$
>
> - 我们想知道，在某个 $t$ 时刻，有多少电子在 $t=0$ 时刻的第 $p$ 个基态轨道 $|\phi_p^{(0)}\rangle$ 上。因此我们需要一个特定轨道的粒子数投影算符：
>
> $$\hat{O} = |\phi_{p}^{(0)}\rangle \langle \phi_{p}^{(0)}| \tag{357}$$
>
> 根据式（328），可观测量期望值等于该算符与密度矩阵乘积的迹：
>
> $$n_{p}(t) = \langle \hat{O} \rangle = \text{Tr}[\hat{O} \hat{P}(t)] = \text{Tr}\Big[ \big( |\phi_{p}^{(0)}\rangle \langle \phi_{p}^{(0)}| \big) \hat{P}(t) \Big] \tag{358}$$
>
> 根据迹的循环不变性：$\text{Tr}[ABC] = \text{Tr}[CAB] = \text{Tr}[BCA]$，上式三个向量可以交换：
>
> $$\text{Tr}\Big[ \langle \phi_{p}^{(0)}| \hat{P}(t) |\phi_{p}^{(0)}\rangle \Big] \tag{359}$$
>
> 我们代入密度算符表达式：
>
> $$n_{p}(t) = \langle \phi_{p}^{(0)} | \left( \sum_{i} f_{i} |\phi_{i}(t)\rangle \langle \phi_{i}(t)| \right) | \phi_{p}^{(0)} \rangle = \sum_{i} f_{i} \langle \phi_{p}^{(0)} | \phi_{i}(t)\rangle \langle \phi_{i}(t) | \phi_{p}^{(0)} \rangle \tag{360}$$
>
> 根据狄拉克符号共轭转置，有 $\langle \phi_{i}(t) | \phi_{p}^{(0)} \rangle = \left( \langle \phi_{p}^{(0)} | \phi_{i}(t)\rangle \right)^*$，并且一个复数乘以它的复共轭等于它的绝对值平方（$z \cdot z^* = |z|^2$），因此我们可以写作：
>
> $$\langle \phi_{p}^{(0)} | \phi_{i}(t)\rangle \left( \langle \phi_{p}^{(0)} | \phi_{i}(t)\rangle \right)^* = |\langle \phi_{p}^{(0)} | \phi_{i}(t)\rangle|^2 \tag{361}$$
>
> 因此，最后我们能够得到单粒子轨道占据数：
>
> $$n_{p}(t) = \sum_{i} f_{i} |\langle \phi_{p}^{(0)}|\phi_{i}(t)\rangle|^2 \tag{362}$$

它代表了，在光场（或者外场）扰动下，原本有电子的占据轨道 $|\phi_i(t)\rangle$ 发生了电子跃迁，我们用某个参考轨道 $|\phi_p^{(0)}\rangle$（若关心激发，通常取虚轨道 $a$，如 LUMO）去做内积，内积的平方 $|\langle \phi_p^{(0)}|\phi_i(t)\rangle|^2$ 就是第 $i$ 个占据轨道里的电子跃迁到该参考轨道的概率，相加就是 $t$ 时刻该参考轨道（如 LUMO）上的真实电子总数。

### LR-TDDFT

#### 微扰密度矩阵演化

RT-TDDFT 是从时域里直接追踪电子密度随外场的变化，那么 LR-TDDFT 则是通过给它一个微小震颤，在频域里直接算出它的固有频率。

哈密顿量可以写成如下表示：

$$\mathbf{H}(t) = \mathbf{H}^{(0)} + \delta v_{KS}(\mathbf{r}, t) \tag{363}$$

其中，$\mathbf{H}^{(0)}$ 是式（286）给出的哈密顿量。后面 $ \delta v_{KS}(\mathbf{r}, t)$ 代表给出的微小扰动。

在 $t>0$ 时刻，体系的密度矩阵和哈密顿量为基态与一阶微小扰动的叠加：

$$\mathbf{P}(t) = \mathbf{P}^{(0)} + \delta\mathbf{P}(t), \qquad \mathbf{H}(t) = \mathbf{H}^{(0)} + \delta\mathbf{H}(t) \tag{364}$$

我们把上式代入 LvN 方程，即式（284）可得：

$$i\frac{\partial}{\partial t}\bigl[\mathbf{P}^{(0)} + \delta\mathbf{P}\bigr] = \bigl[\mathbf{H}^{(0)} + \delta\mathbf{H},\ \mathbf{P}^{(0)} + \delta\mathbf{P}\bigr] \tag{365}$$

然后把右边的对易式展开，并按照微扰阶数分类：

$$i\frac{\partial\,\delta\mathbf{P}}{\partial t} = \underbrace{[\mathbf{H}^{(0)}, \mathbf{P}^{(0)}]}_{\text{零阶}} + \underbrace{[\mathbf{H}^{(0)}, \delta\mathbf{P}] + [\delta\mathbf{H}, \mathbf{P}^{(0)}]}_{\text{一阶}} + \underbrace{[\delta\mathbf{H}, \delta\mathbf{P}]}_{\text{二阶}} \tag{366}$$

> **注意：**
>
> - 左边本来是 $\mathbf{P}^{(0)} + \delta\mathbf{P}$，但是由于 $\mathbf{P}^{(0)}$ 是基态，与时间无关，因此带有 $\mathbf{P}^{(0)}$ 的一项舍去，仅保留 $i\frac{\partial\,\delta\mathbf{P}}{\partial t}$ 这项。
>
> - 同时，根据线性响应理论的定义，可以舍去二阶无穷小项 $[\delta\mathbf{H}, \delta\mathbf{P}]$。

在微扰理论中，不同阶数的方程必须独立成立，其中零阶项为：

$$[\mathbf{H}^{(0)}, \mathbf{P}^{(0)}] = 0 \tag{367}$$

这个式子我们上面推导过，基态无外场不会演化。

在 KS 轨道基底下，$\mathbf{H}^{(0)}$ 是一个对角矩阵，其对角元就是 KS 轨道能量 $\epsilon_{p\sigma}$，这个我们上面也了解过：

$$H^{(0)}_{pq,\sigma} = \epsilon_{p\sigma}\,\delta_{pq} \tag{368}$$

其中，我们定义 $\sigma$ 为自旋下标，$p, q$ 为轨道下标，代表广义 KS 轨道编号。

> **注意：**
>
> - 在习惯约定中，$i, j, k, l$ 通常专门指代已占据轨道，即 Occupied orbital。$a, b, c, d$ 通常专门指代未占据的虚轨道，即 Virtual orbital，或者叫 Unoccupied orbital。$p, q, r, s$ 则是泛指，既可能是占据轨，也可能是虚轨。
> - $H^{(0)}_{pq,\sigma}$ 指的就是，在自旋为 $\sigma$ 的哈密顿矩阵中，第 $p$ 行、第 $q$ 列的那个数字，这也是本征轨道能级。这个我们之前又双了解过了。

既然 $\mathbf{H}^{(0)}$ 和 $\mathbf{P}^{(0)}$ 对易，那么 $\mathbf{P}^{(0)}$ 在这个基底下也必须是对角的。它的矩阵元就是占据数 $f_{p\sigma}$：

$$P^{(0)}_{pq,\sigma} = f_{p\sigma}\,\delta_{pq} \tag{369}$$

其中，占据态 $f=1$，空态 $f=0$。

接下来我们处理剩余的一阶方程：

$$i\frac{\partial\,\delta\mathbf{P}}{\partial t} = [\mathbf{H}^{(0)}, \delta\mathbf{P}] + [\delta\mathbf{H}, \mathbf{P}^{(0)}] \tag{370}$$

我们将两个对易子拆开，然后将上面我们得到的式（369）和式（368）代入：

1. 第一个对易子：

$$[\mathbf{H}^{(0)}, \delta\mathbf{P}]_{pq,\sigma} = \sum_r\bigl(H^{(0)}_{pr,\sigma}\,\delta P_{rq,\sigma} - \delta P_{pr,\sigma}\,H^{(0)}_{rq,\sigma}\bigr) = (\epsilon_{p\sigma} - \epsilon_{q\sigma})\,\delta P_{pq,\sigma} \tag{371}$$

2. 第二个对易子：

$$[\delta\mathbf{H}, \mathbf{P}^{(0)}]_{pq,\sigma} = \sum_r\bigl(\delta H_{pr,\sigma}\,P^{(0)}_{rq,\sigma} - P^{(0)}_{pr,\sigma}\,\delta H_{rq,\sigma}\bigr) = (f_{q\sigma} - f_{p\sigma})\,\delta H_{pq,\sigma} \tag{372}$$

我们重新代回式（370），可以得到一阶演化方程：

$$i\frac{\partial\,\delta P_{pq,\sigma}}{\partial t} = (\epsilon_{p\sigma} - \epsilon_{q\sigma})\,\delta P_{pq,\sigma} + (f_{q\sigma} - f_{p\sigma})\,\delta H_{pq,\sigma} \tag{373}$$

> **注意：**
>
> - 在两个对易子中，下标 $r$ 只代表遍历整个矩阵的临时下标。$H^{(0)}$ 是一个对角矩阵。这意味着，在 $H^{(0)}_{pr}$ 中，只有当行号等于列号（即 $r = p$）时，它才不为 0，其他所有 $r \neq p$ 的地方，它全都是 0。因此：
>
> $$\sum_r H^{(0)}_{pr,\sigma} \delta P_{rq,\sigma} = \epsilon_{p\sigma} \delta P_{pq,\sigma} \tag{374}$$
>
> 整个式子中，$r$ 全被坍缩到了 $p$。（这个好像前面也有过解释，但写都写了还是算了）

> **注意：**
>
> - 在式（373）中，我们看 $(f_{q\sigma} - f_{p\sigma})$ 这一项，如果 $p$ 和 $q$ 都是占据态（$1-1=0$）或者都是虚态（$0-0=0$），那么这一项会直接为 0，这时式（373）式会变成：
>
> $$i\frac{\partial\,\delta P_{pq,\sigma}}{\partial t} = (\epsilon_{p\sigma} - \epsilon_{q\sigma})\,\delta P_{pq,\sigma} + \mathbf{0} \tag{375}$$
>
> - 我们注意到一个函数的导数等于常数乘它本身的函数，应该是指数函数，因此我们可以写出它的通解：
>
> $$y(t) = C \cdot e^{-i\omega t} \tag{376}$$
>
> - 其中，常数 $\omega = \epsilon_p - \epsilon_q$，变量 $y(t) = \delta P_{pq}(t)$。
>
> - $e^{-i\omega t}$ 并不是一个单调递增或递减的指数，根据欧拉公式 $e^{-ix} = \cos x - i\sin x$，它是一个随时间永远振荡的正弦/余弦波。这就是叫它振子（Oscillator）的原因。
>
> - 为了求出来 C 是多少，我们考虑时间起点，即 $t=0$，这时电子处于基态，没有外场微扰，密度的变化量是 0，即：
>
> $$y(0) = \delta P_{pq}(0) = 0 \tag{377}$$
>
> - 此时把 $t=0$ 代入：
>
> $$C = 0 \tag{378}$$
>
> - 这意味着，$\delta P_{pq}(t) \equiv 0$，这说明了在 occ–occ 块（$p,q$ 同为占据）与 virt–virt 块（$p,q$ 同为虚轨）上，密度矩阵不会随外场发生任何变化（并非外场为零）。它也代表着，无论我们施加的外场多强，都无法改变密度矩阵中的“占据-占据 (occ-occ)”块和“虚-虚 (virt-virt)”块。外场必须把电子从占据轨道打到虚轨道，形成电子-空穴对。

依据上面的推导，我们需要定义两种激发模式：

1. 前向激发 (occ $\to$ virt)：

$$X_{ia,\sigma}(t) \equiv \delta P_{ai,\sigma}(t) \tag{379}$$

2. 去激发 (virt $\to$ occ)：

$$Y_{ia,\sigma}(t) \equiv \delta P_{ia,\sigma}(t) \tag{380}$$

> **说明：**
>
> - 在 KS 假想系统中，完美的基态是低于费米能级的轨道 100% 填满，高于费米能级的轨道 100% 全空。只有向前激发，没有去激发。
>
> - 然而，真实的基态会由于电子相互排斥而发生量子涨落，会有微小的双重激发成分，电子会有成对跃迁到虚轨道，然后瞬间再返回。
>
> - 这代表着，我们前面 $(f_{q\sigma} - f_{p\sigma})$ 推导的这一项在实际当中不会完全严格为 0。因此，我们考虑 KS 假想系统，电子之间没有排斥，不会发生量子涨落，基态是完美的单一斯拉特行列式。因此，我们可以在这个系统中让 $(f_q - f_p)$ 严格等于 0。
>

> **说明：**
>
> - 在向前激发中，我们定义从占据激发到空态。令 $X_{ia} = \delta P_{ai}$，这意味着在通式中，我们要令行标 $p = a$（虚轨），列表 $q = i$（占据轨）。（同我们前面式（368）下的习惯约定）
>
> - 能量项变成了 $\epsilon_a - \epsilon_i$，占据数因子变成了：$f_i - f_a = 1$。
>
> - 因此，向前激发变成了：
>
> $$i\frac{\partial X_{ia,\sigma}}{\partial t} = (\epsilon_{a\sigma} - \epsilon_{i\sigma})\,X_{ia,\sigma} + (+1)\delta H_{ai,\sigma} \tag{381}$$
>
> - 同理，在去激发中，我们定义从空态到占据态。令 $Y_{ia} = \delta P_{ia}$，令行标 $p = i$（占据轨），列表 $q = a$（虚轨）。同理能量和占据数因子也都如此，可得：
>
> - $$i\frac{\partial Y_{ia,\sigma}}{\partial t} = -(\epsilon_{a\sigma} - \epsilon_{i\sigma})\,Y_{ia,\sigma} - \delta H_{ia,\sigma} \tag{382}$$
>
> - 在去激发中，我们写 $Y_{ia}$ 而不是 $Y_{ai} = \delta P_{ia}$，这是为了统一跃迁通道，以及后续构建 Casida 矩阵对齐维度。我们把“一个空穴在 $i$，一个电子在 $a$”的这对组合，定义为一个激子通道。并且，在 Casida 方程中（下节），为了能让向量 $\mathbf{X}$ 和向量 $\mathbf{Y}$ 能够上下拼接，并且能够和同一个大矩阵相乘，它们的维度必须绝对一样，并且元素的排序也必须绝对一样，因此我们这里考虑写成 $Y_{ia}$。

#### 微扰哈密顿量与 Hartree–xc 耦合核

我们来看哈密顿量，即式（286）这一项，但是由于 LR-TDDFT 不会在时域求解，因此所有的密度都锁定在基态电子密度上，所有的时间都被抹掉：

$$\mathbf{H}^{(0)} = -\frac{1}{2}\nabla^2 + v_{ext}^{(0)}(\mathbf{r}) + v_H[\rho^{(0)}](\mathbf{r}) + v_{xc}[\rho^{(0)}](\mathbf{r}) \tag{383}$$

上式不管，因为它不随时间改变，我们来到微扰项 $\delta v_{KS}(\mathbf{r}, t)$，由于微扰作用在整个哈密顿量，因此除了动能是准确的，其他的均受到一个微扰势：

$$\delta v_{KS}(\mathbf{r}, t) = \delta v_{ext}(\mathbf{r}, t) + \delta v_H(\mathbf{r}, t) + \delta v_{xc}(\mathbf{r}, t) \tag{384}$$

其中，$\delta v_{ext}(\mathbf{r}, t)$ 是真正受到的外界微扰，$\delta v_H(\mathbf{r}, t)$ 代表了外场导致电子云产生的微小扰动，因此经典的 Hartree 项也需要发生改变，$\delta v_{xc}(\mathbf{r}, t)$ 代表电子云发生扰动后，电子之间的泡利排斥和纠缠作用也会发生改变。

它代表了，电子是在外场和其他电子产生的反作用所驱动。

我们现在需要把密度变化和势能连接起来，把 $\delta v_H$ 和 $\delta v_{xc}$ 写成由 $\delta \rho$ 引起的积分：

$$\delta v_H(\mathbf{r}, t) + \delta v_{xc}(\mathbf{r}, t) = \int \Big[ \frac{1}{|\mathbf{r} - \mathbf{r}'|} + f_{xc}(\mathbf{r}, \mathbf{r}') \Big] \delta \rho(\mathbf{r}', t) d\mathbf{r}' \tag{385}$$

> **说明：**
>
> - 任何势能泛函 $v[\rho](\mathbf{r})$ 的一阶泰勒展开，其标准数学定义是（数学推导不做证明）：
>
> $$\delta v(\mathbf{r}) = \int \frac{\delta v(\mathbf{r})}{\delta \rho(\mathbf{r}')} \delta \rho(\mathbf{r}') d\mathbf{r}' \tag{386}$$
>
> - 它代表了空间某一点 $\mathbf{r}$ 的势能变化，等于全空间所有 $\mathbf{r}'$ 处的密度变化对它的影响总和。
>
> - 接下来我们分别对 Hartree 势，交换关联势进行展开，首先看 Hartree 势，它的定义是全空间电子云对某点 $r$：的库伦排斥：
>
> $$v_H(\mathbf{r}) = \int \frac{\rho(\mathbf{r}'')}{|\mathbf{r} - \mathbf{r}''|} d\mathbf{r}'' \tag{387}$$
>
> - 其中使用 $\mathbf{r}''$ 只是为了积分不重名。
>
> - 我们将其代入式（386），可得：
>
> $$\delta v_H(\mathbf{r}) = \int \left[ \frac{\delta v_H(\mathbf{r})}{\delta \rho(\mathbf{r}')} \right] \delta \rho(\mathbf{r}') d\mathbf{r}' \tag{388}$$
>
> $$\frac{\delta v_H(\mathbf{r})}{\delta \rho(\mathbf{r}')} = \frac{\delta}{\delta \rho(\mathbf{r}')} \int \frac{\rho(\mathbf{r}'')}{|\mathbf{r} - \mathbf{r}''|} d\mathbf{r}'' \tag{389}$$
>
> - 密度对自身求导，等于狄拉克 $\delta$ 函数，即 $\frac{\delta \rho(\mathbf{r}'')}{\delta \rho(\mathbf{r}')} = \delta(\mathbf{r}'' - \mathbf{r}')$。由于 $\delta$ 函数的过滤性质，积分坍缩，强制令 $\mathbf{r}'' = \mathbf{r}'$：
>
> $$\frac{\delta v_H(\mathbf{r})}{\delta \rho(\mathbf{r}')} = \frac{1}{|\mathbf{r} - \mathbf{r}'|} \tag{390}$$
>
> - 我们就能得到泰勒展开式中 Hartree 势的变化：
>
> $$\delta v_H(\mathbf{r}) = \int \frac{1}{|\mathbf{r} - \mathbf{r}'|} \delta \rho(\mathbf{r}') d\mathbf{r}' \tag{391}$$
>
> - 接下来我们看交换关联势：
>
> $$\delta v_{xc}(\mathbf{r}) = \int \left[ \frac{\delta v_{xc}(\mathbf{r})}{\delta \rho(\mathbf{r}')} \right] \delta \rho(\mathbf{r}') d\mathbf{r}' \tag{392}$$
>
> - 和 Hartree 势不同，我们并不知道交换关联势 $v_{xc}$ 长什么样子，我们只能把它对密度的导数记作另一种形式：
>
> $$f_{xc}(\mathbf{r}, \mathbf{r}') \equiv \frac{\delta v_{xc}(\mathbf{r})}{\delta \rho(\mathbf{r}')} \tag{393}$$
>
> - 我们把上式叫做**交换关联核**，记作 $f_{xc}$，重新代回去：
>
> $$\delta v_{xc}(\mathbf{r}) = \int f_{xc}(\mathbf{r}, \mathbf{r}') \delta \rho(\mathbf{r}') d\mathbf{r}' \tag{394}$$
>
> - 这时，我们可以把 Hartree 势和交换关联势两个加起来，得到：
>
> $$\delta v_H(\mathbf{r}, t) + \delta v_{xc}(\mathbf{r}, t) = \int \Big[ \frac{1}{|\mathbf{r} - \mathbf{r}'|} + f_{xc}(\mathbf{r}, \mathbf{r}') \Big] \delta \rho(\mathbf{r}', t) d\mathbf{r}' \tag{395}$$
>
> - 由于在 LR-TDDFT 中，这一切都是在任意给定的时刻 $t$ 发生的（在绝热近似下），需要给所有变量挂上时间标签 $t$。
>
> - 上式大括号中的就是**Hartree-xc 耦合核** $f_{Hxc}(\mathbf{r}, \mathbf{r}')$。

因此，微扰势变成了：

$$\delta v_{KS}(\mathbf{r}, t) = \delta v_{ext}(\mathbf{r}, t) + \int f_{Hxc}(\mathbf{r}, \mathbf{r}') \delta \rho(\mathbf{r}', t) d\mathbf{r}' \tag{396}$$

至此，我们的 LvN 方程就变为了：

$$i \frac{\partial \delta\mathbf{P}(t)}{\partial t} = [\mathbf{H}^{(0)}, \delta\mathbf{P}(t)] + [\delta v_{KS}(t), \mathbf{P}^{(0)}] \tag{397}$$

#### 傅里叶变换

在前面的近似中我们做了一个截断，扔掉了 $[\delta\mathbf{H}, \delta\mathbf{P}]$ 这个二阶项，这个截断使得现在这个体系是一个线性系统。而对于线性系统来说，如果用频率为 $\omega$ 的极其微弱的光去扰动电子云，那么它在达到稳态后，就只能以完全相同的频率 $\omega$ 跟着晃动。但是非线性系统则会产生 $2\omega, 3\omega$ 的倍频振荡。

因为电子云的频率已经被我们施加的外场锁定，既然频率一定，那么时间不再是未知的了，无非就是按照 $\omega$ 继续周期性起伏，我们现在需要解出来的就是，这个起伏的幅度有多大，以及有哪些轨道参与进来了。

根据傅里叶定理，任何复杂的含时信号，都可以被拆解成无数个不同频率的纯正弦/余弦波的叠加：

$$\delta v_{\text{ext}}(t) = \int \delta v_{\text{ext}}(\omega) e^{-i\omega t} d\omega \tag{398}$$

因此，我们把式（397）中，所有含时项都分离成空间振幅和时间相位：

$$\delta\mathbf{P}(t) = \delta\mathbf{P}(\omega) e^{-i\omega t} \tag{399}$$

$$\delta v_{KS}(t) = \delta v_{KS}(\omega) e^{-i\omega t} \tag{400}$$

因此，式（397）可以写作：

$$i \frac{\partial}{\partial t} \Big[ \delta\mathbf{P}(\omega) e^{-i\omega t} \Big] = \Big[ \mathbf{H}^{(0)}, \delta\mathbf{P}(\omega) e^{-i\omega t} \Big] + \Big[ \delta v_{KS}(\omega) e^{-i\omega t}, \mathbf{P}^{(0)} \Big] \tag{401}$$

等式左边，由于 $\delta\mathbf{P}(\omega)$ 已经不含时，因此相当于常数直接提出：

$$i \frac{\partial}{\partial t} \Big[ \delta\mathbf{P}(\omega) e^{-i\omega t} \Big] = i \cdot \delta\mathbf{P}(\omega) \cdot \frac{\partial}{\partial t} \Big( e^{-i\omega t} \Big) = \omega \delta\mathbf{P}(\omega) e^{-i\omega t} \tag{402}$$

等式右边，由于 $e^{-i\omega t}$ 并不是一个矩阵，它只是一个复数标量，可以不参与矩阵运算，因此可以从对易子中提取出来：

$$\Big( [\mathbf{H}^{(0)}, \delta\mathbf{P}(\omega)] + [\delta v_{KS}(\omega), \mathbf{P}^{(0)}] \Big) e^{-i\omega t} \tag{403}$$

因此式（401）可以写作：

$$\omega \delta\mathbf{P}(\omega) = [\mathbf{H}^{(0)}, \delta\mathbf{P}(\omega)] + [\delta v_{KS}(\omega), \mathbf{P}^{(0)}] \tag{404}$$

现在，时间 $t$ 从方程中消失。现在的未知数变成了 $\omega$（激发的能量）和 $\delta\mathbf{P}(\omega)$（在该能量下发生的轨道跃迁振幅）。

> **说明：**
>
> - 我们现在就可以给 $\omega$ 和 $\delta\mathbf{P}(\omega)$ 赋予“激发能”和“跃迁振幅”的物理意义，理由如下：
>
> - 对于 $\omega$ 其实是我们加的外场的频率，而在量子力学中，频率和能量是严格等价的，即 $E = \hbar \omega$。在原子单位制下，$\hbar = 1$，所以 $E = \omega$。既然这些特定的 $\omega$ 是体系能够自发共振的频率，那么根据玻尔的跃迁假设，它们就是体系吸收或发射光子时跨越的能级差。
>
> - 而密度矩阵的非对角元 $\rho_{mn}$ （从状态 $m$ 到状态 $n$）被称为量子相干性。它代表了体系同时处于状态 $m$ 和状态 $n$ 的叠加态的程度。
>
> - 我们讲密度矩阵投影到 KS 轨道基底，他指的就是权重，是很多对跃迁的线性组合。数值大小代表了在当前这个能量为 $\omega$ 的激发态中，电子从轨道 $i$ 跳到轨道 $a$ 这个特定通道，贡献了多大的比例。因此，我们把它叫做轨道跃迁振幅。

#### 响应理论

我们原来把式（404）中右边做过拆解，即式（371）和式（372），如果我们取密度算符的第 $p$ 行、第 $q$ 列的矩阵元：

$$\omega \delta P_{pq}(\omega) = (\epsilon_p - \epsilon_q)\delta P_{pq}(\omega) + (f_q - f_p)\delta v_{KS, pq}(\omega) \tag{405}$$

我们把显式的密度矩阵放在同一边：

$$\delta P_{pq}(\omega) = \frac{f_q - f_p}{\omega - (\epsilon_p - \epsilon_q)} \delta v_{KS, pq}(\omega) \tag{406}$$

上式是**响应理论**的核心，分子代表泡利不相容，只有 $p$ 和 $q$ 是一占一空时，响应才会存在，否则密度不会改变。分母代表了，当外场的能量 $\omega$ 刚好等于两个轨道的能量差，分母就会趋于 $0$。即使外场 $\delta v$ 极弱，但是整个体系的响应 $\delta P$ 也会被无限放大，代表激发态产生。

#### Casida 方程

看起来我们已经能够解出来密度矩阵了，但是 $v_{KS, pq}$ 里面还隐藏着密度矩阵，即式（396）：

$$\delta v_{KS}(\mathbf{r}, \omega) = \delta v_{\text{ext}}(\mathbf{r}, \omega) + \int f_{Hxc}(\mathbf{r}, \mathbf{r}') \delta \rho(\mathbf{r}', \omega) d\mathbf{r}' \tag{407}$$

我们在解 LvN 方程时需要它在 KS 轨道基底下的矩阵元。人话就是把它搞成一个矩阵，上面我们密度算符就投影在了 KS 轨道上，所以我们也把它投影到 KS 轨道上，做投影的数学定义为：

$$V_{pq} = \int \phi_p^*(\mathbf{r}) V(\mathbf{r}) \phi_q(\mathbf{r}) d\mathbf{r} \tag{408}$$

我们把式（407）代入：

$$\delta v_{KS, pq}(\omega) = \int \phi_p^*(\mathbf{r}) \Big[ \delta v_{\text{ext}}(\mathbf{r}, \omega) + \int f_{Hxc}(\mathbf{r}, \mathbf{r}') \delta \rho(\mathbf{r}', \omega) d\mathbf{r}' \Big] \phi_q(\mathbf{r}) d\mathbf{r}  \tag{409}$$

然后拆成两项，一项是外势的矩阵元，还有一项是内部回声项：

$$\delta v_{KS, pq}(\omega) = \underbrace{\int \phi_p^*(\mathbf{r}) \delta v_{\text{ext}}(\mathbf{r}, \omega) \phi_q(\mathbf{r}) d\mathbf{r}}_{\text{外势矩阵元 } \delta v_{\text{ext}, pq}(\omega)} + \underbrace{\int \phi_p^*(\mathbf{r}) \left[ \int f_{Hxc}(\mathbf{r}, \mathbf{r}') \delta \rho(\mathbf{r}', \omega) d\mathbf{r}' \right] \phi_q(\mathbf{r}) d\mathbf{r}}_{\text{内部回声项}} \tag{410}$$

根据前面式（261）（好远的推！），密度矩阵的定义，任意点的电子密度 $\rho(\mathbf{r})$，等于所有轨道的波函数乘积，再乘以它们在密度矩阵中的权重：

$$\delta \rho(\mathbf{r}', \omega) = \sum_{r, s} \phi_r(\mathbf{r}') \phi_s^*(\mathbf{r}') \delta P_{rs}(\omega) \tag{411}$$

我们代入式（410）的回声项：

$$\int \phi_p^*(\mathbf{r}) \left[ \int f_{Hxc}(\mathbf{r}, \mathbf{r}') \delta \rho(\mathbf{r}', \omega) d\mathbf{r}' \right] \phi_q(\mathbf{r}) d\mathbf{r} = \int \phi_p^*(\mathbf{r}) \left[ \int f_{Hxc}(\mathbf{r}, \mathbf{r}') \Big( \sum_{r, s} \phi_r(\mathbf{r}') \phi_s^*(\mathbf{r}') \delta P_{rs}(\omega) \Big) d\mathbf{r}' \right] \phi_q(\mathbf{r}) d\mathbf{r} \tag{412}$$

其中，$\delta P_{rs}(\omega)$ 是权重，只是一个数字，因此提出，同样可以提出求和符号：

$$上式 = \sum_{r, s} \Bigg[ \int \int \phi_p^*(\mathbf{r}) \phi_q(\mathbf{r}) f_{Hxc}(\mathbf{r}, \mathbf{r}') \phi_r(\mathbf{r}') \phi_s^*(\mathbf{r}') d\mathbf{r} d\mathbf{r}' \Bigg] \delta P_{rs}(\omega) \tag{413}$$

在中括号中，所有的波函数和核都已经配对完毕，它是一个涉及到四个轨道的双重空间积分。我们把它叫做**耦合矩阵元** $K_{pq, rs}$：

$$K_{pq, rs} = \int \int \phi_p^*(\mathbf{r}) \phi_q(\mathbf{r}) f_{Hxc}(\mathbf{r}, \mathbf{r}') \phi_r(\mathbf{r}') \phi_s^*(\mathbf{r}') d\mathbf{r} d\mathbf{r}' \tag{414}$$

因此，式（410）变为了：

$$\delta v_{KS, pq}(\omega) = \delta v_{\text{ext}, pq}(\omega) + \sum_{rs} K_{pq, rs}(\omega) \delta P_{rs}(\omega) \tag{415}$$

> **说明：**
>
> - 在 $K_{pq, rs}$ 中，出现了四个轨道，或者叫双电子积分。我们把它们从右往左分成两组来看：
>
> - $\phi_r(\mathbf{r}') \phi_s^*(\mathbf{r}')$ 这一项中存在轨道 $r$ 和 $s$，它代表了原本一个在轨道 $r$ 上的电子，开始往轨道 $s$ 跃迁（这个和我们之前二次量子化中推导的产生湮灭算符类似），这种跃迁导致了空间 $\mathbf{r}'$ 处的电荷震荡。
>
> - $\phi_p^*(\mathbf{r}) \phi_q(\mathbf{r})$ 这一项中同样是跃迁，但是是在空间 $\mathbf{r}$ 处，从轨道 $p$ 跳到轨道 $q$。
>
> - $f_{Hxc}$ 传递震荡，它代表了，空间 $\mathbf{r}'$ 处 $(r \to s)$ 跃迁导致的的电荷振荡，会产生一个不断变化的电场和量子排斥场，这个力场通过 $f_{Hxc}$ 传播到了空间 $\mathbf{r}$ 处，空间 $\mathbf{r}$ 处的 $(p \to q)$ 跃迁感受到了这个力场的推拉，从而改变了自己跃迁的难度（动力或者阻力）。
>
> - 我们把这个项叫做回声项，意思是这是由于微扰的外势导致的电子密度的变化引起的跃迁轨道之内在的关联。

对于式（415）来说，我们施加了一个外场 $\delta v_{ext}$，但是我们需要在这时候把施加的外场撤回，即令 $\delta v_{ext} = 0$。

在外场作用完以后，即使撤掉外场，材料内部依然会出现持续的集体震荡，因此即使我们去掉外场，$\delta P \neq 0$ 。因为回声项代表了系统内部固有的恢复力，而不是外界的推力。

因此，我们去掉外场以后，系统内部的驱动力只剩下了内部的耦合 $\sum K \delta P$：

$$\delta v_{KS, pq} = \sum_{rs} K_{pq, rs} \delta P_{rs} \tag{416}$$

我们还有式（406），即响应方程，我们就能得到完全由密度矩阵和耦合矩阵 K 组成的方程：

$$\omega \delta P_{pq} = (\epsilon_p - \epsilon_q)\delta P_{pq} + (f_q - f_p) \sum_{rs} K_{pq, rs} \delta P_{rs} \tag{417}$$

尽管上式的求和是对全轨道求和，但是我们前面说明过，仅有占据轨道-虚轨道和虚轨道-占据轨道这两个通道的值不为 0，我们定义前向激发和去激发：

$$X_{jb} \equiv \delta P_{bj} \tag{418}$$

$$Y_{jb} \equiv \delta P_{jb} \tag{419}$$

其中，$b$ 是虚轨道，$j$ 是占据轨道，因此，式（417）中的求和项可以被进一步压缩：

$$\sum_{rs} K_{pq, rs} \delta P_{rs} = \sum_{jb} \big[ K_{pq, bj} \delta P_{bj} + K_{pq, jb} \delta P_{jb} \big] \tag{420}$$

我们直接替换成 $X$ 和 $Y$：

$$\sum_{rs} K_{pq, rs} \delta P_{rs} = \sum_{jb} \big[ K_{pq, bj} X_{jb} + K_{pq, jb} Y_{jb} \big] \tag{421}$$

1. 对于前向激发，必须令行标 $p = a$（虚），列标 $q = i$（占），此时能量差 $(\epsilon_p - \epsilon_q) \implies (\epsilon_a - \epsilon_i)$，占据权重为 $(f_q - f_p) = (f_i - f_a) = 1$，因此：

$$\omega \delta P_{ai} = (\epsilon_a - \epsilon_i) \delta P_{ai} + (+1) \sum_{jb} \big[ K_{ai, bj} X_{jb} + K_{ai, jb} Y_{jb} \big] \tag{422}$$

把 $X_{jb} \equiv \delta P_{bj}$ 代入：

$$\omega X_{ia} = (\epsilon_a - \epsilon_i) X_{ia} + \sum_{jb} \big[ K_{ai, bj} X_{jb} + K_{ai, jb} Y_{jb} \big] \tag{423}$$

2. 对于去激发，我们做相同处理，能量差：$(\epsilon_p - \epsilon_q) \implies (\epsilon_i - \epsilon_a)$，占据权重为-1：

$$\omega \delta P_{ia} = (\epsilon_i - \epsilon_a) \delta P_{ia} + (-1) \sum_{jb} \big[ K_{ia, bj} X_{jb} + K_{ia, jb} Y_{jb} \big] \tag{424}$$

把 $Y_{jb} \equiv \delta P_{jb}$ 代入：

$$\omega Y_{ia} = (\epsilon_i - \epsilon_a) Y_{ia} - \sum_{jb} \big[ K_{ia, bj} X_{jb} + K_{ia, jb} Y_{jb} \big] \tag{425}$$

为了保持方程对齐，一般会把式（425）乘以一个-1：

$$-\omega Y_{ia} = (\epsilon_a - \epsilon_i) Y_{ia} + \sum_{jb} \big[ K_{ia, bj} X_{jb} + K_{ia, jb} Y_{jb} \big] \tag{426}$$

> **说明：**
>
> - 我们来看其中前向激发这一项，我们发现，对于 $X_{ia}$ 和 $Y_{ia}$ 来说，二者都是无量纲的，而最前面的 $\omega$ 和 $(\epsilon_a - \epsilon_i)$ 都是能量，那么矩阵元 $K$ 必然具有能量的量纲，我们用能量的视角翻译一下它：
>
> $$\underbrace{\omega X_{ia}}_{\text{真实激子的总能量}} = \underbrace{(\epsilon_a - \epsilon_i) X_{ia}}_{\text{孤立轨道的固有能量}} + \underbrace{\sum_{jb} K_{ai, bj} X_{jb}}_{\text{其他前向跃迁施加的相互作用能}} + \underbrace{\sum_{jb} K_{ai, jb} Y_{jb}}_{\text{其他去激发施加的相互作用能}} \tag{427}$$
>
> - 严格来说，左边 $\omega$ 是真实激子的总激发能，$X_{ia}$ 是 $ia$ 这个特定跃迁通道在激子中的权重，或者叫做振幅。因此左边严格的物理意义是：激子的全局总激发能 $\omega$，分配到 $ia$ 这个特定跃迁通道上的分量是多少。

对于前向激发，我们把所有的 $X$ 放在一起，然后把前面的系数给他命个名 $A$，叫他共振矩阵：

$$\mathbf{A}_{ia, jb} = \delta_{ij}\delta_{ab}(\epsilon_a - \epsilon_i) + K_{ai, bj} \tag{428}$$

它代表了，前向激发自身的轨道能量和不同前向激发轨道之间的扰动。

> **说明：**
>
> - 我们规定，矩阵 $\mathbf{A}$ 的第 $ia$ 行、第 $jb$ 列的那个单一元素是右边那一堆，因此右边的 $K$ 前面的求和符号并没有消失，而是换了个写法，类似于：
>
> $$(\mathbf{A} \mathbf{X})_{ia} = \sum_{jb} \mathbf{A}_{ia, jb} X_{jb} \tag{429}$$
>
> - 而 $(\epsilon_a - \epsilon_i)$ 前面必须有 $\delta_{ij}\delta_{ab}$，它代表了单粒子能隙，代表了电子在没有任何其他人干扰时，孤立地从轨道 $i$ 跳到轨道 $a$ 所需的固有能量。

对于去激发，我们同样定义反共振矩阵 $B$：

$$\mathbf{B}_{ia, jb} = K_{ai, jb} \tag{430}$$

它代表了前向激发和去激发之间的多体量子干涉，就是我们之前考虑的量子涨落引起的。如果我们考虑的电子相关性较弱，B 可以直接砍掉，这就是 Tamm-Dancoff 近似 （TDA），如果是高度离域的共轭体系，或者研究的等离激元共振，基态的量子涨落极其剧烈，此时 B 不能忽视。

这时，前向激发方程可以写成：

$$\mathbf{A} \mathbf{X} + \mathbf{B} \mathbf{Y} = \omega \mathbf{X} \tag{431}$$

去激发则可以写成：

$$\mathbf{B}^* \mathbf{X} + \mathbf{A}^* \mathbf{Y} = -\omega \mathbf{Y} \tag{432}$$

现在，我们把上面两个矩阵方程上下拼在一起，写成一个巨大的分块矩阵方程：

$$\begin{pmatrix}
\mathbf{A} & \mathbf{B} \\
\mathbf{B}^* & \mathbf{A}^*
\end{pmatrix}
\begin{pmatrix}
\mathbf{X} \\
\mathbf{Y}
\end{pmatrix} =
\omega
\begin{pmatrix}
\mathbf{1} & \mathbf{0} \\
\mathbf{0} & -\mathbf{1}
\end{pmatrix}
\begin{pmatrix}
\mathbf{X} \\
\mathbf{Y}
\end{pmatrix} \tag{433}$$

这就是我们最终得到的**Casida 方程**。

> **说明：**
>
> - 这个方程右边有一个度规矩阵：$\begin{pmatrix} \mathbf{1} & \mathbf{0} \\ \mathbf{0} & -\mathbf{1} \end{pmatrix}$。它会导致方程的特征向量会成对出现，如果算出一个激发能是 $\omega$，那必然还会有一个数学解是 $-\omega$。这对应了物理上的吸收一个光子 $\omega$ 和发射一个光子 $\omega$。
>
> - 计算 $\mathbf{B}$ 矩阵非常耗时，我们采用上面说的 TDA，那么 Casida 方程会瞬间坍缩成：
>
> $$\mathbf{A} \mathbf{X} = \omega \mathbf{X} \tag{434}$$
>
> - 这是最简单，最标准的厄密矩阵本征值方程。

我们求解 Casida 方程，可以得到一系列离散的特征值 $\omega_1, \omega_2, \omega_3 \dots$，这些特征值代表了垂直激发能，每一个 $\omega_I$ 都对应着体系从基态 $S_0$ 跃迁到第 $I$ 个单重激发态（$S_1, S_2 \dots$）所需的能量。

如果把这些 $\omega_I$ 换算成波长或者 eV，这就是吸收光谱上的一个个尖峰位置。

而我们想看某个峰多高，就需要求解出振幅 $X$ 和 $Y$，然后结合偶极算符 $\hat{\mathbf{r}}$，计算出体系在基 $G$ 和第 $I$ 个激发态之间的跃迁偶极矩（Transition Dipole Moment, $\mu_{GI}$）：

$$\mu_{GI} = \sum_{ia} (X_{ia} + Y_{ia}) \langle \phi_i | \hat{\mathbf{r}} | \phi_a \rangle \tag{435}$$

有了跃迁偶极矩就能获得振子强度（Oscillator Strength, $f$）：$f_I \propto \omega_I |\mu_{0I}|^2$，它决定了吸收光谱的峰高。

并且通过爱因斯坦自发辐射系数，跃迁偶极矩可以直接用来计算激发态发光（荧光）的速率。

#### 构建激子多体波函数基底

通过求解 Casida 方程，我们可以得到振幅 $X$ 和 $Y$。但是我们想看的是激子的动力学过程，即激子如何在不同激子态之间跃迁，因此我们必须把它们还原成真实的物理波函数 。

按照 **Casida 指认假设（assignment ansatz）**，第 $I$ 个激发态的多体波函数 $\Phi_I$ 可以写成大量单粒子跃迁（从占据态 $i$ 到虚态 $a$）的线性组合 ：

$$\Phi_I = \sum_{ia\sigma} Z_{I,ia} \hat{a}_{a\sigma}^\dagger \hat{a}_{i\sigma} \Phi_0 \tag{436}$$

> **说明：**
>
> - 我们在二次量子化中推导过，可以把多体波函数 $\Phi_I$ 表示成：$\hat{a}_{a\sigma}^\dagger \hat{a}_{i\sigma} \Phi_0$。
>
> - 其中，$\Phi_0$ 是体系的基态波函数，前面是产生和湮灭算符，它表示了一个电子发生了一次 $i \to a$ 跃迁。
>
> - 然而，我们之前讨论过，真实的激子态是很多个跃迁通道的叠加，因此，我们把第 $I$ 个激发态的多体波函数 $\Phi_I$ 写成所有这些单激发电子态的线性组合：
>
> $$\Phi_I = \sum_{ia\sigma} Z_{I,ia} (\hat{a}_{a\sigma}^\dagger \hat{a}_{i\sigma} \Phi_0) = \sum_{ia\sigma} Z_{I,ia} \Phi_{i \to a} \tag{437}$$
>
> - 其中，$Z_{I,ia}$ 就是权重，每一个跃迁通道在总的激发态中占了多少
>
> - Casida 通过严密的数学推导发现，如果要让这个造出来的激子波函数 $\Phi_I$ 能够产生和严格 TDDFT 完全相同的跃迁密度和振子强度，展开系数必须严格按照以下公式构造：
>
> $$Z_{I,ia} = \frac{X_{I,ia} + Y_{I,ia}}{\sqrt{\omega_I}} \tag{438}$$
>
> - 上式中，分子是真实的贡献，即前向激发和去激发的贡献，分母除以一个 $\sqrt{\omega_I}$ 为了满足归一化条件

#### 类 FSSH 含时演化

类似前面 FSSH 推导，我们把体系真实的波函数 $\Psi(t)$ 展开在一套随时间变换的绝热基底 $\Phi_I[\mathbf{R}(t)]$ 上：

$$\Psi(t) = \sum_I C_I(t) \Phi_I[\mathbf{R}(t)] \tag{439}$$

> **注意：**
>
> - 在这里，我们还是用绝热近似，认为原子核不动而去解电子的多体波函数，电子波函数仅仅是通过原子的坐标 $\mathbf{R}$ 来间接地感受时间 $t$，因此我们的这组基底是绝热的。
>
> - 为了偷懒，我把含时基底整体写成 $\Phi_K(t)$。

接下来就和 FSSH 一样，我们代入含时薛定谔方程，然后得到系数随时间的演化就可以，含时薛定谔方程写作：

$$i\hbar \frac{\partial \Psi}{\partial t} = \hat{H} \Psi \tag{440}$$

代入式（439）

$$i\hbar \frac{\partial \Psi(t)}{\partial t} = i\hbar \frac{\partial}{\partial t} \left[ \sum_K C_K(t) \Phi_K(t) \right] \tag{441}$$

提出求和符号：

$$i\hbar \frac{\partial \Psi(t)}{\partial t} = i\hbar \sum_K \left[ \frac{\partial}{\partial t} \Big( C_K(t) \cdot \Phi_K(t) \Big) \right] \tag{442}$$

根据求导法则 $(uv)' = u'v + uv'$，我们把式子展开：

$$\frac{\partial}{\partial t} \Big( C_K(t) \cdot \Phi_K(t) \Big) = \left( \frac{\partial C_K(t)}{\partial t} \right) \Phi_K(t) + C_K(t) \left( \frac{\partial \Phi_K(t)}{\partial t} \right) \tag{443}$$

为了简写，令 $\dot{C}_K \equiv \frac{\partial C_K}{\partial t}$：

$$i\hbar \frac{\partial \Psi}{\partial t} = i\hbar \sum_K \Big( \dot{C}_K \Phi_K + C_K \frac{\partial \Phi_K}{\partial t} \Big) \tag{444}$$

由于我们选择的是绝热基，因此 $\Phi_K$ 是当前时刻哈密顿量的本征态，所以 $\hat{H} \Phi_K = \varepsilon_K \Phi_K$，这时候 $\varepsilon_K$ 就是激子能量 $\omega_K$：

$$\hat{H}\Psi = \hat{H} \left( \sum_K C_K \Phi_K \right) = \hat{H} \left( \sum_K C_K \Phi_K \right) \tag{445}$$

哈密顿量 $\hat{H}$ 只对空间坐标产生作用，因此越过求和和 $C_K$。然后由于选择的绝热基，因此有本征值方程：

$$\hat{H} \Phi_K = \varepsilon_K \Phi_K \tag{446}$$

代入式（445）可得：

$$\hat{H} \Psi = \sum_K C_K \varepsilon_K \Phi_K \tag{447}$$

我们连接上式和式（444）：

$$ i\hbar \sum_K \Big( \dot{C}_K \Phi_K + C_K \frac{\partial \Phi_K}{\partial t} \Big) = \sum_K C_K \varepsilon_K \Phi_K \tag{448}$$

并且左乘一个基底态 $\langle \Phi_J |$，利用正交归一 $\langle \Phi_J | \Phi_K \rangle = \delta_{JK}$ 可以得到：

$$i\hbar \Big[ \dot{C}_J + \sum_K C_K \langle \Phi_J | \frac{\partial \Phi_K}{\partial t} \rangle \Big] = \varepsilon_J C_J \tag{449}$$

对于所有的 $\delta_{JK}$ 来说，只有 $J = K$ 的项能够存活下来，变成 1。

式（449）中，我们就得到了**多体非绝热耦合矩阵元** $\langle \Phi_J | \frac{\partial}{\partial t} | \Phi_K \rangle$，我们定义为 $D_{JK}$。因此，上式可以替换为：

$$i\hbar \dot{C}_J + i\hbar \sum_K C_K D_{JK} = \varepsilon_J C_J \tag{450}$$

移项：

$$i\hbar \dot{C}_J = \varepsilon_J C_J - i\hbar \sum_K C_K D_{JK} \tag{451}$$

我们之前使用 $\delta_{JK}$ 塌缩了求和符号，即：$\sum_K C_K \varepsilon_K \delta_{JK} \implies \varepsilon_J C_J$，为了好看，我们还原回去：

$$i\hbar \frac{\partial C_J(t)}{\partial t} = \sum_K C_K(t) \Big( \varepsilon_K \delta_{JK} - i\hbar D_{JK} \Big) \tag{452}$$

#### 多体非绝热耦合的单粒子表象

我们现在有多体的非绝热耦合：
$$\langle \Phi_J | \frac{\partial}{\partial t} | \Phi_K \rangle \tag{453}$$

但是我们只能通过单粒子表象计算，因此我们需要把多体非绝热耦合拆解能用单粒子表示的非绝热耦合。

根据**Casida 指认假设**，我们有式（436）：$\Phi_K = \sum_{jb} Z_{K,jb} \Phi_{jb}$，我们代入多体非绝热耦合右边：

$$\frac{\partial \Phi_K}{\partial t} = \sum_{jb} \dot{Z}_{K,jb} \Phi_{jb} + \sum_{jb} Z_{K,jb} \frac{\partial \Phi_{jb}}{\partial t} \tag{454}$$

在上式中，我们同样代入左矢，依然有 Casida 指认假设得到：$\langle \Phi_J | = \sum_{ia} Z_{J,ia}^* \langle \Phi_{ia} |$，其中，单激发组态之间是正交的（$\langle \Phi_{ia} | \Phi_{jb} \rangle = \delta_{ij}\delta_{ab}$），可得右边第一项为：

$$ \sum_{ia} Z_{J,ia}^* \dot{Z}_{K,ia} \tag{455}$$

但是第二项较为复杂，我们需要根据之前推导的二次量子化将其拆开。

对于从 $j \to b$ 激发的单激发组态，我们可以写出其二次量子化表示：

$$|\Phi_{jb}\rangle = \hat{a}_{b}^\dagger \hat{a}_{j} |\Phi_0\rangle \tag{456}$$

上式的物理含义前面已经推导过，这里不再赘述。

接下来我们计算其时间导数：$\frac{\partial}{\partial t} \Big( \hat{a}_{b}^\dagger \hat{a}_{j} |\Phi_0\rangle \Big)$，这是三个项相乘，根据求导法则，求导需要作用在每个项，因此需要写作：

$$\frac{\partial}{\partial t} |\Phi_{jb}\rangle = \underbrace{(\partial_t \hat{a}_{b}^\dagger) \hat{a}_{j} |\Phi_0\rangle}_{\text{项 A}} + \underbrace{\hat{a}_{b}^\dagger (\partial_t \hat{a}_{j}) |\Phi_0\rangle}_{\text{项 B}} + \underbrace{\hat{a}_{b}^\dagger \hat{a}_{j} (\partial_t |\Phi_0\rangle)}_{\text{项 C}} \tag{457}$$

> **说明：**
>
> - 我们选择升算符，写出其定义：
>
> $$\hat{a}_{b}^\dagger = \int \phi_{b}(\mathbf{r}, t) \hat{\psi}^\dagger(\mathbf{r}) d\mathbf{r} \tag{458}$$
>
> - 上式积分号里的 $\hat{\psi}^\dagger(\mathbf{r})$ 是常数算符，因此求导算符可以直接穿过，作用在空间波函数 $\phi_{b}(\mathbf{r}, t)$ 上，因此，在式（457）中，我们不能把算符的求导跳过。
>
> $$\frac{\partial}{\partial t} \hat{a}_{b}^\dagger = \int \left( \frac{\partial \phi_{b}(\mathbf{r}, t)}{\partial t} \right) \hat{\psi}^\dagger(\mathbf{r}) d\mathbf{r} \tag{459}$$

上式中波函数是随时间变化的，为了回到 KS 轨道表象，我们需要再把 $\frac{\partial \phi_{b}}{\partial t}$ 使用 $\phi_q$ 展开：

$$\frac{\partial \phi_{b}}{\partial t} = \sum_q \langle \phi_q | \frac{\partial \phi_{b}}{\partial t} \rangle \phi_q = \sum_q d_{qb} \phi_q \tag{460}$$

上式中，我们使用正交归一的 KS 轨道 $\phi_q$，构造了一个单位算符 $\hat{I} = \sum_q |\phi_q\rangle \langle \phi_q|$，作用在了 $|\frac{\partial \phi_{b}}{\partial t}\rangle$ 上面：

$$\left| \frac{\partial \phi_{b}}{\partial t} \right\rangle = \hat{I} \left| \frac{\partial \phi_{b}}{\partial t} \right\rangle \tag{461}$$

代入单位算符的展开式：

$$\left| \frac{\partial \phi_{b}}{\partial t} \right\rangle = \sum_q \langle \phi_q | \frac{\partial \phi_{b}}{\partial t} \rangle |\phi_q\rangle \tag{462}$$

其中，内积 $\langle \phi_q | \frac{\partial \phi_{b}}{\partial t} \rangle$ 就是**单粒子表象下的非绝热耦合** $d_{qb}$。

因此，任意的上升算符的时间导数我们可以写作：

$$\partial_t \hat{a}_p^\dagger = \sum_q \langle \phi_q | \frac{\partial}{\partial t} | \phi_p \rangle \hat{a}_q^\dagger \equiv \sum_q d_{qp} \hat{a}_q^\dagger \tag{463}$$

> **说明：**
>
> - 上面的推导中，我们设置了一个固定的时刻 $t$，在这个时刻体系所有的 KS 轨道（$\phi_1, \phi_2, \dots, \phi_q, \dots, \phi_{b}, \dots$）构成了一套正交归一的基底，彼此之间相互独立不能相互表示。
>
> - 我们把 $\frac{\partial \phi_{b}}{\partial t}$ 使用 $\phi_q$ 展开是因为 $\frac{\partial \phi_{b}}{\partial t}$ 是一个全新的函数，这个函数可以被完备的 KS 表象线性组合。

> **注**：式（463） 是对任意轨道 $p$ 成立的**通用**关系，故 $p,q$ 取泛指。其中 $q$ 来自完备基的单位算符求和，本就遍历全部轨道（占据+虚），符合“$p,q,r,s$ 为泛指”的约定；投影到单激发组态后只有满足条件的 $q$ 存活（见下）。

> **例子：**
>
> - 三维坐标系中基底为 $\hat{x}, \hat{y}, \hat{z}$，他们之间是不能相互表示的。
>
> - 现在这个坐标系旋转起来，其中我们关心的 $\hat{x}$ 轴的端点产生了一个速度向量 $\frac{d\hat{x}}{dt}$，这个速度向量的方向不是沿着 $\hat{x}$ 自身，由于旋转，$\hat{x}$ 的速度方向实际上是指向 $\hat{y}$ 和 $\hat{z}$。
>
> - 我们把速度向量在 $\hat{y}$ 和 $\hat{z}$ 轴上投影：
>
> $$\frac{d\hat{x}}{dt} = \omega_{yx} \hat{y} + \omega_{zx} \hat{z} \tag{464}$$
>
> - 上式中的系数 $\omega_{yx}$ 就等于 $\hat{y} \cdot \frac{d\hat{x}}{dt}$，即非绝热耦合。

扯多了，我们继续回到式（457），对于第一项 A 来说，我们有：

$$(\partial_t \hat{a}_{b}^\dagger) \hat{a}_{j} |\Phi_0\rangle = \left( \sum_q d_{qb} \hat{a}_q^\dagger \right) \hat{a}_{j} |\Phi_0\rangle = \sum_q d_{qb} (\hat{a}_q^\dagger \hat{a}_{j} |\Phi_0\rangle) \tag{465}$$

其中，右边 $\hat{a}_q^\dagger \hat{a}_{j} |\Phi_0\rangle$ 是一个新的单激发组态 $|\Phi_{jq}\rangle$。

> **注意：**
>
> - 这里的 $q$ 必须是虚轨道。

然后我们使用左矢 $\langle \Phi_{ia} |$ 作用在 A 项：

$$\sum_q d_{qb} \langle \Phi_{ia} | \Phi_{jq} \rangle \tag{466}$$

其中，内积写作：$\langle \Phi_{ia} | \Phi_{jq} \rangle = \delta_{ij} \delta_{aq}$，代入上式中，求和号被塌缩，所有 $q$ 都被变成了 $a$：

$$\langle \Phi_{ia} | (\partial_t \hat{a}_{b}^\dagger) \hat{a}_{j} |\Phi_0\rangle = d_{ab} \delta_{ij} \tag{467}$$

它代表了空穴没动（$\delta_{ij}$），电子从虚轨道 $b$ 散射到了虚轨道 $a$。

同理第二项 B 可以写作如下：（具体过程相同，不再赘述）

$$\langle \Phi_{ia} | \hat{a}_{b}^\dagger (\partial_t \hat{a}_{j}) |\Phi_0\rangle = d_{ij} \delta_{ab} \tag{468}$$

第三项 C，我们单独写出：$\hat{a}_{b}^\dagger \hat{a}_{j} (\partial_t |\Phi_0\rangle)$，这项的 $\partial_t |\Phi_0\rangle$ 是一堆单激发组态的线性组合，而前面又作用了一项 $\hat{a}_{b}^\dagger \hat{a}_{j}$，表示挖了一个空穴，多了一个电子，单激发再经过一次单激发，变成了双激发组态。C 的本质是一个有 2 个电子跃迁的多体态。

然而我们作用的左矢 $\langle \Phi_{ia} |$ 是一个单激发组态。在量子力学中，单激发组态和双激发组态是正交的，因此相差电子数不同，内积为 0。因此 C 项对耦合没有贡献。

> **说明：**
>
> - 不仅是单激发和双激发正交，任何相差激发级数不同的态（如单激发与三激发、双激发与四激发），它们的内积均为 0。

因此，式（457）我们可以写成：

$$\langle \Phi_{ia} | \frac{\partial \Phi_{jb}}{\partial t} \rangle = \delta_{ij}\, d_{ab} + \delta_{ab}\, d_{ij} \tag{469}$$

右侧第一项为电子散射，第二项为空穴散射。

我们把上式代回式（454），就能得到**多体非绝热耦合的单粒子表象表示**：

$$D_{JK} = \sum_{ia} Z_{J,ia}^* \dot{Z}_{K,ia} + \sum_{iab} Z_{J,ia}^* Z_{K,ib}\, d_{ab} + \sum_{ija} Z_{J,ia}^* Z_{K,ja}\, d_{ij} \tag{470}$$

第一项是激子成分，这里没有任何单粒子耦合的成分，只有 Casida 展开系数 $Z$ 自身对时间的导数 $\dot{Z}_{K,ia}$，它代表跃迁通道 $i \to a$ 在激子 $K$ 中的权重。

第二项是电子散射成分，空穴的下标被固定，而电子下标从 $b$ 变成了 $a$，它代表了两个虚轨道之间的单粒子非绝热耦合。在这个跃迁通道中，电子从虚轨道 $b$ 跃迁到了虚轨道 $a$ 上，而留在价带的空穴 $i$ 在这个极短的瞬间保持静止。

第三项是空穴散射成分，同理，电子下标固定，空穴从占据轨道 $j$ 转移到了占据轨道 $i$，它代表了两个占据轨道之间的单粒子非绝热耦合。

## OT-SRSH 泛函（最优调控范围分离杂化泛函）

我们已经了解到了激子本身是一个激发的电子和留下的空穴通过库伦引力吸引形成的束缚态，那么我们需要计算准确的激子就需要看两件事：

1. 能不能算准电子跃迁的门槛，即带隙
2. 能不能算准电子空穴相互作用大小，即激子结合能

对于我们经常使用的 GGA （广义梯度近似）来说，严重低估了交换作用，导致对带隙预测偏低，并且激发电子后，缺乏电子和空穴之间库伦相互作用的描述，因此无法描述激子结合能。

而杂化泛函中，我们熟悉的 HSE06 杂化泛函则也无法描述激子，它的完整公式如下：

$$E_{xc}^{\text{HSE06}} = \underbrace{a E_{x}^{\text{HF, SR}}(\omega) + (1-a) E_{x}^{\text{PBE, SR}}(\omega)}_{\text{短程交换 (SR)}} + \underbrace{E_{x}^{\text{PBE, LR}}(\omega)}_{\text{长程交换 (LR)}} + \underbrace{E_{c}^{\text{PBE}}}_{\text{全局关联}} \tag{471}$$

我们可以看到，对于 HSE06来说，在短程部分是 Hartree-Fock 精确交换能和部分 PBE 交换能的混合比例。但是在长程部分，没有任何 HF 交换作用。因此 HSE06 描述长程相互作用和 GGA 完全一样。

在 TDDFT 的 Casida 方程中，电子和空穴之间的相互作用强度完全取决于泛函的交换关联核 $f_{xc}$，而交换关联核必须具有非局域的、随距离 $1/r$ 衰减的长程库仑吸引力，而只有 Hartree-Fock 交换才能提供这种非局域的吸引力，因此，HSE06 同样无法描述准确的激子效应。

而另外一个杂化泛函 PBE0，我们可以写出它的公式：

$$E_{xc}^{\text{PBE0}} = \frac{1}{4} E_{x}^{\text{HF}} + \frac{3}{4} E_{x}^{\text{PBE}} + E_{c}^{\text{PBE}} \tag{472}$$

可以看到，PBE0 并没有区分长程或者短程相互作用，而是有一个全局的25% HF 精确交换能。因此，PBE0 比前面提到的 GGA，HSE06 都要好一些，起码能够定性把激子束缚住。不过缺点是无法改变 25% 的这个值。

我们前面推导的 GW-BSE 能够给出真实的准粒子带隙，它不仅考虑了电子-空穴相互作用，还多考虑了与周围激发电子空穴对之间的复杂相互作用。但是由于 BSE 中同样引入近似，所以这部分也没那么精准，但也足够了。但是慢，算几千个 GW-BSE 明显不现实。

下面主要介绍吕刚老师引入的 OT-SRSH 杂化泛函，它把库伦算符 $1/r$ 拆成长程短程两部分：

$$\frac{1}{r} = \underbrace{\frac{1-[\alpha+\beta \text{erf}(\gamma r)]}{r}}_{\text{短程 (SR)}} + \underbrace{\frac{\alpha+\beta \text{erf}(\gamma r)}{r}}_{\text{长程 (LR)}} \tag{473}$$

两段用不同的交换来算，短程可以交给 GGA 解决，长程就需要精确的 HF 交换。$\alpha$ 定义了短程精确交换比例。$\alpha + \beta$ 则是长程精确交换比例。$\gamma$ 为范围分离参数，它决定了短程切换到长程的长度尺度。$\text{erf}(\gamma r)$ 是误差函数，电子距离很小趋近于0，距离很大趋近于1。

接下来，OT-SRSH 的交换关联能公式可以写为：

$$E_{xc}^{\mathrm{RSH}} = \alpha E_{xx} + \beta E_{xx}^{\mathrm{LR}} + (1-\alpha)E_{\mathrm{KSx}} - \beta E_{\mathrm{KSx}}^{\mathrm{LR}} + E_{\mathrm{KSc}} \tag{474}$$

> **说明：**
>
> - $E_{\mathrm{KSc}}$ 是纯粹的相关能，这里把全部的相关能交给了传统泛函，例如 PBE。
> - $\alpha E_{xx}$ 代表了全空间混入了多少比例的 Fock 精确交换能。
> - 因为前面讲过 PBE 无法描述长程库伦作用，因此用 $-\beta E_{\mathrm{KSx}}^{\mathrm{LR}}$ 扣除传统泛函的长程交换部分，然后用 $+\beta E_{xx}^{\mathrm{LR}}$ 把长程的 Fock 精确交换能给补了回来。

我们前面推导过，在真空中，两个电荷之间的库仑势就是最纯粹的 $1/r$。但是固体材料中充满了电子云，会削弱（或者叫做屏蔽）我们想看的两个电荷之间的相互作用。在极远处，真实库伦势严格渐近于 $1/(\varepsilon_0 r)$，$\varepsilon_0$ 是材料的宏观介电常数。

为了强制泛函在长程下表现出正确的物理渐近行为，OT-SRSH 引入了一个刚性的物理约束：

$$\alpha + \beta = \varepsilon_0^{-1} \tag{475}$$

如果我们做常规的杂化泛函计算，需要在每一个电子步中计算双电子积分的 Fork 精确交换积分：

$$\iint \frac{\phi_i^*(\mathbf{r}) \phi_j^*(\mathbf{r}') \phi_j(\mathbf{r}) \phi_i(\mathbf{r}')}{|\mathbf{r} - \mathbf{r}'|} d\mathbf{r} d\mathbf{r}' \tag{476}$$

这个双电子积分的计算量是随着体系电子数的四次方增长，它的时间复杂度是 $O(N^4)$，而普通的 PBE 泛函复杂度为 $O(N^3)$。

但是 OT-SRSH 中，并不进行常规的杂化泛函 SCF 计算，首先会使用其他泛函如 PBE 跑一个常规的 SCF，得到收敛的基态电子密度和未受微扰的 KS 轨道 $\phi_i^{(0)}$ 和能量 $\varepsilon_i^{(0)}$。

接下来，那部分及其耗时的双电子积分被视为微扰算符 $\hat{H}'_{\text{Fock}}$，利用微扰理论计算单词期望值：

$$\varepsilon_i^{\text{OT-SRSH}} \approx \varepsilon_i^{(0)} + \langle \phi_i^{(0)} | \hat{H}'_{\text{Fock}} | \phi_i^{(0)} \rangle \tag{477}$$

这样我们就获得了一阶修正的 RSH-KS 本征值和本征函数。

> **说明：**
>
> - 在绝大部分情况下，我们认为 PBE 算出来的波函数形状已经非常好了，它错的一般只有对应的能量本征值导致带隙低估。
>
> - 因此我们在做 RSH 或者 GW 计算时候可以直接使用 PBE 的波函数。

