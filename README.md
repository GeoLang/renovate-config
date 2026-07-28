# renovate-config

Shared [Renovate](https://docs.renovatebot.com) config for the GeoLang org. Every repo
extends `default.json`, so dependency policy is set here once instead of drifting across
~20 copies.

## Using it

Each repo has a `renovate.json` at its root:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["local>GeoLang/renovate-config"]
}
```

## Policy

- Weekly, Monday before 06:00, max 5 open PRs per repo.
- Releases must be at least 3 days old, so a compromised package gets caught
  upstream before a PR proposes it. Repos may also gate installs (viewtopia's
  pnpm check requires 24h), which this 3-day floor always satisfies.
- GitHub Actions bumps land as one grouped PR, labelled `ci`.
- Cargo and npm minor/patch land as one grouped PR each. Majors stay separate so a
  breaking bump is never mixed with routine updates.
- Lockfile maintenance runs monthly.
- Everything is labelled `dependencies`; the Dependency Dashboard issue tracks the rest.

Renovate auto-detects package managers, so no per-ecosystem list is needed. This replaced
Dependabot in July 2026; the old `dependabot.yml` declared ecosystems by hand, which is
how viewtopia ended up requesting `cargo` updates for a repo with no Cargo.toml.

## Changing policy

Edit `default.json`. Every repo picks it up on its next run. Validate before pushing:

```bash
pnpm dlx --package renovate renovate-config-validator default.json
```

## License

AGPL-3.0-or-later, see [LICENSE](LICENSE).

Copyright (C) 2026 Grok Image Compression Inc.
