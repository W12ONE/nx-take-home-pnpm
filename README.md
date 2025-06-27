# Git Metrics Gathering CLI (PNPM Workspace)

> **Note:** This is the _sanity check_ version of the tool. Tests exist but are located in the full Nx workspace repo and not fully up to date since I made small changes while creating this.

This monorepo contains a CLI tool built with **TypeScript**, structured as a **PNPM workspace**, designed to analyze Git contributor metrics across multiple packages in a monorepo.

---

## 🛠 Tech Stack

- **TypeScript** – Type-safe implementation
- **Commander** – For CLI argument parsing
- **simple-git** – Lightweight Git wrapper for accessing commit data
- **tsx** – Allows running `.ts` files without precompilation
- **pnpm** – Efficient package manager and workspace orchestrator

---

## 📦 Project Structure

- `apps/git-metrics-gathering-cli-app/`: Entry point for the CLI
- `packages/cli-tools/`: Shared internal libraries
  - `git/data-access/` – Git operations using `simple-git`
  - `git/metrics/` – Contributor aggregation logic
  - `project-discovery/` – Multi-package detection
  - `readme-manager/` – Utility to update the README from CLI output (optional)

---

## ✅ What It Does

Given the path to a monorepo, the CLI tool:

- Detects all local package directories (e.g. `packages/*`)
- Extracts contributor data for each package
- Identifies contributors who worked across multiple packages

---

## 🧪 Tested Scenarios

The CLI was successfully run against:

- ✅ [`count-contributors-sample`](https://github.com/nrwl/count-contributors-sample)
- ✅ [`nrwl/nx`](https://github.com/nrwl/nx)

When run against the full Nx monorepo:

- **Contributors to `packages/`:** 769
- **Cross-project contributors:** 229
- **GitHub reports total contributors:** 1,064

The discrepancy is expected due to GitHub counting contributions outside of the `/packages` folder.  
This tool deliberately scopes its analysis to `packages/*` for performance and relevance.

---

## ⚙️ Usage

### 1. Install & Build

```bash
# Install dependencies
pnpm i

# Compile the TypeScript code (optional if running via tsx)
pnpm build
```

### 2. Run the CLI

You can either use the predefined scripts or run the CLI directly.

#### Option A: Predefined scripts (edit `package.json` paths to match your local environment)

The hardcoded paths (e.g., `/Users/Code/...`) must be updated to match your local system in `package.json`.

```bash
pnpm run run-app-test-folder   # Run against the sample repo
pnpm run run-app-nx            # Run against the Nx monorepo
```

#### Option B: Manual run

```bash
pnpm --filter git-metrics-gathering-cli-app exec tsx src/main.ts <path-to-repo>
```

---

## 🚧 Notes

While running the tool against the large-scale Nx monorepo, I noticed performance bottlenecks in how Git operations were handled.  
There's clear potential for optimization — especially around parallelisation.  
However, to stay within the **2.5-hour time budget** for this take-home assignment, no optimizations were implemented yet.
