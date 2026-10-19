# bartonitsolutions-bartondesk-roster

The independent mirror for BartonDesk, served by GitHub Pages at
<https://barton-it-solutions-ltd.github.io/bartonitsolutions-bartondesk-roster/>.
(Renamed from `bartondesk-roster` in October 2026. GitHub Pages doesn't redirect renamed
repos, so customer apps from before 0.13.3 still look at the old address and can't
auto-update; nobody had one at the time.)

| File | What it is | Written by |
|---|---|---|
| `roster.json` | The signed staff list: anonymous device fingerprints, valid for 7 days | `owner publish` (and approve/revoke), daily via `owner schedule on` |
| `update.json` | The signed auto-update document: version, files, SHA-256 | `owner release` |
| `updates/` | The Windows customer app builds that `update.json` points to (only the newest is kept) | `owner release` |
| `BartonDesk.exe`, `BartonDesk-arm64.exe`, `SHA256SUMS.txt` | The current download, served on the website at `/downloads/...` (its nginx config proxies these) | `owner release` |

Everything that matters is signed with the Barton IT Solutions organisation key, so it's
safe to publish: anyone can read it, but nobody can change it without the signature
failing. Customer apps fetch the staff list here and from the relay and use the newest
valid one, so a compromised relay can't hide a revoked technician by serving an old list.
They install an update only if `update.json`'s signature and the file's SHA-256 match.

Don't edit anything here by hand. Changes are committed and pushed by **BartonDesk Owner**
on the owner's PC, which has this repo set as its mirror (`owner set-mirror <this clone>`).

Keep this repo **separate from the relay's infrastructure**, and protect the GitHub
organisation with 2FA: an attacker would need to compromise both the relay and this repo
to replay an old list, and even then any list expires within 7 days.
