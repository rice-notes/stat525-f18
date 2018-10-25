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
