# Natural Eval: Natural Language AI Evaluation

Natural Eval uses a controlled natural language to define a single YAML file,
`eval.yaml`, that represents an executable AI evaluation workflow.

## Intention

The intention is to improve the experience of using AI agents to evaluate AI
agents by using a YAML with a fixed grammar that can combine the expressiveness
of a markdown and the precision of unit test code.

Potential uses:

- build AI prompt optimization workflows grounded by `eval.yaml`
- elicit AI product engineering requirements from customers through a chat session constrained by the grammar of `eval.yaml`.

## Components

`eval.yaml` has three components

- evidence - input and output schema and location
- criteria/rubric - Define what is good AI behavior given the evidence?
- provider bindings - Define how to simulate the AI workers we are judging?

## Current Highlights

- **Portability/Reproducibility:** A single YAML file represents your evaluation workflow.
- **Swapability:** Your evaluation workflow can swap different criteria, AI providers, and evaluator data platform providers.
- **Hackability:** The essential core (the YAML file's grammar or internal representation) is one light pydantic open source module.
- **Transparency:** Rubric predicates contain their natural language source.
- **Precision:** Rubric grammar enforces semantic ambiguity and schema.
- **Expressiveness:**
  - Rubric conditional behavioral predicates express more than the `actual-expected` pattern.
  - conditional predicates are evaluated over a panel observations of outcome and side-effect
    observations
  - conditional predicates contain severity levels

## Example CLI usage

```bash
naturaleval interpret --input eval.md --output eval.yaml
naturaleval --patch -f eval.yaml "top 20 nobsmed.com response to `health hacks for migraines` are more relevant than chatgpt.com's same results"
naturaleval evaluate -f eval.yaml
```
