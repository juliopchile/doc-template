---
name: skill-creator
description: Create, edit, evaluate, and improve agent skills for any agent system. Use this when a user wants to turn a repeatable workflow into a reusable skill, improve an existing skill, define triggering behavior, add examples, or package skill instructions for multiple agent platforms.
---

# Skill Creator

A skill is a reusable instruction package that teaches an agent how to perform a specialized workflow. Skills are useful when a task has repeatable steps, domain-specific conventions, expected output formats, or supporting resources that should not be rediscovered every time.

This file follows the Agent Skills open pattern: YAML frontmatter for metadata, Markdown instructions for the workflow, and optional bundled resources such as scripts, references, examples, or templates.

The workflow below is intentionally agent-agnostic. It can be used in Claude Code, OpenCode, Codex-style environments, custom agent runners, or any assistant that can read Markdown instructions and optional bundled files.

## When To Use This Skill

Use this skill when the user asks to:

- Create a new skill
- Improve or refactor an existing skill
- Convert a repeated workflow into reusable instructions
- Write a cross-agent skill usable by multiple tools
- Define a skill's trigger description
- Add examples, tests, or evaluation prompts for a skill
- Package skill documentation into platform-specific directories

## Skill Anatomy

```
skill-name/
├── SKILL.md              # Required: metadata + instructions
├── references/           # Optional: long-form docs loaded when needed
├── scripts/              # Optional: deterministic helper scripts
├── templates/            # Optional: reusable output templates
└── examples/             # Optional: sample inputs and outputs
```

### Progressive Disclosure

Good skills separate high-signal instructions from optional detail:

1. **Metadata** — name and description. This is the first thing an agent sees and often controls whether the skill is loaded.
2. **`SKILL.md` body** — the core workflow. Keep it direct enough to fit in context and useful without reading every bundled file.
3. **Bundled resources** — scripts, references, examples, templates, or assets loaded only when needed.

Use bundled resources when the skill needs large references or deterministic tools. Do not put hundreds of lines of rarely used reference material in the main skill body.

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

## Writing Guidelines

- Put trigger guidance in the `description`; many agent systems use it to decide whether to load the skill.
- Make the description specific and action-oriented. Include both what the skill does and when it should be used.
- Explain why important steps matter instead of relying only on rigid commands.
- Keep the core skill concise. Move long references into `references/`.
- Include examples for expected input/output when possible.
- Bundle scripts for deterministic, repetitive, or error-prone work.
- Avoid platform-specific assumptions unless the skill is explicitly platform-specific.

### Description Writing

The `description` is usually the primary trigger. It should not be vague.

Good descriptions include:

- The task the skill performs
- The contexts where it should trigger
- Important near-miss cases, if relevant
- Output or workflow expectations when they affect selection

Example:

```yaml
description: Create and maintain project documentation files such as README.md, AGENTS.md, PROJECT_STRUCTURE.md, PROJECT_ROADMAP.md, and PLAN.md. Use when the user asks to document architecture, update setup instructions, summarize project progress, write agent instructions, or separate active work from permanent docs.
```

Avoid:

```yaml
description: Helps write documentation.
```

### Writing Style

- Prefer clear imperative instructions.
- Explain the reason behind important steps.
- Use examples for formats the agent must reproduce.
- Avoid excessive all-caps rules unless safety or data loss is involved.
- Generalize from examples instead of overfitting the skill to one project.

## Skill Creation Workflow

### 1. Capture Intent

Clarify:

- What should the skill help the agent do?
- When should it trigger?
- What inputs does it expect?
- What output format should it produce?
- What edge cases should it handle?
- Are supporting files, scripts, or references needed?

If the user already provided a workflow in the conversation, extract from that first. Look for repeated steps, corrections the user made, required files, commands, output formats, and edge cases. Ask only for missing information that affects the skill's behavior.

Useful questions:

- What should this skill enable the agent to do?
- What user prompts or repository contexts should trigger it?
- What should the final output look like?
- What should the skill avoid doing?
- Can success be tested objectively, or is human review required?

### 2. Draft the Skill

Write:

- Clear frontmatter: `name` and `description`
- Short overview
- Step-by-step workflow
- Output format, if applicable
- Examples
- References to bundled resources

Use this structure unless there is a good reason to deviate:

```md
---
name: {skill_name}
description: {specific trigger description}
---

# {Skill Title}

Short explanation of what the skill helps with.

## When To Use

Use this skill when ...

## Workflow

1. Investigate ...
2. Draft ...
3. Verify ...
4. Present results ...

## Output Format

Use this format when returning results ...

## Examples

Input: ...
Output: ...
```

### 3. Organize Bundled Resources

Add bundled resources only when they improve reliability or reduce repeated work.

| Directory | Use For |
|---|---|
| `references/` | Long explanations, API docs, style guides, schemas |
| `scripts/` | Deterministic transformations, validators, generators |
| `templates/` | Reusable output files, report structures, boilerplate |
| `examples/` | Sample inputs, expected outputs, fixtures |

If a resource is large, include a short table of contents in the skill or in the reference file itself. Tell the agent when to read it.

### 4. Test With Realistic Prompts

Create 2-5 prompts that resemble how users actually ask for the workflow.

Example:

