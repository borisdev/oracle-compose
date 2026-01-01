# TED: Task Evaluation Definition (TED)

TED allows the following

- use of a single YAML file (`ted-compose.yaml`) to define and run an evaluation of an AI task.
- use natural language to compose the ted-compose YAML

A ted compose requires the author define three main parts before running an evaluation:

- criteria
- task bindings (how to run the task worker)
- observations (pre- and post-state and output)

## Why is a single evaluation YAML such a big deal?

A single declaritive YAML is easier to standardize and share than code of many different frameworks.
Consider this dialog.

- CTO: Why did you choose Claude Code over Codex for this task?
- Dev: Here is my ted compose and results. Go repro it on your own observations.
- CTO: You are planning to vibe code the AI Agent Prototype?
- Dev: I will tell the SWE to not commit any code unless there are no regressions after running `ted evaluate`
- CPO: How will we get encode domain expertise into our AI system for 10 uses cases in one month?
- Dev: Tell the SMEs to write their criteria for good behavior in natural language and look at the ambiguity feedback returned to fine-tune.
- CEO: I have 10 startups giving me demos to propose AI to save us toil and I have no idea how to choose the top candidate?
- Dev: Author a TED compose and tell each time to patch it with how we run their task worker.
- CEO: Can we cheaply swap competing evaluator SaaS services and AI workfflow services
- Dev: yep. 1st Define a canonical ted compose yamls
- Bartender: I dont like all the power in the hands of AI Engineers in making the Traffic Police Bots
- Dev: Oh, just write a ted compose using ChatGPT and email it to your representative

## Example CLI usage

```bash
ted compose --patch "the task is to discover what PMC manuscript facts we can query to help the user with her problem"
ted compose --patch "the task output must contain a field of type list: `audience_memberships`
ted compose --patch "clinical trials on animals are never relevant to the biohacker audience"
ted compose --patch "run evaluations against these raw input conversation threads in dir `/.ted/nobsmed/test_cases`
ted compose --patch "run evaluations against this AI Agent invocation`

#or

ted compose --input instructions.md --output my-ted-compose.yaml

ted evaluate --format=md >> report.model
#or
ted evaluate -f my-ted-compose.yaml
# console output looks like....
{
    "summary": .....
    "faults": ...
}
```

## Language challenge

Pure natural language is imprecise (ambiguousness leads to LLM hallucination) but can express
a wide range of general evaluation concepts.
The IR's schema (including doc strings) of the ted-compose file represents a proposed grammar of a constrained
natural language (CNL) for defining the evaluation of a task.
The challenge being proposed here
is to make an optimal TED CNL that maximizes precision for defining nuances in one's custom task evaluations while minimizing
the loss of general expressiveness.

## Highlights of the current state of the TED grammar.

Expressiveness: Test Cases and Invariants with Predicate assertions (general)
Evaluation is based on predicates, which are more general than the deterministic `actual vs expected` form.
Predicate assertion may carry severity levels.
Multi-state, multi-time (general)
Impact is computed from changes between pre- and post-run states. An oracle may
observe multiple states across multiple times. For example, what was the impact
of an agent pre-intervention, post-intervention, and follow-up evaluation?
