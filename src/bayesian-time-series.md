# Bayesian Time Series

Suppose data $(y_t, x_t)$ arrive sequentially for $t = 1, \ldots, T$.  

Examples:  


---

### Why bother with time series modelling?

1. unmodeled dependence can lead to poor inference
    * regression analysis often assumes i.i.d. errors
    * errors are often not independent but somehow correlated
2. direct interest: looking for structural changes over time
    * how an economic policy or stimulus affects our data
    * performing inference on the effect of an outside event
3. forecasting
    * weather, probability of rain given the previous weather patterns
    * value of future data or distribution of future data

---

### Classical Time Series

(Strong) Stationarity: The joint distribution is invariant under time shifts:  
$$
p(y_t, \ldots, y_{t+s}) = p(y_{t+k}, \ldots, y_{t+k+s}) \forall{k}
$$

(Weak) Stationarity: Fix the first and second moments of our data:  
$$
\begin{aligned}
E[y_t] &= \mu \\
Cov(y_t, y_{t+l}) &= \gamma(l) \forall{t},\forall{l}
\end{aligned}
$$

Notes:  
* strict $\implies$ weak __only when__ 2nd moments exist
* strict $\leftrightarrow$ weak under gaussian assumption

---

#### Autoregression

$$
y_t = \beta_0 + \beta_1y_{t-1} + \epsilon_t
$$

Conditional Moments:  
$$
\begin{aligned}
E[y_t | y_{t-1}] &= \beta_0 + \beta_1 y_{t-1} \\
Var(y_t | y_{t-1}) &= Var(\beta_0 + \beta_1 y_{t-1} + \epsilon_t | y_{t-1}) = Var(\epsilon_t) = \sigma^2
\end{aligned}
$$

Unconditional Moments:  
$$
\begin{aligned}
E[y_t] &= E[ \beta_0 + \beta_1 y_{t-1} + \epsilon_t ] = \beta_0 + \beta_1E[y_{t-1}] \\
\mu &= \beta_0 + \beta_1\mu \\
&= \frac{\beta_0}{1 - \beta_1} \\

Var(y_t) &= Var(\beta_0 + \beta_1 y_{t-1} + \epsilon_t) = \beta_1^2Var(y_{t-1}) + \sigma^2  \\
\gamma(0) &= \beta_1^2\gamma(0) + \sigma^2 \\
&= \frac{\sigma^2}{1 - \beta_1^2}
\end{aligned}
$$

Notes:  
* $\mu$ exists $\implies \beta_1 \ne 1$
* $\mu = 0 \implies \beta_0 = 0$
* variance requires $\beta_1^2 < 1 \implies |\beta_1| < 1$ to ensure that
  $\gamma(0) > 0$.  Known as "stationarity region".

Bayesian Approaches:  
* prior on stationarity region for $\beta_1$ 
* $\beta_0$ is standard in linear models

---

#### Dynamic Linear Models
