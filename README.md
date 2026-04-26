# GitHub Copilot Agent Template — 4-Agent Workflow

A ready-to-use template for orchestrating GitHub Copilot coding agents as a **4-agent team**: Product Manager, Tech Lead, Feature Builder, and QA Reviewer.

## The Idea

Instead of using a single AI agent that tries to do everything, this template splits the software development lifecycle into **four specialized roles**, each with clear responsibilities, inputs, and outputs. The agents hand off work to each other through a structured pipeline — just like a real engineering team.

This creates:
- **Better specs** — the PM agent refines requirements interactively before anything gets built
- **Safer architecture** — the Tech Lead plans file-level changes before code is written
- **Focused implementation** — the Builder follows an approved plan, not vague instructions
- **Quality gates** — the QA agent validates every acceptance criterion before merge

## The 4 Agents

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Product    │     │   Tech      │     │   Feature   │     │     QA      │
│   Manager    │────▶│   Lead      │────▶│   Builder   │────▶│   Reviewer  │
│              │     │             │     │             │     │             │
│ Refines reqs │     │ Writes plan │     │ Implements  │     │ Validates & │
│ Writes specs │     │ Orchestrates│     │ Runs tests  │     │ Opens PR    │
│ Ships issues │     │ Dispatches  │     │ Pushes code │     │ Fixes issues│
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
```

### 1. Product Manager (`productmanager.agent.md`)

**Trigger:** User describes a feature or problem.

**What it does:**
- Refines requirements through interactive Q&A (not a wall of text)
- Decomposes features into user stories with testable acceptance criteria
- Creates spec files in `.github/specs/`
- Ships specs to GitHub as Feature issues + Task sub-issues
- Hands off to the Tech Lead

**Key principle:** User value first. Scope small. Say no often.

### 2. Tech Lead (`techlead.agent.md`)

**Trigger:** Approved spec exists in `.github/specs/`.

**What it does:**
- Reads the spec and writes a file-level implementation plan in `.github/plans/`
- Creates feature branches using **git worktrees** (never touches `main` working tree)
- Computes dependency waves (which user stories can be built in parallel)
- Dispatches Feature Builders in parallel per wave
- Triggers QA reviews after implementation
- Merges PRs, cleans up worktrees, tags releases
- Maintains an execution log for audit/resume

**Key principle:** Plan small, dispatch in parallel, never write production code.

### 3. Feature Builder (`featurebuilder.agent.md`)

**Trigger:** Dispatched by Tech Lead with a plan section + worktree path.

**What it does:**
- Validates it's inside a git worktree (safety check)
- Implements exactly what the plan says — no more, no less
- Writes tests as specified in the plan's Test Plan section
- Runs the full test suite
- Commits, pushes, and hands off to QA
- Logs every command to the execution log

**Key principle:** Surgical precision. No bonus features. Tests are mandatory.

### 4. QA Reviewer (`qareviewer.agent.md`)

**Trigger:** Feature Builder pushes a branch (dispatched by Tech Lead via `task` tool).

**What it does:**
- Validates every acceptance criterion with evidence
- Runs tests independently
- Fixes High/Medium issues directly on the branch
- Opens the PR as its seal of approval (the PR is born clean)
- Posts structured review output (verdict, AC coverage, test evidence, findings)
- Cleans up spec/plan files after all sub-issues pass

**Key principle:** Evidence-based. No pass without test proof. Strict quality bar.

## The Pipeline

```
User describes a feature
        │
        ▼
   ┌─────────┐
   │   PM    │  Refinement → Spec → GitHub Issues (#10 Feature + #11,#12,#15 Tasks)
   └────┬────┘
        │ approved spec + issues
        ▼
   ┌─────────┐
   │  Tech   │  Plan → feat/<slug> branch → Worktrees → Waves
   │  Lead   │
   └────┬────┘
        │ create feat/<slug> from main
        │ dispatch per wave on feat/<slug>-us<N>-i<issue#>
        ▼
   ┌─────────┐  ┌─────────┐  ┌─────────┐
   │ Builder │  │ Builder │  │ Builder │  (parallel within wave)
   │ US1 #11 │  │ US2 #12 │  │ US5 #15 │
   └────┬────┘  └────┬────┘  └────┬────┘
        │            │            │
        ▼            ▼            ▼
   ┌─────────┐  ┌─────────┐  ┌─────────┐
   │   QA    │  │   QA    │  │   QA    │  PR → Closes #issue
   │ US1 #11 │  │ US2 #12 │  │ US5 #15 │
   └────┬────┘  └────┬────┘  └────┬────┘
        │            │            │
        └────────────┼────────────┘
                     │ all PRs merged → feat/<slug>
                     ▼
              Wave 2 begins...
                     │
                     ▼
              Final PR: feat/<slug> → main
```

## Skills

Skills are reusable knowledge modules that agents reference for specific workflows:

| Skill | Purpose |
|-------|---------|
| `spec-writing` | Template and decomposition workflow for product specs |
| `github-shipping` | Creating Feature issues + Task sub-issues from approved specs |
| `issue-context-reading` | Navigating the spec → plan → issue hierarchy |
| `testing-rules` | Standards for running, writing, and reporting tests |
| `project-constraints` | Domain-specific architecture rules *(customize this)* |
| `implementation-reporting` | Required format for PR descriptions and handoff reports |

## Directory Structure

```
your-project/
├── .github/
│   ├── agents/
│   │   ├── productmanager.agent.md    # PM agent definition
│   │   ├── techlead.agent.md          # Tech Lead agent definition
│   │   ├── featurebuilder.agent.md    # Feature Builder agent definition
│   │   └── qareviewer.agent.md        # QA Reviewer agent definition
│   ├── skills/
│   │   ├── github-shipping/           # Issue creation workflow
│   │   ├── spec-writing/              # Spec template & decomposition
│   │   ├── issue-context-reading/     # Issue hierarchy navigation
│   │   ├── testing-rules/             # Test standards
│   │   ├── project-constraints/       # Domain constraints (customize!)
│   │   └── implementation-reporting/  # PR/summary format
│   ├── specs/                         # Product specs (created by PM)
│   ├── plans/                         # Implementation plans + execution logs (created by Tech Lead)
│   └── copilot-instructions.md        # Global Copilot instructions
├── .copilot/
│   └── mcp.json                       # MCP server configuration
├── .worktrees/                        # Git worktrees for parallel agent work (gitignored)
│   ├── <slug>/                        #   feat/<slug> — feature branch worktree
│   ├── us1/                           #   feat/<slug>-us1-i42 — user story worktree
│   ├── us2/                           #   feat/<slug>-us2-i43 — user story worktree
│   └── ...
└── README.md                          # This file
```

## Customizing for Your Project

### 1. Required Changes (search for these markers)

| File | What to customize |
|------|------------------|
| `.github/copilot-instructions.md` | Project description, repo structure, conventions, tech stack |
| `.github/skills/project-constraints/SKILL.md` | Architecture, domain rules, platform constraints |
| `.github/skills/github-shipping/SKILL.md` | Replace `<your-org>/<your-repo>` with your actual org/repo |
| `.copilot/mcp.json` | Add MCP servers your agents need (e.g., Supabase, database tools) |

### 2. Optional Customization

| File | What you might change |
|------|----------------------|
| `productmanager.agent.md` | Product principles, core product knowledge, target user |
| `techlead.agent.md` | Branch naming conventions, test commands, CI labels |
| `featurebuilder.agent.md` | Test framework commands, workspace-specific patterns |
| `qareviewer.agent.md` | Severity definitions, test tier requirements |

### 3. Placeholder Markers

All files that need customization contain `<!-- CUSTOMIZE: ... -->` comments or `<your-...>` placeholders. Search for them:

```bash
grep -r "CUSTOMIZE\|<your-" .github/ .copilot/
```

## Git Worktree Strategy

A key design decision: **all feature work happens in git worktrees**, never in the main working tree. This means:

- Multiple agents can work in parallel without conflicts
- The main worktree stays on `main` — other CLI sessions aren't disrupted
- Each user story gets an isolated checkout

```
.worktrees/
├── user-dashboard/        # feat/user-dashboard (feature branch)
├── us1/                   # feat/user-dashboard-us1-i42 (user story 1 → issue #42)
├── us2/                   # feat/user-dashboard-us2-i43 (user story 2 → issue #43)
└── us5/                   # feat/user-dashboard-us5-i46 (user story 5 → issue #46)
```

## Getting Started

### Step 1 — Clone or copy the template

```bash
# Option A: Use as a GitHub template
gh repo create my-project --template <this-repo> --clone

# Option B: Copy into an existing project
cp -r .github/ .copilot/ README.md /path/to/your-project/
```

### Step 2 — Create the working directories

```bash
mkdir -p .github/specs .github/plans .worktrees
echo ".worktrees/" >> .gitignore
```

### Step 3 — Set your org and repo name

Replace `<your-org>` and `<your-repo>` in the GitHub shipping skill:

```bash
# Find all placeholders
grep -rn "<your-org>\|<your-repo>" .github/skills/

# Edit:
#   .github/skills/github-shipping/SKILL.md
# Replace every <your-org> and <your-repo> with your actual values
```

### Step 4 — Describe your project

Edit `.github/copilot-instructions.md`:
- Replace `<your-project>` with your project name
- Fill in the **Purpose** section (2-3 sentences)
- Update the **Repository Structure** to match your actual layout
- Set your **Conventions** (package manager, test framework, language, etc.)

### Step 5 — Define your product context

Edit `.github/agents/productmanager.agent.md`:
- Fill in the `Core Product Knowledge` section (target user, core loop, differentiator, platform)
- Update the `Product Principles` section with your project's values

### Step 6 — Set your architecture constraints

Edit `.github/skills/project-constraints/SKILL.md`:
- Define your system architecture and module boundaries
- List domain rules agents must respect
- Specify your data layer, API contracts, and platform requirements

### Step 7 — Configure MCP servers (optional)

If your agents need external tools (database, APIs), add them to `.copilot/mcp.json`:

```json
{
  "mcpServers": {
    "your-service": {
      "command": "npx",
      "args": ["-y", "@your/mcp-server"],
      "env": {
        "API_TOKEN": "${{ secrets.YOUR_TOKEN }}"
      }
    }
  }
}
```

### Step 8 — Verify all placeholders are gone

```bash
grep -rn "CUSTOMIZE\|<your-" .github/ .copilot/
```

Fix any remaining placeholders. When this returns nothing, you're ready.

### Step 9 — Start building

Describe a feature to the **`@productmanager`** agent and let the pipeline flow:

> PM → spec → Tech Lead → plan → Builder → code → QA → merge

## How Agents Communicate

Agents don't talk to each other directly. They communicate through **artifacts**:

| From | To | Artifact |
|------|----|----------|
| PM → Tech Lead | `.github/specs/<slug>.md` + GitHub Feature issue |
| PM → GitHub | Feature issue (#N) + Task sub-issues (#N+1, #N+2, …) |
| Tech Lead → Builder | `.github/plans/<slug>.md` section + worktree path + issue number |
| Builder → QA | Git branch + implementation summary (commits reference `#issue`) |
| QA → Merge | PR (body includes `Closes #issue` to auto-link and auto-close) |
| All → Audit | `.github/plans/<slug>.execution.md` |

No agent needs to understand another agent's internals — only its **inputs and outputs**.
