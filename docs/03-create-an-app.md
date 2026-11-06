# Create An App

[Previous: Skills](02-skills.md) | [Next: Modify the App](04-modify-the-app.md)

## 1. Setup Branch/Agent

- Sync this branch `origin/main`, then create a new local feature branch e.g. '[username]/workshop1'
- In vscode Github Copilot chat extension;
  - Set Agent to be "Agent" this is to increase workshop velocity.
  - Set Model model to be either "Auto" or a different model from your neighbour, this is to allow comparisions.
  - Set Permissions to "Allow all" this is to increase workshop velocity, not to be used in productive environments.

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

## 5. ...if the dotnet build fails you'll need your first skill! (optional)

In the MakerSpace repository;

- Create an empty file on the path .github/skills/dotnet-restore/SKILL.md
- Paste in content which will be shared with you...

Then prompt Copilot:

```text
Retry build and launch the endpoint in a vscode browser tab.
```

## 6. Commit All Changes

Prompt Copilot:

```text
Commit all changes to the current feature branch.
```

[Previous: Skills](02-skills.md) | [Next: Modify the App](04-modify-the-app.md)
