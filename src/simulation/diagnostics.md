# MCMC Diagnostics

## Gelman-Rubin Diagnostic

* $m$ independent chains
* $n$ # of simulations after burn-in for each chain
* $\Psi_{ij}$ = parameter of interest ($i$ indicates the simulation entry, $j$
  indicates chain)
* compare between-chain variance with within-chain variance


## Effective Sample Size

Even thought it might seem like we have many independent samples from our trace
plots, we need to check the number of "effective" samples, or uncorrelated
samples.
