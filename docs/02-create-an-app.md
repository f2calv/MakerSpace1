# Create An App

[Previous: Introduction](01-introduction.md) | [Next: Modify the App](03-modify-the-app.md)

## 1. Setup Branch/Agent

- Sync branch `main`
- Set Github Copilot chat model to be "Auto"
- Set Github Copilot chat permissions to "Allow All"

## 2. Create the API

Prompt Copilot:

```text
We need a Web API to collect/organise feedback from colleagues and present that feedback to our manager.
Create a .NET 10 Web API project named 'FeedbackApi' in the src/ folder.
Expose a single REST resource /api/feedback with initial GET, POST, PUT, and DELETE operations.
Do not build.
Commit all changes.
```

## 3. Review the result (optional)

Review the generated files, or ask Copilot to explain the result:

```text
Walk me through the generated project structure and explain how each CRUD operation maps
to /api/feedback
```

## 4. (try to) Build and launch (optional)

Prompt Copilot:

```text
Build the project and launch the endpoint in a vscode browser tab.
```

## 5. ...if the build fails looks like you need your first skill! (optional)

In the MakerSpace repository;

- Create a folder path .github/skills/dotnet-restore
- Create a file SKILL.md
- Paste in content which will be shared with you...

Then prompt Copilot:

```text
Retry the build and launch the endpoint in a vscode browser tab.
```

## 5. Commit All Changes

Prompt Copilot:

```text
Commit all changes to the current feature branch.
```

[Previous: Introduction](01-introduction.md) | [Next: Modify the App](03-modify-the-app.md)
