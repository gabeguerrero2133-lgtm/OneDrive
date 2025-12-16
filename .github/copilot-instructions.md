Copilot / AI Agent Instructions
================================

Repository state: currently empty (only `desktop.ini`). These instructions help AI agents get productive once source files are present. Update this file with concrete examples as code is added.

- Purpose: Provide concise, repo-specific guidance for safely contributing changes.

- Quick discovery checklist (run first):
  1. Detect main language(s) by looking for `package.json`, `pyproject.toml`, `requirements.txt`, `go.mod`, or `Cargo.toml`.
  2. Locate the run-time entrypoints (`main.go`, `cmd/`, `src/index.js`, `src/main.py`, `server.js`).
  3. Identify tests (`tests/`, `__tests__`, `*_test.go`, `pytest.ini`, `jest.config.js`).
  4. Inspect infra and CI: `Dockerfile`, `docker-compose.yml`, `.github/workflows/*`, `deploy/`.
  5. Detect monorepo/workspaces: `packages/`, `workspaces` in `package.json`, or `modules/`.

- Agent workflow (ordered):
  - Scoping: Map top level directories and package manifests to service boundaries.
  - Build & Quick-run: If present, run `make build`, `npm ci && npm test`, `python -m pytest`, or `go test ./...`.
  - Local dev: Find `README.md` or `Makefile` to determine how to run the app locally. If none exist, check Docker compose or `scripts/` folder.
  - Tests and linters: Run the repo’s configured test commands and linters (e.g., `eslint`, `golangci-lint`, `flake8`).
  - PRs: Keep changes small and documented. Include commands used to verify change in PR description.

- Big-picture signals and where to find them:
  - Service boundaries: `services/`, `api/`, `cmd/`, `internal/`, `pkg/` usually show separation of concerns.
  - Shared libraries: `lib/`, `pkg/`, or `internal/` contain utilities and should be reused.
  - Config: check for `config/`, `env` loaders, and `*.env` files to understand configuration patterns.
  - Persistence: look for `migrations/`, `db/` or `orm` configuration for DB interactions.
  - Integration: inspect `routes/`, `controllers/`, `handlers/`, and `grpc/` for surface-level APIs.

- Detection examples (use these to find repo patterns):
  - Search for logging wrappers: `rg "logger|logging|log4j|winston|zap" -n`.
  - Search for database patterns: `rg "migrations|knex|typeorm|sqlalchemy|gorm" -n`.
  - Search for test runners: `rg "pytest|jest|mocha|go test|describe\(|it\(" -n`.

- CI/Build guidance:
  - Mirror commands used in `.github/workflows/*` locally before proposing changes to CI.
  - If new build steps are required, add them via clear, minimal changes to workflow files and explain why.

- Conventions & safety checks:
  - Avoid cross-cutting refactors without maintainer sign-off.
  - When changing public APIs or DB schema, include migration instructions and update tests.
  - Use existing error handling patterns; do not introduce ad-hoc patterns.

- How to update this file when code is present:
  1. Add explicit build/test commands, e.g., `npm ci && npm run build && npm test` or `python -m pytest -q`.
  2. Add examples showing where to find the main file, tests, CI, and deployment configs.
  3. Provide three concrete examples of patterns: logging, config, db access, with file references.

If you want, I can: (1) add a starter `README.md` and `Makefile`, (2) scaffold a basic language layout (Node/Python/Go), or (3) re-run detection after you add files and auto-update this doc with concrete, repo-specific examples.

Please confirm which next step you want me to take.
# Copilot / AI Agent Instructions

This repository currently contains no source files beyond a workspace configuration, so the instructions below are structured to help an AI agent quickly get productive once source files are present. When the project gains code, update this file with concrete examples and references to commonly used files and commands.

- Purpose: Provide actionable, repo-specific guidance so AI agents can make useful changes safely and consistently.

- Repo State: At the time these instructions were generated, the workspace is empty (only `desktop.ini`). If you see code, start by mapping the main directories (`src/`, `cmd/`, `pkg/`, `services/`, `app/`) and the build/test files (`Makefile`, `package.json`, `pyproject.toml`, Dockerfile`, `.github/workflows/`).

- Agent Workflow (ordered):
  - Scoping: Identify language(s) and entrypoints. Locate README.md, package manifests, and `main`/`index` files.
  - Build & Test: If `Makefile`, `package.json`, or `pyproject.toml` present, run common tasks: `make build`, `npm test`, `pytest`, or `poetry run`. For Go, run `go test ./...`.
  - Code Changes: Prefer small, well-scoped PRs. Add unit tests for new behavior where possible. Follow existing testing style.
  - CI: Inspect `.github/workflows/*` to align with CI steps. Update CI only with explicit instruction.

- Finding the "Big Picture":
  - Look for top-level service directories and shared libs. For example, if you find `services/auth` and `services/api`, infer boundaries from the `go.mod`, `package.json` or Dockerfiles.
  - Inspect `docker-compose.yaml` or Kubernetes manifests in `deploy/` for runtime interactions.
  - If multiple services exist, search for HTTP endpoints and grpc service definitions to map integration points.

- Patterns & Conventions (how to detect and follow):
  - Logging: Look for common logging wrappers (e.g., `logger` instance in `internal/logging`) and reuse existing patterns rather than introducing new ones.
  - Configuration: If `config/` or `.env` usage is present, prefer the project's configuration loader (do not add new env var patterns without following existing naming and defaulting). 
  - Errors: Follow existing error wrapping and type patterns; maintain consistent error codes/strings.
  - Database/ORM: Reuse the repo’s DB connection helpers. If migrations exist, prefer the repo's migration flow and directory.

- Testing and Linting:
  - Run test suite and lints before opening PR. Add to your PR description the commands used to verify changes.
  - Test discovery: Look for `pytest.ini`, `tox.ini`, `.eslintrc`, or `golangci.yml`.

- Non-negotiables for PRs:
  - Keep changes minimal and explain key design choices in the PR body.
  - If adding new dependencies, explain necessity and add minimal locked versions in manifest files.
  - Don't modify CI or infra unless the change is required for the PR's tests to pass.

- Helpful Commands (scan for these files, then run accordingly):
  - Python: `python -m pytest` or `poetry run pytest` if `pyproject.toml` present.
  - Node: `npm ci && npm test` or `yarn && yarn test`.
  - Go: `go test ./...`.
  - General: `make build`, `make test`.

- When repo is empty or missing documentation:
  - Ask the maintainers: Request the main language, build/test commands, and primary run target.
  - If asked to scaffold, pick minimal templates that match language conventions and document any assumptions.

- Safety and Team Conventions
  - Avoid broad refactors unless explicitly requested.
  - Any change to public APIs or DB schema must include migration plan and tests.

- Where to extend this file
  - After adding a primary language/service, include examples of the project’s startup command, the most common test command, and a small example of a well-structured PR.

If you want, I can: (1) add starter `README.md` and basic structure for a chosen language, (2) re-run detection when code is added, or (3) create a checklist for PRs tailored to the project once files exist. Which would you like next?