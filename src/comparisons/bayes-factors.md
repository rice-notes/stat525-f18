# Bayes Factors

Goal: Compare two models (or hypotheses) $H_0$ and $H_1$:

$$
H_0 : \theta \in H_0, H_1 : \theta \in H_1
$$

$H_0$ and $H_1 = H \setminus H_0$ are partitions of the parameter space $H$.  

Prior Odds Ratio:  

$$
\frac{\pi_0^{\text{prior}}}{\pi_1^{\text{prior}}}
= \frac{P(\theta \in H_0)}{
    P(\theta \in H_1)} 
= \frac{\int_{H_0} P(\theta) d\theta}{
    \int_{H_1} P(\theta) d\theta)} 
$$

Posterior Odds Ratio:  

$$
\frac{\pi_0^{\text{post}}}{\pi_1^{\text{post}}}
= \frac{p(\theta \in H_0|y)}{
    p(\theta \in H_1|y)} 
= \frac{\int_{H_0} p(\theta|y) d\theta}{
    \int_{H_1} p(\theta|y) d\theta)} 
= \frac{\int_{H_0} p(y|\theta)p(\theta) d\theta}{
    \int_{H_1} p(y|\theta)p(\theta) d\theta)} 
$$

Bayes Factor:  ratio of posterior odds to the prior odds  

$$
BF(H_0, H_1) 
= \frac{\text{posterior odds}}{\text{prior odds}} 
= \frac{\pi_0^{\text{prior}} / \pi_1^{\text{prior}}}{\pi_0^{\text{post}} / \pi_1^{\text{post}}}
= \frac{p(y|H_0)}{p(y|H_1)}
$$



### Comments

1. BF shows how the data update our prior beliefs of hypothesis
1. general hypotheses are allowed
1. can evaluate evidence in favor of a hypothesis; determine how much it
   contributes or doesn't
1. can incorporate ouside information
1. depends on marginal likelihood, generally difficult to compute, additionally
marginal likelihoods can be very sensitive 

## Good Examples

1. Suppose $H_0$ and $H_1$ have some parameterization with parameter $\theta
   \in H = \{\theta_0, \theta_1\}$, with $H_0 : \theta = \theta_0$ and $H_1 :
   \theta = \theta_1$.  Then our Bayes Factor is simply the likelihood ratio
   given our parameter.

1. One-sided Test of Normal Means:  $y_i | \theta, \sigma \sim N(\theta,
   \sigma^2)$.  

## Bad Examples

From BDA p. 183, 7.8 Ex. 4  

1. Point null: $H_0 : \theta = \theta_0$, $H_1 : \theta \ne \theta_0$.
   Difficult to specify $p(H_0)$, especially if $\theta$ is continuous, since
   we are (incompletely) sampling the posterior.  

### Improper Priors

Result in an undefined BF.

## Model Selection: Information Criteria

Deviance $D(\theta) = -2 \log{p(y|\theta)} + 2 \log{h(y)}$

1. $y_i \sim N(\theta, \sigma^2) \implies D(\theta)$ gives SSE
1. Useful for comparing models, but not in isolation.
1. useful for GLMs 
1. mcmc diagnostic?

### BIC

$$
\begin{aligned}
BIC(H_j) &= -2 \max_{\theta \in H_j}\log{p(y|\theta)} + \log{n}p_j \\
&= D(\hat{\theta}_{\text{MLE}}) + \log{n}p_j
\end{aligned}
$$

However, for large $n$, we have that $BF(H_i, H_j) \approx BIC(H_0) -
BIC(H_1)$.

### AIC

$$
\begin{aligned}
BIC(H_j) &= -2 \max_{\theta \in H_j}\log{p(y|\theta)} + 2 * p_j \\
&= D(\hat{\theta}_{\text{MLE}}) + 2 * p_j
\end{aligned}
$$

### DIC

Idea: $D(\theta)$ is a function of $\theta$, so compute posterior expectation.

$$
\bar{D} = E[D(\theta)|y] \approx \frac{1}{S}\sum_s D(\theta^s)
$$

Modification: penalize D with model complexity.  Two approaches:  

#### Spiegelhalter (2002)

Let $\hat{\theta}$ be an estimator of $\theta^* =$ true value. Put
$$
\begin{aligned}
d(y, \hat{\theta}, \theta^*) 
    &= -2\log{p(y|\theta^*)} + 2\log{p(y|\hat{\theta})} \\
    &= D(\theta^*) - D(\hat{\theta})
\end{aligned}
$$

Let $\hat{\theta} = E[\theta | y]$

$$
p_D = E[d(y, \theta, \hat{\theta}) | y] = E[D(\theta) - D(\hat{\theta}) | y] = \bar{D} - D(\hat{\theta})
$$

Intution: posterior mean of the deviance minus the deviance of the posterior
mean.

Then, $DIC = D(\hat{\theta}) + 2p_D = \bar{D} + p_D$.

* easy to compute
* linear models w/ uniform priors: $p_D = p$
* but $p_D$ can be negative!

#### Gelman (2004)

2. $p_D = \frac{1}{2}\text{var}\{D(\theta)\}$
