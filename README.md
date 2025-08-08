# Custom Prompts for AI Coding Agents

This project provides a set of prompts for AI coding agents to implement a structured, two-step Plan & Execute workflow, inspired by the principles of [Kiro](https://kiro.dev/docs/specs/concepts/).

The primary motivation behind this methodology is to minimize the time and prompt iterations required to generate working code, improve overall code quality, and enable seamless switching between AI coding agents (Cursor, Kiro, Claude, Gemini, etc.) within the same project.

Inspired by [Kiro-style Prompts for Spec-Driven Development](https://github.com/andreskull/spec-driven-ai-coding), this implementation significantly simplifies and emphasizes documentation-driven workflows. Feature specifications are captured in a dedicated `prd.md` file, creating a shared source of truth that persists beyond individual chat sessions. This enables team collaboration and work resumption while utilizing Planner and Executor operational modes.

## The Core Workflow: Plan & Execute

The methodology is split into two distinct phases, each handled by a dedicated custom AI agent mode.

1. **Planning phase (Planner mode):** The AI acts as a senior architect. You provide a high-level feature description, and the AI guides you through an interactive process to create a complete technical specification.
1. **Execution phase (Executor mode):** The AI acts as a meticulous engineer. It reads the approved specification and implements the feature, ensuring strict adherence to the plan.

## Setting Up the Workflow

This workflow uses two distinct AI personas, each with a specific role. The setup process varies between different AI coding agents.

### Cursor (custom modes)

**1. Planner Mode (The Architect):**

- Mode Name: "Planner"
- Model: Gemini 2.5 Pro or OpenAI o3 max (powerful models for complex planning)
- Prompt: Copy the entire content from `prompts/planner.md`
- Context: ✅ Enable "Full folder context"
- Tools: ✅ Enable "Search codebase", "Read file", "Edit & Reapply", "Run terminal" (to generate a feature PRD file)
- Automation: ❌ Disable auto-apply, auto-run, auto-fix (manual review required)

**2. Executor Mode (The Engineer)**

- Mode Name: "Executor"
- Model: Claude 4 Sonnet (fast, capable model)
- Prompt: Copy the entire content from `prompts/executor.md`
- Context: ✅ Enable "Full folder context"
- Tools: ✅ Enable all tools (file editing, terminal, etc.)
- Automation: ⚠️ Optional - Enable auto-apply edits, auto-run, and auto-fix errors for autonomous execution. Prefer manual review.

**3. Optional: Coder Mode (Isolated Engineer)**

- Mode Name: "Coder"
- Model: Claude 4 Sonnet (fast, capable model)
- Prompt: Copy the entire content from `prompts/coder.md`
- Context: ✅ Enable "Full folder context"
- Tools: ✅ Enable all tools (file editing, terminal, etc.)
- Automation: ⚠️ Optional - Enable auto-apply edits, auto-run, and auto-fix errors for autonomous execution. Prefer manual review.

[Check out how to set up custom modes in other AI assistant tools.](https://github.com/andreskull/spec-driven-ai-coding#setting-up-the-workflow-in-different-ai-tools)

## Workflow Usage

### Planner persona usage

1. Switch to _Planner_ persona in your chosen AI agent.
1. Provide a high-level feature description (e.g., "Here's the feature I want to build: [Describe your feature in detail]").
1. The _Planner_ will guide you through an interactive process to define the blueprint for your feature (_what_ you're building, for _whom_, and _why_) — `prd.md` in a new `specs/{feature-name}/` directory.
1. Review and approve to complete the plan.

### Executor persona usage

1. Switch to _Executor_ persona in your chosen AI agent.
1. The _Executor_ will automatically pick up the plan created by the _Planner_ mode and implement it.

### Optional: Coder persona usage

1. Switch to _Coder_ persona in your chosen AI agent.
1. It **does no**t know anything about any feature specs and **does not** require a _Planner_ to run first.
1. Give it a command in a prompt.
1. The _Coder_ will process the request based on the pre-defined rules.

## Other Approaches

- [Kiro-style Prompts for Spec-Driven Development](https://github.com/andreskull/spec-driven-ai-coding)
- [Two-step approach to AI coding](https://github.com/sapegin/two-step-ai-coding-modes)
- [🚀 AI Dev Tasks 🤖](https://github.com/snarktank/ai-dev-tasks)
- [Agent OS](https://github.com/buildermethods/agent-os)
- [Prompts for plan & execute workflow in Cursor](https://github.com/carlrannaberg/ai-coding)

## Author and License

[Nick Plekhanov](https://nikkhan.com/), a full stack engineer.

[CC0 1.0 Universal license](LICENSE).
