# Natural Language AI Evaluation

This library is an experiment to see if we can use a controlled natural language 
to define an executable AI evaluation workflow represented with a single YAML file.

## Motivation

The intention of this library is to bootstrap an experiment.

The speculation here is that if a controlled natural language can capture the expressiveness
of all existing major evaluation DSLs, without loosing precision, then we might get
the following outcomes:

- AI evaluation requiring zero knowledge of AI engineering means AI providers become cheaper to swap
- AI evaluation standardization means citizens can build consensus around defining what is good AI behavior.


## AI Evaluation Data Model

High level rationale of AI Evaluation: Is my world somehow better because of some new AI?

`eval.yaml` has three components

- evidence - input and output schema and location
- criteria/rubric - Define what is good AI behavior given the evidence?
- provider bindings - Define how to simulate the AI workers we are judging?

```yaml
meta
    name
    objective
    author
    version
evidence input data 
    precondition
        schema
        source
    baseline_outcome (optional) 
        schema
        source
candidate_providers
    name_1
        bash_command
        kwargs
    name_2
        bash_command
        kwargs
criteria_rules_rubric
    ...
    ...
```

## Current Highlights

- **Semantic Precision:** Schema validation failures give author feedback to fix ambiguous language.
- **Expressiveness:**
  - Evidence can include AI's side-effects
  - Evidence can include baseline interventional results 
  - Criteria can contain conditional scenarios 
  - Criteria can contain severity levels

## Example CLI usage

```bash
nateval define --source eval.md --sink eval.yaml
nateval add "top 20 nobsmed.com response to `health hacks for migraines` are more relevant than chatgpt.com's same results"
nateval evaluate
```
