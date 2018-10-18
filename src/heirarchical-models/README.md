# Heirarchical Models

Often times, we have data with a known heirarchical or multi-level structure,
e.g.:  
* students in a class in a school in a district
* patients in a hospital in a state
* experimental replicates in a batch in a day

Can we combine and share information based on known dependence structure? Does
this impact our ability to perform inference?

## Use Cases

* learning about treatment effects that vary by group
* using all data to make inferences in small samples
* prediction in new units in existing group or new group
* predictors at different levels
* additional layers of uncertainty for prior parameters (like hyperpriors)

## Motivating Example

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

### Without Pooling

Model each $\{\lambda_h\}_h$ independently, e.g. $\lambda_h \sim
\text{Gamma}(\alpha_h, \beta_h)$ or $\hat{\lambda}_h = y_h / x_h$.  We ignore
the likely depedence between $\lambda_i$'s, and $\hat{\lambda}_h$ will be poor
if $x_h$ is small.

### Complete Pooling

Fix $\lambda_1 = \lambda_2 = \ldots = \lambda_H = \lambda$.  This ignores
hospital-hospital variablity.
