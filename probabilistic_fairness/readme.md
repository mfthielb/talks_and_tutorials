# Reusability, Repeatability, Reproducibility, Replicability of the Probabilistic Fairness Methodology 

The goal of this repo is to provide sources for the reusability, repeatability, reproducibility, and replicability of the Probabilistic Fairness Methodology, presented in [Surrogate Modeling to Address the Absence of Protected Membership Attributes in Fairness Evaluation](), in accordance with the [ACM Artifacts](https://www.acm.org/publications/policies/artifact-review-badging). 

We provide two primary sources of artifacts: 
1. **Reusability**: Our probabilistic fairness methodology is implemented and released as part of the open-source fairness evaluation library, [Fidelity Jurity](https://github.com/fidelity/jurity). Other researchers and practitioners can use our method via a simple `pip install jurity`. You can find a quick start usage example below. 
2. **Repeatability & Replicability**: This repo provides the necessary material and source code used to replicate the experimental results from our paper [Surrogate Modeling to Address the Absence of Protected Membership Attributes in Fairness Evaluation]().

Given the open-source library, making the method available to everyone, and the experimental protocol followed in our paper, providing a reference implementation, we hope other researchers can **Reproduce** similar benefits provided by probabilistic fairness evaluation in different domains and applications. 

## 1. How to Reuse Our Method? 

Our probabilistic fairness method is available in the open-source fairness evaluation library, [Fidelity Jurity](https://github.com/fidelity/jurity). 

Here is a quick start example. 

### Quick Start Example 

```
from jurity.fairness import BinaryFairnessMetrics

# Instead of 0/1 deterministic membership at the individual level 
# consider the likelihood of membership to protected classes for each sample 
binary_predictions = [1, 1, 0, 1]
memberships = [[0.2, 0.8], [0.4, 0.6], [0.2, 0.8], [0.9, 0.1]]

# Metric
metric = BinaryFairnessMetrics.StatisticalParity()
print("Binary Fairness score: ", metric.get_score(binary_predictions, memberships))

# Surrogate membership: consider access to surrogate membership at the group level. 
surrogates = [0, 2, 0, 1]
print("Binary Fairness score: ", metric.get_score(binary_predictions, memberships, surrogates))
```

Existing binary fairness metrics assume that we have access to every individual's protected membership attribute. You can read more about these classical metrics in [our documentation](https://fidelity.github.io/jurity/about_fairness.html).

What if we do not know each sample's protected membership attribute? This is a practical scenario that we refer to as probabilistic fairness evaluation.

At a high level, instead of strict 0/1 deterministic membership at the individual level, consider the probability of membership to protected classes for each sample.

An easy baseline is to convert these probabilities to the deterministic setting by taking the maximum likelihood as the protected membership. This is problematic as the goal is not to predict membership but to evaluate fairness.

Taking this a step further, while we do not have membership information at the individual level, consider access to surrogate membership at the group level. We can then infer the fairness metrics directly.

We provide an in-depth study and formal treatment of probabilistic fairness in [Surrogate Membership for Inferred Metrics in Fairness Evaluation (ACM LION 2023)](https://dl.acm.org/doi/10.1007/978-3-031-44505-7_29) and [Surrogate Modeling to Address the Absence of Protected Membership Attributes in Fairness Evaluation](). 

### Installation 

Our fairness library, Jurity, is open-source and is part of the Python Package Index (PyPI), making it easy to install via: 

```
pip install jurity
```

If you wish to build from the source, please follow [installation instructions](https://fidelity.github.io/jurity/install.html).

### Requirements 

Jurity requires **Python 3.7+**, and please see [requirements.txt](https://github.com/fidelity/jurity/blob/master/requirements.txt) for required libraries. 

### Public API 

See [Public API documentation](https://fidelity.github.io/jurity/api.html).


## 2. How to Repeat & Replicate Our Experimental Results? 

This repo provides artifacts to repeat and replicate our experimental results from [Surrogate Modeling to Address the Absence of Protected Membership Attributes in Fairness Evaluation](). All experiments use the Jurity library under the hood.  

Specifically, when we use the quick start example given above, Jurity calls the [`get_bootstrap_results()`](https://github.com/fidelity/jurity/blob/master/jurity/utils_proba.py#L105) as the core internal to calculate the probabilistic fairness metrics. This method then uses the [`BiasCalculator`](https://github.com/fidelity/jurity/blob/master/jurity/utils_proba.py#L204) for the evaluation. 

### Experiment Data  
The datasets used in our paper can be found under [datasets folder](https://github.com/mfthielb/talks_and_tutorials/tree/master/probabilistic_fairness/supporting_data). 

### Experiment Code 
Our analysis of probabilistic fairness and its expected behavior are based on simulations from hypothetical models with different levels of unfairness, as explained in **Table 3** in the paper. 

Our experiments use the same [helper methods provided in Jurity](https://github.com/fidelity/jurity/blob/master/jurity/utils_proba.py). More specifically; 
* [simulation.py](https://github.com/mfthielb/talks_and_tutorials/blob/master/probabilistic_fairness/simulation.py): Runs the code to create simulated model results with different levels of unfairness. This corresponds to **Table 2** and **Figure 3** in the paper.
* [simulation_compare_to_model.py](https://github.com/mfthielb/talks_and_tutorials/blob/master/probabilistic_fairness/simulation_compare_to_model.py): Compares probabilistic fairness estimates to using models that try to predict protected class membership. This corresponds to **Figure 2** in the paper.
* [simulation_counts.py](https://github.com/mfthielb/talks_and_tutorials/blob/master/probabilistic_fairness/simulation_counts.py): Compares the characteristics of the method with respect to the counts of surrogate membership within each group. This corresponds to **Figure 4** in the paper.
  
