# CLI Design Guidelines

This repository serves as a set of established guidelines for designing and structuring command-line interface (CLI) applications, derived from the patterns observed in `contextual` and `fcp-cli`.

This repo contains a set of design guidelines for CLI tools. Inspiration is taken from a couple of existing tools:

- [contextual](https://github.com/jefflunt/contextual)
- [fcp-cli](https://github.com/jefflunt/fcp-cli)

Both of the above tools have an `agent_docs/` folder, which is described in [agent_docs](https://github.com/jefflunt/agent_docs)

---

## 1. Project Structure

Organize projects to clearly separate public entry points from internal logic.

*   `cmd/`: Entry points for the application.
*   `internal/`: Core business logic, not importable by external projects.
*   `pkg/`: Shared libraries usable by other projects.
*   `script/`: Standardized scripts for development operations (`build`, `test`, `install`).
*   `agent_docs/`: Agent-first architectural and pattern documentation (mandatory for complex projects).

## 2. CLI Behavior and Streams

Respect Unix philosophy principles to enable piping and automation.

*   **Stdout (`stdout`):** Reserved exclusively for data/results. This ensures output can be cleanly redirected or piped (`|`) to other tools.
*   **Stderr (`stderr`):** Reserved for logs, warnings, and error messages.
*   **Graceful Degradation:** Where possible, if optional configuration is missing, log a warning to `stderr` and continue functionality rather than failing immediately.

## 3. Interaction Design

*   **Arguments vs. Configuration:**
    *   Use **arguments** for simple, immediate actions (e.g., `tool <item>`).
    *   Use **configuration files (YAML/JSON)** for complex, stateful, or high-configuration tasks (e.g., `tool --config config.yaml`).
    *   **Configuration Location:** Store user-specific configuration files in a hidden directory within the user's home directory (e.g., `~/.toolname/config.yml`) to maintain state and preferences across invocations without needing flags for every run.
*   **Usage Contract:** Clearly define what CLI arguments are expected and what the tool will do with them in `README.md`.

## 4. Documentation

Documentation is a first-class citizen, designed for both humans and AI agents.

*   **README.md Requirements:**
    *   **Build/Installation:** Simple, reproducible steps.
    *   **Usage Examples:** Concrete, executable examples.
    *   **Output Contract:** A description of what will be printed to `stdout` (e.g., block formats).
*   **Agent-First Docs (`agent_docs/`):** For non-trivial projects, maintain a directory dedicated to architectural overview, design patterns, and deep dives. These are based on the patterns established in [agent_docs](https://github.com/jefflunt/agent_docs). This helps AI agents understand the codebase instantly without searching the entire project.

---

### Conflicts
In cases of design conflicts, these guidelines favor the structure and behavior of the `contextual` repository.
