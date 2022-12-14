# 12 - Markov chain Monte Carlo

## Metropolis-Hastings

----------
**Algorithm 12.1.1. (Metropolis-Hastings)** 
Let $s = (s _{1},\dots, s _{M})$ be a desired stationary distribution on state space $\left\{1, \dots, M \right\}$. Assume that $s _{i} >0$ for all $i$. Suppose that $P = (p _{ij})$ is the transition matrix for a Markov chain on state space $\left\{1, \dots,M \right\}$. Intuitively, $P$ is a Markov chain that we know how to run but that doesn't have the desired stationary distribution.

Start at any state $X _{0}$ (chosen randomly or deterministically), and suppose that the new chain is currently at $X _{n}$. To make one move of the new chain, do the following.
1. If $X _{n} = i$, propose a new state $j$ using the transition probabilities in the $i$th row of the original transition matrix $P$.
2. Compute the acceptance probability
$$
\begin{equation*}
	a _{ij} = \min \left(\frac{s _{j}p _{ji}}{s _{i}p _{ij}}, 1\right).
\end{equation*}
$$
3. Flip a coin that lands Heads with probability $a _{ij}$.
4. If the coin lands Heads, accept the proposal (i.e., go to $j$), setting $X _{n +1} = j$. Otherwise, reject the proposal (i.e., stay  at $i$), setting $X _{n+1}= i$.

**Example 12.1.3. (Zipf distribution simulation)**
Let $M \geq 2$ be an integer. An r.v. $X$ has the Zipf distribution with parameter $a>0$ if its PMF is 
$$
\begin{equation*}
	\mathbb{P}(X = k) = \frac{\frac{1}{k ^{a}}}{\sum_{j=1}^{M}\left(\frac{1}{j ^{a}}\right)}.
\end{equation*}
$$
for $k = 1,2,\dots,M$ (and 0 otherwise).

Create a Markov chain $X _{0}, X _{1}, \dots$ whose stationary distribution is the Zipf distribution, and such that $|X _{n+1}- X _{n}|\leq 1$.

`Solution`

Since $|X _{n}- X _{n-1}|\leq 1$ for all $n$, then at each step we can move in the space state only one step forward or backwards. A natural candidate for this chain is the birth/death Markov process:
$$
\begin{align*}\scriptsize p _{ij _{(i \neq 1,M)}}=
    & \scriptsize \begin{aligned} & \begin{cases}
		p & \text{if } j = i+1\\
		1-p & \text{if } j = i-1\\ 
		0 & \text{if } j = i\\
  \end{cases}\\
  \end{aligned}
    & \hskip 1em &
  \begin{aligned}\scriptsize p _{i,j _{(i = 1)}}=
  & \scriptsize\begin{cases}
		 p & \text{,if } j = i+1\\
		0 & \text{,if } j = i-1\\ 
		1-p & \text{,if } j = i\\
  \end{cases} \\
  \end{aligned}
    & \hskip 1em &
  \begin{aligned} \scriptsize p _{i,j _{(i = M)}}=
  & \scriptsize\begin{cases}
		0 & \text{,if } j = i+1\\
		1-p & \text{,if } j = i-1\\ 
		p & \text{,if } j = i\\
  \end{cases} \\[3.3ex]
  \end{aligned}
\end{align*}
$$
We will use the above Markov chain as our proposal. Then, let $X _{0}$ be any starting state, and generate a chain $X _{0}, X _{1},\dots$ as follows. If the chain is currently at state $i$, then: 
1. Generate a proposal state $j$ according to the proposal chain $P$.
2. Accept the proposal with probability $a _{ij}=\min \left(\frac{i ^{a}}{j ^{a}},1\right)$. If the proposal is accepted, go to $j$; otherwise, stay at $i$.

**Example 12.1.4 (Beta Simulation)** 
Suppose that we want to generate $W \sim Beta (a,b)$.

**(a)** How can we generate $W$ exactly if $a$ and $b$ are positive integers?

**(b)** How can we generate $W$ which is approximately $Beta (a,b)$ if $a$ and $b$ are any positive real numbers, with the help of a Markov chain on the state space $(0,1)$.

`Solution`

**(a)** If $X \sim Gamma (a,1)$ and $Y \sim Gamma (b,1)$ are independent, then $\frac{X}{X+Y} \sim Beta (a,b)$. So if we can simulate the Gamma distribution, then we can simulate the Beta distribution.

**(b)** Let's use the Metropolis-Hastings algorithm. A simpler proposal chain we have available consists of independent $\mathcal{U}(0,1)$ r.v.s. That is, the proposed state on the interval $(0,1)$ is always a fresh $\mathcal{U}(0,1)$, independent of the current state. The resulting Metropolis-Hastings chain is called an independence sampler.

Let $W _{0}$ be any starting state, and generate a chain $W _{0}, W _{1}, \dots$ as follows. If the chain is currently at state $w$, then:
1. Generate a proposal $u$ by drawing a $\mathcal{U}(0,1)$ r.v.
2. Accept the proposal with probability $\min \left(\frac{u ^{a-1}(1-u)^{b-1}}{w ^{a-1}(1-w)^{b-1}},1\right)$. If the proposal is accepted, go to $u$; otherwise, stay at $w$.
3. third

