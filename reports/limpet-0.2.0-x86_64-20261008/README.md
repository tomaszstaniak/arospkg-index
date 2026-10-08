# Limpet 0.2.0 x86_64 ABIv11: apkg from 0.1.0 to 0.2.0, 2026-10-08

Slot: pool v11-2 (AROS One 1.3 x86_64 ABIv11, QEMU TCG, headless), reserved by
claude-limpet-apkg020-20261008, released after the run. apkg 0.4.0 from
arospkg-0.4.0.x86_64-aros-v11.zip (SHA-256 88d0f4b6...4249, checked against the
release's SHA256SUMS), run from RAM:a, package root RAM:lp (new, empty).
Network: AROSTCP already running at boot.

Host: the release asset
https://github.com/tomaszstaniak/limpet/releases/download/v0.2.0/Limpet-0.2.0-abiv11.zip,
downloaded with gh, 3577972 B, SHA-256
15d13d87c9774dfa2cca95adc0ef5384bfe92e1928cc5179d7b5aab1b5f42e56, the same
bytes as the author's local build and as the manifest. ci_check_index.py
--base origin/main on this branch: all checks pass (download, size, hash,
subdir and icon; the generated catalogue files equal the committed ones).

Test catalogue: index-v2.json from this branch (commit b9474b7, SHA-256
dce2eca36adabc0f1d55852eb06e048e40c8e48c3bb9d84518acf6f0170da0c7), copied to
the guest and passed with `apkg --index`. The public catalogue was used only
to install 0.1.0 first.

Guest (guest/, screens/):
- update from the public catalogue: 110 packages for this system.
- install limpet (public catalogue): 0.1.0-aros1 into RAM:lp/limpet;
  Version: "Limpet 0.1".
- show limpet with the test catalogue: 0.2.0-aros1, the release URL and
  hash above, requirements satisfied, installed 0.1.0.
- upgrade limpet: refused by apkg's rule, "installed is version 0.1.0, the
  index offers 0.2.0: no rule orders one upstream version against another
  yet". Expected for apkg 0.4.0; not a problem of this entry.
- remove limpet (0.1.0): removed; RAM:lp held only db, cache, tmp.
- install limpet with the test catalogue: downloaded via GitHub, verified,
  installed 0.2.0-aros1. Version FULL: "Limpet 0.2.0 (10/07/26)".
- verify: 15 files as installed, 0 changed, 0 missing; drawer icon ours.
- started RAM:lp/limpet/Limpet --shell: Shell in its window, Version and
  Echo worked (screens/04); EndCLI and close.
- remove limpet (0.2.0): removed; RAM:lp held only db, cache, tmp; list:
  nothing installed.

So a user goes from 0.1.0 to 0.2.0 with `apkg remove limpet` and
`apkg install limpet`, not `apkg upgrade`.

Not shown: launch from the icon, Limpet features beyond starting a Shell
(tested separately in the Limpet repository, doc/sdk/verification-0.2.0.md).

## Follow-up the same day: `apkg upgrade limpet` with apkg 0.4.1

The refusal above was apkg 0.4.0's rule. apkg 0.4.1 orders dotted numeric
versions. On v11-2 with the 0.4.1 release archive, Limpet 0.1.0 (installed
from the catalogue as it was before 0.2.0) was upgraded to 0.2.0 with
`apkg update` and `apkg upgrade limpet` against the public catalogue: plan
2 replaced, 13 unchanged, a theme the user edited kept, verify clean,
rollback to 0.1.0 and upgrade again, the upgraded Limpet started. PkgManager
0.4.1 did the same upgrade from its Upgrade button. Evidence:
arospkg docs/reports/2026-10-08-version-upgrade (lu2-console-1008.txt,
screens g01-g06).
