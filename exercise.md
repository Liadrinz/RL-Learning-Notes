# RL Exercise

## Chapter 5

### 5.13

$$
\begin{aligned}
\mathbb{E}(\rho_{t:T-1}R_{t+1})&=\mathbb{E}(
    \frac{\pi(A_t|S_t)}{b(A_t|S_t)}
    \frac{\pi(A_{t+1}|S_{t+1})}{b(A_{t+1}|S_{t+1})}
    \frac{\pi(A_{t+2}|S_{t+2})}{b(A_{t+2}|S_{t+2})}
    \cdots
    \frac{\pi(A_{T-1}|S_{T-1})}{b(A_{T-1}|S_{T-1})}
    R_{t+1}
)\\
&(
    \frac{\pi(A_{t+1}|S_{t+1})}{b(A_{t+1}|S_{t+1})}\perp
    \frac{\pi(A_{t+2}|S_{t+2})}{b(A_{t+2}|S_{t+2})}\perp
    \cdots\perp
    \frac{\pi(A_{T-1}|S_{T-1})}{b(A_{T-1}|S_{T-1})}
)\\
&=\mathbb{E}(\frac{\pi(A_t|S_t)}{b(A_t|S_t)}R_{t+1})
  \mathbb{E}(\frac{\pi(A_{t+1}|S_{t+1})}{b(A_{t+1}|S_{t+1})})
  \mathbb{E}(\frac{\pi(A_{t+2}|S_{t+2})}{b(A_{t+2}|S_{t+2})})
  \mathbb{E}(\frac{\pi(A_{T-1}|S_{T-1})}{b(A_{T-1}|S_{T-1})})\\
&=\mathbb{E}(\frac{\pi(A_t|S_t)}{b(A_t|S_t)}R_{t+1})\\
&=\mathbb{E}(\rho_{t:t}R_{t+1})
\end{aligned}
$$

## Chapter 7

### 7.1

时序差分误差

$$
\delta_t \triangleq R_{t+1}+\gamma V(S_{t+1})-V(S_t)
$$

蒙特卡洛误差

$$
\begin{aligned}
G_t-V(S_t)&=R_{t+1}+\gamma G_{t+1}-V(S_t)\\
&=R_{t+1}+\gamma G_{t+1}-V(S_t)+\gamma V(S_{t+1})-\gamma V(S_{t+1})\\
&=(R_{t+1}+\gamma V(S_{t+1})-V(S_t))+\gamma(G_{t+1}-V(S_{t+1}))\\
&=\delta_t+\gamma(G_{t+1}-V(S_{t+1}))\\
&=\delta_t+\gamma \delta_{t+1}+\gamma^2(G_{t+2}-V(S_{t+2}))\\
&=\delta_t+\gamma \delta_{t+1}+\cdots+\gamma^{T-t-1}\delta_{T-1}+\gamma^{T-t}(G_T-V(S_T))\\
&=\delta_t+\gamma \delta_{t+1}+\cdots+\gamma^{T-t-1}\delta_{T-1}+\gamma^{T-t}(0-0)\\
&=\sum_{k=t}^{T-1}\gamma^{k-t}\delta_{k}\\
\end{aligned}
$$

n步时序差分误差

$$
\begin{aligned}
G_{t:t+n}-V_{t+n-1}(S_t)&=R_{t+1}+\gamma G_{t+1:t+n}-V(S_t)\\
&=R_{t+1}+\gamma G_{t+1:t+n}-V(S_t)+\gamma V(S_{t+1})-\gamma V(S_{t+1})\\
&=(R_{t+1}+\gamma V(S_{t+1})-V(S_t))+\gamma(G_{t+1:t+n}-V(S_{t+1}))\\
&=\delta_t+\gamma(G_{t+1:t+n}-V(S_{t+1}))\\
&=\delta_t+\gamma \delta_{t+1}+\gamma^2(G_{t+2:t+n}-V(S_{t+2}))\\
&=\delta_t+\gamma \delta_{t+1}+\cdots+\gamma^{n-1}\delta_{t+n-1}+\gamma^n(G_{t+n:t+n}-V(S_{t+n}))\\
&=\delta_t+\gamma \delta_{t+1}+\cdots+\gamma^{n-1}\delta_{t+n-1}+\gamma^n(0-0))\\
&=\sum_{k=t}^{t+n-1}\gamma^{k-t}\delta_{k}\\
\end{aligned}
$$

