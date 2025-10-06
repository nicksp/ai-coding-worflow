# Spec-driven AI Coding Workflow

This project provides a set of prompts for AI coding agents to implement a structured, two-step Plan & Execute workflow, inspired by the principles of [Kiro](https://kiro.dev/docs/specs/concepts/).

The primary motivation behind this methodology is to minimize the time and prompt iterations required to generate working code, improve overall code quality, and enable seamless switching between AI coding agents within the same project.

Inspired by [Kiro-style Prompts for Spec-Driven Development](https://github.com/andreskull/spec-driven-ai-coding), this implementation significantly simplifies documentation-driven workflow. Feature specifications are captured in a dedicated `prd.md` file, creating a shared source of truth that persists beyond individual chat sessions. This enables team collaboration and work resumption while utilizing Planner and Executor operational modes.

## The Core Workflow: Plan & Execute

The methodology is split into two distinct phases, each handled by a dedicated custom AI agent mode.

1. **Planning phase (Planner mode):** The AI acts as a senior architect. You provide a high-level feature description, and the AI guides you through an interactive process to create a complete technical specification.
1. **Execution phase (Executor mode):** The AI acts as a meticulous engineer. It reads the approved specification and implements the feature, ensuring strict adherence to the plan.

## Setting Up the Workflow

This workflow uses two distinct AI personas, each with a specific role. The setup process varies between different AI coding tools.

### Cursor (custom modes)

**1. Planner mode**

- **Name:** "☂ Planner"
- **Model:** Gemini 2.5 Pro or OpenAI GPT-5 (powerful models for complex planning)
- **Keybinding:** Cmd+Shift+1
- **Prompt:** Copy the entire content from `prompts/planner.md`
- **Context:** Enable "Full folder context"
- **Tools:** Enable "Search" section and "Edit" → "Edit & Reapply" (to generate a feature PRD file)
- **Automation:** Disable auto-apply, auto-run (manual review required)

**2. Executor mode**

- **Name:** "✈ Executor"
- **Model:** Claude Sonnet 4.5 (fast, capable model)
- **Keybinding:** Cmd+Shift+2
- **Prompt:** Copy the entire content from `prompts/executor.md`
- **Context:** Enable "Full folder context"
- **Tools:** Enable all tools (search, edit, run, etc.)
- **Automation:** Disable auto-apply edits, enable auto-run (for autonomous execution)

### Kilo Code (modes)

**1. Planner mode**

- **Name:** "☂ Planner"
- **API Configuration:** Gemini 2.5 Pro or OpenAI GPT-5 (powerful models for complex planning)
- **Save Location:** "Global" (available in all workspaces)
- **Role Definition:** Copy `<persona>` tag content from `prompts/planner.md`
- **Short description (for humans):** "Plan and design technical specification"
- **When to Use (optional):** "Use this mode when you need to plan, design, or strategize before implementation. Perfect for breaking down complex problems, creating technical specifications, designing system architecture, or brainstorming solutions before coding."
- **Available Tools:** Enable "Read Files", "Edit Files (Markdown files only)", "Use Browser"
  - Use this mode's `group` configuration to limit to markdown files only:

    ```yaml
    groups:
      - read
      - - edit
        - fileRegex: \.md$
          description: Markdown files only
      - browser
    ```

- **Custom Instructions (optional):** Copy the entire content except `<persona>` tag from `prompts/planner.md`

**2. Executor mode**

- **Name:** "✈ Executor"
- **API Configuration:** Claude Sonnet 4.5 (fast, capable model)
- **Save Location:** "Global" (available in all workspaces)
- **Role Definition:** Copy `<persona>` tag content from `prompts/executor.md`
- **Short description (for humans):** "Implement features based on pre-approved specification"
- **When to Use (optional):** "Use this mode when you need to implement a feature whose spec you approved during the Planning mode. Ideal for implementing features, fixing bugs, creating new files, or making code improvements across any programming language or framework."
- **Available Tools:** Enable "Read Files", "Edit Files", "Use Browser", "Run Commands", "Use MCP"
- **Custom Instructions (optional):** Copy the entire content except `<persona>` tag from `prompts/executor.md`

### Amp (modes via prompting)

Amp doesn't have mode selection by design. We can work around this "limitation" via prompt engineering by using the [AGENTS.md](https://ampcode.com/manual#AGENTS.md) file and defining our custom modes in there.

Create `AGENTS.md` file at a user level (`$HOME/.config/AGENTS.md`) or at the workspace root level with the following content:

```md
/Planner
[the entire content from `prompts/planner.md`]

/Executor
[the entire content from `prompts/executor.md`]
```

Now, when you prompt Amp, just switch any particular mode on by calling either:

```
/Planner
YOUR PROMPT HERE
```

or

```
/Executor
YOUR PROMPT HERE
```

Once "turned on", Amp will remain in the same mode throughout the session, so you don't need to repeat the enabling command with every prompt.

Starting a new thread resets the mode to "normal", or you can reset it manually by telling Amp you want to go into "normal" mode.

### GitHub Copilot (custom chat modes)

To edit modes, select **Configure Modes...** in the mode dropdown in the chat window.

Choose **User Data Folder** to make it available in all workspaces.

**1. Planner mode**

- **Name:** "☂ Planner"
- Copy the content below into the markdown file that opens automatically:

  ```md
  ---
  description: 'Plan and design technical specification'
  tools: ['codebase', 'usages', 'fetch', 'githubRepo', 'editFiles', 'search']
  model: Gemini 2.5 Pro (copilot)
  ---

  Copy the entire content from `prompts/planner.md`.
  ```

**2. Executor mode**

- **Name:** "✈ Executor"
- Copy the content below into the markdown file that opens automatically:

  ```md
  ---
  description: 'Implement features based on pre-approved specification'
  tools: ['codebase', 'usages', 'vscodeAPI', 'problems', 'changes', 'testFailure', 'terminalSelection', 'terminalLastCommand', 'openSimpleBrowser', 'fetch', 'findTestFiles', 'searchResults', 'githubRepo', 'extensions', 'editFiles', 'runNotebooks', 'search', 'new', 'runCommands', 'runTasks']
  model: Claude Sonnet 4.5 (Preview) (copilot)
  ---

  Copy the entire content from `prompts/executor.md`.
  ```

[Check out how to set up custom modes in other AI assistant tools.](https://github.com/andreskull/spec-driven-ai-coding#setting-up-the-workflow-in-different-ai-tools)

## Workflow Usage

1. Switch to the **Planner** persona in your AI tool.
2. Provide a detailed feature description (core functionality requirements, expected user interactions, any technical constraints or dependencies, etc.).
3. The Planner will guide you through an interactive process to create a feature blueprint — `prd.md` in a new `docs/specs/{feature-name}/` directory.
4. Review and approve to complete the plan.
5. Switch to the **Executor** persona.
6. The Executor will read relevant system documentation for context.
7. The Executor will automatically pick up the approved plan and implement it.

## Other Approaches

- [GitHub Spec Kit](https://github.com/github/spec-kit)
- [Kiro-style Prompts for Spec-Driven Development](https://github.com/andreskull/spec-driven-ai-coding)
- [Two-step approach to AI coding](https://github.com/sapegin/two-step-ai-coding-modes)
- [🚀 AI Dev Tasks 🤖](https://github.com/snarktank/ai-dev-tasks)
- [Agent OS](https://github.com/buildermethods/agent-os)
- [Prompts for plan & execute workflow in Cursor](https://github.com/carlrannaberg/ai-coding)

## Sponsoring

Software engineering runs on determination and energy — help me keep the engine running!

<a href="https://buymeacoffee.com/plekhanov" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/lato-yellow.png" alt="Buy Me A Coffee" height="51" width="217"></a>

## Author and License

[Nick Plekhanov](https://plekhanov.me/), a fullstack JS engineer.

[CC0 1.0 Universal license](LICENSE).
