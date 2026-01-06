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


High-level rationale

AI evaluation answers: “Is the world (or this workflow) measurably better because this AI exists?”
Meaning: does it improve outcomes on real tasks, for real users, under real constraints—compared to a baseline (no AI, or a previous system)?

The three parts of an AI evaluation

1) Problem space

What the AI is allowed to use and what it must handle.

Inputs and evidence: manuscripts, references, user questions, metadata

Context & constraints: time, cost, latency, privacy, policy, allowed tools

Task definition: what success looks like in the real workflow

(Your “raw discovery data” fits here.)

2) Solution space

What an acceptable solution must look like from the outside.

The interface contract: e.g., JSON-in / JSON-out REST endpoint

Request/response schema: required fields, error model, versioning

Behavioral requirements: citations, uncertainty, refusal rules, determinism, etc.

Key idea: evaluation shouldn’t depend on how it’s built, only on the observable behavior through the interface.

3) Evaluation space

How you judge whether the solution improves the world.

Metrics: correctness, faithfulness, coverage, usefulness, time saved, cost

Test sets: representative + adversarial cases

Protocols: human rating rubrics, inter-rater reliability, A/B tests, thresholds

Baselines: “no AI,” “current system,” “smaller model,” etc.



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
nateval add --text "top 20 nobsmed.com response to `health hacks for migraines` are more relevant than chatgpt.com's same results"
nateval run eval.yaml
```


```
apiVersion: ai-sla.dev/v1
kind: AISLA
metadata:
  name: support-agent-quality
  owner: acme-customer

service:
  id: providerX/support-agent
  interface:
    type: http
    endpoint: https://api.providerx.com/v1/respond

observability:
  # What evidence is collected per interaction
  record:
    - request
    - response
    - retrieved_context
    - tool_calls
    - latency_ms
    - cost_tokens
    - model_id
    - timestamp
  storage:
    sink: s3
    prefix: ai-sla/support-agent-quality/

slo:
  # The customer’s definition of “good enough”
  windows:
    - name: rolling_7d
      duration: 7d
      burn_alerts: true

  objectives:
    - id: O_LATENCY_P95
      metric: latency_ms
      selector: { traffic: production }
      target: { p95: 1200 }

    - id: O_REFUSAL_RATE
      metric: refusal_rate
      selector: { intent: "normal_help" }
      target: { max: 0.5% }

    - id: O_HALLUCINATION_RATE
      metric: eval.hallucination
      selector: { domain: "policy_answers" }
      target: { max: 1.0% }

    - id: O_TASK_SUCCESS
      metric: eval.task_success
      selector: { intent: "ticket_resolution" }
      target: { min: 95% }

evaluations:
  # Your EvalSpec → EvalOracle idea fits here
  evalspecs:
    - id: policy_quality
      ref: specs/policy_quality.evalspec.yaml
    - id: safety_email
      ref: specs/safety_email.evalspec.yaml

sampling:
  strategy: stratified
  rate:
    production: 1%
    high_risk_intents: 10%
    regressions: 100%

error_budget:
  # When is the service “not acceptable” to the customer?
  policy:
    window: rolling_30d
    budget: 5%          # allowable failures vs objectives
    actions:
      - when: burn_rate_gt_2x_for_6h
        do: [notify, route_to_fallback_model]
      - when: burn_rate_gt_4x_for_1h
        do: [page, pause_rollout]

reporting:
  cadence: daily
  outputs:
    - type: dashboard
    - type: pdf_summary
    - type: webhook
      url: https://acme.internal/hooks/ai-sla
```
