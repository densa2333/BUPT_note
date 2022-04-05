# *大学物理 C 章末总结*

----

# 力学

## 一、质点运动学







## 二、质点动力学











# 电磁学

## 一、真空中的静电场

### 1. 电荷 库仑定律

#### 电荷

**来源：**

- 摩擦起电

**性质：**

- 电荷是基本粒子的固有属性
- 电荷守恒定律
- 电荷的量子化
- 电荷的电量运动不变性



#### 库仑定律

**库仑定律矢量表达式：**
$$
\vec{F} = k\cfrac{q_1q_2\vec{r}}{r^3} = k\cfrac{q_1q_2}{r^2}\hat{r_0}
$$
**引入真空介电常数：**(目的是使后面大量电磁学公式不出现 $4\pi$ 因子)
$$
\varepsilon_0 = \cfrac1{4\pi k} = 8.854187817\times 10^{-12}\rm C^2N^{-1}m^{-2}
$$
**库仑定律：**
$$
\vec F = \cfrac1{4\pi \varepsilon_0}\cfrac{q_1q_2\vec{r}}{r^3} = \cfrac1{4\pi \varepsilon_0}\cfrac{q_1q_2}{r^2}\hat{r_0}
$$
**适用条件：**

- 点电荷 - 理想模型
- 真空中
- 施力电荷对观测者静止



#### 静电力叠加原理

**对于点电荷集合：**
$$
\vec F = \sum_\limits{i=1}^\limits{n} \vec F_i = \sum_\limits{i=1}^\limits{n} \cfrac{q_0q_i\vec r_i}{4\pi \varepsilon_0 r_i^3}
$$
**对于连续均匀带电体：**
$$
\vec F = \cfrac1{4\pi \varepsilon_0} \int \cfrac{q_0dq}{r^2}\hat r
$$




### 2. 电场 电场强度

#### 静电场

**静电场：相对于观察者静止的带电体周围的电场**

- 场中任何带电体都受电场力作用——动量传递
- 带电体在电场中移动时，场对带电体做功——能量传递

用 $\vec E \ , \ U$ 来分别描述静电场的上述两项性质



#### 电场强度

**定义：**
$$
\vec E = \cfrac{\vec F}{q_0}
$$
大小：等于单位检验电荷在该点所受电场力

方向：与 $+q_0$ 受力方向相同

单位： $\rm N/C \ ;\  V/m$

**场强叠加原理：**
$$
\vec E = \sum_\limits{i=1}^\limits{n} \vec E_i
$$
研究静电场也就是研究各种场源电荷的空间矢量函数 $\vec E(r)$ 分布。



#### 电场强度的计算

**1. 点电荷：**
$$
\vec E = \cfrac{\vec F}{q_0} = \cfrac q{4\pi \varepsilon_0r^2} \hat r_0
$$
静止点电荷场强特性：

- 球对称
- 与 $r$ 平方反比的非均匀场

*讨论：*

- 当 $r \to 0$ 时， $E \to \infin$ 。点电荷模型失效，公式不再适用。

**2. 点电荷系：**
$$
\vec E = \sum_\limits{i=1}^\limits{n} \cfrac{q_i}{4\pi \varepsilon_0 r_i^2}\hat r_i
$$
原则上讲由点电荷的电场强度公式和场强叠加原理可以求得任意点电荷系的场强。

##### 电偶极子

电偶极子是一对靠得很近的等量异号点电荷，它是个相对的概念，也是一种实际的物理模型。

当 $r >> l$ 时，电偶极子

- 轴线延长线上的场强：

$$
\vec E = \cfrac {\vec p}{2\pi \varepsilon_0 r^3}
$$

- 中垂面上的场强；

$$
\vec E = - \cfrac {\vec p}{4\pi \varepsilon_0 r^3}
$$

**3. 连续带电体**
$$
\begin{gather}
d\vec E = \cfrac{\vec r dq}{4\pi \varepsilon_0 r^3} \\
dq = 
\begin{cases}
\lambda dl \\
\sigma dS \\
\rho dV \\
\end{cases} \\
\vec E = \int d\vec E
\end{gather}
$$




##### 均匀带电细棒

$$
d \vec E = \cfrac{dq}{4\pi \varepsilon_0 r^3} \vec r \\

