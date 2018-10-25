# Comments on Heirarchical Models

### centered vs. non-centerd

Many different ways to parameterize a model.  Two common examples are to 

#### centered models

$$
\begin{aligned}
  [y_{ij}|\ldots] &\sim \text{Normal}(\theta_j, \sigma_y^2) \\
  [\theta_j|\ldots] &\sim \text{Normal}(\mu, \sigma_\theta^2)
\end{aligned}
$$

#### non-centered



$$
\begin{aligned}
  [y_{ij}|\ldots] &\sim \text{Normal}(\mu + \gamma_j, \sigma_y^2) \\
  [\gamma_j|\ldots] &\sim \text{Normal}(0, \sigma_\theta^2)
\end{aligned}
$$
