# Taskfile is Great

[Taskfile](https://taskfile.dev) is a modern task runner written in Go. It uses a simple YAML format
(`Taskfile.yml`) to define tasks and their dependencies.

## Why Taskfile?

- **No magic, just YAML** — readable, and easy to onboard new contributors
- **Cross-platform** — works on Linux, macOS, and Windows
- **Built-in features** — variable interpolation, dependencies, includes, watch mode, and more
- **Single binary** — no runtime dependencies, fast startup

## Taskfile vs Makefile

Makefiles are powerful, but they carry decades of baggage:

- Whitespace is significant — a space instead of a tab silently breaks everything
- Designed for build systems, not task runners phony targets, order-only prerequisites, and pattern
  rules are noise when you just want `make test`
- Portability is painful — GNU Make vs BSD Make behavior differs, and many tricks rely on bash-isms
- No native support for variables with defaults, namespacing, or task documentation
- Looks like shell, but variables (`$`) work very differently in a hard to understand way.

Taskfile does one thing well: running tasks. The syntax is straightforward and the error messages
are helpful.

## Taskfile vs Just

[Just](https://just.systems) is a fine tool and solves many of the same Makefile problems. My
preference for Taskfile comes down to:

- **YAML over a custom DSL** — `Justfile` syntax is its own language you have to learn and remember.
  YAML is already everywhere in modern toolchains (CI configs, Docker Compose, Kubernetes). One
  mental model, not two.
- **Structured data** — because it's YAML, tasks are first-class structured objects. Descriptions,
  aliases, environment variables, and dependencies are explicit fields, not comment conventions.
- **Includes** — Taskfile supports splitting tasks across multiple files and including them, which
  scales well in monorepos. Just's import support is more limited.
- **Active ecosystem** — Taskfile has good editor support, a well-maintained CLI, and clear
  versioning of the schema.

If you're starting fresh, Taskfile's YAML foundation makes it easier to integrate with the rest of
your infrastructure-as-code tooling.

## Getting Started

[Getting Started](https://taskfile.dev/docs/getting-started)

## Missing Feature

One feature I still miss is native support for "skip this task unless files in
this path changed since the last successful run".

In this repository, the `lint` task works around that gap with
[`scripts/git-worktree-has-changed.sh`](./scripts/git-worktree-has-changed.sh).
The script stores a stamp file after a successful run and checks whether any
tracked or untracked, non-ignored file under the target path is newer than that
stamp.

That gives you fast incremental behavior for expensive tasks, but it feels like
something Taskfile could support directly. A built-in primitive for
content-aware task freshness would be simpler, more portable, and easier to
reuse across projects.

[git-worktree-has-changed.sh](https://github.com/guettli/git-tips/blob/main/scripts/git-worktree-has-changed.sh)
