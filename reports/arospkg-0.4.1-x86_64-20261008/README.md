# arospkg 0.4.1 x86_64 ABIv11: self-update and upgrade through this entry, 2026-10-08

Slot: pool v11-2 (AROS One 1.3 x86_64 ABIv11, QEMU TCG, headless), reserved by
claude-apkg041-20261008, released after each run. Release asset
https://github.com/tomaszstaniak/arospkg/releases/download/v0.4.1/arospkg-0.4.1.x86_64-aros-v11.zip,
sha256 14f076caede95eec614c118477cf0a08c4894241922e76fa0c7c44f496c9330a,
checked against the release's SHA256SUMS; ci_check_index.py on this branch:
all checks pass. Test catalogue: index-v2.json of this branch.

Second run (guest/su2-console-1008.txt), from the published 0.4.0 archive:
- `apkg self-update --check`: latest stable 0.4.1; `apkg self-update`:
  downloaded, verified, installed; apkg.old kept; `--version` 0.4.1 with the
  release binary's hash 4d86b342...9912.
- package arospkg from the public catalogue: 0.4.0 installed; with this
  branch's catalogue `show` says "upgrade: available, 0.4.0 -> 0.4.1";
  `upgrade arospkg` rc 0: 4 files replaced, 11 unchanged (the archive's top
  directory changes from arospkg-0.4.0... to arospkg-0.4.1...); the installed
  apkg reports 0.4.1; verify clean; remove left nothing.

First run (guest/su1-console-1008-tls-failure.txt): the same steps, but the
package upgrade failed to fetch the archive (TLS handshake with github.com
failed, -0x7280) right after self-update had fetched the same URL. The
upgrade changed nothing (0.4.0 still verified clean). Not repeated in the
second run. See "Follow-up: TLS download series" below.

## Follow-up, 2026-10-08 evening: Upgrade.rexx and a TLS download series

Slot: pool v11-2 again (AROS One 1.3 x86_64 ABIv11, QEMU TCG), reserved by
claude-apkg041v-20261008. apkg, PkgManager and Examples/ARexx/Upgrade.rexx
were taken from the published release archive (downloaded from the release,
checked against its SHA256SUMS; apkg --version shows binary 4d86b342...9912).
QEMU display cocoa; the window was not raised, and its state was not controlled.
Scripts: scripts/

### Upgrade.rexx with a version change (guest/rx-*)

Root RAM:rl. Limpet 0.1.0 installed from the catalogue as it was before 0.2.0,
Themes/light.theme edited, `apkg update` from the public catalogue, then
PkgManager 0.4.1 started on that root (scripts/RXSETUP).

- First `rx Upgrade.rexx limpet`: PkgManager showed the plan (0.1.0-aros1 to
  0.2.0-aros1, 2 replaced, 13 unchanged; screens/rx-01-confirm.png); Cancel
  pressed. The script printed "declined in PkgManager; nothing changed" and
  returned 5 (rx: "Error executing script 5/0"). Afterwards: Limpet 0.1,
  the edited theme unchanged, verify 14 as installed + 1 changed locally,
  registry 0.1.0.
- Second `rx Upgrade.rexx limpet`: Proceed pressed. The script printed
  "limpet upgraded" and returned 0. Afterwards: Limpet 0.2 with the release
  binary's hash (3ff6b5d3...), the edited theme unchanged, verify 14 + 1
  changed locally, registry 0.2.0 with the previous version recorded for
  rollback. PkgManager's log (guest/rx-pm-1008b.log): job 1 declined, job 2
  plan add=0 replace=2 remove=0 kept=0 unchanged=13 conflicts=0, done.

### TLS download series (guest/tls-*.log, host/markers-host-time.log)

The earlier failure was `TLS handshake with github.com failed (-0x7280)`.
-0x7280 is MBEDTLS_ERR_SSL_CONN_EOF: the connection was closed during the
handshake (recv returned 0). From the client's side this does not show who
closed it: the server, the network, or QEMU's user-mode network.

Each step below logs guest time (Date) before and after, the exit code and
the full `--verbose` output, then sends a short marker to the host
(tests/tools/putfile), where the arrival time is recorded.

- Series 1 (same boot, after the Upgrade.rexx runs): 30 installs of
  arospkg 0.4.1 from the public catalogue, each on a fresh root. 30 of 30
  exit 0; each fetched the github.com URL, followed the redirect to
  release-assets.githubusercontent.com and verified the archive. Guest
  5-8 s per install. Guest clock against host clock over the series: 0.99.
- Series 2 (fresh boot). A: the failing sequence repeated 3 times (0.4.0
  release unpacked, `self-update` to 0.4.1, install arospkg 0.4.0 from the
  catalogue as it was before 0.4.1, `upgrade` with the public catalogue,
  `verify`): 12 of 12 exit 0. B: 100 installs as in series 1: 100 of 100
  exit 0. Guest clock against host clock over the series: 0.61. Host time
  per install rose from 2.7 s to about 6.7 s during part B, while the host
  was building other targets; the cause was not measured.

Result: the failure was not reproduced in 142 steps. That does not explain
it. If it comes back, the useful next step is a packet capture of the
guest's connection on the host (QEMU filter-dump), which shows which side
closed it.
