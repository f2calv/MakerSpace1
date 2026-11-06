# Instructions

[Previous: Workshop Contents](README.md) | [Next: Skills](02-skills.md)

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

## 2. Why Instructions?

* State everyday conventions once instead of repeating them in each prompt.
* Make expectations explicit: "Document every public C# API" or "Keep controllers thin."
* Keep team rules beside the code, where they can be reviewed and maintained.
* Scope specialised rules to the files that need them rather than loading every rule everywhere.

Instructions guide the agent; they do not enforce compliance or replace builds, tests and
review. Keep them short, specific and free of private configuration. Verify that they were
included and that the resulting work actually follows them.

For supported locations and discovery, see
[Appendix C](appendices.md#appendix-c-where-do-instructions-and-skills-live-optional).
For checking what reached the agent, see
[Appendix B](appendices.md#appendix-b-debugging-and-analyzing-copilot-activity-optional).

## 3. Targeted instructions beyond .NET

Instructions can cover languages, infrastructure, pipelines and documentation. Keep each
topic in its own file under `.github/instructions/`, with an `applyTo` pattern matching
the files it governs. These complete, minimal examples are inspired by the
[shared instruction library](https://github.com/f2calv/.github/tree/main/.github/instructions)
and show different scopes.

### Go

Example file: `.github/instructions/go.instructions.md`

```markdown
---
name: Go conventions
description: Use when writing or reviewing Go code.
applyTo: '**/*.go,**/go.mod,**/go.sum'
---

* Use gofmt for formatting.
* Handle errors rather than discarding them.
```

### Docker

Example file: `.github/instructions/docker.instructions.md`

```markdown
---
name: Container builds
description: Use when creating or changing Dockerfiles and build contexts.
applyTo: '**/Dockerfile,**/Dockerfile.*,**/*.dockerfile,**/.dockerignore'
---

* Give every build stage a descriptive name.
* Keep build tools out of the runtime image.
* Prefer chiseled or other minimal runtime images for production when they support the application's runtime dependencies.
```

### GitHub Actions

Example file: `.github/instructions/github.actions.instructions.md`

```markdown
---
name: Workflow permissions
description: Use when creating or reviewing GitHub Actions workflows.
applyTo: '.github/workflows/**,.github/actions/**,**/action.yml,**/action.yaml'
---

* Declare an explicit permissions block for every job.
* Grant only the access each job needs.
* Use only a YML extension for files never YAML.
```

### Markdown

Example file: `.github/instructions/markdown.instructions.md`

```markdown
---
name: Documentation conventions
description: Use when writing or reviewing Markdown documentation.
applyTo: '**/*.md'
---

* Use descriptive link text rather than "click here".
* Use synthetic values in examples, never real credentials or private identifiers.
* Deep link to key files wherever possible, such as interfaces/abstractions and project configuration files.
```

Comma-separated patterns let one instruction file cover several related file types.
Keep the rules relevant to that scope: a Markdown edit should not need your Go conventions.
These are examples to adapt to your team, not extra files you need to create for this workshop.

[Previous: Workshop Contents](README.md) | [Next: Skills](02-skills.md)
