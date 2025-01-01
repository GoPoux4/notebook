# 机器学习复习

## 基本术语和模型评估

### 术语

- 特征：输入数据的属性
- 属性值：特征的离散取值
- 样本维度：特征的数量
- 属性空间/特征空间/输入空间：特征张成的空间
- 标记空间/输出空间：标记张成的空间
- 示例（instance）/样本（sample）：一个对象的输入，不含标记
- 样例（example）：一个对象的输入和输出
- 训练集（training set）：训练样例的集合
- 测试集（test set）：测试样例的集合
- 任务：
    1. 根据标记的取值划分：
        - 分类（classification）：标记空间是有限个离散值的情况
        - 回归（regression）：标记空间是连续值的情况
        - 聚类（clustering）：无标记，将样本划分为若干组
    2. 根据标记的完整性划分：
        - 监督学习（supervised learning）：训练集有标记
        - 无监督学习（unsupervised learning）：训练集无标记
        - 半监督学习（semi-supervised learning）：训练集有部分标记
        - 噪声标记（noise label）：训练集中的标记可能有误
- 目标：泛化能力

### 概念学习

最理想的机器学习技术是学习到概念。

- 假设空间：设第 \(i\) 个属性的取值有 \(n_i\) 种，则假设空间大小为 \(1 + \prod_{i=1}^{d} (n_i + 1)\)。获得与训练集一致（即对所有训练样本 能够进行正确判断）的假设
- 版本空间：与训练集一致的假设集合。
- 归纳偏好：面临新样本时，不同假设可能会产生不同的输出。算法对某些假设的偏好称为归纳偏好。
- No Free Lunch Theorem：没有一个算法能够在所有问题上表现最好。

### 模型评估与选择

- 错误率：分类错误的样本数占总样本数的比例
- 误差：样本真实输出与模型输出之间的差异
- 过拟合：将训练样本的特点当作所有样本的一般性质，泛化性能差。
    
    解决方法：优化目标函数加入正则项，early stop

- 欠拟合：模型过于简单，对一般性质的学习不足。
    
    解决方法：扩展分支（决策树），增加层数、训练轮数（神经网络）

评估方法：

- 留出法（hold-out）：将数据集划分为训练集和测试集。尽可能保证训练集和测试集的分布一致（分层采样），重复多次取平均值。
- 交叉验证法（cross validation）：将数据集分层采样划分为 \(k\) 个子集，每次用 \(k-1\) 个子集训练，剩下的子集测试，重复 \(k\) 次取平均值。

    \(k\) 最常用的取值是 10，称为 10 折交叉验证。随机使用不同的划分重复 \(p\) 次取平均值，称为 \(p\) 次 \(k\) 折交叉验证。

- 留一法（LOO）：令交叉验证法中 \(k\) 等于样本数，则为留一法。

    优点：不受随机样本划分方式影响，结果往往比较准确。

    缺点：计算开销大。

- 自助法（bootstrap）：对于大小为 \(N\) 的数据集，每次从中随机采样一个样本，放回，重复 \(N\) 次，得到大小为 \(N\) 的训练集。剩下的样本作为测试集。一般用于数据集较小的情况。
- 参数调优

### 性能度量

衡量模型泛化能力的指标。

- 均方误差（mean squared error）：

    \[
    E(f;D) = \frac{1}{N} \sum_{i=1}^{N} (f(x_i) - y_i)^2
    \]

    对于数据分布 \(\mathcal{D}\) 和概率密度函数 \(p(\cdot)\)，均方误差为：

    \[
    E(f;\mathcal{D}) = \int_{x \sim \mathcal{D}} (f(x) - y)^2 p(x) \ \mathrm{d}x
    \]

- 错误率（error rate）：

    分类错误率：

    \[
    E(f;D) = \frac{1}{m} \sum_{i=1}^{m} \mathbb{I}(f(x_i) \neq y_i)
    \]

- 精度（accuracy）：

    \[
    \mathrm{acc}(f;D) = \frac{1}{m} \sum_{i=1}^{m} \mathbb{I}(f(x_i) = y_i) = 1 - E(f;D)
    \]

#### 混淆矩阵

列为预测结果，行为真实结果：

|  | 预测正例 | 预测反例 |
|:-:|:-:|:-:|
| 真实正例 | TP | FN |
| 真实反例 | FP | TN |

- 查准率（precision）：

    \[
    P = \frac{TP}{TP + FP}
    \]

- 查全率（recall）：

    \[
    R = \frac{TP}{TP + FN}
    \]

- P-R 曲线：横轴为查全率（R），纵轴为查准率（P）。若一个学习器的 P-R 曲线被另一个学习器的曲线完全"包住"，则可断言后者的性能优于前者。
- 平衡点（break-even point BEP）：P-R 曲线上查准率等于查全率的点。可以基于平衡点来比较不同学习器的性能。
- F1 度量：

    \[
    F1 = \frac{2 \times P \times R}{P + R}
    \]

    实际上是调和平均数：

    \[
    \frac{1}{F1} = \frac{1}{2} \left( \frac{1}{P} + \frac{1}{R} \right)
    \]

- \(F_\beta\) 度量：

    \[
    F_\beta = \frac{(1 + \beta^2) \times P \times R}{\beta^2 \times P + R}
    \]

    当 \(\beta = 1\) 时，\(F_\beta\) 退化为 \(F1\)。实际上是 \(P\) 和 \(R\) 的加权调和平均数：

    \[
    \frac{1}{F_\beta} = \frac{1}{1 + \beta^2} \left( \frac{1}{P} + \frac{\beta^2}{R} \right)
    \]