### 7.4

由7.1可知

$$
\begin{aligned}
G_{t:t+n}-Q(S_t,A_t)&=R_{t+1}+\gamma G_{t+1:t+n}-Q(S_t,A_t)\\
&=R_{t+1}+\gamma G_{t+1:t+n}-Q(S_t,A_t)+\gamma Q(S_{t+1},A_{t+1})-\gamma Q(S_{t+1},A_{t+1})\\
&=[R_{t+1}+\gamma Q(S_{t+1},A_{t+1})-Q(S_t,A_t)]+\gamma(G_{t+1:t+n}-Q(S_{t+1},A_{t+1}))\\
&(令\delta_t=[R_{t+1}+\gamma Q(S_{t+1},A_{t+1})-Q(S_t,A_t)]) \\
&=\delta_t+\gamma(G_{t+1:t+n}-Q(S_{t+1},A_{t+1}))\\
&=\delta_t+\gamma \delta_{t+1}+\gamma^2(G_{t+2:t+n}-Q(S_{t+2},A_{t+2}))\\
&=\delta_t+\gamma \delta_{t+1}+\cdots+\gamma^{n-1}\delta_{t+n-1}+\gamma^n(G_{t+n:t+n}-Q(S_{t+n},A_{t+n}))\\
&=\delta_t+\gamma \delta_{t+1}+\cdots+\gamma^{n-1}\delta_{t+n-1}+\gamma^n(0-0))\\
&=\sum_{k=t}^{t+n-1}\gamma^{k-t}\delta_{k}\\
&=\sum_{k=t}^{t+n-1}\gamma^{k-t}[R_{t+1}+\gamma Q(S_{k+1},A_{k+1})-Q(S_k,A_k)]\\
G_{t:t+n}&=Q(S_t,A_t)+\sum_{k=t}^{t+n-1}\gamma^{k-t}[R_{t+1}+\gamma Q(S_{k+1},A_{k+1})-Q(S_k,A_k)]
\end{aligned}
$$

### 7.6

$$
\begin{aligned}
\mathbb{E}(\gamma \bar{V}_{h-1}(S_{t+1}))
&=\gamma \mathbb{E}(\bar{V}_{h-1}(S_{t+1})) \\
&=\gamma \mathbb{E}(\sum_{a}\pi(a|S_{t+1})Q_{h-1}(S_{t+1}, a)) \\
&=\gamma \sum_{a} \mathbb{E}(\pi(a|S_{t+1})Q_{h-1}(S_{t+1}, a))
\end{aligned}
$$

## Chapter 12

### 12.1

$$
\begin{aligned}
G_t^\lambda &= (1-\lambda)\sum_{n=1}^\infty \lambda_{n-1}(R_{t+1}+G_{t+1:t+n})\\
&=(1-\lambda)(\frac{1}{1-\lambda}R_{t+1}+\sum_{n=1}^{\infty}\lambda^{n-1}G_{t+1:t+n})\\
&=R_{t+1}+(1-\lambda)\sum_{n=1}^{\infty}\lambda^{n-1}G_{t+1:t+n}\\
&=R_{t+1}+G_{t+1}^\lambda
\end{aligned}
$$

### 12.3

$$
\begin{aligned}
G_t^\lambda - \hat{v}(S_t, \boldsymbol{w}_t) &=
R_{t+1} + \gamma G_{t+1}^\lambda - \hat{v}(S_t, \boldsymbol{w}_t) + \gamma \hat{v}(S_{t+1}, \boldsymbol{w}_t) - \gamma \hat{v}(S_{t+1}, \boldsymbol{w}_t)\\
&= \delta_t + \gamma(G_{t+1}^\lambda - \hat{v}(S_{t+1}, \boldsymbol{w}_t))\\
&= \delta_t + \gamma \delta_{t+1} + \gamma^2(G_{t+2}^\lambda - \hat{v}(S_{t+2}, \boldsymbol{w}_t))\\
&= \sum_{k=t}^\infty \gamma^{k-t}\delta_{k}
\end{aligned}
$$