# Skills

[Previous: Instructions](01-instructions.md) | [Next: Create an App](03-create-an-app.md)

## 1. What are skills?

Reusable instructions for **how to complete a particular task**. Instructions set everyday
rules; a skill supplies the procedure for a specific job, such as diagnosing a failed build,
reviewing an API or improving a README.

Store each skill at `.github/skills/<skill-name>/SKILL.md`. Copilot uses its name and
description for discovery, loads the body when relevant, and reads supporting files as needed.
You can also invoke it through `/skill-name` when it is exposed in the slash-command menu.
For personal folders and other supported sources, see
[customization locations](01-instructions.md#3-where-do-instructions-and-skills-live).

## 2. Start with a short procedure

Minimal example for `.github/skills/build-project/SKILL.md`:

```markdown
---
name: build-project
description: Build the .NET project and explain failures. Use when asked to build.
---

1. Identify the requested project; ask if the target is ambiguous.
2. Check that the required .NET SDK is available.
3. Run `dotnet build` against the project path.
4. Summarize the result and explain any errors. Do not run tests or deploy.
```

A useful skill states when to use it, what inputs it needs, the steps and decisions,
and how to verify the result. Include stopping conditions rather than encouraging the
agent to guess when prerequisites or permissions are missing.

Keep the scope narrow: a build skill need not also own releases, deployments and API design.
Skills can cover non-coding tasks too, such as investigating pipelines or refining work items.

## 3. Add supporting files only when needed

A skill can bundle scripts, templates and reference material:

```text
.github/skills/build-project/
   SKILL.md
   references/
      build-failures.md
   scripts/
      check-sdk.ps1
```

These extra folders are optional. Link to supporting files from `SKILL.md` and say when
to read or run them. Keep the main procedure short; move detailed troubleshooting into
references and repeatable mechanical work into reviewed scripts.

Bundling a script does not execute it or grant permission to run it. A skill guides the
agent; it does not install tools, change terminal commands or enforce compliance.

## 4. Frontmatter reference

Only `name` and `description` are required. The following shows fields documented by
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

## 5. Verify discovery and behavior

1. Open **Chat: Open Customizations** and check the skill's name and source path.
2. In a fresh chat, ask for a task matching its description, such as "Build the project."
3. Check whether the skill was loaded, then inspect the commands and result separately.
4. Compare with an explicit `/build-project` invocation in another fresh chat.
5. Improve the existing description or procedure when selection or execution misses the goal.

A loaded skill is not proof that its steps were followed. A fresh chat also retains installed
personal and extension customizations; check those before comparing workshop results.
See [Appendix B](appendices.md#appendix-b-debugging-and-analyzing-copilot-activity-optional)
for diagnostics and [Lessons Learned](05-lessons-learned.md) for maintaining shared guidance.

References: [VS Code agent skills](https://code.visualstudio.com/docs/copilot/customization/agent-skills)
and the [Agent Skills specification](https://agentskills.io/specification).

[Previous: Instructions](01-instructions.md) | [Next: Create an App](03-create-an-app.md)