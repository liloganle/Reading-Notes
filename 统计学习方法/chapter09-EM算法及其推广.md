# 第九章　EM算法及其推广

EM算法是一种迭代算法，被用来估计含有隐变量的概率模型的参数的极大似然估计，或者最大后验概率估计。该算法每次迭代都由两步组成：E步求期望；M步极大化，故这一算法被称为期望极大算法。

## 9.1 EM算法引入

我们用$Y$表示可观测变量的数据，$Z$表示隐变量的数据。而$Y$和$Z$合在一起统称为完全数据，可观测数据$Y$又称为不完全数据。假设给定可观测数据$Y$，及其概率分布是$P(Y|\theta)$，其中$\theta$是模型需要估计的参数，那么它的对数似然函数为：
$$
L(\theta)=\log P(Y|\theta).
$$
假设$Y$和$Z$的联合概率分布是$P(Y,Z|\theta)$，那么完全数据的对数似然函数是：
$$
\log P(Y,Z|\theta).
$$
EM算法是迭代求解$L(\theta)=\log P(Y|\theta)$的极大似然估计。

（**Q函数**）完全数据的对数似然函数$L(\theta)=\log P(Y|\theta)$关于不可观测数据$Z$的条件概率分布的期望称为Q函数，即
$$
Q(\theta,\theta^{i})=\mathbb{E}_{Z}[\log P(Y,Z|\theta)|Y,\theta^{i}]=\sum_{Z}P(Z|Y,\theta^{i})\log P(Y,Z|\theta).
$$
Q函数是EM算法的核心。值得注意的是EM算法对初始值比较敏感，选择不同的初始值可能得到不同的参数估计值。

EM算法：

- 输入：可观测变量数据$Y$，隐变量数据$Z$，联合概率分布$P(Y,Z|\theta)$，条件概率分布$P(Z|Y,\theta)$;

1. 初始化参数$\theta^{0}$；

2. E步：记$\theta^{i}$为估计参数第$i$次迭代的估计值，那么在第$i+1$次迭代，依据
   $$
   Q(\theta,\theta^{i})=\sum_{Z}P(Z|Y,\theta^{i})\log P(Y,Z|\theta).
   $$
   计算期望；

3. M步：求解极大化$Q(\theta,\theta^{i})$的参数$\theta​$，即
   $$
   \theta^{i+1}=\arg\max_{\theta} Q(\theta,\theta^{i})
   $$

4. 重复第2和3步直至参数收敛到稳定值$\theta^{*}$。

- 输出：模型参数估计值$\theta^{*}$。

## 9.2 EM算法在高斯混合模型中的应用

EM算法的一个重要的应用是高斯混合模型的参数估计。

（**高斯混合模型**）具有如下形式：
$$
P(y|\theta)=\sum_{k=1}^{K}\alpha_{k}\phi(y|\theta_{k})
$$
的概率分布模型都称为高斯混合模型。其中，$\alpha_{k} \geq 0, \sum_{k=1}^{K}\alpha_{k}=1$，$\alpha_{k}$对应第$k$个高斯分布的系数；$\phi(y|\theta_{k})$是对应于第$k$个高斯分布概率密度函数，$\theta_{k}=(\mu_k, \sigma_k^{2})$，即
$$
\phi(y|\theta_{k})=\frac{1}{\sqrt{2\pi}\sigma_k}\exp(-\frac{(y-\mu_k)^2}{2\sigma_k^2}).
$$
高斯混合模型参数估计的EM算法：

- 输入：可观测数据$y_1, y_2, \cdots, y_N$，高斯混合模型；

1. 初始化模型参数$\theta$；

2. E步：依据当前模型的参数，计算第$k$个模型对观测数据$y_{j}$的响应度
   $$
   \hat{\gamma}_{jk}=\frac{\alpha_k\phi(y_j|\theta_k)}{\sum_{k=1}^{K}\alpha_k \phi(y_j|\theta_k)},\quad j=1,2,\cdots,N;\quad k=1,2,\cdots,K
   $$

3. M步：迭代计算新一轮的模型参数
   $$
   \hat{\mu}_k=\frac{\sum_{j=1}^{N}\hat{\gamma}_{jk}y_j}{\sum_{j=1}^{N}\hat{\gamma}_{jk}} \\
   \hat{\sigma}_k^2 = \frac{\sum_{j=1}^{N}\hat{\gamma}_{jk}(y_j-\mu_k)^2}{\sum_{j=1}^{N}\hat{\gamma}_{jk}} \\
   \hat{\alpha}_k = \frac{\sum_{j=1}^{N}\hat{\gamma}_{jk}}{N}
   $$

4. 重复第2和3步直至参数收敛到稳定值。

- 输出：高斯混合模型参数的估计值。

