# OpenCode Orchestrator Agents

A collection of specialized AI agents for [OpenCode](https://opencode.ai) designed to automate full-stack web application development through orchestrated workflows.

## Overview

This repository contains a set of custom OpenCode agents that work together to build complete web applications from concept to implementation. The agents follow a structured workflow that handles everything from requirements gathering to deployment-ready code.

**Tech Stack**: Vite + React + Tailwind CSS (frontend), Express + TypeScript + SQLite (backend)

## Compatibility

These agents are specifically designed for **OpenCode** and use OpenCode's agent configuration format. They will not work with other AI coding assistants.

Learn more about OpenCode agents: https://opencode.ai/docs/agents/

## Agent Types

### Primary Agent

**orchestrator** - The main coordinator that manages all subagents and drives the development workflow from start to finish.
- **Mode**: `primary`
- **Purpose**: Orchestrates the entire development lifecycle by delegating tasks to specialized subagents

### Subagents

The orchestrator manages these specialized agents:

1. **problem_expander** - Translates high-level user requests into detailed, comprehensive problem statements
   - Creates: `agent_works/PROBLEM.md`

2. **tasker** - Breaks down requirements into actionable, granular tasks with clear checkboxes
   - Creates: `agent_works/TASKS.md`

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

7. **archiver** - Archives completed project phases to keep the workspace focused
   - Moves completed work to `agent_works/archives/part_N/`
   - Refreshes active work files with only pending tasks

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

The orchestrator will guide you through clarification questions, then automatically:
- Expand your request into a detailed problem statement
- Break it down into actionable tasks
- Design the database, frontend, and API architecture
- Implement the backend and frontend
- Create necessary utility scripts

### Continuing an Existing Project

If `agent_works/` already exists, the orchestrator will ask for confirmation before continuing and may suggest archiving completed work to keep the workspace focused.

### Invoking Specific Agents

You can also invoke individual subagents using @ mentions:

```
@architect please design the database schema for a blog platform

@backend_developer implement the user authentication endpoints

@scripter create a database seeding script
```

## Workflow

The standard full-stack development workflow follows these phases:

1. **Clarification** - Orchestrator asks questions to understand requirements
2. **Requirement Expansion** - Problem statement created
3. **Task Breakdown** - Detailed task list generated
4. **Technical Architecture** - Database, frontend, and API designed
5. **Backend Implementation** - Server-side code and database implemented
6. **Frontend Implementation** - User interface built and integrated
7. **Utility Scripting** - Helper scripts created
8. **Checkpointing & Archiving** - Completed work archived, workspace refreshed

## Project Structure

When you use these agents, they create and manage the following structure:

```
agent_works/
├── PROBLEM.md          # Detailed problem statement
├── TASKS.md            # Actionable task checklist
├── DB.md              # Database schema design
├── FRONTEND.md        # Frontend architecture
├── API.md             # API specifications
└── archives/          # Historical project phases
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
