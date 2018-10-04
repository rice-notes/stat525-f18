# Gibbs Sampler

* $$\theta \sim p(\theta | y)$$, $$\theta = (\theta_1, \ldots, \theta_B)$$
* $$p(\theta_b | y, \theta_{-b})$$ (conditional less $$\theta_b$$)

Want to sample from the posterior distribution, $$p(\theta | y)$$, however the
distribution is unknown.  

1. Initialize $$\theta^0 = (\theta^0_1, \ldots, \theta^0_B)$$
2. For $$s = 1, \ldots, S$$
    1. $$\theta^s_1 \sim p(\theta_1 | \theta_2=\theta_2^{s-1}, \ldots, \theta_B=\theta_B^{s-1})$$
    1. $$\theta^s_2 \sim p(\theta_2 | \theta_1=\theta_1^{s-1}, \ldots, \theta_B=\theta_B^{s-1})$$
    1. $$\theta^s_B \sim p(\theta_B | \theta_1=\theta_1^{s-1}, \ldots, \theta_{B-1}=\theta_{B-1}^{s-1})$$

## Issues

Since we sample each dimension independently, we can get into issues with highly
correlated parameters.  For instance, if we consider a sampling of the bivariate
normal distribution, 

## Example: British Coal Mining Disasters

Let $$y_t$$ be the number of disasters in year $$t$$, $$t \in [1851, 1962]$$.  

We have count valued data, so we're going to use the Poisson distribution to
model this, but we're interested in determining if there was a change in
distribution at some point.  We'd like to measure the likelihood of a changepoint
(the tick that the distributions changed at) at an unknown $$k$$:  
$$
[ y_t | \lambda_t, k ] \sim Poisson(\lambda_t)
$$

with  
$$
\begin{aligned}
    \lambda_t &=
    \begin{cases}
      \lambda_1 & t = 1, \ldots, k \\
      \lambda_2 & t = k + 1, \ldots, n
    \end{cases} \\ \\
    \lambda_1, \lambda_2 &\sim \text{Gamma}(\alpha, \beta) \\
    k &\sim \text{Unif}(1, n)
\end{aligned}
$$

Using this setup, we can write the likelihood of the data as
$$
\begin{aligned}
    p(y | \lambda_1, \lambda_2, k) &= \prod_t \frac{\lambda_t^{y_t}\exp{-\lambda_t}}{y_t!} \\
    &= \prod_{t=1}^k \frac{\lambda_1^{y_t}\exp{( -\lambda_1 )}}{y_t!}
       \prod_{t=k+1}^n \frac{\lambda_2^{y_t}\exp{( -\lambda_2 )}}{y_t!}
\end{aligned}
$$

And the full conditionals of $$\lambda_1, \lambda_2$$ as
$$
\begin{aligned}
    p(\lambda_1 | y, \lambda_2, k) &\propto p(\lambda_1) p(y | \lambda_1, \lambda_2, k) \\
    &= \lambda_1^{\alpha - 1} \exp{(-\lambda_1\beta)} \lambda_1^{\sum_{t=1}^k y_t} \exp{(-\lambda_1 k)} \\
    &\sim \text{Gamma}(\alpha + \sum_{t=1}^k y_t, \beta + k)
\end{aligned}
$$
$$
\begin{aligned}
    p(\lambda_2 | y, \lambda_1, k) &\propto p(\lambda_2) p(y | \lambda_1, \lambda_2, k) \\
    &= \lambda_2^{\alpha - 1} \exp{(-\lambda_2\beta)} \lambda_2^{\sum_{t=k+1}^n y_t} \exp{(-\lambda_2 (n - k))} \\
    &\sim \text{Gamma}(\alpha + \sum_{t=k+1}^n y_t, \beta + (n - k))
\end{aligned}
$$

What we're truly after, is the likelihood that $$k = j$$ for some $$j = 1,
\ldots, n$$.  

