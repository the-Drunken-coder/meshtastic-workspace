# Issue tracker: GitHub

Specs, tickets, wayfinding maps, and decision records live in GitHub Issues on `the-Drunken-coder/meshtastic-workspace`. GitHub is the source of truth. Use the `gh` CLI with `--repo the-Drunken-coder/meshtastic-workspace`, or the matching `repos/the-Drunken-coder/meshtastic-workspace/...` API endpoint.

## Artifact kinds

Apply one artifact-kind label to each new artifact, alongside its triage, category, and `wayfinder:*` labels:

| Artifact                                     | Label         |
| -------------------------------------------- | ------------- |
| Specification                                | `kind:spec`   |
| Individual implementation or decision ticket | `kind:ticket` |
| Wayfinder planning map                       | `kind:map`    |

Create a missing kind label on this workspace tracker before publishing, then verify it on the new artifact. If label creation is unavailable, report it. Preserve existing parent labels when creating children. Scratch drafts remain unpublished working files.

## Tracker operations

- Publish a spec or ticket with `gh issue create --repo the-Drunken-coder/meshtastic-workspace --title "..." --body-file <file>`.
- Fetch a ticket with `gh issue view <number> --repo the-Drunken-coder/meshtastic-workspace --comments`. Read its body, labels, and comments before acting.
- List work with `gh issue list --repo the-Drunken-coder/meshtastic-workspace --state open --json number,title,body,labels,assignees` and the relevant label filters.
- Write discussion or evidence with `gh issue comment <number> --repo the-Drunken-coder/meshtastic-workspace --body-file <file>`.
- Update labels with `gh issue edit <number> --repo the-Drunken-coder/meshtastic-workspace --add-label "..." --remove-label "..."`. Use the role mapping in `triage-labels.md`.
- Close a ticket with `gh issue close <number> --repo the-Drunken-coder/meshtastic-workspace` after recording the result and satisfying its acceptance criteria.

Use temporary files for multiline bodies. `.scratch/` may hold drafts, research working files, or archived exports, but a skill instruction to publish to the tracker means create or update a GitHub issue. Published bodies must link to GitHub issues, issue comments, or accessible source URLs rather than local scratch files.

## Specs and implementation tickets

- Store the specification in a parent issue and publish one issue per approved ticket. Keep the spec open until its own acceptance criteria are met.
- Link each ticket to its parent as a native sub-issue. Add it with `gh api --method POST repos/the-Drunken-coder/meshtastic-workspace/issues/<parent>/sub_issues -F sub_issue_id=<child-database-id>`.
- Create tickets in dependency order and apply `ready-for-agent` to approved, specified implementation tickets. A label describes readiness; current user authorization still controls execution. Record hardware or operator requirements in the body.
- Use native issue dependencies for blockers: `gh api --method POST repos/the-Drunken-coder/meshtastic-workspace/issues/<child>/dependencies/blocked_by -F issue_id=<blocker-database-id>`.
- Fetch database IDs with `gh api repos/the-Drunken-coder/meshtastic-workspace/issues/<number> --jq .id`. An issue number and a GraphQL node ID are different identifiers.
- Keep human-readable parent and blocker links in the body as well. If native relationships are unavailable, use the parent's task list and each child's `Part of #<parent>` and `Blocked by: #<number>` lines, and record the API limitation.

## Wayfinding operations

- The map is one issue labelled `wayfinder:map`, containing Notes, Decisions-so-far, and Fog. Store decision details in child issues and link them from the map.
- Child issues use `wayfinder:research`, `wayfinder:prototype`, `wayfinder:grilling`, or `wayfinder:task`. Attach them as native sub-issues and wire blockers as above.
- Find the frontier by fetching the map's open sub-issues with `gh api repos/the-Drunken-coder/meshtastic-workspace/issues/<map>/sub_issues --paginate`. Drop assigned children and those with open blockers. The first remaining child in map order is next.
- Read blockers with `gh api repos/the-Drunken-coder/meshtastic-workspace/issues/<child>/dependencies/blocked_by`. Closed blockers do not prevent starting a child; the issue's `issue_dependencies_summary.blocked_by` reports the live gate.
- Claim with `gh issue edit <number> --repo the-Drunken-coder/meshtastic-workspace --add-assignee @me` before beginning ticket work.
- Resolve by recording the answer in a comment, closing the child issue, then updating the map's Decisions-so-far with a short pointer to that answer. Preserve unresolved engineering and measurement gates.

## Pull requests as a triage surface

**PRs as a request surface: no.**

GitHub shares issue numbers with pull requests. Resolve an ambiguous reference with `gh issue view` and `gh pr view` on this workspace. Open a PR only when the user explicitly requests one, following the repository's PR rules.

## Repository scope

Create tracker records only on `the-Drunken-coder/meshtastic-workspace`. Never create issues or pull requests on upstream `meshtastic/firmware`. Keep `.scratch/` and `docs/agents/` out of branches proposed upstream.
