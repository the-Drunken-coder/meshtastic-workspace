# Meshtastic workspace

This repository coordinates the Meshtastic forks listed in `.gitmodules`. All project issues, specifications, decisions, and verification records live in GitHub Issues on `the-Drunken-coder/meshtastic-workspace`.

## Working together

Prefer small, clear solutions. Explain system behavior and tradeoffs in plain language. Answer exploratory questions without changing files; complete authorized action requests without repeatedly asking permission. Preserve existing work. Write concisely and use no em dashes.

## Before working

- Read `docs/agents/issue-tracker.md` before creating, fetching, claiming, or updating tracker records.
- Read the relevant component's instructions before changing its files. For firmware, read `firmware/AGENTS.md` and `firmware/.github/copilot-instructions.md` first.
- Use the Matt Pocock skills exposed under `.agents/skills/`, which points to the firmware's vendored skill definitions. Their tracker configuration comes from this workspace.
- Use `docs/agents/triage-labels.md` for label roles and `docs/agents/domain.md` for domain-document conventions.
- A ticket label describes readiness. Follow the user's current authorization for implementation, publishing, commits, and hardware operations.

## Component ownership

| Directory    | Owns                                                     |
| ------------ | -------------------------------------------------------- |
| `firmware/`  | Firmware behavior, hardware variants, and firmware tests |
| `protobufs/` | Authoritative protocol schema changes                    |
| `python/`    | Python API, configuration client, and CLI                |

Each component is a Git submodule with its own fork remote and history. Change and test code in the component that owns it. Push its commits to that fork before updating the workspace's recorded submodule commit. Keep workspace commits limited to coordination files and component revision pins.

Firmware and Python retain their existing nested protobuf checkouts because their build and generation tools expect them. Their nested submodule URLs point to the protobuf fork so schema commits from our fork can be fetched. Their initial schema revisions differ. During an authorized schema change, coordinate the standalone schema commit, nested revision pins, generated firmware bindings, and generated Python bindings through each project's normal workflow. Generate bindings; do not hand-edit generated firmware files.

## Issues and pull requests

- Use the workspace tracker for work across all components. Child fork Issues are disabled.
- Reference the workspace issue URL or qualified `the-Drunken-coder/meshtastic-workspace#<number>` in component commits and PRs.
- Apply a result to the issue only after its acceptance criteria are met. Preserve unfinished hardware and measurement checks.
- Open a PR only when explicitly requested, ready for review. Follow the destination repository's title and validation conventions and end its description with the model and harness used.
- Rebase onto the intended latest target before opening a PR, preserving stacked PR bases. Merge only when explicitly authorized.
- Create tracker records and pushes in the operator's workspace/forks. Upstream issues or PRs require an explicit user request.

## Files and verification

Keep specifications and research records in the workspace's GitHub Issues. `.scratch/` contains ignored local working files and historical snapshots. Keep it out of commits. Firmware's documentation restrictions still apply inside `firmware/`.

Use component conventions, proportional abstractions, and behavior-based tests. Run required formatting and checks for files being committed. Do not broaden testing after appropriate checks pass without a new reason.

Use subagents for independent investigation or review when useful; assign file ownership before parallel edits. Device resets, erases, reboot/shutdown, and destructive history operations require operator authorization. Sequence serial operations one at a time per port and preserve device identities and keys.
