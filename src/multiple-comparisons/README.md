# Multiple Comparisons

Overview / Review of multiple testing (comparing models):

Test $H_{\sigma_j} : \theta_j = 0$ for $j = 1, \ldots, T$.  Note that we might
also be interested in contrasts $(H_{\sigma_{jk}} : \theta_j - \theta_k = 0)$

Say we test $M$ hypotheses at the $\alpha$ level:

$$
P(\text{reject } H_{\theta_j} | H_{\theta_j}) = \alpha
$$

If tests are independent, Family Wise Error Rate (FWER) is simply the
probability that we experience at least one false positive, or 1 less the
probability we experience none, which comes out to $1 - (1 - \alpha)^M$, where
$M$ is our number of tests.  

#### Bonferroni Correction

Put $\alpha^* = \alpha / M$.  For depdendent testing, FWER $\le M\alpha^*$.

Consequences:  
* lose power
* wider confidence intervals
* as $M$ and $J$ increase, your estimates become more conservative

#### Key Points

* MC adjustments __widen__ intervals but keep __means fixed__
* partial pooling (heir. models, mixed models) __shift means__ toward each
  other and __can shrink__ intervals

#### Comments

* type "S" (sign) errors: WLOG, estimate $\theta_j > 0$ when $\theta_j < 0$
* type "M" (magnitude) errors: WLOG, estimate $|\theta_j|$ small when
  $|\theta_j|$ large

shrinkage of heirarchical models help with both of these errors.
