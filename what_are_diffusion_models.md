# __什么是扩散模型？__
本文是对[扩散模型](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/#connection-with-stochastic-gradient-langevin-dynamics)的内容补充。
## **1.对$x_t=\sqrt{\bar\alpha_t}x_0+\sqrt{1-\bar\alpha_t}\epsilon$的证明**
已知$x_t=\sqrt{\alpha_t}x_{t-1}+\sqrt{1-\alpha_t}\epsilon$，其中$\epsilon\sim\mathcal{N}(0,I)$.

若$$x_t=\sqrt{\alpha_t...\alpha_k}x_{k-1}+\sqrt{1-\alpha_t...\alpha_k}\epsilon$$

则有
$$
\begin{aligned}
x_t &= \sqrt{\alpha_t...\alpha_k}(\sqrt{\alpha_{k-1}}x_{k-2}+\sqrt{1-\alpha_{k-1}}\epsilon_1)+\sqrt{1-\alpha_t...\alpha_k}\epsilon_2
\\&= \sqrt{\alpha_t...\alpha_k\alpha_{k-1}}x_{k-2}+\sqrt{\alpha_t...\alpha_k(1-\alpha_{k-1})}\epsilon_1+\sqrt{1-\alpha_t...\alpha_k}\epsilon_2
\\&= \sqrt{\alpha_t...\alpha_k\alpha_{k-1}}x_{k-2}+\sqrt{1-\alpha_t...\alpha_k\alpha_{k-1}}\epsilon
\end{aligned}
$$
其中 $\epsilon$ 均为标准高斯分布，为了偷懒，仅在同时出现时进行编号区分。

由数学归纳法可知,取 $k=1$ 时
$$x_t = \sqrt{\bar\alpha_t}x_0+\sqrt{1-\bar\alpha_t}\epsilon$$

## **2.对于$q(x_{t-1}|x_t)$对错误理解**
由于
$$x_t = \sqrt{\alpha_t}x_{t-1}
+\sqrt{1-\alpha_t}\epsilon$$
通过移项可得
$$x_{t-1} = \frac{x_t - \sqrt{1-\alpha_t}\epsilon}{\sqrt{\alpha_t}}$$
因此，根据重参数技巧得（高斯分布的对称性，故取正负号无影响）
$$q(x_{t-1}|x_t) = \mathcal{N}\left(\frac{x_t}{\sqrt{\alpha_t}}, \frac{1-\alpha_t}{\alpha_t}I\right)$$
此处可作为原文中 (Note that if $\beta_t$ is small enough, $q(x_{t-1}|x_t)$ will also be Gaussian) 的补充。

但有一点需要补充说明，即$q(x_{t-1}|x_t)$已算出，为何还要使用神经网络来预测呢？

这是因为在前向过程中，所加入噪声虽然是随机取出的，但取出后就是确定的值了，并不是在反向过程中重新随机取出一次，就对和前向过程所加入的噪声一样。