$$
\begin{aligned}
    p(k = j | y, \lambda_1, \lambda_2) &= 
        \frac{p(y | \lambda_1, \lambda_2, k = j)p(k = j)}
        {\sum_{i=1}^n p(y | \lambda_1, \lambda_2, k = i)p(k = i)} \\
    &= \frac{p(y | \lambda_1, \lambda_2, k = j)}
       {\sum_{i=1}^n p(y | \lambda_1, \lambda_2, k = i)} \\
   p(y | \lambda_1, \lambda_2, k = j) 
    &= \prod_{t=1}^j \frac{\lambda_1^{y_t}\exp{( -\lambda_1 )}}{y_t!}
       \prod_{t=j+1}^n \frac{\lambda_2^{y_t}\exp{( -\lambda_2 )}}{y_t!} \\
    &= \prod_{t=1}^n \frac{1}{y_t!}
       \prod_{t=1}^j \lambda_1^{y_t} \exp{(-\lambda_1)}
       \prod_{t=1}^n \lambda_2^{y_t} \exp{(-\lambda_2)}
       \prod_{t=1}^j \lambda_2^{-y_t} \exp{(\lambda_2)} \\
    &= \left\{
        \prod_{t=1}^n \frac{1}{y_t!}
        \prod_{t=1}^n \lambda_2^{y_t} \exp{(-\lambda_2)}
       \right\}
       \left\{
        \prod_{t=1}^j \lambda_1^{y_t}\lambda_2^{-y_t} \exp{(\lambda_2-\lambda_1)}
       \right\} \\
    &= f(y, \lambda_1, \lambda_2) g(j, y, \lambda_1, \lambda_2)
\end{aligned}
$$

Note that we break up the marginal distribution into two components, one which
depends on $$j$$ and one that does not.  This allows us to define the posterior
distribution of $$k = j$$ solely based on $$g(j, y, \lambda_1, \lambda_2)$$

$$
p(k = j | y, \lambda_1, \lambda_2) = 
    \frac{g(j, y, \lambda_1, \lambda_2)}
    {\sum_{j=1}^n g(j, y, \lambda_1, \lambda_2)}
$$

### Accompanying R Code

```R
#---------------------------------------------------------------------------
# Example: British Coal Mining Disasters
#---------------------------------------------------------------------------
# Source: http://people.reed.edu/~jones/141/Coal.html
year = (1851:1962)
y = c(4, 5, 4, 1, 0, 4, 3, 4, 0, 6, 3, 3, 4, 0, 2, 6, 3, 3, 5, 4, 5, 3,
      1, 4, 4, 1, 5, 5, 3, 4, 2, 5, 2, 2, 3, 4, 2, 1, 3, 2, 2, 1, 1,
      1, 1, 3, 0, 0, 1, 0, 1, 1, 0, 0, 3, 1, 0, 3, 2, 2, 0, 1, 1, 1,
      0, 1, 0, 1, 0, 0, 0, 2, 1, 0, 0, 0, 1, 1, 0, 2, 3, 3, 1, 1, 2,
      1, 1, 1, 1, 2, 4, 2, 0, 0, 0, 1, 4, 0, 0, 0, 1, 0, 0, 0, 0, 0,
      1, 0, 0, 1, 0, 1)
plot(year, y, main = 'British Coal Mining Disasters');

n = length(y)

# Hyperparameters:
alpha = 0.5; beta = 0.01

# Pick some initial values:
k = ceiling(n/2) # midpoint is CP
lambda_1 = mean(y[1:k])
lambda_2 = mean(y[(k+1):n])

# Sample from the posterior distribution:
S = 10^4

# Storage:
post_lambda_1 = array(0, c(S, 1))
post_lambda_2 = array(0, c(S, 1))
post_k = array(0, c(S, 1));

for(s in 1:S){
  # Sample lambda_1:
  lambda_1 = rgamma(n = 1,
                    shape = alpha + sum(y[1:k]),
                    rate = beta + k)
  # Sample lambda_2:
    # NOTE: assuming k < n throughout!
  lambda_2 = rgamma(n = 1,
                    shape = alpha + sum(y[(k+1):n]),
                    rate = beta + (n-k))
   # Sample k:
  log_g = cumsum(y)*log(lambda_1/lambda_2) + (1:n)*(lambda_2 - lambda_1)
  #pm<-exp(log_g)/sum(exp(log_g)); k<-min((1:n)[runif(1)<cumsum(pm)])
  k = sample(1:n, 1, prob = exp(log_g))

  # Store:
  post_lambda_1[s] = lambda_1
  post_lambda_2[s] = lambda_2
  post_k[s] = k
}

# In terms of year:
hist(post_k + 1850)

# HPD interval of the year of changepoint:
(ci = HPDinterval(as.mcmc(post_k + 1850)))
abline(v = ci, lwd=5, col='blue')

plot(year, y, main = 'British Coal Mining Disasters');
abline(v = ci, lwd=5, col='blue')
abline(v = mean(post_k) + 1850, lwd=5,col='red')
lines(min(year):ci[1], rep(colMeans(post_lambda_1),
                           length(min(year):ci[1])),
      lwd = 5, col='green')
lines(ci[2]:max(year), rep(colMeans(post_lambda_2),
                           length(ci[2]:max(year))),
      lwd = 5, col='green')
```
