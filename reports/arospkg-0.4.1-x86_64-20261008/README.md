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
second run; a network failure under TCG, not investigated further.
