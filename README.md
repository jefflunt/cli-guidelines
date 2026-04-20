# CLI Design Guidelines

This repository serves as a set of established guidelines for designing and structuring command-line interface (CLI) applications. The first draft of these guidelines were derived from the patterns observed in the following repos:

- [contextual](https://github.com/jefflunt/contextual)
- [fcp-cli](https://github.com/jefflunt/fcp-cli)

NOTE: Both of the above tools have an `agent_docs/` folder, which is described in [agent_docs](https://github.com/jefflunt/agent_docs). See the "LLM-friendly documentation" section below.

---

## Prefer sub-commands over flags to alter behavior

A good design:

```
<cmd> <sub-cmd> [argument ...]
git status
git log
git help
```

## `help` should be a sub-command, not a flag

If your CLI provides help, make it a sub-command rather than a flag. If your CLI has a complex manual

Do this:

```
contextual help
fcp-cli help
```

Not this:

```
contextual --help
fcp-cli --help
```

## Easy to configure

```
~/.<tool name>/config.yml
~/.contextual/config.yml
~/.fcp-cli/config.yml
```

## Reserve `-v` / `--verbose` and `-c` / `--config`  flags

### `-v` / `--verbose`

Reserve the `-v` / `--verbose` flag for producing verbose/debug ouput, and print debug output to `stderr` (see streams, below). When the user doesn't use `-v` / `--verbose`, then the user should only get standard output suitable for piping to other commands.

### `-c` / `--config`

By default, configuration for a CLI tool should localed in `~/.<cli name>/config.yml`. But users should be able to specify the path to a config file using the `-c` / `--config` flag.

## Easy to build & install

```
script/
    build               # just builds
    test                # just tests
    install             # just installs
    build-test-install  # does all three
```

## Cross-platform, and ready-to-use

If building a tool for adoption by others, provide zero-dependency compiled binaries that users can:

- directly download
- put in their `PATH`
- run from anywhere
- run without an external DLLs, `libc`, or anything else
- are cross-platform if possible

In other words, give users a ready-to-use executable. The one exception to the no-dependency rule is that your CLI can depend on other no-dependency CLI tools in order to combine/compose their capabilities. But by not trying to do too much, you allow your tool to be recombined with other tools more easily.

## CLI Behavior and Streams

Respect Unix philosophy principles to enable piping and automation.

*   **`stdout`:** reserved exclusively for data/results. This ensures output can be cleanly redirected or piped (`|`) to other tools.
*   **`stderr`:** reserved for logs, warnings, and error messages.

## Prefer `go` as the programming language

`go` is preferred for the following reasons:

- in many cases, `go` supports statically linked, zero-dependency binaires
- the simplicity of `go` make it easy to learn, and easy for people and LLMs to write
- `go` supports cross-compilation out of the box

## Standard folder structure

Organize projects to clearly separate public entry points from internal logic.

*   `cmd/`: Entry points for the application.
*   `internal/`: Core business logic, not importable by external projects.
*   `pkg/`: Shared libraries usable by other projects.
*   `script/`: Standardized scripts for development operations (`build`, `test`, `install`).
*   `agent_docs/`: Agent-first architectural and pattern documentation (mandatory for complex projects).

## LLM-friendly documentation

Documentation is a first-class citizen, designed for both humans and AI agents.

*   **README.md Requirements:**
    *   **Build/Installation:** Simple, reproducible steps.
    *   **Usage Examples:** Concrete, executable examples.
    *   **Output Contract:** A description of what will be printed to `stdout` (e.g., block formats).
*   **Agent-First Docs (`agent_docs/`):** For non-trivial projects, maintain a directory dedicated to architectural overview, design patterns, and deep dives. These are based on the patterns established in [agent_docs](https://github.com/jefflunt/agent_docs). This helps AI agents understand the codebase instantly without searching the entire project.
