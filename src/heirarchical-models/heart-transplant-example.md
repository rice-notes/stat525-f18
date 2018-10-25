# Example: Poisson + Gamma

Consider the heart transplant data we've looked at before.  Our goal
here is to accurately model the rate of success of heart transplants for *many
hospitals jointly*.

Data: For each hospital $h$:
$$
\begin{aligned}
y_h &= \text{\# deaths within 30d} \\
x_h &= \text{units of exposure (\# of surgeries)}
\end{aligned}
$$

Model:
$$
y_h | \lambda_h \sim \text{Poisson}(x_h\lambda_h), h = 1, \ldots, H
$$ 

Key Idea: Information from hospital $l$ $(x_l, \lambda_h)$ may be informative
for $\lambda_h$.

## Without Pooling

Model each $\{\lambda_h\}_h$ independently, e.g. $\lambda_h \sim
\text{Gamma}(\alpha_h, \beta_h)$ or $\hat{\lambda}_h = y_h / x_h$.  We ignore
the likely depedence between $\lambda_i$'s, and $\hat{\lambda}_h$ will be poor
if $x_h$ is small.

## Complete Pooling

Fix $\lambda_1 = \lambda_2 = \ldots = \lambda_H = \lambda$.  This ignores
hospital-hospital variablity, and puts $\hat{\lambda} = \frac{\sum y_h}{\sum
\lambda_h}$.  

## Partial Pooling and Shrinkage Factors

Let $[\lambda_h | \alpha, \mu] \sim \text{Gamma}(\alpha, \alpha / \mu)$.  Prior
mean $\mu$, variance $\mu^2/\alpha$.  Here, we share some information across
hospitals, as we use the same prior distributions for each mortality rate.

We've seen this pairing before, and since the Gamma distribution is conjugate
to the Poisson distribution, we get the following posterior:

$$
[\lambda_h | y_h, \alpha, \mu] \sim 
    \text{Gamma}(\alpha + y_h, \alpha / \mu + x_h)
$$

where the posterior mean is

$$
\begin{aligned}
E[\lambda_h | y_h, \alpha, \mu] &= \frac{\alpha + y_h}{\alpha/\mu + x_h} \\
&= (1 - k_h)\frac{y_h}{x_h} + k_h \mu \\
k_h &= \frac{\alpha}{\alpha + x_h\mu} \in [0, 1]
\end{aligned}
$$

So, we can see that $\alpha$ is effectively a mixing parameter that controls
our level of pooling.  A high value of $\alpha$ results in a $k_h$ of
approximately 1, giving us complete pooling, or a single shared mean.  A small
value results in a $k_h$ of approximately 0, giving us no pooling, or
completely independent means.
