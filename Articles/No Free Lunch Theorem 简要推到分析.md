# No Free Lunch Theorem 简要推导分析

​	NFL定理表明，在真实目标函数 $f$ 均匀分布条件下，任何学习算法的 $\mathcal{L}$ 期望性能（这里用总误差反映）永远是相同的. 下面对NFL定理基于二分类问题作详细的推导与分析.

## 符号说明

​	我们用 $x$ 表示某一个特定的样本，用 $\mathcal{X}$ 表示所有样本组成的空间，用 $X$ 表示所有训练集样本组成的空间，用 $\mathcal{H}$ 表示通过某一学习算法 $\mathcal{L_{\alpha}}$ 对训练集进行学习之后得到的所有可能的假设$h$构成的假设空间. 所谓假设 $h$ 就是通过训练得到的一个分类预测模型，并用 $h(x)$ 作为对样本标记 $y$ 的一个预测. 而真实目标函数 $f$则是一个对 $x$ 的正确分类.

## 详细推导过程

​	假设样本空间 $\mathcal{X}$ 和假设空间 $\mathcal{H}$ 均为离散情形，且每一个样本 $x$ 均可被划分到 $y\in\{0,1\}$ 中的一个标记值，也即是说这是一个二分类问题.

​	先考虑某一个特定的假设（模型）$h$ 对所有“训练集外样本”的预测错误情况（即误差）：
$$
E_{rr}=\sum_{x\in\mathcal{X}-X}P(x)·\coprod(h(x)\neq f(x))\tag{1}
$$
 其中 $P(x)$ 为某一个样本 $x$ 在$\mathcal{X}-X$所表示的训练集外的样本空间中出现的概率，而$\coprod(h(x)\neq f(x))$则用来体现预测模型 $h$ 对样本的分类预测是否准确. 由于我们计算的是误差值，因此当预测结果与实际结果不相同时，即 $h(x)\neq f(x)$ 时应赋值为1，反之则赋值为0，这与 $\coprod(·)$ 在 · 为真时取值为1，· 为假时取值为0的性质相吻合. 

​	公式的意义是计算出所有“训练集外”样本再运用 $h$ 模型进行分类时的总错误概率，用最简单的情形来进行说明就是——若测试集中共有 $N$ 个样本，利用 $h$ 模型分类错误的又 $M$ 个，则可记${E_{rr}}'=\frac{M}{N}$.

​	但是一个算法 $\mathcal{L_\alpha}$ 再对训练集样本进行学习时可能得到多个假设模型，为计算出 $\mathcal{L_\alpha}$ 的“总误差”，就应当将所有的假设模型都考虑在内：
$$
E_{ote}(\mathcal{L_\alpha}|X,f)=\sum_h\sum_{x\in\mathcal{X}-X}P(x)\cdot\coprod(h(x)\neq f(x))·P(h|X,\mathcal{L_\alpha})\tag{2}
$$
其中 $P(h|X,\mathcal{L_\alpha})$ 表示利用学习算法 $\mathcal{L_\alpha}$ 对训练集 $X$ 进行训练后得到假设 $h$ 的概率.

​	到目前为止，我们的所有计算都是基于真实目标函数 $f$ 已经被唯一确定时的情形，但实际上 $f$ 可能时任何从样本空间 $\mathcal{X}$ 到分类标记空间 $\{0,1\}$的任何一个函数. 基于 $f$ 均匀分布的假设，我们有以下推导：
$$
\begin{align*}
&\because f\thicksim U(\mathcal{X}\rightarrow\{0,1\})\\
&\therefore\forall x\in\mathcal{X}-X,总存在一半的\space f\space使得\space f(x)=1,而另一半的\space f\space使得\space f(x)=0\\
&\because\forall h对讨论的样本x进行分类预测时,h(x)只能为1或0中的一个\\
&\therefore\forall x,\forall h,总存在一般的\space f\space对该讨论的样本x的分类与\space h\space预测的不同，即存在误差
\end{align*}
$$
​	下面的问题就是分析出 $f$ 总共有多少种可能.

​	每一个 $f$ 都作用于样本空间 $\mathcal{X}$ 中的所有样本 $x$（共计 $|\mathcal{X}|$ 个），而对于每一个 $x$，都有两种被 $f$ 分类的可能结果，即有所有 $f$ 产生的分类结果为：
$$
f_1:(0,0,...,0)\\f_2:(1,0,...,0)\\...\\f_n:(1,1,...,1)
$$
其中每一个“分类结果向量”均为 $|\mathcal{X}|$ 维向量. 由此易得 $f$ 的个数 $n=2^{|\mathcal{X}|}$.

​	再由前面的讨论可知任何一个假设 $h$ 在对某一个确定的样本 $x$ 进行分类时，总有 $2^{|\mathcal{X}|-1}$ 个 $f$ 对该样本的分类结果不同，即：
$$
\sum_f\coprod(h(x)\neq f(x))=2^{|\mathcal{X}|-1}\tag{3}
$$
​	最后我们再对所有的“真实目标函数” $f$ 求使用学习算法 $\mathcal{L_\alpha}$时的“总误差”：
$$
\begin{align*}
\sum_fE_{ote}(\mathcal{L_\alpha}|X,f)&=\sum_f\sum_h\sum_{x\in\mathcal{X}-X}P(x)\cdot\coprod(h(x)\neq f(x))·P(h|X,\mathcal{L_\alpha})\\
&=\sum_{x\in\mathcal{X}-X}P(x)\cdot\sum_h\cdot P(h|X,\mathcal{L_\alpha})\cdot \sum_f\coprod(h(x)\neq f(x))=2^{|\mathcal{X}|-1}\tag{4}
\end{align*}
$$
​	将（3）式代入（4）式得：
$$
\begin{align*}
sum_fE_{ote}(\mathcal{L_\alpha}|X,f)&=\sum_{x\in\mathcal{X}-X}P(x)\cdot\sum_h\cdot P(h|X,\mathcal{L_\alpha})\cdot2^{|\mathcal{X}|-1}\\
&=\sum_{x\in\mathcal{X}-X}P(x)\cdot1\cdot2^{|\mathcal{X}|-1}\tag{5}
\end{align*}
$$
​	由（5）式可知任何学习算法 $\mathcal{L_\alpha}$ 得性能（总误差）与该算法的 “聪明”程度无关.

## 简要说明

​	NFL定理说明了两个学习算法在进行比较时，不可能存在一种算法在解决所有的问题时都优于另一种算法，也即是说明评价学习算法的优劣必须结合具体的问题进行分析，如果考虑所有潜在的问题，那么一切学习算法的总体表现都是一致的.

​	那么这与我们现实中的分析有什么异同呢？

​	首先是相同之处，比如我要解决一道微积分（上）的考试题，我可以考虑使用洛必达法则或者利用泰勒公式展开. 一定会存在某些问题上使用洛必达法则求解优于利用泰勒公式展开，但是在另一些问题的考虑中，我们会倾向于使用泰勒公式.

​	尽管如此，我们仍然需要讨论不同的算法对某一个或某一类问题解决的优劣程度. 这是因为在现实中，定理成立的条件——真实目标函数 $f$ 均匀分布，并不是总能被满足甚至可以说不能被满足. 例如，尽管我们在解决“所有潜在”的可用洛必达法则和泰勒公式解决的题目时两种方法的总体性能相当，但是现实中并不是”所有潜在”的问题都“存在”（或者说会被我们遇到），因此总会有一个方法在解决我们遇到的这一类问题上总体情况上由于另一个方法.

