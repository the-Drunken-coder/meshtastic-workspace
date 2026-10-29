# Meshtastic workspace

One workspace and one issue tracker for coordinated Meshtastic development. The source repositories remain separate forks, registered here as Git submodules.

| Directory    | Push destination                                                                  |
| ------------ | --------------------------------------------------------------------------------- |
| `firmware/`  | [meshtastic-firmware](https://github.com/the-Drunken-coder/meshtastic-firmware)   |
| `protobufs/` | [meshtastic-protobufs](https://github.com/the-Drunken-coder/meshtastic-protobufs) |
| `python/`    | [meshtastic-python](https://github.com/the-Drunken-coder/meshtastic-python)       |

[All issues](https://github.com/the-Drunken-coder/meshtastic-workspace/issues) live here. The [FLRC specification](https://github.com/the-Drunken-coder/meshtastic-workspace/issues/1) includes six implementation tickets, research, and decision records. The coordinated software implementation was merged in [workspace PR #8](https://github.com/the-Drunken-coder/meshtastic-workspace/pull/8) on October 4, 2026. RF and real-board acceptance remain open.

## Current coordination state

The [October 8 explanatory handoff](https://github.com/the-Drunken-coder/meshtastic-workspace/issues/1#issuecomment-6063525228) is the entry point for the later fork work. It separates delivered software and verified fixes from default-off experiments and unfinished hardware/reliability/RF checks.

Workspace main `e872588` pins firmware `6c4e6f9`, Python `cff7d62`, and schema `82adae8`. The [coordinated merge record](https://github.com/the-Drunken-coder/meshtastic-workspace/issues/1) links the exact commits, builds and acceptance workflow. Successful native tests and board builds do not close real-board acceptance, prove field performance, or approve a normal FLRC transmission profile.

This workspace coordinates the Meshtastic forks. The later standalone W12 FLRC network experiment is separate work, not a new set of component pins in this repository. Keep its source and results separate from the fork's preserved evidence. Project specifications and verification records for these forks remain in the workspace issue tracker.

## Open the workspace

```sh
git clone --recurse-submodules https://github.com/the-Drunken-coder/meshtastic-workspace.git
cd meshtastic-workspace
```

For an existing workspace clone:

```sh
git submodule update --init
git submodule sync --recursive
git submodule update --init --recursive
```

The workspace records exact component commits. A fresh clone checks components out at those revisions. Before editing a component, switch to its working branch, for example:

```sh
git -C firmware switch flrc
git -C protobufs switch flrc
git -C python switch flrc
```

## Apply coordinated changes

1. Make and verify changes in the owning component. Use its normal tests and formatting.
2. Commit and push each changed component to its own fork.
3. Record the resulting component revisions in a workspace commit and push the workspace.
4. Record results against the workspace issue. Keep unfinished acceptance checks open.

Reference workspace issue URLs in component commits and PRs. Issues are disabled on the child forks. PRs, when explicitly requested, belong to the repository whose code they change.

The standalone `protobufs/` checkout owns schema changes. Firmware and Python also keep the nested protobuf checkouts expected by their existing generation tools. These checkouts fetch from our protobuf fork, preserving each project's existing schema revision. The initial schema revisions differed. The coordinated implementation generated compatible firmware and Python bindings and recorded the reviewed component pins together. Keep those pins coordinated; do not treat their historical mismatch as unfinished configuration implementation.

Local research snapshots are preserved in ignored `.scratch/`. Published planning records are in GitHub Issues. The root MCP configuration targets `firmware/` for device/build tools; workspace setup does not perform device operations.
