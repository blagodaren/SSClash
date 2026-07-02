# projects/SSClash AGENTS.md

This directory inherits the workspace rules from `../../AGENTS.md`.

## What this is

This is our **working fork** of SSClash: `https://github.com/blagodaren/SSClash`.

Unlike `sources/SSClash` (read-only upstream truth from `zerolabnet/SSClash`),
this checkout is editable. Use it for:

- Patches, experiments, and PRs we intend to push back to the fork.
- OpenWrt/LuCI integration work that builds on the upstream package.
- Local testing of router-side changes before they land upstream.

Do not treat this directory as a knowledge source. For factual claims about
SSClash behavior, still consult `sources/SSClash` (matched to a known commit
via `knowledge/maps/ssclash-map.md`).

## Source priorities (when this project is in scope)

1. `projects/SSClash/` for in-flight changes we own.
2. `sources/SSClash/` for upstream behavior we have not changed.
3. `sources/mihomo/` (branch `Meta`) only when kernel behavior is needed.
4. `sources/Meta-Docs/` for user-facing configuration semantics.

If our fork has diverged from upstream on a given file, `projects/SSClash`
wins for our build; cite both paths when explaining behavior.

## Stack

- Shell + Lua (LuCI app under `luci-app-ssclash/`).
- Packaged for OpenWrt via the package Makefile in the repo root.
- Runtime kernel: mihomo (Clash.Meta).

## Working commands

From the workspace root:

```bash
# Pull latest from the fork
git -C projects/SSClash fetch --all --prune
git -C projects/SSClash checkout main
git -C projects/SSClash pull --ff-only

# After making changes inside projects/SSClash:
git -C projects/SSClash status
git -C projects/SSClash add -p
git -C projects/SSClash commit -m "..."
git -C projects/SSClash push origin main

# Then bump the parent repo's pin to the new submodule commit:
git add projects/SSClash
git commit -m "Bump projects/SSClash to <short-sha>"
```

## Tests / validation

- LuCI views and shell scripts: run on a real OpenWrt target with mihomo
  installed; SSH in and exercise the LuCI pages.
- Config validation: `/opt/clash/bin/clash -t -d /opt/clash -f /opt/clash/config.yaml`.
- Cross-check any kernel-facing change against `sources/mihomo` to confirm
  the option/field actually exists at the kernel version SSClash ships.

## Notes

- Upstream remote (read-only reference) lives at `sources/SSClash`. We do not
  add it as a second remote inside `projects/SSClash` — keep boundaries clean.
- AGENTS.md inside a submodule is untracked from the parent's perspective.
  Commit it into the fork (`blagodaren/SSClash`) when you want it persisted,
  or leave it as a local working note.
