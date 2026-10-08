# GrafX2 2.9 revision 5 x86_64 ABIv11: apkg from revision 4 to 5, 2026-10-08

Slot: pool v11-1 (AROS One 1.3 x86_64 ABIv11, QEMU TCG, headless), reserved by
claude-888770b2, released after the run. apkg 0.4.1 from
arospkg-0.4.1.x86_64-aros-v11.zip (checked against the release's SHA256SUMS),
run from RAM:a/apkg041, package root RAM:gp (new). Network: AROSTCP already
running at boot.

Host: the release asset
https://github.com/tomaszstaniak/aros-grafx2/releases/download/v2.9-r5/GrafX2-2.9-r5.x86_64-aros-v11.lha,
downloaded through GitHub's redirect, 4213786 B, SHA-256
2587bc80ca648963c2fbe4826cbd9f7c9119cb3845adf4cc8491cb8f365c3d00, the same
bytes as the author's local build, as the release's SHA256SUMS and as the
manifest. ci_check_index.py --base origin/main on this branch: all checks pass
(download, size, hash, subdir and icon; the generated catalogue files equal
the committed ones).

Test catalogue: index-v2.json from this branch (commit 9bbe3e9, SHA-256
01eeb5bc19b4017d2136df8003f6d43c8de26599d48f23cfdcfda347bc605289), copied to
the guest and passed with `apkg --index`. The public catalogue was used only
to install revision 4 first.

Guest (guest/, scripts t1 and t2; screens/):
- update from the public catalogue: 110 packages for this system.
- install grafx2 (public catalogue): 2.9-aros4 into RAM:gp/grafx2.
- show grafx2 with the test catalogue: 2.9-aros5, the release URL and hash
  above, requirements satisfied, installed 2.9-aros4, upgrade available.
- upgrade grafx2 with the test catalogue: downloaded via GitHub, verified,
  "upgrading grafx2 2.9-aros4 -> 2.9-aros5"; plan 0 add, 2 replace
  (GrafX2, ReadMe.txt), 0 remove, 450 unchanged. The installed GrafX2 is
  9991264 B, the revision 5 binary.
- verify: 452 as installed, 0 changed, 0 missing; drawer icon ours.
- started from a Shell after `Stack 8388608` (Stack reported 8388608), with
  `SetKeyboard pc105_i`: the window opened (screens/03). In the Save
  dialog's comment field, ì, Shift+ì, Shift++, AltGr+ò, Ctrl+a and k typed
  `ì^*@k` (screens/04); the AROS Shell typed `ì^*@` for the same four keys
  (screens/02), and Ctrl+a inserted nothing, as intended. Quit normally.
- remove grafx2: "Removed grafx2 2.9-aros5"; it kept gfx2.lck, gfx2-sdl2.cfg
  and gfx2.ini in RAM:gp/grafx2, which GrafX2 wrote and apkg did not install.
  list: nothing installed.

Not shown: launch from the icon, mainline AROS (the entry says it does not
run there), features beyond the keyboard fix and the Save dialog (tested in
the aros-grafx2 repository, docs/attachments/2026-10-08-keyboard-layouts/).