多个混淆矩阵：

先分别计算查准率和查全率：

- 宏查准率（macro-precision）：

    \[
    \text{macro-}P = \frac{1}{n} \sum_{i=1}^{n} P_i
    \]

- 宏查全率（macro-recall）：

    \[
    \text{macro-}R = \frac{1}{n} \sum_{i=1}^{n} R_i
    \]

- 宏 \(F1\) 度量：

    \[
    \text{macro-}F1 = \frac{2 \times \text{macro-}P \times \text{macro-}R}{\text{macro-}P + \text{macro-}R}
    \]

计算混淆矩阵的平均值，再计算查准率和查全率：

- 微查准率（micro-precision）：

    \[
    \text{micro-}P = \frac{\overline{TP}}{\overline{TP} + \overline{FP}}
    \]

- 微查全率（micro-recall）：

    \[
    \text{micro-}R = \frac{\overline{TP}}{\overline{TP} + \overline{FN}}
    \]

- 微 \(F1\) 度量：

    \[
    \text{micro-}F1 = \frac{2 \times \text{micro-}P \times \text{micro-}R}{\text{micro-}P + \text{micro-}R}
    \]

### ROC 与 AUC

根据学习器输出的实数值对样例进行排序，按此顺序逐个把样本作为正例进行预测，每次计算出假正例率（FPR）和真正例率（TPR）：

- 真正例率（TPR）：

    \[
    TPR = \frac{TP}{TP + FN}
    \]

- 假正例率（FPR）：

    \[
    FPR = \frac{FP}{FP + TN}
    \]

以 FPR 为横轴，TPR 为纵轴作图，得到 ROC 曲线。

若一个学习器的 ROC 曲线被另一个学习器的曲线完全"包住"，则可断言后者的性能优于前者。若两个学习器的 ROC 曲线发生交叉，则可以通过比较 ROC 曲线下的面积（AUC）来判断性能：

\[
    \text{AUC} = \frac{1}{2} \sum_{i=1}^{m-1} (x_{i+1} - x_i) (y_i + y_{i+1})
\]

AUC 考虑的是样本预测的排序质量。

### 代价敏感错误率

权衡不同类型错误所造成的不同损失，可为错误赋予“非均等代价”（unequal cost）。

真实类别为 \(i\)，预测类别为 \(j\) 的代价为 \(cost_{ij}\)，则代价敏感错误率为：

\[
E(f;D) = \frac{1}{m} \sum_{i=1}^{m} (\sum_{x_i \in D^+} \mathbb{I}(f(x_i) \neq y_i) \times cost_{01} + \sum_{x_i \in D^-} \mathbb{I}(f(x_i) \neq y_i) \times cost_{10})
\]

### 二项检验

设泛化错误率为 \(\epsilon\)，测试错误率为 \(\hat{\epsilon}\)，使用二项检验检测 \(\epsilon \leq \epsilon_0\) 的显著性。

若测试错误率小于

\[
\bar\epsilon = \max \epsilon \ \text{s.t.} \ \sum_{i=\epsilon_0 m + 1}^m \binom{m}{i} \epsilon^i (1 - \epsilon)^{m-i} < \alpha
\]

则在 \(\alpha\) 的显著度下，假设不能被拒绝。即能以置信度 \(1 - \alpha\) 认为泛化错误率不大于 \(\epsilon_0\)。

### T 检验

多次重复留出法或交叉验证法等进行多次训练/测试，会得到 \(k\) 个测试错误率 \(\hat{\epsilon}_1, \hat{\epsilon}_2, \cdots, \hat{\epsilon}_k\)，计算平均错误率 \(\mu\) 和样本方差 \(\sigma^2\)：

\[
\begin{aligned}
\mu &= \frac{1}{k} \sum_{i=1}^{k} \hat{\epsilon}_i \\
\sigma^2 &= \frac{1}{k-1} \sum_{i=1}^{k} (\hat{\epsilon}_i - \mu)^2
\end{aligned}
\]

变量

\[
\tau_t = \frac{\sqrt{k}(\mu - \epsilon_0)}{\sigma}
\]

服从自由度为 \(k-1\) 的 t 分布。考虑双边假设，若 \(|\mu - \epsilon_0| \in [t_{-\alpha/2}, t_{\alpha/2}]\) 则在置信度 \(1 - \alpha\) 下，假设不能被拒绝。

### 交叉验证 T 检验

对两个学习器 A 和 B ，若使用 k 折交叉验证法得到的测试错误率分别为 \(\hat{\epsilon}_1^A, \hat{\epsilon}_2^A, \cdots, \hat{\epsilon}_k^A\) 和 \(\hat{\epsilon}_1^B, \hat{\epsilon}_2^B, \cdots, \hat{\epsilon}_k^B\)，则可以使用成对 T 检验检验两个学习器的性能是否有显著差异。

基本思路是计算出 \(\Delta_i = \hat{\epsilon}_i^A - \hat{\epsilon}_i^B\)，计算平均差值 \(\mu_\Delta\) 和样本方差 \(\sigma_\Delta^2\)：

## 线性模型

一般形式：

\[
f(\mathbf{x}) = \omega_1 x_1 + \omega_2 x_2 + \cdots + \omega_d x_d + b
\]

优点：形式简单、易于建模；可解释性强。

### Perceptron 感知机

线性模型中，若误分类，则有：

\[
-y_i(\omega^T x_i + b) > 0
\]

定义损失函数

