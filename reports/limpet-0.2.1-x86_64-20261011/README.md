# Limpet 0.2.1 x86_64 ABIv11: apkg upgrade from 0.2.0, 2026-10-11

Slot: pool v11-1 (AROS One 1.3 x86_64 ABIv11, QEMU TCG), reserved by
claude-limpet-rel021-20261011, released after the run. apkg 0.4.1 from
arospkg-0.4.1.x86_64-aros-v11.zip (SHA-256 14f076ca...330a, checked against
the release's SHA256SUMS), run from RAM:a, package root RAM:lu (new, empty).
Network: AROSTCP running at boot. The steps ran as one guest script
(`up021`); the start of the upgraded program was typed by hand.

Host: the release asset
https://github.com/tomaszstaniak/limpet/releases/download/v0.2.1/Limpet-0.2.1-abiv11.zip,
downloaded, 3642491 B, SHA-256
71dee9c638ba7676e8089f75d65511b71df9d066e0436feabd75ee27230c768d, the same
bytes as the author's local build and as the manifest. ci_check_index.py
--base origin/main (tools at the pinned c8b4eff): all checks pass.

Test catalogue: index-v2.json from this branch (commit 29ae97d, SHA-256
023594394de87fabe930515b2bcf403a186a2caa79a2af8904aafcb0f4417bf1), passed
with `apkg --index`. The public catalogue was used only to install 0.2.0
first.

Guest (guest/, screens/):
- update from the public catalogue: 110 packages for this system.
- install limpet (public catalogue): 0.2.0-aros1 into RAM:lu/limpet, rc 0;
  Version FULL "Limpet 0.2.0 (10/07/26)".
- show limpet with the test catalogue: 0.2.1-aros1, the release URL and
  hash above, requirements satisfied, "upgrade: available, 0.2.0-aros1 ->
  0.2.1-aros1".
- upgrade limpet: rc 0; plan 0 add, 2 replace (Limpet, README.txt),
  13 unchanged; "Upgraded limpet 0.2.1-aros1". Version FULL
  "Limpet 0.2.1 (10/11/26)".
- verify: 15 as installed, 0 changed, 0 missing; drawer icon ours.
- started RAM:lu/limpet/Limpet --shell: Shell in its window, Version and
  Echo to a file worked (screens/u04-running.png); EndCLI and close.
- remove limpet (0.2.1): removed; RAM:lu held only db, cache, tmp; list:
  nothing installed. The drawer was not "in use" after the session.

Not shown here: launch from the icon, rollback to 0.2.0, PkgManager.
Limpet features beyond starting a Shell were tested in the Limpet
repository (doc/verification-0.2.1.md).
