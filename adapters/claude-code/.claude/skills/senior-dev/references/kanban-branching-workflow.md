# Kanban Branching Workflow

Use this workflow for feature-level continuous promotion. Each environment may contain a different selected set of features.

## Branches and statuses

| Branch | Purpose | Typical status |
| --- | --- | --- |
| `main` | Production source of truth | `DONE` after release |
| `feat/*`, `fix/*` | Independent feature or bug-fix development | `IN DEV` |
| `develop` | Completed development selected for integration | `DEV DONE` |
| `sit` | Selected features undergoing system integration testing | `READY TO SIT` |
| `uat` | Selected SIT-passed features undergoing acceptance testing | `READY TO UAT` |
| `release/vX.Y.Z` | Only UAT-approved features for a production candidate | `READY TO PRODUCTION` |
| `hotfix/vX.Y.Z` | Production repair based on current `main` | release-controlled |
| `epic/*` | Approved feature integration when no production release occurs | no-production path |

## Standard promotion

```text
feat/* or fix/*
  -> develop
  -> sit
  -> uat
  -> release/vX.Y.Z
  -> main
  -> tag/release/deploy
```

1. Create each feature or fix branch from `main`.
2. Complete development and code review on the original branch.
3. Promote the selected branch to `develop`.
4. Promote only SIT-ready features to `sit`; features do not need to travel together.
5. If SIT finds a defect, fix it on the original feature branch, then promote it again through `develop` and `sit`.
6. Promote only SIT-passed features to `uat`; postpone other features independently.
7. Create `release/vX.Y.Z` after UAT approval and include only approved features.
8. Perform final verification, regression testing, release validation, and production preparation.
9. Merge the approved release branch into `main`, create the version tag or GitHub Release, and deploy only with explicit authorization.

## Hotfix

1. Create `hotfix/vX.Y.Z` from the current production `main`.
2. Develop, test, and release it through the controlled production workflow.
3. Merge the hotfix into `main`.
4. Do not merge the hotfix back into `develop`.
5. Before their next promotion, rebase every active feature branch onto the latest `main` so it inherits the production fix.

`main` remains the source of truth; production fixes flow from `main` to active feature branches through rebase.

## Cleanup after any production release

1. Merge the release into `main` and update the local/remote view of `main`.
2. Identify every still-active feature branch.
3. Rebase each active feature branch onto the latest `main`.
4. Resolve conflicts and verify affected behavior.
5. Update the remote feature branch using `--force-with-lease`, never an unguarded force push.
6. Continue normal promotion through `develop`, `sit`, and `uat`.

Rebase and force-push rewrite branch history. Inspect the exact targets and obtain explicit authorization before executing them.

## No-production release path

Follow normal promotion through `develop`, `sit`, and `uat`. Merge all approved feature branches into an `epic/*` integration branch, then merge `epic/*` into `main` with `[ci skip]` so history remains synchronized without triggering production CI/CD. Confirm that `[ci skip]` is supported by the actual pipeline before relying on it, and obtain explicit authorization before merging into `main`.

## Conflict strategy

Environment branches may intentionally contain different combinations of features. Conflicts commonly arise when independent features touch shared logic, helpers, formatting, imports, or the same files.

Choose based on dependency and release relationship:

- Create a new shared fix branch from `main` when several features need common code.
- Create a child branch from an unfinished feature when the new work temporarily depends on it.
- Resolve during promotion when the conflict exists only in a selected environment combination; then validate every affected feature.

Keep branches small and short-lived, rebase regularly, promote frequently, resolve conflicts early, and verify all affected features. Do not treat conflict resolution as a mechanical Git operation; preserve feature independence and combined behavior.

## Operational model

Think "Which environment is this feature in?" rather than "Which sprint contains it?" The release unit is the feature, release scope is selected feature branches, and `develop`, `sit`, and `uat` are not assumed to be identical snapshots.
