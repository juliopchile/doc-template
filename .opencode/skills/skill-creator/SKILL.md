---
name: skill-creator
description: Create, edit, evaluate, and improve agent skills for any agent system. Use this when a user wants to turn a repeatable workflow into a reusable skill, improve an existing skill, define triggering behavior, add examples, or package skill instructions for multiple agent platforms.
---

# Skill Creator

A skill is a reusable instruction package that teaches an agent how to perform a specialized workflow. Skills are useful when a task has repeatable steps, domain-specific conventions, expected output formats, or supporting resources that should not be rediscovered every time.

This template follows the Agent Skills open pattern: YAML frontmatter for metadata, Markdown instructions for the workflow, and optional bundled resources such as scripts, references, examples, or templates.

## When To Use

Use this skill when the user asks to create, improve, evaluate, package, or generalize an agent skill.

## Skill Anatomy

```
skill-name/
├── SKILL.md              # Required: metadata + instructions
├── references/           # Optional: long-form docs loaded when needed
├── scripts/              # Optional: deterministic helper scripts
├── templates/            # Optional: reusable output templates
└── examples/             # Optional: sample inputs and outputs
```

Minimum `SKILL.md`:

```md
---
name: {skill_name}
description: {what_the_skill_does_and_when_to_use_it}
---

# {Skill Title}

Use this skill when ...

## Workflow
1. ...
2. ...
3. ...
```

## Workflow

1. Capture the user's intent: what the skill should do, when it should trigger, expected inputs, expected outputs, and edge cases.
2. Draft clear frontmatter. Put trigger guidance in `description`; many agent systems use it to decide whether to load the skill.
3. Write a concise workflow with examples and output formats where useful.
4. Move long references into `references/` and deterministic repetitive work into `scripts/`.
5. Test with 2-5 realistic prompts. Prefer prompts that resemble how users actually ask for the workflow.
6. Improve iteratively. Generalize from feedback instead of overfitting to one example.
7. If supporting multiple agent tools, copy the final skill into platform directories.

## Multi-Platform Layout

```
skills/{skill_name}/SKILL.md
.claude/skills/{skill_name}/SKILL.md
.opencode/skills/{skill_name}/SKILL.md
.codex/skills/{skill_name}/SKILL.md
.agents/skills/{skill_name}/SKILL.md
```

Keep copies consistent unless a platform requires a small formatting difference.

## Description Examples

Good:

```yaml
description: Update project documentation after code or architecture changes. Use when setup, commands, endpoints, file layout, architecture, roadmap status, or active implementation plans change.
```

Weak:

```yaml
description: Helps with docs.
```

## Quality Checklist

- The skill has clear trigger conditions.
- A new agent can follow the workflow without chat history.
- The instructions explain why important steps matter.
- Examples are realistic.
- Any platform-specific content is explicitly marked.
- The skill avoids secrets, unsafe behavior, or surprising side effects.
