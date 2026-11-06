---
description: Hands-on workshop for customizing GitHub Copilot with instructions and skills.
---

# MakerSpace: Copilot - Lessons from the Field

<p align="center">
   <img src="docs/images/liam-neeson-skills.png" alt="Liam Neeson, what a legend!" width="480">
</p>

A hands-on, follow-along guide to customizing GitHub Copilot output with instructions and skills.

> Your output will differ from your neighbour's. That is expected.

## 1. Requirements

Choose one of the three setups below. All use VS Code and GitHub Copilot;
options 2 and 3 also let you build, run, and preview the .NET application.

### Shared Prerequisites

1. **An up-to-date [Visual Studio Code](https://code.visualstudio.com/)** with GitHub Copilot signed in.
   Check for updates with **Help > Check for Updates** (macOS: **Code > Check for Updates**).
2. **This repository downloaded or cloned** locally and opened in VS Code.
   If you have [Git](https://git-scm.com/downloads) installed, run these commands
   from the folder where you want to keep the repository:

   ```shell
   git clone https://github.com/f2calv/MakerSpace1.git
   code MakerSpace1
   ```

### Option 1: VS Code Only

No .NET SDK or Docker installation is needed. You can follow the prompting,
code-review, instructions, and skills exercises.

You cannot build or run the .NET application, or preview its API locally, with
this setup. Skip the workshop's build and launch steps and keep the repository
open locally rather than reopening it in a container.

### Option 2: VS Code with the Local .NET 10 SDK

Install the [.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0)
on your computer. Choose the **SDK**, not only the runtime.

Keep the repository open locally in VS Code. Docker and the Dev Containers
extension are not required. This setup lets you build and run the application
and preview its API in a browser.

In a new VS Code terminal, run:

```shell
dotnet --list-sdks
```

Confirm that the output includes a `10.0.x` SDK version before the session.

### Option 3: VS Code with a Dev Container

The dev container includes the .NET 10 SDK, so you can build, run, and preview
the application without installing the SDK directly on your computer.
Docker is required on your computer instead.

1. **On Windows, install [WSL 2](https://learn.microsoft.com/windows/wsl/install)
   and a Linux distribution first.** In Windows Terminal (as administrator), run
   `wsl --install`, restart if prompted, and complete the Linux distribution's
   first-run setup. Run `wsl --list --verbose` and confirm your default distribution
   (marked with `*`) uses version `2`. WSL is not required on macOS or Linux.
2. Install and start [Docker Desktop](https://www.docker.com/products/docker-desktop/).
   On Windows, use Linux containers and enable
   **Settings > General > Use the WSL 2 based engine**.

   In **Settings > Resources > WSL Integration**, enable
   **Enable integration with my default WSL distro**, then select **Apply & Restart**.
   This gives Docker Desktop access to your default WSL distribution.
   These WSL settings apply only on Windows.
3. Install the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
   in VS Code. Its extension ID is `ms-vscode-remote.remote-containers`.
4. Open this repository in its dev container **before the session**.

   VS Code may show a notification in the **bottom-right corner** saying that
   the folder contains a Dev Container configuration. Click **Reopen in Container**.

   If the notification does not appear, or you have dismissed it:

   * Press **Ctrl+Shift+P** (macOS: **Cmd+Shift+P**) to open the Command Palette.
   * Type **Dev Containers: Reopen in Container** and select that command.

   Wait for the image download, container setup, and extension installation to finish.
   The first launch can take several minutes, so complete it **before joining the call**.
5. Confirm the **bottom-left corner** shows **Dev Container: MakerSpace .NET 10**
   and the repository files appear in the **Explorer panel on the left**.

### Final Setup Check: Copilot Chat

For all three options, open the **Copilot Chat** window in VS Code and send a test
prompt, such as `Ready for the workshop?` Confirm that Copilot responds
before the session. Sign in to GitHub if prompted.

For option 3, perform this check after reopening the repository in the dev container.

## 2. The Only Rules of MakerSpace

1. **Only prompt.** Don't write code or edit files by hand; ask Copilot to do it.
2. **See rule 1.**

## 3. Workshop Pages

1. [Instructions](docs/01-instructions.md)
2. [Skills](docs/02-skills.md)
3. [Create an App](docs/03-create-an-app.md)
4. [Modify the App](docs/04-modify-the-app.md)
5. [Lessons Learned](docs/05-lessons-learned.md)

[Optional appendices](docs/appendices.md) cover tokens, cost, Copilot diagnostics and customization locations.
