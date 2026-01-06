# OpenCode Orchestrator Agents

A collection of specialized AI agents for [OpenCode](https://opencode.ai) designed to automate full-stack web application development through orchestrated workflows.

## Overview

This repository contains a set of custom OpenCode agents that work together to build complete web applications from concept to implementation. The agents follow a structured, Agile-inspired workflow that handles everything from requirements gathering to deployment-ready code, with strict user-guided checkpoints.

**Tech Stack**: Vite + React + Tailwind CSS (frontend), Express + TypeScript + SQLite (backend)

## Compatibility

These agents are specifically designed for **OpenCode** and use OpenCode's agent configuration format. They will not work with other AI coding assistants.

Learn more about OpenCode agents: https://opencode.ai/docs/agents/

## Agent Types

### Primary Agent

**orchestrator** - The main coordinator that manages all subagents. It follows a strict "Stop-and-Wait" protocol, never executing a task without explicit user confirmation.
- **Mode**: `primary`
- **Purpose**: Orchestrates the development lifecycle via Sprint Planning and Task Execution.

### Subagents

The orchestrator manages these specialized agents:

1. **problem_expander** - Translates high-level user requests into detailed, comprehensive problem statements
   - Creates: `agent_works/PROBLEM.md`

2. **tasker** - Manages the project backlog and generates detailed sprint plans.
   - Creates: `agent_works/TASKS.md` (Global Backlog)
   - Creates: `agent_works/SPRINT_XX.md` (Active Execution Plan)

3. **architect** - Designs the technical foundation of the application
   - Creates: `agent_works/DB.md` (database schema)
   - Creates: `agent_works/FRONTEND.md` (UI architecture)
   - Creates: `agent_works/API.md` (API specifications)

4. **backend_developer** - Implements the backend using Express, TypeScript, and SQLite
   - Follows specifications from architect's design documents

5. **frontend_developer** - Implements the frontend using Vite, React, and Tailwind CSS
   - Integrates with backend APIs and follows UI architecture

6. **scripter** - Creates utility scripts for running, testing, and debugging the application
   - Updates `package.json` scripts or creates standalone shell scripts

7. **archiver** - Archives completed sprints to keep the workspace focused
   - Moves completed sprints to `agent_works/archives/part_N/`

## Installation

### Global Installation

Place the agent files in your OpenCode global configuration directory:

```bash
mkdir -p ~/.config/opencode/agent
cp agent/*.md ~/.config/opencode/agent/
```

### Project-Specific Installation

Alternatively, install them for a specific project:

```bash
mkdir -p .opencode/agent
cp agent/*.md .opencode/agent/
```

## Usage

### Starting a New Project

1. Open OpenCode in your project directory
2. Switch to the `orchestrator` agent (use Tab key or configured `switch_agent` keybind)
3. Describe your web application idea:

```
I want to build a task management app with user authentication, 
project boards, and real-time collaboration features.
```

The orchestrator will guide you through:
1. **Clarification**: Asking questions to refine the idea.
2. **Backlog Creation**: Generating `agent_works/TASKS.md`.
3. **Sprint Planning**: Asking you which backlog items to tackle first.
4. **Execution**: Creating `agent_works/SPRINT_01.md` and executing tasks one by one, waiting for your approval after each step.

### Continuing an Existing Project

If `agent_works/SPRINT_XX.md` exists, the orchestrator will read the active sprint and propose the next pending task.

```
Next Task: [BE-01] Setup Express Server
Assignee: @backend_developer
Description: Initialize the server with Helmet and CORS.
Shall I proceed?
```

### Invoking Specific Agents

You can also invoke individual subagents using @ mentions:

```
@architect please design the database schema for a blog platform

@backend_developer implement the user authentication endpoints

@scripter create a database seeding script
```

## Workflow

The workflow follows an Agile Sprint model:

1. **Inception**
   - User Request -> `PROBLEM.md`
   - Problem Analysis -> `TASKS.md` (Backlog)

2. **Sprint Planning**
   - User Selects Backlog Items -> `SPRINT_XX.md`
   - Tasker generates detailed execution steps with Assignees and Verification steps.

3. **Execution Loop**
   - Orchestrator reads `SPRINT_XX.md`.
   - Proposes next task to User.
   - User Confirms.
   - Subagent executes task.
   - Verification (Tests/Checks).
   - Repeat until Sprint is done.

4. **Closure**
   - Sprint Archived.
   - User selects new items for next Sprint.

## Project Structure

When you use these agents, they create and manage the following structure:

```
agent_works/
├── PROBLEM.md          # Detailed problem statement
├── TASKS.md            # Global Project Backlog
├── SPRINT_01.md        # Active Execution Plan
├── DB.md               # Database schema design
├── FRONTEND.md         # Frontend architecture
├── API.md              # API specifications
└── archives/           # Historical project phases
    ├── part_1/
    ├── part_2/
    └── ...
```

## Configuration

All agents use `google/antigravity-gemini-3-flash` as their default model. You can override this in your OpenCode configuration:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "agent": {
    "orchestrator": {
      "model": "anthropic/claude-sonnet-4-20250514"
    }
  }
}
```

## Learn More

- [OpenCode Documentation](https://opencode.ai/docs/)
- [Agent Configuration Guide](https://opencode.ai/docs/agents/)
- [OpenCode GitHub](https://github.com/anomalyco/opencode)

## License

These agents are provided as-is for use with OpenCode.
