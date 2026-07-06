# tenant-meta-foundation

**Cookiecutter layout for tenant meta workspaces** — the product-management and factory-control plane in a [Launchpad](https://github.com/drivestream-lab/launchpad) engineering org.

A `<client>-meta` repository holds PRDs, work manifests, wiki, and harness configuration for one customer or product line. This foundation scaffolds the **static folder layout**; factory YAML (`config/*.yaml`) is rendered separately by `launchpad onboard apply`.

| | |
|---|---|
| **License** | [MIT](LICENSE) |
| **Default remote** | `gh:drivestream-lab/tenant-meta-foundation` |
| **Harness profile** | `meta-pm` (PM skills — no rules submodule) |
| **Pairs with** | [launchpad](https://github.com/drivestream-lab/launchpad) · [prayog-skills](https://github.com/drivestream-lab/prayog-skills) |

---

## What this repo is

```text
launchpad (kit)                tenant-meta-foundation (this repo)
     │                                    │
     │  onboard apply                     │  cookiecutter layout
     ▼                                    ▼
<client>-meta/  ──────────────────►  prd/  work/  wiki/  playbook/
     │                               planning/  programs/
     │  renders config/*.yaml
     ▼
config/harness-<org>.yaml   gitflow / org / platform YAML
```

| Layer | Lives in | Purpose |
|-------|----------|---------|
| **PM narrative** | `prd/`, `planning/` | Initiatives, validation reports, pre-build archives |
| **Delivery control** | `work/` | Wave manifests (`INIT-*.yaml`) for board seeding |
| **Operator docs** | `wiki/`, `playbook/` | Onboarding, RBAC, client-specific process deltas |
| **Factory config** | `config/` | Rendered by Launchpad — **not** in this cookiecutter |
| **Harness overrides** | `templates/` | Optional pin / AGENTS templates (tenant wins over kit) |

**Private by design:** each `<client>-meta` repo is tenant-specific and typically stays private. This foundation is the **public template**; your rendered meta repo holds customer PRDs and secrets-adjacent planning.

---

## Generated layout

```text
<client>-meta/
├── README.md
├── prd/                  # Initiative PRDs (PM-owned)
├── work/                 # Dev-generated wave manifests
├── planning/             # Pre-build archives
├── programs/             # Program-level grouping
├── wiki/                 # Operator / onboarding docs
├── playbook/             # Client process deltas (kit SSOT: launchpad/playbook)
└── templates/            # Optional harness / AGENTS overrides
```

Factory YAML (`config/*.yaml`, `.github/`, gitflow policies) is added by **`launchpad onboard apply`** or copied from [launchpad examples](https://github.com/drivestream-lab/launchpad/tree/main/examples/tenant-meta).

---

## Quick start

### With Launchpad (recommended)

```bash
pipx install -e /path/to/launchpad   # once per machine

# Greenfield: render config + registry from OnboardingSpec
launchpad onboard apply --spec onboarding.yaml

# Apply this foundation's folder layout (preserves prd/, work/, config/ on upgrade)
launchpad scaffold-meta --apply --force

# PM harness: skills, .harness-pin.yaml, AGENTS.md
launchpad sync-harness-meta --apply
launchpad verify-harness-meta
```

Override template path: `LAUNCHPAD_META_FOUNDATION=/path/to/tenant-meta-foundation`

### Standalone cookiecutter

```bash
pip install cookiecutter
cookiecutter gh:drivestream-lab/tenant-meta-foundation

# Or from a local clone
cookiecutter /path/to/tenant-meta-foundation --no-input \
  meta_repo=acme-meta org=acme-org display_name=Acme client_id=acme
```

| Variable | Example | Description |
|----------|---------|-------------|
| `meta_repo` | `acme-meta` | Repository name for the tenant meta workspace |
| `org` | `acme-org` | GitHub org or GitLab group |
| `display_name` | `Acme` | Human-readable client name |
| `client_id` | `acme` | Registry id for `~/.config/launchpad/clients.yaml` |
| `forge_type` | `github` | `github` or `gitlab` |

---

## PM lane vs dev lane

| Lane | Workspace | Agent skills | Rules submodule |
|------|-----------|--------------|-----------------|
| **PM** | `<client>-meta` | [prayog-skills](https://github.com/drivestream-lab/prayog-skills) PM profile | None |
| **Dev** | App repos | prayog-skills dev profile | `*-rules` @ `.cursor/rules/` |

PM skills (`validate-requirements`, `prd-impact-map`, …) install **only** in meta. Dev skills (`spec-draft`, `pre-implement`, `verify`, …) install in app repos via `launchpad sync-harness-app`.

See [launchpad delivery workflow](https://github.com/drivestream-lab/launchpad/blob/main/playbook/delivery-workflow.md).

---

## Greenfield sequence

1. **`launchpad onboard apply`** — render `config/*.yaml`, client registry
2. **`launchpad doctor`** — token, tenant root, harness reachability
3. **`launchpad scaffold-meta --apply`** — this foundation's directories
4. **`launchpad setup-platform --apply`** — repos, teams, gitflow, board
5. **`launchpad sync-harness-meta --apply`** — PM harness surface
6. PM work in `prd/` → spec handoff → app repos scaffolded with [python-fastapi-foundation](https://github.com/drivestream-lab/python-fastapi-foundation) or your profile

**Kit upgrades:** `launchpad scaffold-meta --apply --force` overlays layout stubs; it does not delete `prd/`, `work/`, or `config/`.

---

## Maintainer notes

- Bump `VERSION` and tag on meaningful layout changes.
- Keep cookiecutter **generic** — no customer PRDs or private harness pins in this repo.
- CI: `.github/workflows/smoke.yml` validates cookiecutter output.

---

## Related repositories

| Repo | Role |
|------|------|
| [launchpad](https://github.com/drivestream-lab/launchpad) | CLI, playbook, onboarding, harness sync |
| [prayog-skills](https://github.com/drivestream-lab/prayog-skills) | PM + dev Cursor Agent skills |
| [python-fastapi-foundation](https://github.com/drivestream-lab/python-fastapi-foundation) | App repo scaffold (python-backend profile) |

---

## License

MIT — see [LICENSE](LICENSE).
