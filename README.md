# My Agent Configuration

This repository contains the configuration, rules, and workflows for my AI Agent (**Antigravity**). It defines how development should be conducted, serving as the "Brain" and "Operating Manual" for the agent.

## 📂 Structure

```text
.agent/
├── rules/               # Core principles and guidelines
│   ├── development-principles.md  # Code quality, Architecture, Git
│   ├── development-workflow.md    # The Step-by-Step process
│   ├── safety-rules.md           # Deployment & Ops safety boundaries
│   ├── workspace-setup.md        # Folder structure standards
│   └── retrospective.md          # How to improve these rules
└── workflows/           # Executable shortcodes
    ├── init.md          # /init
    ├── dev-req.md       # /dev-req
    ├── dev-plan.md      # /dev-plan
    ├── dev-go.md        # /dev-go
    ├── chk.md           # /chk
    ├── status.md        # /status
    ├── retro.md         # /retro
    └── learn.md         # /learn
```

## 🚀 Workflows (Shortcodes)

Use these shortcodes to trigger agent behaviors and automate steps. You can also **chain** them (e.g., `/dev-req /dev-plan`).

### Setup & Ops
| Command | Description |
| :--- | :--- |
| **`/init`** | **Initialize Workspace**. Sets up folders, `README`, and allows Tech Stack selection. |
| **`/chk`** | **Environment Check**. Verifies Git, GitHub CLI, and Project Docs. Auto-connects repo. |
| **`/status`** | **Project Status**. Summarizes current branch, active issue, and recent logs. |

### Development Cycle
| Command | Phase | Description |
| :--- | :--- | :--- |
| **`/dev-req`** | **Phase 1: Requirements** | Analyzes request, **creates GitHub Issue**, and **creates Git Branch**. |
| **`/dev-plan`** | **Phase 2: Plan** | Creates a detailed implementation plan (`docs/tasks/` or inline). |
| **`/dev-go`** | **Phase 3: Execution** | Develops code, runs tests, updates logs, updates GitHub Issue, and **creates PR**. |

### Continuous Improvement
| Command | Description |
| :--- | :--- |
| **`/retro`** | **Retrospective**. Creates a template to reflect on a sprint or task. |
| **`/learn`** | **Learn & Evolve**. Reads past retrospectives to propose updates to these rules. |
| **`/learn-agent`** | **Cross-Pollination**. Learns rules and best practices from other agent workspaces (Local/GitHub). |

## 📜 Core Rules

1.  **Development Principles**: strict rules on Code Quality (1k LOC limit), Architecture (Client-Server), and Logging.
2.  **Workflow**: The enforced lifecycle from Requirement -> PR.
3.  **Safety**: "Safety First" operations—no force pushes, no deleting without confirmation.
4.  **Workspace**: Standardized folder separation (`docs/`, `src/` or `frontend/`/`backend/`).

## 🛠️ Usage

1.  **Start a new project**: Run `/init`.
2.  **Verify tools**: Run `/chk`.
3.  **Start a feature**:
    *   Run `/dev-req` (Answer prompts to create Issue/Branch).
    *   Run `/dev-plan` (Map out the steps).
    *   Run `/dev-go` (Write code, test, and ship PR).
4.  **Finish**: Merge PR (manual or via agent assistance), then run `/retro` if needed.
