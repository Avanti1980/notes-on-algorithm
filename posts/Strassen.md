　　设矩阵$\Av = (a_{ij})$和$\Bv = (b_{ij})$是$n$阶方阵，乘积$\Cv = (c_{ij})$亦是$n$阶方阵，其中
\begin{align\*}
    c\_{ij} = \sum\_{k \in [n]} a\_{ik} b\_{kj}
\end{align\*}
因此按标准的矩阵乘法，计算$\Cv$的时间开销为$\Omega(n^3)$，事实上这个时间复杂度是可以改进的。

## 分治递归

　　设计算$\Cv$的时间开销为$T(n)$，将矩阵分成$2 \times 2 = 4$块，由分块矩阵乘法有
\begin{align\*}
    \begin{bmatrix}
        \Cv\_{11} & \Cv\_{12} \\\\ \Cv\_{21} & \Cv\_{22}
    \end{bmatrix} =
    \begin{bmatrix}
        \Av\_{11} & \Av\_{12} \\\\ \Av\_{21} & \Av\_{22}
    \end{bmatrix}
    \begin{bmatrix}
        \Bv\_{11} & \Bv\_{12} \\\\ \Bv\_{21} & \Bv\_{22}
    \end{bmatrix} =
    \begin{bmatrix}
        \Av\_{11} \Bv\_{11} + \Av\_{12} \Bv\_{21} & \Av\_{11} \Bv\_{12} + \Av\_{12} \Bv\_{22} \\\\
        \Av\_{21} \Bv\_{11} + \Av\_{22} \Bv\_{21} & \Av\_{21} \Bv\_{12} + \Av\_{22} \Bv\_{22}
    \end{bmatrix}
\end{align\*}
其中包含$8$个$n/2$阶方阵相乘、$4$个$n/2$阶方阵相加，注意每个$n/2$阶方阵有$n^2/4$个元素，因此共需进行$n^2$次加法，综上有递推关系
\begin{align\*}
    T(n) = 8 \cdot T(n/2) + c\_1 n^2
\end{align\*}
其中$c_1$为单次加法的时间开销。设$n = 2^k$，则
\begin{align\*}
    T(2^k)               & = 8^1 \cdot T(2^{k-1}) + 8^0 \cdot c\_1 4^k     \\\\
    8^1 \cdot T(2^{k-1}) & = 8^2 \cdot T(2^{k-2}) + 8^1 \cdot c\_1 4^{k-1} \\\\
    8^2 \cdot T(2^{k-2}) & = 8^3 \cdot T(2^{k-3}) + 8^2 \cdot c\_1 4^{k-2} \\\\
                         & \vdots                                         \\\\
    8^{k-1} \cdot T(2^1) & = 8^k \cdot T(2^0) + 8^{k-1} \cdot c\_1 4^1
\end{align\*}
注意$8^k = n^3$，$T(1) = c_2$是单次乘法的时间开销，累加可得
\begin{align\*}
    T(n) & = c\_2 n^3 + c\_1 4^k + 2^1 \cdot c\_1 4^k + 2^2 \cdot c\_1 4^k + \cdots + 2^{k-1} \cdot c\_1 4^k = c\_2 n^3 + c\_1 4^k \frac{1-2^k}{1-2} \\\\
         & = c\_2 n^3 + c\_1 n^2 (n-1) = (c\_2 + c\_1) n^3 - c\_1 n^2
\end{align\*}
即直接分治递归并不能改进时间复杂度。

## 基本想法

　　要想改进时间复杂度，必须得减少子问题的个数，即乘法的次数。将乘积拉直，易知
\begin{align\*}
    \begin{bmatrix}
        \Av\_{11} \Bv\_{11} + \Av\_{12} \Bv\_{21} \\\\
        \Av\_{11} \Bv\_{12} + \Av\_{12} \Bv\_{22} \\\\
        \Av\_{21} \Bv\_{11} + \Av\_{22} \Bv\_{21} \\\\
        \Av\_{21} \Bv\_{12} + \Av\_{22} \Bv\_{22}
    \end{bmatrix} =
    \begin{bmatrix}
        \Av\_{11} & \zerov   & \Av\_{12} & \zerov   \\\\
        \zerov   & \Av\_{11} & \zerov   & \Av\_{12} \\\\
        \Av\_{21} & \zerov   & \Av\_{22} & \zerov   \\\\
        \zerov   & \Av\_{21} & \zerov   & \Av\_{22}
    \end{bmatrix}
    \begin{bmatrix}
        \Bv\_{11} \\\\ \Bv\_{12} \\\\ \Bv\_{21} \\\\ \Bv\_{22}
    \end{bmatrix} = \widetilde{\Av}
    \begin{bmatrix}
        \Bv\_{11} \\\\ \Bv\_{12} \\\\ \Bv\_{21} \\\\ \Bv\_{22}
    \end{bmatrix}
