# Example: Gaussian case (8 schools data)

Suppose we have $J$ experiments and $n_j$ replicates in each.  

Model:

$$
\begin{aligned}
  \bar{y}_{\cdot j} | \theta_j, \sigma_j &\sim \text{Normal}(\theta_j, \sigma_j^2) \\
  \theta_j | \mu, \sigma_\theta &\sim \text{Normal}(\mu, \sigma_\theta^2)
\end{aligned}
$$

## Likelihood and Conditionals



## Results

$$
\begin{aligned}
  E[\theta_j | \ldots] &= (1 - \kappa_{\theta_j})\bar{y}_{\cdot j} + \kappa_{\theta_j}\mu \\
  \kappa_{\theta_j} &= \frac{\sigma_j^2}{\sigma_j^2 + \sigma_\theta^2}
\end{aligned}
$$

## Shrinkage Factor

Here again we have a shrinkage factor $\kappa_{\theta_j}$ that controls how
much data we want to share between our schools.  Put differently, it controls
whether we're performing complete pooling ($E[\theta_j | \ldots] \rightarrow
\mu$) or no pooling ($E[\theta_j | \ldots] \rightarrow \bar{y}_{\cdot j}$).

Of course, our shrinkage factor is wholly controlled by our value of
$\sigma_\theta$, so we'd really like to place a prior on $\sigma_\theta$.

## Reasonable Priors for $\sigma_\theta$

Our choice really matters when $J$ is rather small or $\sigma_\theta$ is small.
Improper priors as the limit of proper priors:

* uniform: $\sigma_\theta \sim \text{Uniform}(0, A)$, let $A \rightarrow
  \infty$
* inverse-gamma: $\sigma_\theta^{-2} \sim \text{Gamma}(\epsilon, \epsilon)$,
  let $\epsilon \rightarrow 0$

So we begin with a proper distribution, and then go to a limit which decreases
how informative our prior is.  Do we get proper posterior distributions under
these priors?  For the above uniform prior, it is proper so long as $J \ge 3$,
but the inverse-gamma prior is not.

1. inverse-gamma: No!  $\epsilon \rightarrow 0$ results in an improper
   posterior.  Additionaly, if $\sigma_\theta$ may be small, choice of
   $\epsilon$ can be highly informative.
2. $\sigma_\theta \sim \text{Unif}(0, A)$: Yes! So long as $J \ge 3$.  
    * however, miscalibration towards larger values
    * finite integral near 0
3. $\sigma_\theta^2 \sim \text{Unif}(0, A)$
    * proper but requires more groups ($J \ge 9$)
    * more extreme 
4. $\sigma_\theta \sim \text{Half-Cauchy}(0, A)$ or $t$ or normal
    * conditionally conjugate
    * $p(\sigma_\theta) = z / (\pi A [1 + ( \frac{\sigma_\theta}{A} )^2])$
    * as $A \rightarrow \infty$, the distribution becomes uniform


### Implementation for $\sigma_\theta \sim \text{Unif}(0, A)$

Reparameterization: 

$p(\sigma_\theta) \propto 1$

$$
\begin{aligned}
  p(\sigma_\theta^2) &\propto 1 \det{\frac{d}{d(\sigma_\theta^2)}\sqrt{\sigma_\theta^2}} \\
    &\propto \frac{1}{\sqrt{\sigma_\theta^2}} \\
    &= \frac{1}{\sigma_\theta}
\end{aligned}
$$

Let $\eta_\theta = \sigma_\theta^{-2}$

$$
\begin{aligned}
p(\eta_\theta) &\propto \eta^0.5 \det{\frac{d}{d\eta_\theta}\eta_\theta^{-1}} \\
    &\propto \eta_\theta^{0.5}\eta_\theta^{-2} \\
    &= \eta_\theta^{-1.5} \\
    &\sim \text{Gamma-ish}(\alpha = -0.5, \beta = 0)
\end{aligned}
$$

To constrain $0 \le \sigma_\theta \le A$, we constrain $\eta_\theta =
\sigma_\theta^{-2} \ge \frac{1}{A^2}$.

Full Conditional:

$$
[\eta_\theta | y, \theta_j, \ldots] \sim \text{Gamma}(-0.5 + \frac{J}{2}, )
$$