dq = \lambda dx \ \ \ \  \lambda = \cfrac qL \\ \\
dE = \cfrac{\lambda dx}{4\pi \varepsilon_0 r^2} \\
dE_x = dE\cos \theta \ \ \ \  dE_y = dE\sin \theta \\
E_x = \int dE_x = \int \cfrac{\lambda dx}{4\pi \varepsilon_0 r^2} \cos \theta \\
E_y = \int dE_y = \int \cfrac{\lambda dx}{4\pi \varepsilon_0 r^2} \sin \theta \\ \\
统一积分变量：\\
x = - y \cot \theta \\
dx = y \csc ^2\theta d\theta \\
r^2 = y^2 + x ^2 = y^2 \csc^2 \theta \\ \\
E_x = \int_{\theta_1}^{\theta_2} \cfrac{\lambda}{4\pi \varepsilon_0 y} \cos \theta d\theta = \cfrac{\lambda}{4\pi \varepsilon_0 y} (\sin \theta_2 - \sin \theta_1) \\
E_y = \int_{\theta_1}^{\theta_2} \cfrac{\lambda}{4\pi \varepsilon_0 y} \sin \theta d\theta = \cfrac{\lambda}{4\pi \varepsilon_0 y} (\cos \theta_1 - \cos \theta_2) \\
$$

- 无限长带电直线场强公式：

$$
E = \cfrac {\lambda}{2\pi \varepsilon_0 y}
$$

- 离细杆无穷远可看作点电荷：

$$
E = \cfrac q{4\pi \varepsilon_0 y^2}
$$



##### 均匀带电细圆环

$$
d \vec E = \cfrac{dq}{4\pi \varepsilon_0 r^3} \vec r \\
dq = \lambda dl = \cfrac q{2\pi R} dl \\
E = E_x = \int \cfrac {dq}{4\pi \varepsilon_0 r^2} \cos \theta = \cfrac 1{4\pi \varepsilon_0 r^2} \cfrac {qdl}{2\pi R} \cfrac xr \\
= \cfrac {qx}{4\pi \varepsilon_0 r^3} \cfrac1{2\pi R}\int_0^{x\pi R}dl = \cfrac{qx}{4\pi \varepsilon_0 (x^2 + R^2)^{\frac32}}
$$

- 环心处

$$
E = 0
$$

- 无穷远处

$$
E \approx \cfrac q{4\pi \varepsilon_0 x^2}
$$



##### 均匀带电圆盘

- 无限大均匀带电平面

$$
E = \cfrac{\sigma}{2\varepsilon_0}
$$

- 无穷远处

$$
E = \cfrac q{4\pi \varepsilon_0 x^2}
$$





### 3. 静电场的高斯定理

#### 电场线

形象地描述电场的性质。由一系列有向曲线构成。

- 方向：电场线上每点的切线方向就是该点的场强方向
- 大小：通过某点处垂直于 $\vec E$ 的单位面积的电场线条数与该点场强大小成正比、

性质：

- 电场线起始于正电荷（或无限远处）
- 在没有点电荷的空间，任何两条电力线不会相交
- 电场线不会形成闭合曲线



#### 电通量

$$
\Phi_e = \vec E \cdot \vec S
$$





#### 高斯定理

在真空电场中，穿过任意闭合曲面的电场强度通量等于该闭合曲面所包围的所有电荷的代数和除以真空介电常数，即：
$$
\Phi_e = \oiint_S \vec E \ \cdot \ d\vec S = \cfrac 1{\varepsilon_0} \sum\limits_{i=1}^n q_i^{\rm in}
$$






#### 高斯定理的应用

**1. 定性分析一些问题**

例如分析电场线性质

**2. 由 $\vec E$ 的分部求空间电荷分布**

**3. 求解某些对称分布的电场**

- 成立条件：静电场
- 求解条件：电场分布具有某些对称性

找高斯面

##### 均匀带电球体（面）

$$
\Phi_e = E \cdot 4\pi r^2 = \cfrac 1{\varepsilon_0}\sum q^{\rm in}
$$

- 球体外部：

$$
E_外 = \cfrac q{4\pi \varepsilon_0 r^2}
$$

- 球体内部：

$$
E_内 = \cfrac {qr}{4\pi \epsilon_0 R^3}
$$



##### 无限长均匀带电直线

$$
\Phi_e = E \cdot 2\pi rh = \cfrac 1{\varepsilon_0} \lambda h
$$

- 无限长均匀带电直线：

$$
E = \cfrac {\lambda}{2\pi \varepsilon_0 r}
$$



##### 无限大均匀带电平面

$$
\Phi_e = 2ES = \cfrac 1{\varepsilon_0} \sigma S
$$

- 无限大均匀带电平面；

$$
E = \cfrac{\sigma}{2\varepsilon_0}
$$





##### 无限长均匀带电圆柱体（面）

- 柱内：

$$
E = 0
$$

- 柱外：

$$
E = \cfrac {\lambda}{2\pi \varepsilon_0 r}
$$





### 4. 电势能