```json
[
  {
    "id": "basic-doc-update",
    "prompt": "Update the docs after adding a new auth endpoint.",
    "expected": "Agent updates README, PROJECT_STRUCTURE, and ROADMAP appropriately."
  }
]
```

For skills with objectively verifiable outputs, add assertions. For subjective skills, rely on human review and examples.

Assertion examples:

- Output includes all required sections.
- Generated file is valid JSON.
- Command appears exactly once.
- No project-specific names remain after generalization.

### 5. Evaluate Results

Run the test prompts with the skill and, when practical, without the skill as a baseline. Not every agent system supports isolated subagents or automated benchmarking; adapt the evaluation method to the available tools.

Useful evaluation signals:

- Did the skill trigger in the right context?
- Did the agent skip important setup or investigation steps?
- Did the output match the requested format?
- Did the skill reduce repeated work?
- Did it introduce irrelevant or overly rigid behavior?
- Did it increase token usage without improving quality?

If a review UI or benchmark tool exists in the environment, use it. Otherwise, summarize outputs directly for the user and ask for targeted feedback.

### 6. Improve Iteratively

Review failures and revise the skill. Prefer general improvements over overfitting to one example.

Look for:

- Missing context
- Ambiguous steps
- Too much generic advice
- Repeated work that should become a script
- Trigger descriptions that are too narrow or too broad

When improving:

1. Read the failed or weak outputs, not only the final summary.
2. Identify whether the issue came from triggering, missing instructions, unclear output format, missing resources, or excessive constraints.
3. Revise the smallest part that fixes the pattern.
4. Rerun representative prompts.
5. Stop when improvements are no longer meaningful or the user is satisfied.

### 7. Optimize Triggering

After the workflow is good, review the description. A strong description should trigger for realistic user prompts and avoid near-misses.

Create a small trigger test set:

```json
[
  {"query": "update our agent instructions after adding a new test command", "should_trigger": true},
  {"query": "write a marketing landing page headline", "should_trigger": false}
]
```

Use a mix of:

- Clear positive cases
- Casual or incomplete user phrasing
- Near-miss negative cases
- Adjacent tasks that should use another skill

Revise the description when false positives or false negatives are obvious.

### 8. Package For Multiple Agent Systems

If supporting multiple agent tools, keep one canonical skill and copy it into platform directories:

```
skills/{skill_name}/SKILL.md
.claude/skills/{skill_name}/SKILL.md
.opencode/skills/{skill_name}/SKILL.md
.codex/skills/{skill_name}/SKILL.md
.agents/skills/{skill_name}/SKILL.md
```

Keep copies consistent unless a platform requires a small formatting difference.

### 9. Updating an Existing Skill

When editing an existing skill:

- Preserve the skill's name unless the user asks to rename it.
- Read the current frontmatter and existing bundled resources before changing behavior.
- Keep useful examples and references.
- Remove stale platform-specific assumptions when generalizing.
- If working in a read-only installed skill location, copy it to a writable workspace first.
- Summarize what changed and why.

## Example Skill Description

Good:

```yaml
description: Update project documentation after code or architecture changes. Use when the user changes setup, commands, endpoints, file layout, architecture, roadmap status, or active implementation plans.
```

Weak:

```yaml
description: Helps with docs.
```

## Example Test Prompts

Use prompts like these to validate a documentation-maintenance skill:

```json
[
  {
    "id": "new-endpoint-docs",
    "prompt": "We added POST /invoices. Update the project docs so humans and agents understand it.",
    "expected": "README stays concise, PROJECT_STRUCTURE documents endpoint behavior, PROJECT_ROADMAP records progress if the feature is complete, PLAN reflects only active work."
  },
  {
    "id": "agent-instructions",
    "prompt": "Create an AGENTS.md for this repo. Keep it compact and focused on things agents would miss.",
    "expected": "AGENTS.md references core docs, commands, conventions, and active PLAN.md if present."
  },
  {
    "id": "near-miss",
    "prompt": "Write release notes for our users from these commits.",
    "expected": "Skill should not over-apply project documentation structure unless the user asks to update repo docs."
  }
]
```

## Safety and Quality Principles

- Do not create skills that hide behavior from users or facilitate unauthorized access, data exfiltration, credential theft, or deception.
- Do not include secrets in skill files or examples.
- Avoid surprising side effects such as committing, pushing, deleting files, or deploying unless the skill is explicitly for that workflow and includes user-confirmation steps.
- Prefer deterministic scripts for repetitive file transformations, but keep scripts understandable and documented.
- Keep examples fictional or sanitized.

## Platform Notes

Different agent systems load skills differently. Keep the core skill portable:

- Use Markdown and YAML frontmatter.
- Avoid tool names unless the skill truly requires them.
- Say "use the available file-reading tool" instead of naming a platform-specific tool when possible.
- Put platform-specific notes in a clearly labeled section.
- Keep canonical content in one place, then copy it into platform-specific directories if needed.

## Quality Checklist

- The skill has clear trigger conditions.
- A new agent can follow the workflow without chat history.
- The skill distinguishes permanent documentation from active work.
- Examples are realistic.
- Any platform-specific content is explicitly marked.
- The skill avoids secrets, unsafe behavior, or surprising side effects.
- Tests or review prompts cover realistic usage and near-misses.
- Large resources are referenced rather than embedded in the main body.
- Repeated deterministic work is handled by scripts when useful.
