# 有限马尔可夫决策过程

## “智能体-环境”交互接口

- 智能体：进行学习及实施决策的机器
- 环境：智能体之外与其相互作用的事物

### 交互过程

在每个时刻$t$，智能体观察到所在的环境状态的某种特征表达$S_t$，在此基础上选择一个动作$A_t$。**下一时刻**，作为其动作的结果，智能体从环境接收一个数值化的收益$R_{t+1}$

- 轨迹：$S_0,A_0,R_1,S_1,A_1,R_2,S_2,A_2,R_3,...$

### 状态和动作

- 一般来说，动作可以是任何想要做的决策，而状态则可以是任何对决策有帮助的事情

- 不同任务之间具体的状态、动作的定义差异很大

## MDP动态特性

$$
p(s',r|s,a)=\Pr\{S_t=s',R_t=r|S_{t-1}=s,A_{t-1}=a\}
$$

解释：给定当前状态s和动作a，下一状态s'和收益r的概率分布。即在状态s下执行动作a之后，下一状态s'和收益r的状态分布。

### 相关性质

#### 概率之和

$$
\sum_{s'\in\mathcal{S}}\sum_{r\in\mathcal{R}}p(s',r|s,a)=1, \forall s\in\mathcal{S}, a\in\mathcal{A(s)}
$$

#### 状态转移概率

$$
p(s'|s,a) \doteq \Pr\{S_t=s'|S_{t-1}=s, A_{t-1}=a\}=\sum_{r\in\mathcal{R}}p(s',r|s,a)
$$

#### “状态-动作”期望收益

$$
r(s,a)=\mathbb{E}[R_t |S_{t-1}=s,A_{t-1}=a]=\sum_{r\in\mathcal{R}}r\sum_{s'\in\mathcal{S}}p(s',r|s,a)
$$

## 目标和收益(Goals and Rewards)

- 收益产生在环境中，而不是在智能体中
- 强化学习的目标是最大化智能体接收到的收益累积和的概率期望值，而不是最大化当前收益
- 算法提供收益的方式必须要使智能体在最大化收益的同时实现我们的目标，否则最大化收益将没有现实意义，不能解决现实问题

## 回报和分幕(Returns and Episodes)

- 回报：$G_t \doteq R_{t+1}+R_{t+2}+\dots+R_T$
- 分幕式任务(episodic task)：$T$是随机变量，即每个时刻都有一定概率终止
- 持续性任务(continuing task)：$T=\infty$
- 带折扣回报：
$$
G_t \doteq R_{t+1}+\gamma R_{t+2}+\gamma^2 R_{t+3}+\dots=\sum_{k=0}^\infty \gamma^kR_{t+k+1}
$$

### 统一表示法

$$
G_t \doteq \sum_{k=t+1}^T \gamma^{k-t-1}R_k
$$

其中$T=\infty$和$\gamma=1$不同时成立

## 策略和价值函数

### 策略函数

- 映射关系：$\pi: \mathcal{S} \mapsto \mathcal{A}$
- 定义：$\pi(a|s) = \Pr\{A_t=a|S_t=s\}$
- 解释：给定当前状态，动作的概率分布。动作的概率分布取决于策略和当前状态

### 状态价值函数

$$
v_\pi(s) \doteq \mathbb{E}_\pi[G_t |S_t=s]=\mathbb{E}_\pi[\sum_{k=0}^\infty R_{t+k+1}|S_t=s], \forall s\in\mathcal{S}
$$

表达了一个状态的价值

### 动作价值函数

$$
q_\pi(s,a) \doteq \mathbb{E}_\pi[G_t |S_t=s,A_t=a]=\mathbb{E}_\pi[\sum_{k=0}^\infty R_{t+k+1}|S_t=s,A_t=a]
$$

表达了一个“状态-动作”二元组的价值

### 贝尔曼方程

$$
\begin{aligned}
v_\pi(s) &\doteq \mathbb{E}_\pi[G_t|S_t=s] \\
&= \mathbb{E}_\pi[R_{t+1}+\gamma G_{t+1}|S_t=s] \\
&= \sum_{a}\pi(a|s)\sum_{s'}\sum_{r}p(s',r|s,a)[r + \gamma\mathbb{E}_\pi[G_{t+1} |S_{t+1}=s']] \\
&= \sum_{a}\pi(a|s)\sum_{s'}\sum_{r}p(s',r|s,a)[r + \gamma v_\pi(s')]]
\end{aligned}
$$

$$
\begin{aligned}
q_\pi(s,a) &\doteq \mathbb{E}[R_{t+1} + \gamma \sum_{a'} \pi(a'|S_{t+1})q_\pi(S_{t+1},a') | S_t=s, A_t=a] \\
&= \sum_{s',r}p(s',r|s,a)[r + \gamma \sum_{a'} \pi(a'|s')q_\pi(s',a')]
\end{aligned}
$$

表达了状态价值和后继状态价值之间的关系

### 回溯图

![回溯图](image/backup.png)

空心圆表示一个状态，实心圆表示一个“状态-动作”二元组，“状态-动作”二元组到下一个状态之间产生收益r. 分支代表所有可能，考虑各分支的过程即为求期望/积分的过程。

## 最优策略和最优价值函数

### 定义

$$
\begin{aligned}
v_*(s) &\doteq \max_\pi v_\pi(s), \\
q_*(s,a) &\doteq \max_\pi q_\pi(s,a).
\end{aligned}
$$

### 最优贝尔曼方程

$$
\begin{aligned}
v_*(s) &= \max_{a \in \mathcal{A}(s)} q_{\pi_*}(s,a) \\
&= \max_a \mathbb{E}_{\pi_*}[G_t | S_t=s, A_t=a] \\
&= \max_a \mathbb{E}_{\pi_*}[R_{t+1}+\gamma G_{t+1} | S_t=s, A_t=a] \\
&= \max_a \mathbb{E}_{\pi_*}[R_{t+1}+\gamma v_*(S_{t+1}) | S_t=s, A_t=a] \\
&= \max_a \sum_{s',r}p(s',r|s,a)[r + \gamma v_*(s')].
\end{aligned}
$$

$$
\begin{aligned}
q_*(s,a) &\doteq \mathbb{E}[R_{t+1} + \gamma \max_{a'}q_*(S_{t+1},a') | S_t=s, A_t=a] \\
&= \sum_{s',r}p(s',r|s,a)[r + \gamma \max_{a'}q_*(s', a')]
\end{aligned}
$$

与贝尔曼方程相比，最优贝尔曼方程不再考虑所有的动作，而只考虑最优的动作，将求期望变为求最大值。
