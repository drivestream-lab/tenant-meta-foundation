# Tenant meta foundation (cookiecutter)

Static layout for `<client>-meta` repos. Factory YAML (`config/*.yaml`) is **not** here — Launchpad renders those from `OnboardingSpec`.

## Use

```bash
cookiecutter . --no-input meta_repo=my-meta org=my-org display_name=MY client_id=my
```

Launchpad consumes this via profile `tenant-meta`:

- `launchpad scaffold-meta --apply`
- `launchpad onboard apply` (greenfield meta)

Override template path: `LAUNCHPAD_META_FOUNDATION=/path/to/tenant-meta-foundation`

Default remote: `gh:drivestream-lab/tenant-meta-foundation`

Harness PM surface: `launchpad sync-harness-meta --apply` (writes `.harness-pin.yaml`, `skills-lock.json`, root `AGENTS.md` in tenant meta — not in this foundation).
