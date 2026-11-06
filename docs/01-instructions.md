# Instructions

[Previous: Workshop README](../README.md) | [Next: Skills](02-skills.md)

## Why This Workshop?

LLMs can write code, but their probabilistic nature means the same request can produce
different implementations. Instructions and skills guide those choices toward your team's
conventions and repeatable approaches. They can make the work more consistent, not deterministic.

In this workshop, you will generate an API, capture your preferences as instructions and
skills, then repeat the original request in a fresh chat. Compare the before and after:
what followed your conventions, what still varied, and what needed fewer corrections?
You will leave knowing how to create, apply and verify reusable guidance.

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

## 2. Why Instructions?!

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

[Previous: Workshop README](../README.md) | [Next: Skills](02-skills.md)
