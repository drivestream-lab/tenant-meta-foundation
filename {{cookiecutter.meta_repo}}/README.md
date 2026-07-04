# {{ cookiecutter.display_name }} meta

Tenant factory workspace for **{{ cookiecutter.org }}** (`{{ cookiecutter.meta_repo }}`).

Factory YAML (`config/*.yaml`) is rendered by `launchpad onboard apply` from your onboarding spec — not from this cookiecutter.

## Greenfield sequence

1. `launchpad onboard apply --spec onboarding.yaml` — render configs + registry
2. `launchpad doctor` — token + tenant root
3. `launchpad setup-platform --apply` — repos, gitflow, board, catalog
4. `launchpad sync-harness-meta --apply` — PM skills, `.harness-pin.yaml`, `AGENTS.md`
5. PM work in `prd/`, `work/` → push to `develop`
6. `launchpad scaffold-app --repo <app> --apply --force` + `sync-harness-app --apply`

Kit upgrades: `launchpad scaffold-meta --apply --force` (preserves `prd/`, `work/`, `config/`).
