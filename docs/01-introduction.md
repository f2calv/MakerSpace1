# Introduction

[Previous: Workshop README](../README.md) | [Next: Create an App](02-create-an-app.md)

## 1. What are instructions?

Reusable rules that guide **how Copilot should work**: coding conventions, documentation,
safety checks and team preferences. They save you repeating the same guidance in every prompt.

* Project-wide: `.github/copilot-instructions.md`, plain Markdown with no YAML required.
* Targeted: `.github/instructions/<topic>.instructions.md`, selected by file patterns,
   task relevance or manual attachment.

Minimal example, showing all documented VS Code instruction frontmatter fields:

```markdown
---
name: C# documentation
description: Use when creating or changing public C# APIs.
applyTo: '**/*.cs'
---

Add XML documentation to public classes and methods.
```

All three fields are optional: `name` is the display label, `description` helps task-based
discovery, and `applyTo` automatically attaches guidance for matching files being changed.
Without `description` or `applyTo`, attach the file manually. Use `applyTo: '**'` only for
rules relevant to every file.

## 2. What are skills?

Reusable instructions for **how to complete a particular task**. A skill can bundle scripts,
templates and reference material, but a short Markdown procedure is enough to start.

Store each skill at `.github/skills/<skill-name>/SKILL.md`. Copilot uses its name and
description for discovery, loads the body when relevant, and reads supporting files as needed.
You can also invoke it through `/skill-name` when it is exposed in the slash-command menu.

Minimal example:

```markdown
---
name: build-project
description: Build the .NET project and explain failures. Use when asked to build.
---

1. Find the project file.
2. Run `dotnet build` against it.
3. Summarize the result and explain any errors.
```

### Frontmatter reference

Only `name` and `description` are required. The following shows all fields documented by
VS Code and the Agent Skills standard, not a recommended minimum:

```yaml
---
name: build-project
description: Build the .NET project and explain failures. Use when asked to build.
argument-hint: '[project path]'
user-invocable: true
disable-model-invocation: false
context: inline
license: MIT
compatibility: Requires the .NET 10 SDK.
metadata:
   version: '1.0'
allowed-tools: Bash(dotnet:*) Read
---
```

| Field | Meaning |
| --- | --- |
| `name` | Required; matches the folder name. Lowercase letters, digits and hyphens; at most 64 characters. |
| `description` | Required; what the skill does and when to use it. At most 1,024 characters. |
| `argument-hint` | Optional VS Code hint shown when invoking the slash command. |
| `user-invocable` | VS Code: show in the `/` menu; defaults to `true`. |
| `disable-model-invocation` | VS Code: prevent automatic selection; defaults to `false`. |
| `context` | Experimental VS Code setting: `inline` (default) or `fork` for a separate subagent context. |
| `license` | Standard metadata: license name or reference to bundled terms. |
| `compatibility` | Standard metadata: environment requirements, at most 500 characters. |
| `metadata` | Standard metadata: arbitrary string-to-string values. |
| `allowed-tools` | Experimental standard field: space-separated pre-approved tools; support and tool names vary by agent. |

The `allowed-tools` example uses another agent's tool syntax; do not assume it grants
permissions or restricts tools in VS Code. Forked context requires
`github.copilot.chat.skillTool.enabled`. Skills do not use `applyTo`.

## 3. Why instructions and skills?

* Instructions keep everyday work consistent: "Document every public C# API."
* Skills capture repeatable procedures: "Build the project and diagnose failures."
* Both make team knowledge reusable, reviewable and easier to maintain than repeated prompts.
* Neither guarantees compliance or changes tools by itself. A restore skill guides Copilot;
   it does not change what happens when you type `dotnet build` yourself.

Keep rules short, descriptions specific and private configuration out of public examples.
Verify discovery and actual use in a fresh chat; inspect references and the resulting behavior.

## 4. Where do instructions and skills live?

Start in the repository's `.github/` folder for team-shared Copilot customizations.
It is not the only source: personal files, extensions and organization guidance can also
contribute context, even in a fresh chat. The selected agent harness (Copilot, Local,
Claude or Codex), not just the model, determines which locations are supported.

| Scope or source             | Instructions                                                                                             | Skills                                                        |
| --------------------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| Repository                  | `.github/copilot-instructions.md` and `.github/instructions/**/*.instructions.md`                        | `.github/skills/<name>/SKILL.md`                              |
| Repository alternatives     | Root or nested `AGENTS.md`; discovery depends on the harness and settings                                | Local also reads `.agents/skills/` and `.claude/skills/`      |
| VS Code user profile        | Local sessions: `*.instructions.md` in profile storage, created through **Chat: Open Customizations**    | Use a supported personal skill directory instead              |
| Personal Copilot folders    | Copilot harness: `~/.copilot/copilot-instructions.md` and `~/.copilot/instructions/**/*.instructions.md` | Copilot and Local: `~/.copilot/skills/<name>/SKILL.md`        |
| Other personal folders      | Harness-specific formats, such as `~/.claude/CLAUDE.md`                                                  | Local also reads `~/.agents/skills/` and `~/.claude/skills/`  |
| Additional configured paths | `chat.instructionsFilesLocations` adds search locations                                                  | Local's deprecated `chat.agentSkillsLocations` adds locations |
| Extensions and plugins      | Can contribute instructions or rules, depending on the harness                                           | Can contribute bundled skills                                 |
| Organization                | Managed on GitHub; availability depends on the client, account and settings                              | Do not assume a corresponding organization skill folder       |

On Windows, the default VS Code profile commonly stores instructions under
`%APPDATA%/Code/User/prompts/`. Named profiles can use different profile storage.
Personal Copilot skills instead live under `%USERPROFILE%/.copilot/skills/`.
These are separate mechanisms: the profile folder is not the personal skill folder,
and `.copilot/` can contain both instructions and skills.

`~` means the home directory where the agent runs. In a dev container, that can differ
from your Windows home directory. VS Code may also supply profile or extension files
from the host, so verify the actual paths in **Chat: Open Customizations** for that session.

### Discovery is not a precedence hierarchy

The arrows below show sources contributing to a request, not which source wins:

```mermaid
flowchart TD
   R["Repository: .github and supported alternatives"] --> D["Discovery by harness and settings"]
   U["Personal: VS Code profile and harness home folders"] --> D
   E["Extensions, plugins and configured locations"] --> D
   O["Organization guidance, where supported"] --> D
   D --> I["Applicable instructions included"]
   D --> S["Relevant skills loaded on demand or invoked"]
   I --> C["Context for the model request"]
   S --> C
```

For Copilot, applicable user and repository instructions are combined without a general
precedence order between instruction files. Local can also include multiple applicable
files. Do not assume that repository rules override personal rules, a more deeply nested
file always wins, or a skill overrides an instruction. Remove or narrow conflicting rules.
Other harnesses have their own combination rules; see the
[instruction combination reference](https://code.visualstudio.com/docs/agents/reference/custom-instructions-reference#_combine-instruction-sources).

Before a workshop demo, inspect both **Instructions** and **Skills**, including their source
paths. A fresh chat clears conversation history, not installed customizations. For a
controlled before/after comparison, disable the relevant personal customization first,
start a new chat, and verify what was actually included using
[Appendix B](appendices.md#appendix-b-debugging-and-analyzing-copilot-activity-optional).

For optional background on tokens and cost, see
[Appendix A](appendices.md#appendix-a-tokens-and-cost-optional).

[Previous: Workshop README](../README.md) | [Next: Create an App](02-create-an-app.md)
