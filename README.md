# Skills

Custom skills for [VM0](https://vm0.ai) agents.

## Available Skills

| Skill | Description | API Key Required |
|-------|-------------|-----------------|
| [exa-search](./exa-search) | Neural web search, tweet search, blog search, and content retrieval via [Exa AI](https://exa.ai) | `EXA_API_KEY` |
| [perplexity](./perplexity) | AI-powered search with real-time web grounding and citations via [Perplexity AI](https://perplexity.ai) | `PERPLEXITY_API_KEY` |

## Usage

Reference skills in your `vm0.yaml`:

```yaml
skills:
  - https://github.com/nnguyen01/skills/tree/main/exa-search
  - https://github.com/nnguyen01/skills/tree/main/perplexity
```

## Adding New Skills

Create a new directory with a `SKILL.md` file following the [VM0 skill format](https://docs.vm0.ai/docs/core-concept/skills).
