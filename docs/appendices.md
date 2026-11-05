---
description: Optional reference material on Copilot token costs and activity diagnostics.
---

# Appendices

## Appendix A: Tokens and cost (optional)

Instructions and skills are extra text supplied to the model behind the scenes. That text
uses input tokens and takes up space in its context window, even when you do not type it
into the chat yourself.

* Project-wide instructions are included in applicable requests. Long, always-on rules
   create a recurring context cost.
* C# instructions with `applyTo: '**/*.cs'` are automatically attached when Copilot creates
   or changes matching files. Their text can accompany successive model requests, not just
   the first prompt. Merely asking a read-only question about C# does not guarantee that
   pattern-based instructions are attached; task relevance or manual attachment can also load them.
* Skills load progressively. Copilot sees the available skills' names and descriptions,
   then selects a skill when those suggest it is relevant. The full body is loaded on demand,
   followed by supporting files only as needed. Discovery still costs tokens, and automatic
   selection is not guaranteed.
* Once loaded, a skill can remain in the conversation context and contribute to later
   requests. On-demand loading does not mean its token cost disappears immediately afterward.

Keep everyday rules short and narrowly scoped; put detailed, task-specific procedures in skills.
Both can also save tokens overall by reducing repeated explanations, failed attempts and rework.

More input tokens do not necessarily mean a proportional increase in your bill: actual charges
depend on your Copilot plan, selected model, billing rules and any prompt caching.

## Appendix B: Debugging and analyzing Copilot activity (optional)

Do not rely only on asking Copilot "Did you use my instructions?" Check the evidence for
the affected request. These are different stages:

| Stage | What to look for | What it does not prove |
| --- | --- | --- |
| Discovered | The file appears in customization diagnostics or the Agent Customizations editor. | That it was selected for this request. |
| Selected or loaded | Discovery events, response references, or a tool call reading the skill body. | That all of its content reached a particular model request. |
| Sent | The expected text appears in that request's prompt, context or tool results. | That the model followed it. |
| Followed | Generated code and tool activity satisfy the rule or procedure. | That the customization caused the behavior; compare a controlled before/after run. |

### Where to look

* Run **Chat: Open Customizations** from the Command Palette and inspect **Instructions**
   or **Skills** for the selected agent. Check the actual file path, not just its display name.
   In a dev container, inspect the container session, not a separate host window.
* In the Chat view's context menu, select **Diagnostics** to check discovered instructions
   and parsing errors. A missing file suggests a location, naming, discovery-setting or
   frontmatter problem. Available menus vary with the VS Code version and agent harness.
* Expand **References** on the response and inspect the agent's tool activity. For skills,
   look for a skill invocation or a read of the actual `SKILL.md` body. A name and description
   in the available-skill list mean discovery, not full-body loading.
* Run **Developer: Open Agent Debug Logs**, or choose **Show Agent Debug Logs** from Chat's
   `...` menu. Select the affected session and filter **Discovery**, model requests, tool calls
   and errors. Check whether the file was loaded, skipped or rejected, and why.
* Run **Developer: Show Chat Debug View**, or choose **Show Chat Debug View** from Chat's
   overflow menu. Inspect the affected request's **System prompt**, **User prompt**,
   **Context** and **Tool responses**. Search for the file path or a distinctive sentence
   from its body. A skill may enter context through a tool result, not the initial system prompt.
* Use the debug panel's **Summary** for reported token usage, tool calls, errors and duration;
   its **Agent Flow Chart** helps trace subagent work. If cache reuse is unexpectedly low,
   **Cache Explorer** compares consecutive requests. These are diagnostics, not a billing statement.
* For extension or connectivity errors, open **View > Output** and select **GitHub Copilot**
   or **GitHub Copilot Chat**, where available. Temporarily use **Developer: Set Log Level**
   to enable **Trace**, reproduce the problem, then restore the previous level.

### Capture before reproducing

Agent Debug Logs are a preview feature. Enable the appropriate experimental setting before
the run you want to inspect; capture is not retroactive:

* Agent Host sessions: `chat.agentHost.agentDebugLog.enabled`.
* Local harness sessions: `github.copilot.chat.agentDebugLog.fileLogging.enabled`;
   reload VS Code before using `/troubleshoot`.

Where supported, ask `/troubleshoot Which customizations were loaded, skipped or rejected?`
to analyze the captured events. Treat its answer as a summary to verify against those events.

### A quick verification exercise

1. Start a fresh chat with the intended agent, model and workspace.
2. Ask for a small change with an observable requirement, such as XML comments on a public C# method.
3. Check discovery, request content and the resulting code separately.
4. For a skill, compare automatic selection with an explicit `/skill-name` invocation in another
    fresh chat. If explicit invocation works but automatic selection does not, review the description
    and `disable-model-invocation`; if the slash command is missing, check `user-invocable`.
5. Change one condition at a time and repeat the same task. Check glob matching, malformed YAML,
    duplicate names, conflicting user/repository rules, approvals and failed tool calls.

A read of a skill or a reference badge is evidence of loading, not proof of compliance.
Conversely, an absent badge alone is not proof that the model never received the content.

Debug views and exports can contain source code, prompts, local paths and tool output.
Remove private data and credentials before screen-sharing or publishing logs.

References: [VS Code custom instructions](https://code.visualstudio.com/docs/copilot/customization/custom-instructions),
[VS Code skills](https://code.visualstudio.com/docs/copilot/customization/agent-skills),
the [Agent Skills specification](https://agentskills.io/specification),
[debugging chat interactions](https://code.visualstudio.com/docs/agents/agent-troubleshooting/chat-debug-view),
and [Copilot troubleshooting](https://code.visualstudio.com/docs/agents/agent-troubleshooting/troubleshooting).

[Back to introduction](01-introduction.md)

[Back to workshop README](../README.md)
