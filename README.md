# Natural Language AI Evaluation

This library is an experiment to see if we can use a controlled natural language 
to define an executable AI evaluation workflow represented with a single YAML file.

## Motivation

We use AI evaluations to answer the question, “Is my world measurably better because this AI exists?”

The problem with existing AI evaluation tools is that they require software engineering experience.

The proposed solution is to use a controlled natural language to capture the expressiveness
of all existing major evaluation software engineering tooling, without loosing precision.


## AI Evaluation Data Model


An AI evaluation has three parts.

1. Problem space - What do we show the AI builder, what raw discovery data?
2. Solution space - The interface contract: e.g., JSON-in / JSON-out REST endpoint
3) Evaluation space - Reproducible rules for measuring "good behavior" 


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
nateval add --text "top 20 nobsmed.com response to `health hacks for migraines` are more relevant than chatgpt.com's same results"
nateval run eval.yaml
```

