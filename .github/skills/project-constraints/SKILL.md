---
name: project-constraints
description: "Domain-specific constraints and architecture boundaries for this project. Use when making changes to core systems, APIs, or database schemas."
---

# Project Constraints

<!-- CUSTOMIZE: Replace all sections below with your project's actual architecture and constraints -->

## System Architecture

- `src/` — Main application source code
- `tests/` — Test suites
- Define your key modules and their boundaries here.

## Domain Rules

- List the core business rules that agents must respect.
- Example: "Never expose internal IDs in API responses."
- Example: "All user-facing text must be internationalized."

## Data Layer

- For DB changes, update migration + shared types consistently.
- Define your database technology and access patterns.

## API Contracts

- Define how APIs should be versioned and documented.
- List any protocol constraints (REST, GraphQL, WebSocket, etc.).

## Platform

- Define your target platforms and runtime requirements.
- List package manager, module format, and workspace conventions.
