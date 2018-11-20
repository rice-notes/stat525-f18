# Generalized Linear Models

Two assumptions of linear gaussian models:  
1. Linear: $E[y | X, \beta] = X\beta$
2. Gaussian: $y = X\beta + \epsilon$, $\epsilon \sim N(0, \Sigma)$

Sometimes limiting, GLMs are a bit more general:
1. Linear Predictor: $\eta = X\beta$
2. Link Function: $g(\cdot)$: $\mu = E[y | X] = g^{-1}(X\beta)$
    * so that $g(\mu) = X\beta$
    * maps expected value to the real line
    * $g$ must be linear and monotonic
3. Distribution for $y$: mean $\mu$, dispersion $\phi$
    * $\phi = 1$ for binomial, poisson

## Example: rate of capture

$y_i =$ rate of capture of prey by hunting animal
$z_i =$ density of prey

$y_i$ increases with $z_i$, but will taper off


Model:  
$$
\begin{aligned}
  E[y] = mu_i &= \frac{\alpha_0z_i}{\alpha_1 + z_i} \\
  \alpha_0 &= \text{max rate of capture} \\
  g(\mu) = 
\end{aligned}
$$

## Example: Binary (Logistic / Probit)

$y_i \in \{0, 1\}$

Common Model: $\pi_i = \mu_i / n_i =$ probability of success

$y_i | \pi_i \sim Binom(n_i, \pi_i)$

Logistic Regression: $g(\pi_i) = log(\pi_i / (1 - \pi_i)) = x_i \beta$

---

Probit Regression:

$\pi_i \in \{0, 1\}$, need $g(\pi_i) \in \mathbb{R}$, use Inverse CDF
$g(\pi_i) = \Phi^{-1}(\pi_i) \implies pi_i = \Phi(x_i\beta)$

## Example: Count Data

Poission Regression:  
$$
\begin{aligned}
  y_i | \mu_i &\sim \text{Poisson}(\mu_i) \\
  g(\mu_i) &= \log(\mu_i) = x_i\beta
\end{aligned}
$$

Note: $var(y_i|\mu_i) = \mu_i = \exp{x_i\beta}$

* variance varies with your estimate
* lots of zeros usually can cause issues

## Example: Negative Binomial