\end{align\*}
<span class="blue">现假设$\widetilde{\Av}$可以分解成$m$个“块秩$1$矩阵”的和：</span>
\begin{align} \label{eq: decomposition}
    \widetilde{\Av} =
    \begin{bmatrix}
        \Av\_{11} & \zerov   & \Av\_{12} & \zerov   \\\\
        \zerov   & \Av\_{11} & \zerov   & \Av\_{12} \\\\
        \Av\_{21} & \zerov   & \Av\_{22} & \zerov   \\\\
        \zerov   & \Av\_{21} & \zerov   & \Av\_{22}
    \end{bmatrix} = \sum\_{i \in [m]}
    \begin{bmatrix}
        \Pv\_{i1} \\\\ \Pv\_{i2} \\\\ \Pv\_{i3} \\\\ \Pv\_{i4}
    \end{bmatrix} \Rv\_i
    \begin{bmatrix}
        \Qv\_{i1} \\\\ \Qv\_{i2} \\\\ \Qv\_{i3} \\\\ \Qv\_{i4}
    \end{bmatrix}^\top
\end{align}
<span class="blue">其中$\Rv_i$只由$\Av_{11}, \Av_{12}, \Av_{21}, \Av_{22}$进行加减运算得到且$\Pv_{i1}, \ldots,\Pv_{i4}, \Qv_{i1}, \ldots, \Qv_{i4} \in \{ \pm \Iv, \zerov \}$</span>。则
\begin{align\*}
    \begin{bmatrix}
        \Av\_{11} \Bv\_{11} + \Av\_{12} \Bv\_{21} \\\\
        \Av\_{11} \Bv\_{12} + \Av\_{12} \Bv\_{22} \\\\
        \Av\_{21} \Bv\_{11} + \Av\_{22} \Bv\_{21} \\\\
        \Av\_{21} \Bv\_{12} + \Av\_{22} \Bv\_{22}
    \end{bmatrix} = \sum\_{i \in [m]}
    \begin{bmatrix}
        \Pv\_{i1} \\\\ \Pv\_{i2} \\\\ \Pv\_{i3} \\\\ \Pv\_{i4}
    \end{bmatrix} \Rv\_i
    \class{green}{\begin{bmatrix}
                   \Qv\_{i1} \\\\ \Qv\_{i2} \\\\ \Qv\_{i3} \\\\ \Qv\_{i4}
               \end{bmatrix}^\top
        \begin{bmatrix}
            \Bv\_{11} \\\\ \Bv\_{12} \\\\ \Bv\_{21} \\\\ \Bv\_{22}
        \end{bmatrix}} = \sum\_{i \in [m]}
    \begin{bmatrix}
        \Pv\_{i1} \\\\ \Pv\_{i2} \\\\ \Pv\_{i3} \\\\ \Pv\_{i4}
    \end{bmatrix} \Rv\_i \class{green}{\Sv\_i} = \sum\_{i \in [m]}
    \begin{bmatrix}
        \Pv\_{i1} \\\\ \Pv\_{i2} \\\\ \Pv\_{i3} \\\\ \Pv\_{i4}
    \end{bmatrix} \Tv\_i
\end{align\*}
其中$\Sv_i = \Qv_{i1} \Bv_{11} + \Qv_{i2} \Bv_{12} + \Qv_{i3} \Bv_{21} + \Qv_{i4} \Bv_{22}$只由$\Bv_{11}, \Bv_{12}, \Bv_{21}, \Bv_{22}$进行加减运算得到。计算全部$m$个$\Tv_i = \Rv_i \Sv_i$会产生$m$个子问题。又$\Pv_{i1}, \ldots, \Pv_{i4} \in \{ \pm \Iv, \zerov \}$，因此
\begin{align\*}
    \begin{bmatrix}
        \Av\_{11} \Bv\_{11} + \Av\_{12} \Bv\_{21} \\\\
        \Av\_{11} \Bv\_{12} + \Av\_{12} \Bv\_{22} \\\\
        \Av\_{21} \Bv\_{11} + \Av\_{22} \Bv\_{21} \\\\
        \Av\_{21} \Bv\_{12} + \Av\_{22} \Bv\_{22}
    \end{bmatrix} = \sum\_{i \in [m]}
    \begin{bmatrix}
        \Pv\_{i1} \\\\ \Pv\_{i2} \\\\ \Pv\_{i3} \\\\ \Pv\_{i4}
    \end{bmatrix} \Tv\_i =
    \begin{bmatrix}
        \Pv\_{11} \Tv\_1 + \cdots + \Pv\_{m1} \Tv\_m \\\\
        \Pv\_{12} \Tv\_1 + \cdots + \Pv\_{m2} \Tv\_m \\\\
        \Pv\_{13} \Tv\_1 + \cdots + \Pv\_{m3} \Tv\_m \\\\
        \Pv\_{14} \Tv\_1 + \cdots + \Pv\_{m4} \Tv\_m
    \end{bmatrix}
