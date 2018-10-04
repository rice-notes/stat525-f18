# Metropolis Hastings

Setting:  
* cannot simulate from $p(\theta|y)$
* can evaluate $q(\theta|y) = p(y|\theta)p(\theta) \propto p(\theta|y)$
* can simulate from $J(\theta | \theta^{s-1})$

Intution: $J(\theta | \theta^{s-1})$ is transition probability from $\theta
\rightarrow \theta^{s-1}$.

### Metropolis Algorithm

The transition kernel $J$ must be symmetric, i.e.  
$$
J(X | Y) = J(Y | X)
$$

1. Initialize $\theta^0$
2. For $s = 1, \ldots, S$ do:
    1. Sample $\theta^* \sim J(\theta^* | \theta^{s-1})$
    2. Compute $r = \frac{P(\theta^* | y)}{P(\theta^{s-1} | y)} =
       \frac{q(\theta^* | y)}{q(\theta^{s-1} | y)}$
    3. Set 

$$
\theta^s = \begin{cases} 
    \theta^* & \text{with prob } min(r, 1) \\
    \theta^{s-1} & \text{else}
\end{cases}
$$


### Observations 

* Draw $\theta^*$ depends on $\theta^{s-1}$, hence MCMC
* $r > 1$ when $p(\theta^* | y) > p(\theta^{s-1} | y)$
    * $\theta^*$ increases posterior density, accept and move towards mode
* $r < 1$ else:
    * $\theta^*$ decreases posterior density
    * move away from mode with probability $r$ (accept $\theta^*$)
    * stay at current position with probability $1-r$


## Metropolis Hastings Algorithm

1. Initialize $\theta^0$
2. For $s = 1, \ldots, S$ do:
    1. Sample $\theta^* \sim J(\theta^* | \theta^{s-1})$
    2. Compute $r = 
        \frac{P(\theta^* | y)J(\theta^* | \theta^{s-1})}
             {P(\theta^{s-1} | y)J(\theta^{s-1} | \theta^{s-1})} =
        \frac{q(\theta^* | y)J(\theta^* | \theta^{s-1})}
             {q(\theta^{s-1} | y)J(\theta^{s-1} | \theta^{s-1})}$
    3. Set 
        $$
        \theta^s = \begin{cases} 
            \theta^* & \text{with prob } min(r, 1) \\
            \theta^{s-1} & \text{else}
        \end{cases}
        $$


## MCMC Foundations

Transition kernel of a Markov process:

$$
\begin{aligned}
K(x_t, A) &= P(X_{t+1} \in A | X_1 = x_1, \ldots, X_t = x_t) \\
&= \int_A K(x_t, y)
\end{aligned}
$$

Invariant Distribution:  
$$
\begin{aligned}
\pi(A) &= \int_A K(x, y) \pi(x) dy
\end{aligned}
$$

Suppose that $K(x, dy) = p(x, y)dy + r(x)\delta_x(dy)$ for some function

### Takeaways

* Find a kernel $K(\cdot, \cdot)$ of Markov chain with stationary 
  distribution $p(\theta|y)$
* MH: Given **any** $J(\cdot | \cdot)$ with correct support, construct kernel indirectly (detailed balance)
* only need to choose $J(\cdot | \cdot)$

## Special Cases of MH

### Random Walk Metropolis

### Independence Sampler

Motivation: Note that we always travel closer to the mode if we can.  What if
we always travel closer to the mode but not quickly enough?  Very slow progress
that doesn't explore much of the posterior density.

For some density $g(\cdot)$, $J(\theta^* | \theta^{s-1}) = g(\theta^*)$ so that
draws of $\theta^*$ are independent of $\theta^{s-1}$.

Example: $J(\theta^* | \theta^{s-1}) = N(\theta^*; \hat{theta}, \hat{\Sigma})$

### Gibbs Sampler
