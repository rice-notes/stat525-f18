# Estimators



## Rao-Blackwellization

Consider some statistic $\theta$ that we compute over a specific chain. The
estimate for that statistic on the posterior distribution can be written as a
mean of that statisic over all chains:  
$$
\hat{\theta}^{\text{sim}}_1 = \frac{1}{S} \sum_{s=1}^S \theta^s_1
$$

We can reduce the variance of this estimator if we use the conditional 
expectation of this estimator:  
$$
\hat{\theta}^{\text{RB}}_1 = \frac{1}{S} \sum_{s=1}^S E\left[\theta_1 | y, \theta_2=\theta_2^{s-1} \right]
$$

### Example

Setup:  
$$
\begin{aligned}
y_i | \mu, \tau &\sim N(\mu, \tau^{-1}) \\
\mu &\sim N(\mu_0, \tau_0^{-1}) \\
\tau &\sim \text{Gamma}(\alpha, \beta)
\end{aligned}
$$

Full Conditionals:  
$$
\begin{aligned}
[ \mu | y, \tau ] &\sim N(Q^{-1}\ell, Q^{-1}) \\
[ \tau | y, \mu ] &\sim \text{Gamma}(
    \alpha + \frac{n}{2}, \beta + \frac{1}{2}\sum_{i=1}^n (y_i - \mu)^2) \\
Q &= n\tau + \tau_0 \\
\ell &= n\tau\bar{y} + \mu_0\tau_0
\end{aligned}
$$

#### RB Estimator

This is just the average of the conditional expectation of each of our
parameters over all chains.

Conditional Expectations:  
$$
\begin{aligned}
E[ \mu | y, \tau = \tau^{s-1}] &\sim Q^{-1}_s\ell_s \\
E[ \tau | y, \mu = \mu^s] &\sim \frac{
    \alpha + \frac{n}{2}}{
    \beta + \frac{1}{2}\sum_{i=1}^n (y_i - \mu^s)^2}
\end{aligned}
$$

RB Estimators:  
$$
\begin{aligned}
\hat{\mu}^{\text{RB}} &= \frac{1}{S} \sum_{s=1}^S E\left[\mu | y, \tau=\tau^{s-1} \right] \\
\hat{\tau}^{\text{RB}} &= \frac{1}{S} \sum_{s=1}^S E\left[\tau | y, \mu=\mu^s \right]
\end{aligned}
$$

### Comments

Consider transformations of variables, e.g. $\psi = h(\theta_1)$.  It's
straightforward to compute a simulation esimate for $\psi$, just apply
$h(\cdot)$ to each sample of $\theta_1$ and average.  However, we might not
have a simple conditional distribution for $\psi$.  In the above example, if
we're interested in $\sigma$ rather than $\tau$, we can squint and see that we
can use an Inverse-Gamma for $\sigma$, but in general this will be challenging.
For linear (can this be relaxed to monotonic?) functions, we can simply swap
the expectation and the transformation, e.g. $h(E[\theta | \ldots ]) =
E[h(\theta) | \ldots ]$, but in general this is not the case.
