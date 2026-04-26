# Copilot Instructions — <your-project>

## Purpose

<Describe your project in 2-3 sentences. What does it do? Who is it for?>

---

## Repository Structure

```
<your-repo>/
├── src/                        # Application source code
├── tests/                      # Test suites
├── .github/
│   ├── agents/                 # AI agent definitions (PM, Tech Lead, Builder, QA)
│   ├── skills/                 # Reusable skill modules for agents
│   ├── specs/                  # Product specs (created by PM agent)
│   ├── plans/                  # Implementation plans (created by Tech Lead)
│   └── copilot-instructions.md # This file
├── .copilot/
│   └── mcp.json                # MCP server configuration
└── ...
```

---

## Conventions

<!-- Customize these to match your project -->

- **Package manager:** <npm / pnpm / yarn>
- **Test framework:** <vitest / jest / mocha / etc.>
- **Language:** <TypeScript / JavaScript / Python / etc.>
- **Module format:** <ESM / CommonJS>
- **Monorepo?** <Yes (tool) / No>

---

## Documentation Hygiene

When modifying **CLI commands, pipeline behavior, or agent specs**, update the corresponding documentation to reflect the change.

- **New CLI flag or command?** → Update the relevant README.
- **New or modified agent?** → Update the agent's `.agent.md` file.
- **New environment variable?** → Add it to `.env.example` with a comment.

---

## Workspace-Specific Instructions

<!-- If you have a monorepo, list each workspace and its scope here -->

| Workspace | Instructions | Scope |
|-----------|-------------|-------|
| `src/` | Main application | Core logic |