\end{align\*}
只由$\Tv_1, \ldots, \Tv_m$进行加减运算得到。综上，关键就是如何使式(\ref{eq: decomposition})中的$m < 8$。

　　下面给出一个$m = 7$的分解方法，首先去掉左上的$\Av_{11}$和右下的$\Av_{22}$
\begin{align\*}
    \widetilde{\Av} -
     &
    \begin{bmatrix}
        \Av\_{11} & \zerov & \Av\_{11} & \zerov \\\\
        \zerov   & \zerov & \zerov   & \zerov \\\\
        \Av\_{11} & \zerov & \Av\_{11} & \zerov \\\\
        \zerov   & \zerov & \zerov   & \zerov
    \end{bmatrix} -
    \begin{bmatrix}
        \zerov & \zerov & \zerov & \zerov \\\\ \zerov & \Av\_{22} & \zerov & \Av\_{22} \\\\ \zerov & \zerov & \zerov & \zerov \\\\ \zerov & \Av\_{22} & \zerov & \Av\_{22}
    \end{bmatrix} =
    \begin{bmatrix}
        \zerov & \zerov & \Av\_{12} - \Av\_{11} & \zerov \\\\ \zerov & \Av\_{11} - \Av\_{22} & \zerov & \Av\_{12} - \Av\_{22} \\\\ \Av\_{21} - \Av\_{11} & \zerov & \Av\_{22} - \Av\_{11} & \zerov \\\\ \zerov & \Av\_{21} - \Av\_{22} & \zerov & \zerov
    \end{bmatrix} \\\\
     & =
    \begin{bmatrix}
        \zerov & \zerov & \zerov & \zerov \\\\ \zerov & \Av\_{11} - \Av\_{22} & \Av\_{11} - \Av\_{22} & \zerov \\\\ \zerov & \Av\_{22} - \Av\_{11} & \Av\_{22} - \Av\_{11} & \zerov \\\\ \zerov & \zerov & \zerov & \zerov
    \end{bmatrix} +
    \begin{bmatrix}
        \zerov & \zerov & \Av\_{12} - \Av\_{11} & \zerov \\\\ \zerov & \zerov & \Av\_{22} - \Av\_{11} & \Av\_{12} - \Av\_{22} \\\\ \Av\_{21} - \Av\_{11} & \Av\_{11} - \Av\_{22} & \zerov & \zerov \\\\ \zerov & \Av\_{21} - \Av\_{22} & \zerov & \zerov
    \end{bmatrix} \\\\
     & = \begin{bmatrix}
             \zerov & \zerov & \zerov & \zerov \\\\ \zerov & \Av\_{11} - \Av\_{22} & \Av\_{11} - \Av\_{22} & \zerov \\\\ \zerov & \Av\_{22} - \Av\_{11} & \Av\_{22} - \Av\_{11} & \zerov \\\\ \zerov & \zerov & \zerov & \zerov
         \end{bmatrix} +
    \begin{bmatrix}
        \zerov & \zerov & \zerov              & \zerov              \\\\
        \zerov & \zerov & \Av\_{22} - \Av\_{12} & \Av\_{12} - \Av\_{22} \\\\
        \zerov & \zerov & \zerov              & \zerov              \\\\
        \zerov & \zerov & \zerov              & \zerov
    \end{bmatrix} +
    \begin{bmatrix}
        \zerov & \zerov & \Av\_{12} - \Av\_{11} & \zerov \\\\
        \zerov & \zerov & \Av\_{12} - \Av\_{11} & \zerov \\\\
        \zerov & \zerov & \zerov              & \zerov \\\\
        \zerov & \zerov & \zerov              & \zerov
    \end{bmatrix}                                                                                                                                                                                \\\\
     & \qquad +
    \begin{bmatrix}
        \zerov              & \zerov              & \zerov & \zerov \\\\
        \zerov              & \zerov              & \zerov & \zerov \\\\
        \Av\_{21} - \Av\_{11} & \Av\_{11} - \Av\_{21} & \zerov & \zerov \\\\
        \zerov              & \zerov              & \zerov & \zerov
    \end{bmatrix} +
    \begin{bmatrix}
        \zerov & \zerov              & \zerov & \zerov \\\\
        \zerov & \zerov              & \zerov & \zerov \\\\
        \zerov & \Av\_{21} - \Av\_{22} & \zerov & \zerov \\\\
        \zerov & \Av\_{21} - \Av\_{22} & \zerov & \zerov
    \end{bmatrix}                                                                                                                                                                                \\\\
     & =
    \begin{bmatrix}
        \Iv \\\\ \zerov \\\\ \Iv \\\\ \zerov
    \end{bmatrix} \Av\_{11}
    \begin{bmatrix}
        \Iv \\\\ \zerov \\\\ \Iv \\\\ \zerov
    \end{bmatrix}^\top +
    \begin{bmatrix}
        \zerov \\\\ \Iv \\\\ \zerov \\\\ \Iv
    \end{bmatrix} \Av\_{22}
    \begin{bmatrix}
        \zerov \\\\ \Iv \\\\ \zerov \\\\ \Iv
    \end{bmatrix}^\top +
    \begin{bmatrix}
        \zerov \\\\ \Iv \\\\ -\Iv \\\\ \zerov
    \end{bmatrix} (\Av\_{11} - \Av\_{22})
    \begin{bmatrix}
        \zerov \\\\ \Iv \\\\ \Iv \\\\ \zerov
    \end{bmatrix}^\top +
    \begin{bmatrix}
        \zerov \\\\ \Iv \\\\ \zerov \\\\ \zerov
    \end{bmatrix} (\Av\_{22} - \Av\_{12})
    \begin{bmatrix}
        \zerov \\\\ \zerov \\\\ \Iv \\\\ -\Iv
    \end{bmatrix}^\top                                                                                                                                                                                                \\\\
     & \qquad +
    \begin{bmatrix}
        \Iv \\\\ \Iv \\\\ \zerov \\\\ \zerov
    \end{bmatrix} (\Av\_{12} - \Av\_{11})
    \begin{bmatrix}
        \zerov \\\\ \zerov \\\\ \Iv \\\\ \zerov
    \end{bmatrix}^\top +
    \begin{bmatrix}
        \zerov \\\\ \zerov \\\\ \Iv \\\\ \zerov
    \end{bmatrix} (\Av\_{21} - \Av\_{11})
    \begin{bmatrix}
        \Iv \\\\ -\Iv \\\\ \zerov \\\\ \zerov
    \end{bmatrix}^\top +
    \begin{bmatrix}
        \zerov \\\\ \zerov \\\\ \Iv \\\\ \Iv
    \end{bmatrix} (\Av\_{21} - \Av\_{22})
    \begin{bmatrix}
        \zerov \\\\ \Iv \\\\ \zerov \\\\ \zerov
    \end{bmatrix}^\top
