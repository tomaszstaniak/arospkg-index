# Limpet 0.3.0 x86_64 ABIv11: apkg upgrade from 0.2.1, 2026-10-11

Slot: pool v11-1 (AROS One 1.3 x86_64 ABIv11, QEMU TCG), reserved by
claude-limpet-rel030-20261011, two boots, released after the run. apkg 0.4.2
from arospkg-0.4.2.x86_64-aros-v11.zip (checked against the release's
SHA256SUMS), run from RAM:a, package root RAM:lu (new, empty at each boot).
Network: AROSTCP running at boot. The steps ran as guest scripts (`u030`,
then `u031` and `u031b`); starting the upgraded program was typed by hand.

Host: the release asset
https://github.com/tomaszstaniak/limpet/releases/download/v0.3.0/Limpet-0.3.0-abiv11.zip,
downloaded, 3648767 B, SHA-256
7e2514029e0e6fadc7d331ceeb817df4b7f780dfed12409f347cf244d8a31676, the same
bytes as the author's build from the tagged commit and as the manifest.
Against 0.2.1 only `Limpet/Limpet` and `Limpet/README.txt` differ; the
program opens the same libraries and devices. ci_check_index.py --base
origin/main (tools at the pinned c8b4eff): all checks pass.

Test catalogue: index-v2.json from this branch (commit c841ca7, SHA-256
e42d679b0c96b3dcffd279b03574a06b9538075826b7c2e720b058c709ef3dc1), passed
with `apkg --index`. The public catalogue was used only to install 0.2.1.

## Run u031 (the counted run)

- update from the public catalogue: 110 packages for this system.
- install limpet (public catalogue): 0.2.1-aros1 into RAM:lu/limpet, rc 0;
  Version FULL "Limpet 0.2.1 (10/11/26)".
- a user change: `Themes/light.theme` replaced by `light-mod.theme` (the
  same theme with `cursor=#c00000`); verify: 14 as installed, 1 changed
  locally.
- show limpet with the test catalogue: 0.3.0-aros1, the release URL and
  hash above, requirements satisfied, "upgrade: available, 0.2.1-aros1 ->
  0.3.0-aros1".
- upgrade limpet: rc 0; plan 0 add, 2 replace (Limpet, README.txt), 0
  remove, 0 kept, 13 unchanged; "Upgraded limpet 0.3.0-aros1". Version FULL
  "Limpet 0.3.0 (10/11/26)".
- the changed theme after the upgrade is byte-identical to the one before
  it (u031-05 and u031-10, both equal to light-mod.theme). The theme file is
  the same in both versions, so apkg left it alone.
- verify: 14 as installed, 1 changed locally (Themes/light.theme), 0
  missing; drawer icon ours.
- started `RAM:lu/limpet/Limpet --theme RAM:lu/limpet/Themes/light.theme
  --shell`: light theme with the changed red cursor
  (screens/u031-03-theme-kept.png), title `Wait - Limpet` during `Wait 20`
  and `Shell - Limpet` after it, Version and Echo to a file worked; EndCLI
  (Shell status 0) and the close gadget.
- remove limpet (0.3.0): removed, "Kept files in RAM:lu/limpet:
  Themes/light.theme (you changed it)", rc 0; list: nothing installed. The
  drawer was not "in use" after the session.

## Run u030 (first boot, the user change was invalid)

The same steps, but the change appended a second `cursor=` line to the
theme. apkg behaved as in u031 (upgrade rc 0, the changed file kept byte
for byte, verify "1 changed locally"). Limpet's theme parser rejects a file
that repeats a key, so the upgraded Limpet started with its built-in dark
theme (screens/u030-03-duplicate-key-fallback.png; the message went to
NIL:). That is the documented fallback, not an upgrade fault; u031 repeats
the check with a valid change.

Not shown here: launch from the icon, rollback to 0.2.1, PkgManager, a
theme file that differs between the two versions. Limpet features beyond
starting a Shell were tested in the Limpet repository
(doc/verification-0.3.0.md).