\[
L(\omega, b) = -\sum_{x_i \in M} y_i(\omega^T x_i + b)
\]

梯度：

\[
\begin{aligned}
\frac{\partial L}{\partial \omega} &= -\sum_{x_i \in M} y_i x_i \\
\frac{\partial L}{\partial b} &= -\sum_{x_i \in M} y_i
\end{aligned}
\]

### 线性回归

目标：

\[
f(x) = \omega^T x + b, \ \text{s.t.} \ f(x_i) \approx y_i
\]

最小二乘法：

\[
\begin{aligned}
(\omega^*, b^*) &= \arg\min_{\omega, b} \sum_{i=1}^{m} (f(x_i) - y_i)^2 \\
&= \arg\min_{\omega, b} \sum_{i=1}^{m} (y_i - \omega x_i - b)^2
\end{aligned}
\]

解析解：

\[
\begin{aligned}
\omega^* &= \frac{\sum_{i=1}^{m} y_i (x_i - \bar{x})}{\sum_{i=1}^{m} x_i^2 - \frac{1}{m} (\sum_{i=1}^{m} x_i)^2} \\
b^* &= \frac{1}{m} \sum_{i=1}^{m} (y_i - \omega^* x_i)
\end{aligned}
\]

#### 广义线性模型

一般形式：

\[
y = g^{-1}(\omega^T x + b)
\]

其中 \(g(\cdot)\) 为连接函数（link function）。

#### 对数几率回归

二分类任务中，输出标记 \(y \in \{0, 1\}\)，需要将线性回归的实值输出 \(z\) 转换为 0-1 输出。

- 单位阶跃函数（step function）：

    \[
    y = \begin{cases}
    1, & z > 0 \\
    0.5, & z = 0 \\
    0, & z < 0
    \end{cases}
    \]

    当 \(z = 0\) 时，可以任意判别为 0 或 1。

- 对数几率函数（logistic function）：

    单位阶跃函数不连续，不能用作 \(g(\cdot)\)。对数几率函数为单位阶跃函数的连续近似：

    \[
    y = \frac{1}{1 + e^{-z}}
    \]

把对数几率函数代入广义线性模型，得到对数几率回归模型：

\[
y = \frac{1}{1 + e^{-(\omega^T x + b)}}
\]

变换为：

\[
\ln \frac{y}{1-y} = \omega^T x + b
\]

将 \(y\) 作为正例的概率，\(1-y\) 作为反例的概率，两者比值 \(y/(1-y)\) 称为几率（odds），反映了正例相对于反例的可能性。

取对数则为对数几率（log odds） \(\ln y/(1-y)\)。

#### 极大似然估计

对数几率为：

\[
\ln\frac{p(y=1|x)}{p(y=0|x)} = \omega^T x + b
\]

有

\[
\begin{aligned}
p(y=1|x) &= \frac{e^{\omega^T x + b}}{1 + e^{\omega^T x + b}} \\
p(y=0|x) &= \frac{1}{1 + e^{\omega^T x + b}}
\end{aligned}
\]

通过极大似然法来估计 \(\omega\) 和 \(b\)。给定数据集 \(\{(x_i, y_i)\}_{i=1}^{m}\)，对数回归模型最大化对数似然函数：

\[
l(\omega, b) = \sum_{i=1}^{m} \ln p(y_i|x_i; \omega, b)
\]

即令每个样本属于其真实标记的概率越大越好。

令 \(\beta = (\omega, b), \hat{x} = (x, 1)\)，则 \(\omega^T x + b = \beta^T \hat{x}\)。再令

\[
\begin{aligned}
p_0(\hat{x}; \beta) &= p(y=1|\hat{x}; \beta) \\
p_1(\hat{x}; \beta) &= p(y=0|\hat{x}; \beta) \\
p(y_i | \hat{x}_i; \beta) &= y_i p_0(\hat{x}_i; \beta) + (1 - y_i) p_1(\hat{x}_i; \beta)
\end{aligned}
\]

即最小化负对数似然函数：

\[
l(\beta) = \sum_{i=1}^{m} (-y_i \beta^T \hat{x}_i + \ln(1 + e^{\beta^T \hat{x}_i}))
\]

求解

\[
\beta^* = \arg\min_{\beta} l(\beta)
\]

使用牛顿法求解 \(\frac{\partial l}{\partial \beta} = 0\) 得到 \(\beta^*\)：

\[
\beta^{t+1} = \beta^t - (\frac{\partial^2 l}{\partial \beta \partial \beta^T})^{-1} \frac{\partial l}{\partial \beta}
\]

### 线性判别分析

线性判别分析（Linear Discriminant Analysis, LDA）。监督降维技术。使同类样本的投影点尽可能接近，不同类样本的投影点尽可能远离。

- 同类样本协方差小
- 异类样本类中心距离大

定义：

- 第 \(i\) 类样本集合：\(X_i\)
- 第 \(i\) 类样本均值：\(\mu_i\)
- 第 \(i\) 类样本协方差矩阵：\(\Sigma_i\)
- 直线方向向量：\(\omega\)
- 样本中心在直线上的投影：\(\omega^T \mu_i\)
- 协方差：\(\omega^T \Sigma_i \omega\)

最大化目标（广义瑞利熵）：

\[
\begin{aligned}
J &= \frac{\|\omega^T \mu_1 - \omega^T \mu_2\|_2^2}{\omega^T \Sigma_1 \omega + \omega^T \Sigma_2 \omega} \\
&= \frac{\omega^T (\mu_1 - \mu_2)(\mu_1 - \mu_2)^T \omega}{\omega^T (\Sigma_1 + \Sigma_2) \omega}
\end{aligned}
\]

