# Build Docs (bdoc) Standard Library

This repository contains a set of OpenCode commands and agents designed to create a standardized, machine-readable interface between your codebase and AI agents.

## Core Concepts

### 1. The `build_docs/` Directory
The `bdoc` standard dictates that all project-level documentation, implementation plans, and error logs should reside in a `build_docs/` directory at the project root. This keeps the root clean and provides a "Source of Truth" for AI agents.

### 2. Standardized Error Reporting
Unlike traditional human-readable logs, `bdoc` requires **JSON Lines (.jsonl)** error logs placed in `build_docs/errors/`. This allows AI agents to parse, group, and diagnose errors without guessing.

**Required Schema:**
```json
{
  "timestamp": "ISO-8601",
  "level": "ERROR|CRITICAL",
  "error_id": "unique-fingerprint",
  "session_id": "UUID-for-execution-session",
  "message": "Summary of the error",
  "stack_trace": "Full raw trace",
  "context": { "component": "name", "metadata": {} }
}
```

## Tooling Ecosystem

This package provides a unified ecosystem of **Commands** and **Agents** that work together to automate the software development lifecycle.

### Specialized Agents

| Agent | Role | Core Responsibility |
|-------|------|----------------------|
| `bdoc_bug` | Researcher | Analyzes bugs, identifies root causes, and creates implementation plans. |
| `bdoc_feature` | Architect | Scans the codebase to design and plan new features. |
| `bdoc_engineer` | Builder | Executes implementation plans, writes code, and runs tests. |
| `bdoc_triage` | Diagnostician | Maps raw error logs back to the source code to find where things broke. |
| `bdoc_idea` | Sounding Board | Helps brainstorm and refine high-level concepts before they become plans. |

### Command Suite

#### Project Management & Documentation
| Command | Agent | Description |
|---------|-------|-------------|
| `/bdoc-init` | `build` | Initializes the `build_docs` structure and versioning. |
| `/bdoc-update` | `build` | Syncs docs with code and ensures `bdoc` compliance. |
| `/bdoc-version` | `build` | Displays the current `bdoc` version. |
| `/bdoc-status` | `plan` | Provides a high-level overview of project health and active plans. |
| `/bdoc-plans` | `plan` | List all implementation plans and their current status. |

#### Development Workflow
| Command | Agent | Description |
|---------|-------|-------------|
| `/bdoc-feature` | `bdoc_feature` | Research and plan a new feature. |
| `/bdoc-bug` | `bdoc_bug` | Research and plan a bug fix. |
| `/bdoc-engineer` | `bdoc_engineer` | Implement an existing plan file. |
| `/bdoc-verify` | `build` | Execute testing and verification steps from a plan. |
| `/bdoc-next` | `plan` | Analyzes pending plans and recommends what to build next. |
| `/bdoc-auto` | `general` | Automatically prioritizes and executes pending plans. |

#### Error Monitoring & Analysis
| Command | Agent | Description |
|---------|-------|-------------|
| `/bdoc-log-report` | `bdoc_triage` | Aggregates and summarizes error logs in `build_docs/errors/`. |
| `/bdoc-triage <id>` | `bdoc_triage` | Maps a log to code and suggests a `/bdoc-bug` command. |
| `/bdoc-audit-logs` | `build` | Searches for legacy logs and captured output for debugging. |

#### Maintenance
| Command | Agent | Description |
|---------|-------|-------------|
| `/bdoc-pkg-update` | `build` | Updates these tools from the upstream Standard Library repo. |

## Distribution & Installation

### Option A: Git Submodule (Recommended)
To use these tools in a new project and keep them updated:
```bash
git submodule add <this-repo-url> .opencode
```

### Option B: Direct Clone
```bash
git clone <this-repo-url> .opencode
```

## Versioning
The standard follows semantic versioning.
The version is tracked in each project via `build_docs/version.md`.
