# Clinical Trials Research

An instruction-only [Agent Skill](https://agentskills.io/specification) for researching clinical trials from a biostatistician's perspective. It organizes trial identity, protocol history, prespecified statistics, actual conduct, and reported results into a traceable evidence record.

The method presents sourced facts and attributes interpretations and conclusions to investigators or sponsors. It keeps the initial plan, amendments, final statistical analysis plan (SAP), and actual results distinct, with explicit source versions, data cutoffs, and evidence gaps.

## Workflow

For a named trial or trial series, follow nine steps:

1. Verify trial names, registry identifiers, sponsor study numbers, sponsor, intervention, and indication.
2. Summarize the indication and mechanism relevant to the trial design.
3. Describe phase, design, allocation, masking, control, cohorts, and stages.
4. Present the final protocol's detailed eligibility criteria.
5. Reconstruct the initial protocol's planned flow, with a rendered diagram.
6. Track major protocol amendments, compare initial and final versions, and plot the protocol/SAP and database freeze or lock timeline.
7. Extract prespecified objectives, endpoints, sample-size assumptions, and statistical analyses, primarily from the final SAP.
8. Document actual conduct, participant flow, baseline characteristics, efficacy and safety results, and applicable testing sequences.
9. Present investigators' and sponsors' conclusions with attribution.

For a therapy, indication, or broader scope, first deliver a deduplicated trial list, search coverage, unresolved identity questions, and a separate launch prompt for each trial. This handoff ends the discovery task; individual reviews begin when the user requests them.

Default deliverables are a Markdown report, a source register, an evidence ledger, and relevant rendered figures with editable sources. Trial-specific folders keep evidence and outputs separate.

## Files and language

- [`SKILL.md`](SKILL.md): self-contained English instructions and the canonical installation entry point.
- [`docs/SKILL.zh-CN.md`](docs/SKILL.zh-CN.md): retained Chinese original for reference.
- `agents/openai.yaml`, when included: optional Codex UI metadata; the method is in `SKILL.md`.

Reports default to **Chinese**, retaining useful English terms. Request English or another language in your prompt to change the output language. Install one copy of the skill; keep the Chinese reference in `docs/` rather than installing both language files as separate skills with the same name.

## Installation

Download or clone [0alpha0iota/agentSkills](https://github.com/0alpha0iota/agentSkills). Copy the contents of `skills/clinical-trial-research/` into a directory named `clinical-trial-research` at **one** of the locations below. The result must contain `clinical-trial-research/SKILL.md` directly, without another nested repository folder. `~` means your home directory.

| Host | Personal installation | Project installation | Official documentation |
| --- | --- | --- | --- |
| Codex | `~/.agents/skills/clinical-trial-research/` | `.agents/skills/clinical-trial-research/` | [Build skills](https://learn.chatgpt.com/docs/build-skills) |
| Claude Code | `~/.claude/skills/clinical-trial-research/` | `.claude/skills/clinical-trial-research/` | [Extend Claude with skills](https://code.claude.com/docs/en/skills) |
| Gemini CLI | `~/.gemini/skills/clinical-trial-research/` | `.gemini/skills/clinical-trial-research/` | [Agent Skills](https://geminicli.com/docs/cli/skills/) |

Check for an existing installation first. Back up and compare local customizations before replacing files, and preserve unrelated skills. Choose a single active location to avoid same-name conflicts.

Gemini CLI also recognizes `~/.agents/skills/` and `.agents/skills/` as aliases. To select this skill from the repository, use `gemini skills install https://github.com/0alpha0iota/agentSkills.git --path skills/clinical-trial-research`. See its [official installation guidance](https://geminicli.com/docs/cli/skills/).

These paths and invocation forms follow the linked host documentation; they are not a claim of end-to-end testing in every product.

## Usage

In Codex, invoke the skill with `$clinical-trial-research`, as described in the [official OpenAI documentation](https://learn.chatgpt.com/docs/build-skills):

```text
Use $clinical-trial-research to review [trial name and registry ID].
Follow the nine-step workflow and write the report in English.
```

In Claude Code, use the [skill command](https://code.claude.com/docs/en/skills):

```text
/clinical-trial-research Review [trial name and registry ID] using the nine-step workflow. Write the report in English.
```

For any assistant that can read files or attachments, provide `SKILL.md` explicitly:

```text
Read the attached SKILL.md and follow its clinical-trial-research method.
Research [trial identity, or therapy/indication and selection criteria].
Use information available through [date], write in [language], and save
evidence and deliverables in separate trial-specific folders under [workspace].
```

The core method requires no particular model, MCP server, Python installation, or external service. Live research needs access to relevant registry, publication, regulatory, and sponsor sources, plus suitable storage and rendering tools. Use the host's available capabilities; when access is blocked, the workflow records the gap and gives the user exact file destinations for supplying materials.

## License

[MIT](LICENSE), copyright 2026 0alpha0iota. The license covers this skill's original content. Source publications, screenshots, and other third-party materials retain their own rights and terms.
