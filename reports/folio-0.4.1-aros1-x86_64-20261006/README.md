# Folio 0.4.1-aros1 x86_64 ABIv11: live apkg install, 2026-10-06

Slot: pool v11-1 (AROS One 1.3 x86_64 ABIv11, QEMU TCG, headless), reserved by
claude-folio041-verify-30e6ffc2, released after the run. apkg 0.4.0 from
arospkg-0.4.0.x86_64-aros-v11.zip (SHA-256 88d0f4b6...4249), run from RAM:F6,
package root RAM:FR6 (new, empty). Network: AROSTCP already running at boot.

Host: public ZIP and local dist ZIP both 28551337 B, SHA-256
024ba46165b07e10bd44312f9cb2fe5ed8c412755e77958a750eb7d8b8065193; same size,
hash and URL in index-v2.json (packages[47]) and index.json (packages[42])
fetched from raw.githubusercontent.com main (host/).

Guest (guest/, screens/):
- update rc=0 from index-v2.json over the network: 110 packages for this system.
- show folio: 0.4.1-aros1, native, release URL, the sha256 above. rc=0.
- install rc=0: downloaded via GitHub redirect, verified, cached
  RAM:FR6/cache/folio.zip (28551337 B); assert-hash PASS.
- layout: RAM:FR6/folio.info + RAM:FR6/folio/ (Folio, Folio.info, CHANGELOG,
  LICENSE, README, SHA256SUMS, licenses/ 9 files).
- list shows folio; info: revision 1, origin URL, archive sha/size; verify rc=0.
- open rc=0: Wanderer drawer RAM:FR6/folio opened.
- Folio (Shell, Stack 8388608) opened pages3.pdf: 3 thumbnails and page 1
  rendered (screens/06). Amiga+Q, rc=0. Version: "Folio 0.4".
- remove rc=0: installed/ empty; cache and db remain.

Not shown: launch from icon, Folio features beyond first-page render.
f6-run.log is a first launch attempt with a wrong path (object not found).

Kept in this repository: the guest logs and three screenshots (install,
apkg open, rendered page). The downloaded ZIP and catalogue copies are not
committed; the catalogue files fetched were index-v2.json SHA-256
ae501ad01b0b24d20fd82aea630e8882fc8ae071afbcb8f8a6aae20bd9f8dbe8 and index.json SHA-256
0812437245bcaef4d831769c686fa982b0f888d5bffb2cba1956b5027fb4e8fb.
