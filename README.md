# Oracle Spec: A Standard Language for Non-Coders to Define Good AI Behavior

Oracle-spec is a controlled natural language (CNL) and schema for declaring
quality-control predicates that compile into executable checks.

Oracle-spec enables non-coders to author verifiable definitions of “good
behavior”—rules, constraints, and rubrics—without writing custom evaluator
code. The oracle-spec runtime enforces unambiguous syntax and defined
semantics, so authors can safely use LLM-assisted tools (e.g., Codex, Claude
Code) to draft and refine rules while preserving determinism.

After execution, oracle-spec produces human-readable feedback from (1)
schema/grammar validation and (2) predicate evaluation results. This feedback
helps authors clarify intent, resolve ambiguity, and iteratively converge on a
precise behavioral contract within the controlled grammar.

## Small Design Innovations

1. Declarative YAML Artifact (portable, transparent)
2. Schema validation (natural language expression with precision via feedback)
   By authoring an oracle using a schema-validated declarative specification (ie., a
   controlled natural language) a non-coder can unambiguously express normative
   behavioral assertions on changes in key observable states in his natural language
   since they can get feedback to fix any of their ambiguous directives.
3. Test Cases and Invariants with Predicate assertions (general)
   Evaluation is based on predicates, which are more general than the deterministic `actual vs expected` form.
   Predicate assertion may carry severity levels.
4. Multi-state, multi-time (general)
   Impact is computed from changes between pre- and post-run states. An oracle may
   observe multiple states across multiple times. For example, what was the impact
   of an agent pre-intervention, post-intervention, and follow-up evaluation?

## Illustration

Extraction AI prompt
