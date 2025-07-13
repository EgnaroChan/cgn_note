# DDPM论文解析
## 所需数学基础：
### 1.条件概率的一般形式
$P(A,B,C)=P(C｜B，A)P(B,A)=P(C|B,A)P(B|A)P(A)$
$P(B,C|A)=P(B|A)P(C|A,B)$

### 2.基于马尔可夫假设的条件概率
如果满足马尔可夫链关系$A\rightarrow B\rightarrow C$,那么有
$P(A,B,C)=P(C｜B，A)P(B,A)=P(C|B)P(B|A)P(A)$
$P(B,C|A)=P(B|A)P(C|B)$

### 3.高斯分布的KL散度公式
对于两个单一变量的高斯分布$p$和$q$而言，它们的KL散度为
$$KL(p,q)=log\frac{\sigma_2}{\sigma_1}+\frac{\sigma_1^2+(\mu_1-\mu_2)^2}{2\sigma_2^2}-\frac{1}{2}$$

## 1.公式推导$q(X_{1:T}|X_0):=\prod_{t=1}^Tq(X_t|X_{t-1})$
$q(X_{1:T}|X_0)=\frac{q(X_{0:T})}{q(X_0)}$
$q(X_{0:T})=q(X_0)\cdot q(X_1|X_0)\cdot q(X_2|X_1\cdot X_0)...q(X_T|X_{T-1}X_{T-2}...X_0)$
根据马尔科夫性质，得$q(X_{0:T})=q(X_0)\cdot q(X_1|X_0)\cdot q(X_2|X_1)...q(X_T|X_{T-1})$
即推导出$q(X_{1:T}|X_0):=\prod_{t=1}^Tq(X_t|X_{t-1})$

## 2.公式推导$ q(x_t | x_0) := \mathcal{N}(x_t; \sqrt{\bar{\alpha}_t} x_0, (1 - \bar{\alpha}_t) I) $
已知条件：$q(X_t|X_{t-1})=\mathcal{N}(X_t;\sqrt{1-\beta_t}X_{t-1},\beta_tI)$
令$\alpha_t=1-\beta_t,\bar{\alpha_t}=\prod_{s=1}^{t}\alpha_s$
根据重参数技巧，要证$X_t=\sqrt{\bar{\alpha_t}}X_0+\sqrt{1-\bar{\alpha_t}}\epsilon$,其中$\epsilon\sim\mathcal{N}(0,I)$
当$t=1$时，$X_1=\sqrt{}$

## 3.扩散模型的思想
我们有一个完美的去噪公式，但它需要一个我们没有的输入（$x_0$是我们需要生成的，所以没有），于是使用一个神经网络来对$x_0$给出一个尽可能准确的预测。

## 参考文献
1.https://learnopencv.com/denoising-diffusion-probabilistic-models/
2.https://lilianweng.github.io/posts/2021-07-11-diffusion-models/#reverse-diffusion-process









