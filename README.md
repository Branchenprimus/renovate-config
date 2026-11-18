# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) policy for the Branchenprimus
apps. Each repository contains only:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>Branchenprimus/renovate-config"]
}
```

Policy changes are made here once and apply to every repository.

## Policy

- Runs weekly (Monday before 6:00, Europe/Berlin); security fixes at any time.
- All minor/patch updates of a repository are grouped into one PR.
- Majors get their own PR. Python, Node.js and Go runtime upgrades are separate
  PRs, because they can break an app even as a "minor" image update.
- PostgreSQL and Nextcloud majors are only proposed after approval in the
  repository's Dependency Dashboard issue (data migration required).
- Docker base images and GitHub Actions are pinned to digests/SHAs.
- Lock files are refreshed monthly.
- No automerge: most app repos have no pull-request test workflow yet. Enable
  automerge per repository only once its PRs run tests.
- Own images (`ghcr.io/branchenprimus/*`) are excluded. They are released by
  tag through each app's release workflow, which updates `auto-app-deploy`.

## Deploying an update

Merging a Renovate PR does not deploy anything. App releases still start with a
`v*` tag in the app repository. `auto-app-deploy` updates (third-party images in
`apps/*.yaml`, platform manifests) are deployed by Argo CD after merge.

## Operation

Renovate runs as the Mend Renovate GitHub App. It must be installed for this
repository and every repository that extends it.
