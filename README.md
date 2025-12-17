# RHEL 9 Reference Scanner (ESP)

This repository is a minimal reference implementation of a RHEL 9 compliance scanner built on the Endpoint State Policy (ESP) execution engine.

It demonstrates how declarative ESP policies are executed against live system data to validate endpoint configuration and security posture—without SCAP, XCCDF, or XML.

**The goal of this project is clarity and correctness, not completeness.**

---

## What This Is

- A working RHEL 9 scanner that executes ESP policies
- A concrete example of policy → collection → evaluation
- A reference for building custom compliance scanners using ESP

This repo intentionally avoids abstraction layers, orchestration, or platform generalization. It shows the minimum viable scanner needed to execute real policies on a real system.

## What This Is Not

- A full-featured compliance platform
- A replacement for production SCAP tooling (yet)
- A general-purpose scanning SDK

Those belong in higher-level projects built on top of this pattern.

---

## Why This Exists

SCAP/XCCDF-based tooling is:

- XML-heavy
- Difficult to extend
- Hard to reason about operationally
- Actively avoided by engineers

ESP provides a declarative, executable alternative that separates:

- policy definition
- execution logic
- platform-specific collection

This repository proves that model works on a real RHEL 9 system.

---

## Repository Structure

```
.
├── esp/                # ESP policies
│   └── *.esp
├── src/
│   ├── main.rs         # Scanner entry point
│   └── ...             # Minimal execution logic
├── Dockerfile          # Devcontainer environment
├── .devcontainer/      # VS Code devcontainer config
└── README.md
```

---

## Requirements

This project is intended to be run inside a VS Code devcontainer.

### Why a Devcontainer?

- Ensures a RHEL 9–compatible environment
- Avoids host OS differences
- Provides consistent tooling and permissions
- Matches how scanners would run in controlled environments

---

## Running the Scanner

### 1. Open in VS Code Devcontainer

1. Install:
   - VS Code
   - Docker
   - Dev Containers extension
2. Open this repository in VS Code
3. Select "Reopen in Container"

### 2. Run the Scanner

From inside the devcontainer:

```bash
./scanner esp
```

Runs all ESP policies in the `esp/` directory.

Or run a specific policy:

```bash
./scanner esp/<policy>.esp
```

Example:

```bash
./scanner esp/rhel9_file_permissions.esp
```

---

## Output

The scanner executes the policy and evaluates system state according to the ESP definition. Results are emitted directly to stdout in a structured, deterministic format suitable for:

- local validation
- CI experimentation
- downstream result processing

(Reporting and orchestration are intentionally out of scope for this repo.)

---

## Intended Audience

- Security engineers
- Platform engineers
- Compliance automation teams
- Researchers evaluating alternatives to SCAP/XCCDF

If you understand why SCAP is painful, this repo will make immediate sense.

---

## Relationship to ESP

This scanner depends on the ESP execution model but is intentionally narrow in scope.

For:

- the ESP language
- the compiler
- the execution engine
- the full documentation

See the main [Endpoint State Policy repository](https://github.com/CurtisSlone/Endpoint-State-Policy).

**This is a reference point—not a finished product.**