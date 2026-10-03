# Meshtastic workspace

One workspace and one issue tracker for coordinated Meshtastic development. The source repositories remain separate forks, registered here as Git submodules.

| Directory    | Push destination                                                                  |
| ------------ | --------------------------------------------------------------------------------- |
| `firmware/`  | [meshtastic-firmware](https://github.com/the-Drunken-coder/meshtastic-firmware)   |
| `protobufs/` | [meshtastic-protobufs](https://github.com/the-Drunken-coder/meshtastic-protobufs) |
| `python/`    | [meshtastic-python](https://github.com/the-Drunken-coder/meshtastic-python)       |

[All issues](https://github.com/the-Drunken-coder/meshtastic-workspace/issues) live here. The [FLRC specification](https://github.com/the-Drunken-coder/meshtastic-workspace/issues/1) includes six implementation tickets, research, and decision records. Feature implementation has not begun.

## Open the workspace

```sh
git clone --recurse-submodules https://github.com/the-Drunken-coder/meshtastic-workspace.git
cd meshtastic-workspace
```

For an existing workspace clone:

```sh
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

The standalone `protobufs/` checkout owns schema changes. Firmware and Python also keep the nested protobuf checkouts expected by their existing generation tools. Their schema revisions are initially different; coordinating them and regenerating bindings is part of the future configuration implementation.

Local research snapshots are preserved in ignored `.scratch/`. Published planning records are in GitHub Issues. The root MCP configuration targets `firmware/` for device/build tools; workspace setup does not perform device operations.
