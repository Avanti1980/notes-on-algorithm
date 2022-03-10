**主定理**：令$a \ge 1$和$b > 1$是常数，$f(n)$是一个函数，$T(n)$是定义在**非负整数**上的递归式：
\begin{align\*}
    T(n) = a \cdot \left( \frac{n}{b} \right) + f(n)
\end{align\*}
其中我们将$n/b$解释为$\lfloor n/b \rfloor$或$\lceil n/b \rceil$，那么$T(n)$有如下渐进界：
1. 若对某个常数$\varepsilon > 0$有$f(n) = O(n^{\log_b a - \varepsilon})$，则$T(n) = \Theta (n^{\log_b a})$。
2. 若$f(n) = \Theta (n^{\log_b a})$，则$T(n) = \Theta (n^{\log_b a} \lg n)$。
3. 若对某个常数$\varepsilon > 0$有$f(n) = \Omega(n^{\log_b a + \varepsilon})$，且对某个常数$c<1$和所有足够大的$n$有$a f(n/b) \le c f(n)$，则$T(n) = \Theta (f(n))$。

**证明**：若$n$是$b$的整数幂次，不妨设$n = b^k$，此时$n/b$最后正好会递归成$1$，即
\begin{align\*}
    T \left( \frac{n}{b^0} \right)     & = a \cdot T \left( \frac{n}{b^1} \right) + f \left( \frac{n}{b^0} \right)     \\\\
    T \left( \frac{n}{b^1} \right)     & = a \cdot T \left( \frac{n}{b^2} \right) + f \left( \frac{n}{b^1} \right)     \\\\
                                       & \vdots                                                                        \\\\
    T \left( \frac{n}{b^{k-1}} \right) & = a \cdot T \left( \frac{n}{b^k} \right) + f \left( \frac{n}{b^{k-1}} \right)
\end{align\*}
注意上面共有$k$个递推式，又$a^k = (b^{\log_b a})^k = (b^k)^{\log_b a} = n^{\log_b a}$、$n = b^k$，于是
\begin{align\*}
    T(n) = a^k \cdot T \left( \frac{n}{b^k} \right) + f \left( \frac{n}{b^0} \right) + a f \left( \frac{n}{b^1} \right) + \cdots + a^{k-1} f \left( \frac{n}{b^{k-1}} \right) = n^{\log\_b a} \cdot T(1) + \sum\_{i=0}^{k-1} a^i f \left( \frac{n}{b^i} \right)
\end{align\*}
显然前一项$n^{\log_b a} \cdot T(1) = \Theta(n^{\log_b a})$，故关键就是后面求和这一项，下面分情况讨论：
1. 由$f(n) = O(n^{\log_b a - \varepsilon})$可知
\begin{align\*}
    a^i f \left( \frac{n}{b^i} \right) = a^i O \left( \left( \frac{n}{b^i} \right)^{\log\_b a - \varepsilon} \right) = a^i O \left( \frac{n^{\log\_b a - \varepsilon}}{a^i b^{-\varepsilon i}} \right) = O(n^{\log\_b a - \varepsilon}) b^{\varepsilon i}
\end{align\*}
故
\begin{align\*}
    g(n) = \sum\_{i=0}^{k-1} f \left( \frac{n}{b^i} \right) = O(n^{\log\_b a - \varepsilon}) \sum\_{i=0}^{k-1} b^{\varepsilon i} = O(n^{\log\_b a - \varepsilon}) \frac{1 - b^{k \varepsilon}}{1 - b^\varepsilon} = O(n^{\log\_b a - \varepsilon}) \frac{n^\varepsilon - 1}{b^\varepsilon - 1} = O(n^{\log\_b a})
\end{align\*}
从而$T(n) = \Theta(n^{\log_b a}) + O(n^{\log_b a}) = \Theta(n^{\log_b a})$，即此时$T(n)$的复杂度由前一项决定。

2. 由$f(n) = \Theta (n^{\log_b a})$可知
\begin{align\*}
    a^i f \left( \frac{n}{b^i} \right) = a^i \Theta \left( \left( \frac{n}{b^i} \right)^{\log\_b a} \right) = a^i \Theta \left( \frac{n^{\log\_b a}}{a^i} \right) = \Theta(n^{\log\_b a})
