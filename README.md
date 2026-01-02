# TED Compose (Task Evaluation Definition Composition)

TED Compose is a tool that allows the following

- use of a single YAML file (`ted-compose.yaml`) to define and run an evaluation of an AI task.
- use natural language to compose the ted-compose YAML

A ted compose requires the author define three main parts before running an evaluation of some task:

- criteria
- task bindings (how to run the task worker)
- collection of observations (pre- and post-state and output aka test examples) to run the criteria against

## Aims

- The short term aim is to make it simpler and more expressive for developers to evaluate AI agents.
- The long term aim is for people with zero knowledge of how AI works to control the criteria of good AI behavior.

## Why is a single evaluation YAML such a big deal?

A single declaritive YAML is easier to standardize and share than code of many different frameworks.
Consider this dialog.

- CTO: Why did you choose Claude Code over Codex for this task?
- Dev: Look at my ted compose to see my criteria and observations and resulting report.
- CPO: How will we get encode domain expertise into our AI system for 10 uses cases in one month?
- Dev: Tell the domain experts to author criteria using ChatGPT.
- CEO: I have 10 startups giving me demos to propose AI to save us toil and I have no idea how to choose the top candidate?
- Dev: Author a TED compose and tell each time to interpret it with how we run their task worker.
- CEO: Can we cheaply swap competing evaluator SaaS services and AI workfflow services
- Dev: yep. 1st Define a canonical ted compose yamls
- Bartender: I dont like all the power in the hands of AI Engineers in making the Traffic Police Bots
- Dev: Oh, just write a ted compose using ChatGPT and email it to your representative

## Example CLI usage

```bash
ted compose --interpret "the task is to discover what PMC manuscript facts we can query to help the user with her problem"
ted compose --interpret "the task output must contain a field of type list: `audience_memberships`
ted compose --interpret "clinical trials on animals are never relevant to the biohacker audience"
ted compose --interpret "run evaluations against these raw input conversation threads in dir `/.ted/nobsmed/test_cases`
ted compose --interpret "run evaluations against this AI Agent invocation`

#or

ted compose --interpret --input instructions.md --output my-ted-compose.yaml

ted evaluate --format=md >> report.model
#or
ted evaluate -f my-ted-compose.yaml
# console output looks like....
{
    "summary": .....
    "faults": ...
}
```

## Contributor challenge

The challenge being proposed here is for contributors to make an optimal TED
CNL that maximizes precision and minimizes the loss of general TED
expressiveness.

## Highlights

- the internal schema representation (pydantic) of a ted-compose file
  represents a single proposed standard for defining many customized task
  evaluations in natural language.
- conditional predicates are evaluated over a panel observations of outcome and side-effect
  observations
- conditional predicates contain severity levels
