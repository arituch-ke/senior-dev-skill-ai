# CI/CD Pipeline for the Kanban Workflow

Separate Continuous Integration for non-production promotion from controlled production release and deployment.

## Non-production: develop, SIT, and UAT

Trigger when a selected feature is promoted to `develop`, `sit`, or `uat`, or when an authorized manual CircleCI workflow is started.

The deployment orchestrator:

1. Checks out the target environment branch.
2. Resets the environment composition to the latest `main`.
3. Merges only the selected feature branches for that environment.
4. Builds an immutable Docker image.
5. Tags the image with the Git commit SHA.
6. Pushes it to the configured non-production container registry.

Deployment then follows GitOps:

1. Update the image tag in the Kubernetes deployment repository.
2. Commit the desired-state change.
3. Allow ArgoCD to detect and synchronize it.
4. Kubernetes pulls the immutable image and deploys the service.

Resetting an environment branch and changing the deployment repository are destructive or externally mutating actions. Inspect current state and obtain explicit authorization first.

## Production build

Trigger the production build when an approved `release/vX.Y.Z` or `hotfix/vX.Y.Z` is merged into `main`.

1. Build the production Docker image.
2. Tag it with the release version, such as `v1.2.1`, rather than a commit SHA.
3. Push it to the configured production container registry.

Keep published images immutable.

## Production release and deployment

Production deployment requires a manually created Git Release or Git tag targeting `main`.

1. Verify the version, `main` commit, image, approvals, and release contents.
2. Create the Git tag or GitHub Release only with explicit authorization.
3. Update the production Kubernetes repository to the released image tag.
4. Synchronize ConfigMaps and runtime configuration through the intended GitOps change.
5. Allow ArgoCD to synchronize the production cluster.
6. Verify rollout health and retain a tested rollback path.

Never create releases, update deployment repositories, or deploy production without explicit authorization.

## Invariants

| Concern | Develop / SIT / UAT | Production |
| --- | --- | --- |
| Trigger | Feature promotion or manual CircleCI | Release/hotfix plus Git Release or tag |
| Image tag | Git commit SHA | Release version |
| Registry | Configured non-production registry | Configured production registry |
| Desired state | Kubernetes deployment repository | Production Kubernetes repository |
| Reconciler | ArgoCD | ArgoCD |

- Deploy non-production automatically only after an authorized promotion.
- Version production with Git Releases/tags.
- Manage Kubernetes through GitOps rather than direct imperative deployment.
- Preserve deployment history and rollback through version-controlled desired state.
- Discover registry providers, projects, repositories, paths, credentials, CircleCI workflow names, Kubernetes repositories, ArgoCD applications, and auto-sync policy from the target project; do not reuse identifiers from examples or invent missing values.
