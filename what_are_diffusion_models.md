# __What are diffusion models？__
This article is a supplement to [What are diffusion models?](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/#connection-with-stochastic-gradient-langevin-dynamics)
## **1.Derivation of the formula $x_t=\sqrt{\bar\alpha_t}x_0+\sqrt{1-\bar\alpha_t}\epsilon$**
Given: $x_t=\sqrt{\alpha_t}x_{t-1}+\sqrt{1-\alpha_t}\epsilon$，where $\epsilon\sim\mathcal{N}(0,I)$.

If$$x_t=\sqrt{\alpha_t...\alpha_k}x_{k-1}+\sqrt{1-\alpha_t...\alpha_k}\epsilon$$

then we have:
$$
\begin{aligned}
x_t &= \sqrt{\alpha_t...\alpha_k}(\sqrt{\alpha_{k-1}}x_{k-2}+\sqrt{1-\alpha_{k-1}}\epsilon_1)+\sqrt{1-\alpha_t...\alpha_k}\epsilon_2
\\&= \sqrt{\alpha_t...\alpha_k\alpha_{k-1}}x_{k-2}+\sqrt{\alpha_t...\alpha_k(1-\alpha_{k-1})}\epsilon_1+\sqrt{1-\alpha_t...\alpha_k}\epsilon_2
\\&= \sqrt{\alpha_t...\alpha_k\alpha_{k-1}}x_{k-2}+\sqrt{1-\alpha_t...\alpha_k\alpha_{k-1}}\epsilon
\end{aligned}
$$
All instances of $\epsilon$ follow a standard Gaussian distribution.For convenience, they are only numbered for distinction when they appear simultaneously.

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

## **3.对$L_{simple} = \mathbb{E}_{t,x_0,\epsilon}\left[\| \epsilon - \epsilon_\theta(x_t, t)\|^2\right]$的理解**
从数据集中随机抽一张清晰的图片，这张图片就是$x_0$，它是一个具体的向量，比如尺寸为$[3,64,64]$。

从前向过程中以 $t$ 为强度生成一个尺寸为$[3,64,64]$的随机噪声张量，这个噪声就是 $\epsilon$ 。我们完全知道它的每一个数值。

将 $t$ 和 $x_t$ 作为输入，经过神经网络预测出一个噪声张量 $\epsilon_\theta$ 。

反复进行，从而达到目标。

## **4.流程**
生成真实噪声 $\epsilon$ ，并记录

训练时：

(1)输入：$(x_t,t)$

(2)输出：$\epsilon_{\theta}(x_t,t)$

(3)将输出的 $\epsilon_{\theta}$ 与我们已知的噪声 $\epsilon$ 进行比较，计算损失，更新模型。

生成时：

(1)input: $(x_t,t)$

(2)output: $\epsilon_{\theta}(x_t,t)$

(3)substitute the output $\epsilon_{\theta}$ into the reverse process formula to get $x_{t-1}$ as input for the next step.