\end{align\*}
于是
\begin{align\*}
    g(n) = \sum\_{i=0}^{k-1} a^i f \left( \frac{n}{b^i} \right) = \sum\_{i=0}^{k-1} \Theta(n^{\log\_b a}) = \Theta(n^{\log\_b a}) \log\_b n = \Theta(n^{\log\_b a} \lg n)
\end{align\*}
从而$T(n) = \Theta(n^{\log_b a}) + \Theta(n^{\log_b a} \lg n) = \Theta(n^{\log_b a} \lg n)$，即此时$T(n)$的复杂度由求和这一项决定。

3. 由条件知存在$j$使得当$n' \ge b^{k-j+1} = n / b^{j-1}$时递推式$a f(n'/b) \le c f(n')$恒成立，于是
\begin{align\*}
    a^j f \left( \frac{n}{b^j} \right) \le a^{j-1} c f \left( \frac{n}{b^{j-1}} \right) \le \cdots \le a^2 c^{j-2} f \left( \frac{n}{b^2} \right) \le a c^{j-1} f \left( \frac{n}{b} \right) \le c^j f(n)
\end{align\*}
从而对$\forall i \le j$有$a^i f(n/b^i) \le c^{i} f(n)$，代入可得
\begin{align\*}
    g(n) & = \sum\_{i=0}^{k-1} a^i f \left( \frac{n}{b^i} \right) = \sum\_{i=0}^j a^i f \left( \frac{n}{b^i} \right) + \overbrace{a^{j+1} f \left(\frac{n}{b^{j+1}} \right) + \cdots + a^{k-1} f \left(\frac{n}{b^{k-1}} \right)}^{\text{不满足递推式的部分，复杂度为}O(1)} \\\\
    & \le \sum\_{i=0}^j c^i f(n) + O(1) < f(n) \sum\_{i=0}^\infty c^i + O(1) = \frac{f(n)}{1-c} + O(1) = O(f(n))
\end{align\*}
又$f(n)$是$g(n)$中的一项且$g(n)$所有求和项非负，故$g(n) \ge f(n)$，从而
\begin{align\*}
    g(n) = \Theta (f(n)) = \Omega(n^{\log\_b a + \varepsilon}) > \Theta(n^{\log\_b a})
\end{align\*}
于是$T(n) = \Theta(n^{\log_b a}) + g(n) = \Theta (f(n))$，即此时$T(n)$的复杂度由求和这一项决定。

----------

　　若$n$不是$b$的整数幂次，则递推中某一轮$n/b$不一定为整数，又$T(n)$是定义在非负整数上的，因此还需多做一步取整，此时递推式可写成
\begin{align\*}
    T(n\_0) & = a \cdot T(n\_1) + f(n\_0), \quad n\_1 = [n\_0/b] \\\\
    T(n\_1) & = a \cdot T(n\_2) + f(n\_1), \quad n\_2 = [n\_1/b] \\\\
    T(n\_2) & = a \cdot T(n\_3) + f(n\_2), \quad n\_3 = [n\_2/b] \\\\
    & \vdots
\end{align\*}
不妨先考虑向上取整，即
\begin{align\*}
    n\_i = \begin{cases}
              n & i = 0 \\\\ \lceil n\_{i-1} / b \rceil & i > 0
          \end{cases}
\end{align\*}
根据$x \le \lceil x \rceil \le x + 1$易知
\begin{align\*}
    \frac{n}{b}   \le n\_1 & \le \frac{n}{b} + 1                                 \\\\
    \frac{n}{b^2} \le n\_2 & \le \frac{n}{b^2} + \frac{1}{b} + 1                 \\\\
    \frac{n}{b^3} \le n\_3 & \le \frac{n}{b^3} + \frac{1}{b^2} + \frac{1}{b} + 1 \\\\
    \vdots                &
\end{align\*}
于是
\begin{align} \label{eq: ni-upper-bound}
    \frac{n}{b^i} \le n\_i \le \frac{n}{b^i} + \frac{1}{b^{i-1}} + \cdots + \frac{1}{b} + 1 < \frac{n}{b^i} + \frac{1}{1 - 1/b} = \frac{n}{b^i} + \frac{b}{b-1}
\end{align}
令$k = \lfloor \log_b n \rfloor \ge \log_b n - 1$，即$n \le b^{k+1}$，代入式(\ref{eq: ni-upper-bound})可得
\begin{align\*}
    n\_k < \frac{n}{b^k} + \frac{b}{b-1} \le \frac{b^{k+1}}{b^k} + \frac{b}{b-1} = b + \frac{b}{b-1} = O(1)
\end{align\*}
故递推到第$k$轮，子问题$T(n_k)$已经是$O(1)$复杂度了，同前面一样有
\begin{align\*}
    T(n) = a^k T(n\_k) + f(n\_0) + a f(n\_1) + \cdots + a^{k-1} f(n\_{k-1}) = \Theta(n^{\log\_b a}) + \sum\_{i=0}^{k-1} a^i f(n\_i)
\end{align\*}
关键依然是后面求和这一项，继续分情况讨论：
1. 由$f(n) = O(n^{\log_b a - \varepsilon})$可知存在正常数$c_i$和$n_i$使得对$\forall n \ge n_i$有
\begin{align\*}
    a^i f(n\_i) & \le a^i c\_i n\_i^{\log\_b a - \varepsilon} < a^i c\_i \left( \frac{n}{b^i} + \frac{b}{b-1} \right)^{\log\_b a - \varepsilon} = a^i c\_i \left( \frac{n}{b^i} \right)^{\log\_b a - \varepsilon} \left( 1 + \frac{b^i}{n} \frac{b}{b-1} \right)^{\log\_b a - \varepsilon} \\\\
    & \le a^i c\_i n^{\log\_b a - \varepsilon} \frac{b^{\varepsilon i}}{a^i} \left( 1 + \frac{b}{b-1} \right)^{\log\_b a - \varepsilon} = c\_i n^{\log\_b a - \varepsilon} b^{\varepsilon i} \left( \frac{2b-1}{b-1} \right)^{\log\_b a - \varepsilon}
\end{align\*}
令$\cbar = \max_i \{c_i\}$、$\nbar = \max_i \{n_i\}$，于是对$\forall n \ge \nbar$有
\begin{align\*}
    g(n) & = \sum\_{i=0}^{k-1} a^i f(n\_i) < \sum\_{i=0}^{k-1} c\_i n^{\log\_b a - \varepsilon} b^{\varepsilon i} \left( \frac{2b-1}{b-1} \right)^{\log\_b a - \varepsilon} \le \cbar n^{\log\_b a - \varepsilon} \left( \frac{2b-1}{b-1} \right)^{\log\_b a - \varepsilon} \sum\_{i=0}^{k-1} b^{\varepsilon i} \\\\
    & \le \cbar n^{\log\_b a - \varepsilon} \left( \frac{2b-1}{b-1} \right)^{\log\_b a - \varepsilon} \frac{n^\varepsilon - 1}{b^\varepsilon - 1} < \frac{\cbar}{b^\varepsilon - 1} \left( \frac{2b-1}{b-1} \right)^{\log\_b a - \varepsilon} n^{\log\_b a} = O(n^{\log\_b a})
\end{align\*}
从而$T(n) = \Theta(n^{\log_b a}) + O(n^{\log_b a}) = \Theta(n^{\log_b a})$，即此时$T(n)$的复杂度由前一项决定。

2. 由$f(n) = \Theta (n^{\log_b a})$可知存在正常数$c_i$、$d_i$和$n_i$使得对$\forall n \ge n_i$有
\begin{align\*}
    a^i f(n\_i) \le a^i c\_i n\_i^{\log\_b a} < a^i c\_i \left( \frac{n}{b^i} \right)^{\log\_b a} \left( 1 + \frac{b^i}{n} \frac{b}{b-1} \right)^{\log\_b a} \le c\_i \left( 1 + \frac{b}{b-1} \right)^{\log\_b a} n^{\log\_b a} = O(n^{\log\_b a})
\end{align\*}
以及
\begin{align\*}
    a^i f(n\_i) \ge a^i d\_i n\_i^{\log\_b a} \ge a^i d\_i \left( \frac{n}{b^i} \right)^{\log\_b a} = d\_i n^{\log\_b a} = \Omega (n^{\log\_b a})
\end{align\*}
故$a^i f(n_i) = \Theta(n^{\log_b a})$，于是
\begin{align\*}
    g(n) = \sum\_{i=0}^{k-1} a^i f(n\_i) = \Theta(n^{\log\_b a}) k = \Theta(n^{\log\_b a} \lg n)
\end{align\*}
从而$T(n) = \Theta(n^{\log_b a}) + \Theta(n^{\log_b a} \lg n) = \Theta(n^{\log_b a} \lg n)$，即此时$T(n)$的复杂度由求和这一项决定。

3. 由条件知存在$j$使得当$n \ge n_{j-1}$时递推式$a f(\lceil n/b \rceil) \le c f(n)$恒成立，于是
\begin{align\*}
    a^j f(n\_j) \le a^{j-1} c f(n\_{j-1}) \le \cdots \le a^2 c^{j-2} f(n\_2) \le a c^{j-1} f(n\_1) \le c^j f(n\_0)
\end{align\*}
从而对$\forall i \le j$有$a^i f(n_i) \le c^{i} f(n)$，代入可得
\begin{align\*}
    g(n) & = \sum\_{i=0}^{k-1} a^i f(n\_i) = \sum\_{i=0}^j a^i f(n\_i) + \overbrace{a^{j+1} f(n\_{j+1}) + \cdots + a^{k-1} f(n\_{k-1})}^{\text{不满足递推式的部分，复杂度为}O(1)} \\\\
    & \le \sum\_{i=0}^j c^i f(n) + O(1) < f(n) \sum\_{i=0}^\infty c^i + O(1) = \frac{f(n)}{1-c} + O(1) = O(f(n))
\end{align\*}
又$f(n)$是$g(n)$中的一项且$g(n)$所有求和项非负，故$g(n) \ge f(n)$，从而
\begin{align\*}
    g(n) = \Theta (f(n)) = \Omega(n^{\log\_b a + \varepsilon}) > \Theta(n^{\log\_b a})
\end{align\*}
于是$T(n) = \Theta(n^{\log_b a}) + g(n) = \Theta (f(n))$，即此时$T(n)$的复杂度由求和这一项决定。

----------

　　最后考虑向下取整，即
\begin{align\*}
    n\_i = \begin{cases}
        n & i = 0 \\\\ \lfloor n\_{i-1}/b \rfloor & i > 0
    \end{cases}
\end{align\*}
根据$x-1 \le \lfloor x \rfloor \le x$易知
\begin{align\*}
    \frac{n}{b} - 1   \le n\_1                               & \le \frac{n}{b}   \\\\
    \frac{n}{b^2} - \frac{1}{b} - 1    \le n\_2              & \le \frac{n}{b^2} \\\\
    \frac{n}{b^3} - \frac{1}{b^2} - \frac{1}{b} - 1 \le n\_3 & \le \frac{n}{b^3} \\\\
    \vdots                                                  &
\end{align\*}
于是
\begin{align} \label{eq: ni-lower-bound}
    \frac{n}{b^i} - \frac{b}{b-1} = \frac{n}{b^i} - \frac{1}{1 - 1/b} < \frac{n}{b^i} - \frac{1}{b^{i-1}} - \cdots - \frac{1}{b} - 1 \le n\_i \le \frac{n}{b^i}
\end{align}
令$k = \lfloor \log_b n(b-1) \rfloor - 2 \ge \log_b n(b-1) - 3$，即$n(b-1) \le b^{k+3}$，代入式(\ref{eq: ni-lower-bound})可得
\begin{align\*}
    n\_k \le \frac{n}{b^k} \le \frac{b^{k+3}}{b^k} = \frac{b^3}{b-1} = O(1)
\end{align\*}
故递推到第$k$轮，子问题$T(n_k)$已经是$O(1)$复杂度了，同前面一样有
\begin{align\*}
    T(n) = a^k T(n\_k) + f(n\_0) + a f(n\_1) + \cdots + a^{k-1} f(n\_{k-1}) = \Theta(n^{\log\_b a}) + \sum\_{i=0}^{k-1} a^i f(n\_i)
\end{align\*}
关键依然是后面求和这一项，继续分情况讨论：
1. 由$f(n) = O(n^{\log_b a - \varepsilon})$可知存在正常数$c_i$和$n_i$使得对$\forall n \ge n_i$有
\begin{align\*}
    a^i f(n\_i) \le a^i c\_i n\_i^{\log\_b a - \varepsilon} < a^i c\_i \left( \frac{n}{b^i} \right)^{\log\_b a - \varepsilon} = c\_i n^{\log\_b a - \varepsilon} b^{\varepsilon i}
\end{align\*}
令$\cbar = \max_i \{c_i\}$、$\nbar = \max_i \{n_i\}$，于是对$\forall n \ge \nbar$有
\begin{align\*}
    g(n) = \sum\_{i=0}^{k-1} a^i f(n\_i) < \sum\_{i=0}^{k-1} c\_i n^{\log\_b a - \varepsilon} b^{\varepsilon i} \le \cbar n^{\log\_b a - \varepsilon} \sum\_{i=0}^{k-1} b^{\varepsilon i} \le \cbar n^{\log\_b a - \varepsilon} \frac{n^\varepsilon - 1}{b^\varepsilon - 1} = O(n^{\log\_b a})
\end{align\*}
从而$T(n) = \Theta(n^{\log_b a}) + O(n^{\log_b a}) = \Theta(n^{\log_b a})$，即此时$T(n)$的复杂度由前一项决定。

2. 由$f(n) = \Theta (n^{\log_b a})$可知存在正常数$c_i$、$d_i$和$n_i$使得对$\forall n \ge n_i$有
\begin{align\*}
    a^i f(n\_i) \le a^i c\_i n\_i^{\log\_b a} \le a^i c\_i \left( \frac{n}{b^i} \right)^{\log\_b a} \le c\_i n^{\log\_b a} = O(n^{\log\_b a})
\end{align\*}
以及
\begin{align\*}
    a^i f(n\_i) & \ge a^i d\_i n\_i^{\log\_b a} \ge a^i d\_i \left( \frac{n}{b^i} - \frac{b}{b-1} \right)^{\log\_b a} = a^i d\_i \left( \frac{n}{b^i} \right)^{\log\_b a} \left( 1 - \frac{b^i}{n} \frac{b}{b-1} \right)^{\log\_b a} \\\\
    & = d\_i n^{\log\_b a} \left( 1 - \frac{b^i}{n} \frac{b}{b-1} \right)^{\log\_b a}
\end{align\*}
注意$k = \lfloor \log_b n(b-1) \rfloor - 2 \le \log_b n(b-1) - 2$，即$n(b-1) \ge b^{k+2}$，于是
\begin{align\*}
    1 > 1 - \frac{b^i}{n} \frac{b}{b-1} \ge 1 - \frac{b^{i+1}}{b^{k+2}} > 0 \Longrightarrow 1 - \frac{b^i}{n} \frac{b}{b-1} \in (0,1) \Longrightarrow a^i f(n\_i) = \Omega (n^{\log\_b a})
\end{align\*}
故$a^i f(n_i) = \Theta(n^{\log_b a})$，于是
\begin{align\*}
    g(n) = \sum\_{i=0}^{k-1} a^i f(n\_i) = \Theta(n^{\log\_b a}) k = \Theta(n^{\log\_b a} \lg n)
\end{align\*}
从而$T(n) = \Theta(n^{\log_b a}) + \Theta(n^{\log_b a} \lg n) = \Theta(n^{\log_b a} \lg n)$，即此时$T(n)$的复杂度由求和这一项决定。

3. 由条件知存在$j$使得当$n \ge n_{j-1}$时递推式$a f(\lfloor n/b \rfloor) \le c f(n)$恒成立，于是
\begin{align\*}
    a^j f(n\_j) \le a^{j-1} c f(n\_{j-1}) \le \cdots \le a^2 c^{j-2} f(n\_2) \le a c^{j-1} f(n\_1) \le c^j f(n\_0)
\end{align\*}
从而对$\forall i \le j$有$a^i f(n_i) \le c^{i} f(n)$，代入可得
\begin{align\*}
    g(n) & = \sum\_{i=0}^{k-1} a^i f(n\_i) = \sum\_{i=0}^j a^i f(n\_i) + \overbrace{a^{j+1} f(n\_{j+1}) + \cdots + a^{k-1} f(n\_{k-1})}^{\text{不满足递推式的部分，复杂度为}O(1)} \\\\
    & \le \sum\_{i=0}^j c^i f(n) + O(1) < f(n) \sum\_{i=0}^\infty c^i + O(1) = \frac{f(n)}{1-c} + O(1) = O(f(n))
\end{align\*}
又$f(n)$是$g(n)$中的一项且$g(n)$所有求和项非负，故$g(n) \ge f(n)$，从而
\begin{align\*}
    g(n) = \Theta (f(n)) = \Omega (n^{\log\_b a + \varepsilon}) > \Theta(n^{\log\_b a})
\end{align\*}
于是$T(n) = \Theta(n^{\log_b a}) + g(n) = \Theta (f(n))$，即此时$T(n)$的复杂度由求和这一项决定。