类内散度矩阵：

\[
S_w = \sum_{i=1}^{c} \Sigma_i = \sum_{i=1}^{c} \sum_{x \in X_i} (x - \mu_i)(x - \mu_i)^T
\]

类间散度矩阵：

\[
S_b = (\mu_1 - \mu_2)(\mu_1 - \mu_2)^T
\]

广义瑞利熵：

\[
J = \frac{\omega^T S_b \omega}{\omega^T S_w \omega}
\]

令 \(\omega^T S_w \omega = 1\)，最大化广义瑞利熵等价于

\[
\min_{\omega} -\omega^T S_b \omega \quad \text{s.t.} \ \omega^T S_w \omega = 1
\]

由拉格朗日乘子法，上式等价于

\[
S_b \omega = \lambda S_w \omega
\]

令 \(S_b \omega = \lambda (\mu_1 - \mu_2)\)，则

\[
\omega = S_w^{-1} (\mu_1 - \mu_2)
\]

将 \(S_w\) 进行奇异值分解 \(S_w = U \Sigma V^T\)，则

\[
\omega = V \Sigma^{-1} U^T (\mu_1 - \mu_2)
\]

### 多分类任务

拆分为多个二分类任务：

- 一对一（one-vs-one）：\(N\) 类样本两两配对，训练 \(N(N-1)/2\) 个分类器，对于每个样本，每个分类器投票，最终投票最多的类别为预测类别。

    优点：每个分类器只需关注两个类别，训练速度快。

    缺点：分类器数量多，测试速度慢。

- 一对其余（one-vs-rest）：某一类别为正例，其余类别为反例，训练 \(N\) 个分类器，对于每个样本，每个分类器输出正例的概率，最终概率最大的类别为预测类别。

    优点：分类器数量少，存储开销小，测试速度快。

    缺点：训练用到全部数据，训练速度慢。

- 多对多（many-vs-many）：若干类别为正例，若干类别为反例，训练若干个分类器，对于每个样本，每个分类器投票，最终投票最多的类别为预测类别。

#### 纠错输出码

- 编码：对 \(N\) 个类别进行 \(M\) 次划分，得到 \(M\) 个分类器。
- 解码：\(M\) 个分类器分别对测试样本进行预测，组成一个 \(M\) 位的编码，将这个预测编码与每个类别各自的编码进行比较，返回其中距离最小的类别作为最终预测结果。

<div align="center"><img src="/assets/img/CS/ml/ecoc.png" width="80%"></div>

