# Natural Eval: Natural Language AI Evaluation

The aim of `naturaleval` is to allow a controlled natural language (CNL) to define
an executable and reproducible AI evaluation in as precise a manner as code and
as expressive a manner as natural language.

## Current Highlights

- **Portability/Reproducibility:** A single YAML file represents your evaluation workflow (rubric, evidence schema, provider bindings).
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
naturaleval interpret --input eval-spec.md --output eval-spec.yaml
naturaleval --patch -f eval-compose.yaml "top 20 nobsmed.com response to `health hacks for migraines` are more relevant than chatgpt.com's same results"
naturaleval run --ai-input observations.jsonl --eval-spec eval-spec.yaml
naturaleval evaluate --ai-output observations.jsonl --eval-spec eval-spec.yaml
```

console output ...

```json
{
    "summary": .....
    "faults": ...
}
```

## `eval-spec.yaml` has three components

- evidence - input and output schema and location
- criteria/rubric - Define what is good AI behavior given the evidence?
- provider bindings - Define how to simulate the AI workers we are judging?