\end{align\*}

## 算法实现

　　根据上面的分解易知计算
\begin{align\*}
    \begin{bmatrix}
        \Sv\_1 \\\\ \Sv\_2 \\\\ \Sv\_3 \\\\ \Sv\_4 \\\\ \Sv\_5 \\\\ \Sv\_6 \\\\ \Sv\_7
    \end{bmatrix} =
    \begin{bmatrix}
        \Iv    & \zerov & \Iv    & \zerov \\\\
        \zerov & \Iv    & \zerov & \Iv    \\\\
        \zerov & \Iv    & \Iv    & \zerov \\\\
        \zerov & \zerov & \Iv    & -\Iv   \\\\
        \zerov & \zerov & \Iv    & \zerov \\\\
        \Iv    & -\Iv   & \zerov & \zerov \\\\
        \zerov & \Iv    & \zerov & \zerov
    \end{bmatrix}
    \begin{bmatrix}
        \Bv\_{11} \\\\ \Bv\_{12} \\\\ \Bv\_{21} \\\\ \Bv\_{22}
    \end{bmatrix} =
    \begin{bmatrix}
        \Bv\_{11} + \Bv\_{21} \\\\ \Bv\_{12} + \Bv\_{22} \\\\ \Bv\_{12} + \Bv\_{21} \\\\ \Bv\_{21} - \Bv\_{22} \\\\ \Bv\_{21} \\\\ \Bv\_{11} - \Bv\_{12} \\\\ \Bv\_{12}
    \end{bmatrix}, \quad
    \begin{bmatrix}
        \Rv\_1 \\\\ \Rv\_2 \\\\ \Rv\_3 \\\\ \Rv\_4 \\\\ \Rv\_5 \\\\ \Rv\_6 \\\\ \Rv\_7
    \end{bmatrix} =
    \begin{bmatrix}
        \Av\_{11} \\\\ \Av\_{22} \\\\ \Av\_{11} - \Av\_{22} \\\\ \Av\_{22} - \Av\_{12} \\\\ \Av\_{12} - \Av\_{11} \\\\ \Av\_{21} - \Av\_{11} \\\\ \Av\_{21} - \Av\_{22}
    \end{bmatrix}