- 海明距离：两个编码之间不同的位数。
- 欧式距离：\(\sqrt{\sum_{i=1}^{M} (y_i - y'_i)^2}\)

### 类别不平衡问题

不同类别的样本数量差异较大。

解决方法：转换为类别平衡问题。

再缩放方法：

- 欠采样：去除一些反例使得正例和反例数量接近。
- 过采样：增加一些正例使得正例和反例数量接近。
- 阈值移动：调整分类器的输出阈值。

## 决策树

决策树基于树结构进行预测。

<div align="center"><img src="/assets/img/CS/ml/dt.png" width="60%"></div>

终止情况：

- 当前结点包含的样本全属于同一类别。
- 当前结点包含的样本为空。结点类别设定为其父结点包含样本最多的类别。
- 当前结点包含样本的剩余特征的取值相同。结点类别设定为包含样本最多的类别。

### 划分选择

#### ID3 决策树

设当前样本集合中第 \(k\) 类样本的比例为 \(p_k\)，则信息熵为

\[
Ent(D) = -\sum_{k=1}^{K} p_k \log_2 p_k
\]

信息熵越小，样本的纯度越高。

使用属性 \(a\) 对样本进行划分所得到的信息增益为

\[
Gain(D, a) = Ent(D) - \sum_{v=1}^{V} \frac{|D^v|}{|D|} Ent(D^v)
\]

其中 \(a\) 有 \(V\) 个不同的取值，\(D^v\) 为 \(D\) 中属性 \(a\) 取值为 \(a^v\) 的样本子集。

信息增益越大，使用属性 \(a\) 划分样本的效果越好。ID3 算法使用信息增益选择划分属性。

#### C4.5 决策树

增益率：

\[
\text{Gain_ratio}(D, a) = \frac{\text{Gain}(D, a)}{\text{IV}(a)}
\]

其中 IV 为属性 \(a\) 的固有值：

\[
\text{IV}(a) = -\sum_{v=1}^{V} \frac{|D^v|}{|D|} \log_2 \frac{|D^v|}{|D|}
\]

相当于对信息增益进行了归一化。C4.5 算法使用增益率选择划分属性。

C4.5 决策树：先找出信息增益高于平均水平的属性，再从中选择增益率最高的属性进行划分。

#### CART 决策树

数据集 \(D\) 的基尼值为

\[
\text{Gini}(D) = \sum_{k=1}^{K} \sum_{k' \neq k} p_k p_{k'} = 1 - \sum_{k=1}^{K} p_k^2
\]

基尼值越小，样本的纯度越高。

选择属性 \(a\) 进行划分，得到的基尼指数为

\[
\text{Gini_index}(D, a) = \sum_{v=1}^{V} \frac{|D^v|}{|D|} Gini(D^v)
\]

CART 算法使用基尼指数选择划分属性，选择基尼指数最小的属性进行划分。

### 连续与缺失值

#### 连续属性

使用二分法进行划分。对于连续值 \(a\)，考虑划分点

\[
T_a = \{\frac{a_i + a_{i+1}}{2} | 1 \leq i \leq n-1\}
\]

选择最优的划分点进行划分。即

\[
\text{Gain}(D, a) = \max_{t \in T_a} \text{Gain}(D, a, t)
\]

### 剪枝

决策树容易过拟合，剪枝是防止过拟合的重要手段。

#### 预剪枝

边建树边剪枝。若当前结点进行划分不能带来泛化能力的提升，则停止划分，将当前结点标记为叶结点。

预留验证集，分别计算划分前后的验证集精度，判断是否进行划分。

- 优点：降低过拟合风险，提高泛化能力。减少训练和测试时间。
- 缺点：可能会导致欠拟合。

#### 后剪枝

先建树再剪枝。自底向上对非叶结点进行考察，若将其划分为叶结点能带来泛化能力的提升，则将其划分为叶结点。

- 优点：保留了更多分支，欠拟合风险较小。
- 缺点：训练时间较长，需要生成完整的决策树。

## 神经网络

### 神经元模型

#### MP 神经元

- 输入：\(x_1, x_2, \cdots, x_d\)
- 权重：\(\omega_1, \omega_2, \cdots, \omega_d\)
- 阈值：\(\theta\)
- 激活函数：\(f(\cdot)\)
- 输出：\(y = f(\sum_{i=1}^{d} \omega_i x_i - \theta)\)

#### 激活函数

- 阶跃函数（step function）：

    \[
    f(z) = \begin{cases}
    1, & z \geq 0 \\
    0, & z < 0
    \end{cases}
    \]

- Sigmoid 函数：

    \[
    f(z) = \frac{1}{1 + e^{-z}}
    \]

- ReLU 函数：

    \[
    f(z) = \max(0, z)
    \]

- Tanh 函数：

    \[
    f(z) = \frac{e^z - e^{-z}}{e^z + e^{-z}}
    \]

### 感知机与多层网络

#### 感知机

感知机由两层神经元组成，输入层接收输入信号，输出层进行输出（MP 神经元）。

#### 感知机学习

学习权重 \(\omega\)

\[
\begin{aligned}
\omega_i &\leftarrow \omega_i + \Delta \omega_i \\
\Delta \omega_i &= \eta (y - \hat{y}) x_i
\end{aligned}
\]

其中 \(\eta\) 为学习率。

若两类模式线性可分, 则感知机的学习过程一定会收敛。

单层感知机只能解决线性可分问题。

#### 多层感知机

加入隐藏层，可以解决非线性可分问题。

#### 多层前馈神经网络

每层神经元与下一层神经元全连接，信息单向传播。

### 误差逆传播算法

训练权重、阈值。

只需要一个包含足够多神经元的隐藏层，就可以以任意精度逼近任意连续函数。

缓解过拟合：

- 早停法：若训练误差降低，但验证误差升高，则停止训练。
- 正则化：在误差函数中加入权重衰减项。

#### 交叉熵损失函数

对于样本 \((x_i, y_i)\)，输出层的输出为 \(\hat{y}_i\)，则交叉熵损失函数为

\[
L = -y_i \log \hat{y}_i - (1 - y_i) \log (1 - \hat{y}_i)
\]

### 局部最小与全局最小

跳出局部最小：

- 多组不同初始化参数优化神经网络，选择最优参数。
- 模拟退火
- 随机梯度下降
- 遗传算法

### 深度学习

很深层的神经网络。

## 支持向量机

间隔和支持向量：

<div align="center"><img src="/assets/img/CS/ml/svm.png" width="60%"></div>

- 超平面：\(\omega^T x + b = 0\)。
- 间隔：两个异类支持向量到超平面的距离之和 \(\gamma = \frac{2}{\|\omega\|}\)。

寻找参数 \(\omega\) 和 \(b\) 使得间隔最大化：

\[
\begin{aligned}
&\max_{\omega, b} \frac{2}{\|\omega\|} \\
&\text{s.t.} \ y_i(\omega^T x_i + b) \geq 1, \ i = 1, 2, \cdots, m
\end{aligned}
\]

等价于

\[
\begin{aligned}
&\min_{\omega, b} \frac{1}{2} \|\omega\|^2 \\
&\text{s.t.} \ y_i(\omega^T x_i + b) \geq 1, \ i = 1, 2, \cdots, m
\end{aligned}
\]

上式为支持向量机的基本型。

### 对偶问题

使用拉格朗日乘子法，得到拉格朗日函数：

\[
L(\omega, b, \alpha) = \frac{1}{2} \|\omega\|^2 + \sum_{i=1}^{m} \alpha_i (1 - y_i(\omega^T x_i + b))
\]

令偏导数为 0，得到对偶问题：

\[
\begin{aligned}
&\max_{\alpha} \sum_{i=1}^{m} \alpha_i - \frac{1}{2} \sum_{i=1}^{m} \sum_{j=1}^{m} \alpha_i \alpha_j y_i y_j x_i^T x_j \\
&\text{s.t.} \ \sum_{i=1}^{m} \alpha_i y_i = 0 \\
&\alpha_i \geq 0, \ i = 1, 2, \cdots, m
\end{aligned}
\]

求解得到 \(\alpha^*\)，得到 \(\omega^* = \sum_{i=1}^{m} \alpha_i^* y_i x_i\)。

KKT 条件：

- \(\alpha_i \geq 0\)
- \(y_i f(x_i) \geq 1\)
- \(\alpha_i (y_i f(x_i) - 1) = 0\)

当 \(y_i f(x_i) = 1\) 时，\(\alpha_i > 0\)，\(x_i\) 为支持向量。

### SMO

序列最小最优化算法（Sequential Minimal Optimization, SMO）。每次选择两个变量进行优化，固定其他变量。

设对 \(\alpha_i, \alpha_j\) 进行优化，其他变量固定，则约束为

\[
\alpha_i y_i + \alpha_j y_j = -\sum_{k \neq i, j} \alpha_k y_k = c
\]

求解对偶形式，更新 \(\alpha_i, \alpha_j\)：

令

\[
\alpha_i = \frac{c - \alpha_j y_j}{y_i}
\]

带入目标函数，得到单变量二次规划问题，解得 \(\alpha_j\)，再更新 \(\alpha_i\)。

根据每个支持向量计算出 \(b\)，取平均值。

### 核函数

若数据线性不可分，可使用核函数将数据映射到高维空间，再在高维空间中寻找最优超平面。

将 \(x_i\) 映射到高维空间 \(\phi(x_i)\)，则优化问题变为

\[
\begin{aligned}
&\max_{\alpha} \sum_{i=1}^{m} \alpha_i - \frac{1}{2} \sum_{i=1}^{m} \sum_{j=1}^{m} \alpha_i \alpha_j y_i y_j \phi(x_i)^T \phi(x_j) \\
&\text{s.t.} \ \sum_{i=1}^{m} \alpha_i y_i = 0 \\
&\alpha_i \geq 0, \ i = 1, 2, \cdots, m
\end{aligned}
\]

### 软间隔与正则化

允许一些样本不满足约束条件。使用 0/1 损失函数：

\[
l_{0/1}(z) = \begin{cases}
1, & z < 0 \\
0, & z \geq 0
\end{cases}
\]

则优化目标变为

\[
\min_{\omega, b} \frac{1}{2} \|\omega\|^2 + C \sum_{i=1}^{m} l_{0/1}(y_i(\omega^T x_i + b) - 1)
\]

然而 0/1 损失函数非凸，不连续，不可导，不易优化。使用 Hinge 损失函数进行替代：

\[
\min_{\omega, b} \frac{1}{2} \|\omega\|^2 + C \sum_{i=1}^{m} \max(0, 1-y_i(\omega^T x_i + b))
\]

当 \(C\) 较大时，对误分类的惩罚较大，模型更关注正确分类；当 \(C\) 较小时，对误分类的惩罚较小，模型更关注间隔的最大化。

转换为对偶问题：

\[
\begin{aligned}
&\max_{\alpha} \sum_{i=1}^{m} \alpha_i - \frac{1}{2} \sum_{i=1}^{m} \sum_{j=1}^{m} \alpha_i \alpha_j y_i y_j x_i^T x_j \\
&\text{s.t.} \ \sum_{i=1}^{m} \alpha_i y_i = 0 \\
&0 \leq \alpha_i \leq C, \ i = 1, 2, \cdots, m
\end{aligned}
\]

正则化：更一般的形式为

\[
\min_{f} \Omega(f) + C \sum_{i=1}^{m} l(f(x_i), y_i)
\]

其中 \(\Omega(f)\) 为结构风险，\(l(f(x_i), y_i)\) 为经验风险。

### 支持向量回归

允许模型输出和实际输出之间存在 \(2\epsilon\) 的偏差。

落入间隔带的样本不计算损失

## 贝叶斯分类器

### 贝叶斯决策论

类别标记 \(\mathcal{Y} = \{c_1, c_2, \cdots, c_N\}\)，\(\lambda_{ij}\) 为将类别 \(c_j\) 分类为 \(c_i\) 的损失。

后验概率 \(P(c_i|x)\) 为样本 \(x\) 属于类别 \(c_i\) 的概率。将 \(x\) 分类为 \(c_i\) 的期望损失为

\[
R(c_i|x) = \sum_{j=1}^{N} \lambda_{ij} P(c_j|x)
\]

任务是寻找一个判定准则 \(h : \mathcal{X} \mapsto \mathcal{Y}\)，使得期望损失最小：

\[
R(h) = \mathbb{E}_x[R(h(x)|x)]
\]

贝叶斯判定准则：对于每个样本 \(x\)，选择使得 \(R(c_i|x)\) 最小的类别 \(c_i\) 作为预测类别：

\[
h^*(x) = \arg\min_{c \in \mathcal{Y}} R(c|x)
\]

\(h^*\) 为贝叶斯最优分类器，\(R(h^*)\) 为贝叶斯风险，\(1-R(h^*)\) 反映了分类器能达到的最好性能。

若目标是最小化分类错误率，则损失为

\[
\lambda_{ij} = \begin{cases}
0, & i = j \\
1, & i \neq j
\end{cases}
\]

则期望损失为

\[
R(c_i|x) = \sum_{j \neq i} P(c_j|x) = 1 - P(c_i|x)
\]

则最优分类器为

\[
h^*(x) = \arg\max_{c \in \mathcal{Y}} P(c|x)
\]

获取后验概率 \(P(c|x)\) 的方法：

- 判别式模型：对 \(P(c|x)\) 建模
- 生成式模型：先对联合概率分布 \(P(x, c)\) 建模，再由此得到 \(P(c|x)\)

    \[
    P(c|x) = \frac{P(x, c)}{P(x)} = \frac{P(x|c)P(c)}{P(x)}
    \]

    其中

    - \(P(c)\) 为类别先验概率
    - \(P(x|c)\) 为类条件概率，称为似然
    - \(P(x)\) 为证据因子

### 极大似然估计

先假设具有某种概率分布，被参数 \(\theta_c\) 唯一确定，再通过训练数据估计参数。

将 \(P(x|c)\) 记为 \(P(x|\theta_c)\)。设 \(D_c\) 为类别 \(c\) 的训练数据集，则 \(D_c\) 的似然为

\[
P(D_c|\theta_c) = \prod_{x \in D_c} P(x|\theta_c)
\]

代表取得 \(D_c\) 的概率。通常使用对数似然：

\[
LL(\theta_c) = \log P(D_c|\theta_c) = \sum_{x \in D_c} \log P(x|\theta_c)
\]

最大化对数似然，得到参数估计 \(\hat{\theta}_c\)：

\[
\hat{\theta}_c = \arg\max_{\theta_c} LL(\theta_c)
\]

### 朴素贝叶斯分类器

假设每个属性独立地对分类结果产生影响，即

\[
P(x|c) = \frac{P(c)P(c|x)}{P(x)} = \frac{P(c)}{P(x)} \prod_{i=1}^{d} P(x_i|c)
\]

则朴素贝叶斯分类器为

\[
h_{nb}(x) = \arg\max_{c \in \mathcal{Y}} P(c) \prod_{i=1}^{d} P(x_i|c)
\]

类先验概率 \(P(c) = \frac{|D_c|}{|D|}\)。

令 \(D_{c, x_i}\) 为 \(D_c\) 中第 \(i\) 个属性取值为 \(x_i\) 的样本子集，估计类条件概率 \(P(x_i|c)\) 为

\[
P(x_i|c) = \frac{|D_{c, x_i}|}{|D_c|}
\]

过程：

- 计算类先验概率 \(P(c)\)
- 计算类条件概率 \(P(x_i|c)\)
- 计算使得 \(P(c) \prod_{i=1}^{d} P(x_i|c)\) 最大的类别 \(c\) 作为预测类别

平滑：使用拉普拉斯修正，避免概率为 0。

令 \(N\) 表示训练集 \(D\) 中可能的类别数，\(N_i\) 表示第 \(i\) 个属性可能的取值数，则

\[
\begin{aligned}
P(c) &= \frac{|D_c| + 1}{|D| + N} \\
P(x_i|c) &= \frac{|D_{c, x_i}| + 1}{|D_c| + N_i}
\end{aligned}
\]

### 半朴素贝叶斯分类器

适当考虑一部分属性问的相互依赖信息。

独依赖估计：假设每个属性在类别之外最多仅依赖于一个其他属性。

### 贝叶斯网

贝叶斯网是一个有向无环图，结点表示随机变量，边表示变量之间的依赖关系。使用条件概率表描述结点之间的依赖关系。

### EM 算法

存在隐变量的情况下，使用 EM 算法进行参数估计。

令 \(X\) 为观测变量，\(Z\) 为隐变量，\(\Theta\) 为模型参数，则要最大化对数似然

\[
LL(\Theta \mid X, Z) = \ln P(X, Z \mid \Theta)
\]

通过对 \(Z\) 求期望，最大化边际似然

\[
LL(\Theta \mid X) = \ln P(X \mid \Theta) = \ln \sum_Z P(X, Z \mid \Theta)
\]

EM 算法：

- E 步：已知参数 \(\Theta\)，计算对数似然关于隐变量 \(Z\) 的期望
- M 步：已知 \(Z\)，寻找参数 \(\Theta\) 使得对数似然期望最大化

## 集成学习

集成学习（ensemble learning）通过构建并结合多个学习器来提升性能。

## 聚类

### 性能度量

- 外部指标：与某个“参考模型”进行比较
- 内部指标：不依赖于任何参考模型

目标：簇内相似度高，簇间相似度低。

### 距离计算

闵可夫斯基距离：

\[
\text{dist}_{\text{mk}}(x_i, x_j) = (\sum_{u=1}^{d} |x_{iu} - x_{ju}|^p)^{1/p}
\]

- \(p = 1\) 为曼哈顿距离
- \(p = 2\) 为欧氏距离

### 聚类方法

- 原型聚类：有簇中心的聚类。K 均值聚类。
- 密度聚类：从样本密度的角度来考察样本之间的可连接性。DBSCAN 算法。
- 层次聚类：自底向上或自顶向下的聚类方法。AGNES 算法。

#### K 均值聚类

簇中心为簇内样本的均值。

## 降维与度量学习

### k 近邻学习 k-NN

给定测试样本，基于某种距离度量找出训练集中与其最靠近的 \(k\) 个训练样本，然后基于这 \(k\) 个“邻居”的信息来进行预测。

最近邻分类器，1-NN 分类器，出错的概率

\[
P(err) = 1 - \sum_{c\in\mathcal{Y}} P(c|x) P(c|z)
\]

其中 \(z\) 为样本 \(x\) 的最近邻。

最近邻分类器的泛化误差不超过贝叶斯最优分类器的误差的两倍。

### 低维嵌入

将高维数据映射到低维空间，保留数据的主要结构。

目标：保持数据之间的相对距离。

多维缩放（MDS）：最小化原始数据点之间的距离与嵌入数据点之间的距离之间的差异。思路：保持内积不变。

### 主成分分析 PCA

给定 \(d\) 维空间中的样本 \(X = (x_1, x_2, \cdots, x_m) \in \mathbb{R}^{d\times m}\)，变换之后得到 \(d'< d\) 维空间中的样本

\[
Z = W^T X
\]

其中 \(W \in \mathbb{R}^{d \times d'}\) 为变换矩阵。

PCA 的优化目标：

\[
\max_{W} \mathrm{tr}(W^T X X^T W) \quad \text{s.t.} \quad W^T W = I
\]

对 \(X\) 进行中心化后，对 \(X X^T\) 进行特征值分解，得到特征值和特征向量，按照特征值大小选择前 \(d'\) 个特征向量组成变换矩阵 \(W = (w_1, w_2, \cdots, w_{d'})\)，这几个特征向量对应的平面就是数据的主成分。

### 流形学习

- 构造近邻图
- 使用最短路算法近似两点之间的测地线距离
- 基于距离矩阵使用 MDS 算法进行降维

### 度量学习

通过参数化学习距离度量。

马氏距离：

\[
\text{dist}^2_{\text{mah}}(x_i, x_j) = (x_i - x_j)^T M (x_i - x_j)
\]

其中 \(M\) 为度量矩阵，对度量矩阵进行学习。

## 特征选择与稀疏学习

特征的分类：

- 相关特征：对当前学习任务有用的特征
- 无关特征：对当前学习任务无用的特征
- 冗余特征：与其他特征线性相关的特征

### 特征选择

确保不丢失重要特征。

方法：

1. 产生初始候选子集
2. 评价候选子集的好坏
3. 根据结果产生新的候选子集，重复 2

#### 子集搜索

用贪心策略选择包含重要信息的特征子集。

- 前向搜索：逐步增加特征
- 后向搜索：逐步减少特征
- 双向搜索：每一轮逐渐增加相关特征，同时减少无关特征

#### 子集评价

比较特征子集对应的划分与样本标记对应的划分的差异。

方法：信息熵

#### 特征选择方法

- 过滤式（filter）
- 包裹式（wrapper）
- 嵌入式（embedded）

### 过滤式特征选择

先对数据集进行特征选择，然后再训练学习器，特征选择过程与学习器无关。

Relief 算法：对每个样本，先在同类样本中找出最近邻，再在异类样本中找出最近邻，计算特征的权重。

### 包裹式特征选择

把最终将要使用的学习器的性能作为特征子集的评价准则。

需多次训练学习器，因此包裹式特征选择的计算开销通常比较大。

LVM 算法：使用随机策略进行子集搜索，将最终分类器的误差作为特征子集的评价准则。

### 嵌入式特征选择

将特征选择过程与学习器训练过程融为一体，两者在同一个优化过程中完成。

若最优化问题为

\[
\max_{\omega} \sum_{i=1}^{m} (y_i - \omega^T x_i)^2
\]

使用嵌入式特征选择，加入正则项

\[
\max_{\omega} \sum_{i=1}^{m} (y_i - \omega^T x_i)^2 + \lambda \|\omega\|^2_2
\]

引入 \(L_2\) 范数正则化，降低过拟合的风险。

- \(L_2\) 正则化：岭回归
- \(L_1\) 正则化：LASSO，更易于获得稀疏解

### 稀疏表示

将数据集考虑成一个矩阵，每行对应一个样本，每列对应一个特征。考虑每一行有很多 0 元素。

### 字典学习

为普通稠密表达的样本找到合适的字典，将样本转化为稀疏表示，这一过程称为字典学习。

### 压缩感知

利用接收到的压缩、丢包后的数字信号，精确重构出原信号。

## 半监督学习

### 未标记样本

- 聚类假设：假设数据存在簇结构，同一个簇的样本属于同一个类别。
- 流形假设：假设数据分布在一个流形结构上，邻近的样本拥有相似的输出值。

半监督学习分类：

- 纯半监督学习：假定训练数据中的未标记样本并非待预测的数据。
- 直推学习：假定学习过程中所考虑的未标记样本恰是待预测数据，学习的目的就是在这些未标记样本上获得最优泛化性能。

### 生成式方法

假设所有数据（无论是否标记）都是由一个模型生成的。未标记数据的标记可以看作模型的参数。

### 半监督 SVM

半监督 SVM （S3VM）：找到能将两类有标记样本分开且穿过数据低密度区域的超平面。

TSVM 算法：考虑对未标记样本进行各种可能的样本指派，寻找一个在所有样本上间隔最大化的划分超平面。

### 图半监督学习

样本对应结点，边的强度正比于样本之间的相似度。

将已标记的样本视作染过色，半监督学习就对应于颜色的传播。

### 基于分歧的方法

使用多学习器，利用不同学习器之间的分歧。

多视图数据：一个数据对象往往同时拥有多个"属性集" (attribute set) ，每个属性集就构成了一个“视图”(view)。

训练步骤：

- 在每个视图上基于有标记样本分别训练出一个分类器
- 让每个分类器分别去挑选自己"最有把握的"未标记样本赋予伪标记，并将伪标记样本提供给另一个分类器作为新增的有标记样本用于训练更新
- 重复上述过程直到收敛

### 半监督聚类

- 必连约束：两个样本必属于同一个簇
- 勿连约束：两个样本必不属于同一个簇

## 概率图模型

## 规则学习

## 强化学习

马尔可夫决策过程（Markov Decision Process, MDP）：机器通过在环境中不断尝试从而学到一个策略，使得长期执行该策略后得到的累积奖赏最大。

没有有标记样本，通过执行动作之后反馈的奖赏来学习。强化学习在某种意义上可以认为是具有“延迟标记信息”的监督学习。