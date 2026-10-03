# i386 test of packages already published for x86_64, 2026-10-03

The AROS Archives i386 uploads of programs this index already lists for
x86_64 were installed, started and removed on AROS One i386, with the
procedure of `reports/2026-09-29-bulk/`.

## Setup

- Client: `apkg` 0.3.1 for i386 (`arospkg-0.3.1.i386-aros-v0.zip`, checked
  against the release's SHA256SUMS).
- System: AROS One 2.9 32-bit (ABIv0), one installed disk image used as a
  read-only base (qcow2 SHA-256
  `ff4895dbbddd330b62558e9a203d6f90c9c2a3c4a103553e3ba588a4a0dea1bb`); each
  batch booted a fresh copy-on-write overlay of it.
- Machines: QEMU with KVM on a Fedora host, one CPU and 2 GB per guest,
  pcnet user networking (AROS One i386 is set up for pcnet32), vmware VGA at
  1024x768, usb-tablet; two or three guests at a time.

## Published: 39 packages

"window": its window or screen was open and the process was running after
15 s. "rc N": a Shell tool that printed its usage or output and returned N.
alac, antiword and potrace (batch w10) were run on input made for the test
(an audio file from the archive, a Word file, a bitmap) and wrote the
expected output (`logs/w10/o-<package>.txt`).

| package | version | batch | stack | started |
|---|---|---|---|---|
| alac | 0.1.3 | w10 | 262144 | rc 0 |
| antiword | 0.37 | w10 | 262144 | rc 0 |
| arrakis | 1.0 | w05 | 262144 | window |
| badapple | 1.0 | w03 | 262144 | window |
| boy | 2008 | w05 | 262144 | window |
| daysleeper | 0.9r2 | w03 | 262144 | window |
| dirtree | 40.8 | w09 | 262144 | rc 0 |
| diskusage | 40.8 | w01 | 262144 | rc 0 |
| dosbox | 0.74 | w05 | 262144 | window |
| dose2 | 2002 | w05 | 262144 | window |
| fheroes2 | r2860 | w05 | 262144 | window |
| finder | 3.1 | w02 | 262144 | window |
| gicriptofilex | 1.0 | w03 | 262144 | window |
| giddy3 | 1.5 | w05 | 262144 | window |
| giocodel15 | 1.0 | w03 | 262144 | window |
| gmore | 1.2 | w01 | 262144 | window |
| hocoslamfy | 2014 | w06 | 262144 | window |
| hydracastle | 1/2019 | w06 | 262144 | window |
| iconmake | 1.2 | w01 | 262144 | window |
| isotool | 1.0 | w01 | 262144 | rc 1 |
| isotools-gui | 1.0 | w03 | 262144 | window |
| ledblur | 2006 | w06 | 262144 | window |
| metemp3 | 0.3 | w03 | 262144 | window |
| nano | 1.2 | w05 | 262144 | window |
| opentyrian | 2.1-fix-1 | w05 | 262144 | window |
| pkg-54321 | v1.0 | w07 | 262144 | window |
| pkg-7mezzoplus | 1.0 | w03 | 262144 | window |
| planethively | 2008 | w07 | 262144 | window |
| potrace | 1.16 | w10 | 262144 | rc 0 |
| qlview | 25.09 | w09 | 262144 | window |
| rescode | 1.2 | w01 | 262144 | window |
| rtf-riddle | 3.97b | w02 | 262144 | rc 22 |
| screentest | 1 | w02 | 262144 | window |
| siddump | 1.08 | w02 | 262144 | rc 1 |
| skandalfoclock | 1.1 | w02 | 262144 | window |
| sploiner | 1.01 | w02 | 262144 | rc 1 |
| timeless | 1.0.1 | w07 | 262144 | window |
| tressette | 0.7.4 | w07 | 262144 | window |
| wildmidi | 0.4.6 | w02 | 262144 | rc 1 |

Screenshots: `shots/<package>.jpg`. Logs: `logs/<batch>/`.

## Tested, not published

| package | reason |
|---|---|
| stercus | started in three runs, but the whole screen stayed black, with no title or picture; the x86_64 build showed its window. Not decided whether the program works |

## Not tested

| package | reason |
|---|---|
| ccd2cue | tar.bz2 archive: the apkg client reads ZIP and LHA only |
| demac | tar.bz2 archive: the apkg client reads ZIP and LHA only |
| pkg-7zdec | tar.gz archive: the apkg client reads ZIP and LHA only |
| protrekkr | no single drawer: files at the top level of the archive (the x86_64 upload has a drawer) |
| sfsobject | no single drawer: files at the top level of the archive (the x86_64 upload has a drawer) |
| stratagus | no single drawer: files at the top level of the archive (the x86_64 upload has a drawer) |
| tinysid | rar archive: the apkg client reads ZIP and LHA only |