\end{align\*}
共会产生$10$次加减运算，计算$\Tv_1 = \Rv_1 \Sv_1, \ldots, \Tv_7 = \Rv_7 \Sv_7$共会产生$7$个子问题，最后计算
\begin{align\*}
    \begin{bmatrix}
        \Av\_{11} \Bv\_{11} + \Av\_{12} \Bv\_{21} \\\\
        \Av\_{11} \Bv\_{12} + \Av\_{12} \Bv\_{22} \\\\
        \Av\_{21} \Bv\_{11} + \Av\_{22} \Bv\_{21} \\\\
        \Av\_{21} \Bv\_{12} + \Av\_{22} \Bv\_{22}
    \end{bmatrix}
     & =
    \begin{bmatrix}
        \Iv \\\\ \zerov \\\\ \Iv \\\\ \zerov
    \end{bmatrix} \Tv\_1 +
    \begin{bmatrix}
        \zerov \\\\ \Iv \\\\ \zerov \\\\ \Iv
    \end{bmatrix} \Tv\_2 +
    \begin{bmatrix}
        \zerov \\\\ \Iv \\\\ -\Iv \\\\ \zerov
    \end{bmatrix} \Tv\_3 +
    \begin{bmatrix}
        \zerov \\\\ \Iv \\\\ \zerov \\\\ \zerov
    \end{bmatrix} \Tv\_4 +
    \begin{bmatrix}
        \Iv \\\\ \Iv \\\\ \zerov \\\\ \zerov
    \end{bmatrix} \Tv\_5 +
    \begin{bmatrix}
        \zerov \\\\ \zerov \\\\ \Iv \\\\ \zerov
    \end{bmatrix} \Tv\_6 +
    \begin{bmatrix}
        \zerov \\\\ \zerov \\\\ \Iv \\\\ \Iv
    \end{bmatrix} \Tv\_7 \\\\
     & =
    \begin{bmatrix}
        \Tv\_1 + \Tv\_5 \\\\ \Tv\_2 + \Tv\_3 + \Tv\_4 + \Tv\_5 \\\\ \Tv\_1 - \Tv\_3 + \Tv\_6 + \Tv\_7 \\\\ \Tv\_2 + \Tv\_7
    \end{bmatrix}
\end{align\*}
共会产生$8$次加减运算。

　　综上，一共会产生$7$个子问题和$18$次加减运算，此时递推关系变成
\begin{align\*}
    T(n) = 7 \cdot T(n/2) + \frac{18}{4} c\_1 n^2
\end{align\*}
设$n = 2^k$，则
\begin{align\*}
    T(2^k)               & = 7^1 \cdot T(2^{k-1}) + \frac{18}{4} c\_1 4^k         \\\\
    7^1 \cdot T(2^{k-1}) & = 7^2 \cdot T(2^{k-2}) + 7^1 \frac{18}{4} c\_1 4^{k-1} \\\\
    7^2 \cdot T(2^{k-2}) & = 7^3 \cdot T(2^{k-3}) + 7^2 \frac{18}{4} c\_1 4^{k-2} \\\\
                         & \vdots                                                \\\\
    7^{k-1} \cdot T(2^1) & = 7^k \cdot T(2^0) + 7^{k-1} \frac{18}{4} c\_1 4^1
\end{align\*}
注意$7^k = (2^{\lg 7})^k = (2^k)^{\lg 7} = n^{\lg 7} \approx n^{2.81}$，
累加可得
\begin{align\*}
    T(n) = c\_2 n^{\lg 7} + \frac{18}{4} c\_1 4^k \frac{1-(7/4)^k}{1-(7/4)} = c\_2 n^{\lg 7} + 6 c\_1 (n^{\lg 7} - n^2) = \left( c\_2 + 6 c\_1 \right) n^{\lg 7} - 6 c\_1 n^2
\end{align\*}

