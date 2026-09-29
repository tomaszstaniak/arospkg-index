# Bulk test of catalogue candidates, 2026-09-29

The candidates imported from AROS Archives (`arospkg/index/candidates/`) were
taken through the procedure in `arospkg/docs/backlog/bulk-catalogue-testing.md`:
installed, started and removed on AROS One. This report covers every candidate
that had not been reviewed: what was published, and why the rest was not.

## Setup

- Client: `apkg` 0.3 from the release archive, SHA-256 `40c2ba9350bc10e6da294d0cf0675aefb8f3dc86c519fd8c992bb4bf675609e0`.
- System: AROS One 1.3 x86_64 (ABIv11), one installed disk image used as a
  read-only base (qcow2 SHA-256 `c766559f856b39bde0c83e7af2119424e448e2260d3007a30170dc00a7e5acaf`); each batch
  booted a fresh copy-on-write overlay of it.
- Machines: QEMU 10.2.2 on a Fedora host, 2 GB RAM per guest, e1000 user
  networking, vmware VGA at 1024x768, usb-tablet. KVM for most batches; TCG
  for v01 and v03 (see the note on stalls below).
- Each batch ran as one AROS script (`logs/<batch>/run.script`), with no
  typing into programs. Logs went to the host over the network as they were
  written; screenshots were taken from the host.

## Procedure

1. Install every package of the batch with `apkg --root SYS:Batch --index
   RAM:b/index.json install <id>`, then `apkg info <id>` (`a.txt`).
2. Start each one from its installed drawer with `Run` and input from NIL:
   (`start.script`, `b-<id>.txt`). After 15 s, `Status` records whether it is
   still running, the screenshot is taken, then Ctrl-C. A program that
   returns records its return code; its standard output is in `o-<id>.txt`.
   What a program writes to standard error shows only on the screenshot: the
   AROS One Shell cannot redirect it.
3. Remove every package, list `SYS:Batch`, and `apkg list` must say
   `nothing installed` (`c.txt`).

A crash ends what a boot can prove: packages started after a crash in the
same boot were run again from a fresh boot, and only that run counts.
The stack given for each package is the one it was started with: 40960
bytes unless its icon or ReadMe asks for more.

## Published: 81 packages

"window": its window or screen was open and the process was running after
15 s. "rc N": a Shell tool that printed its usage or output and returned N.
"data message": a game whose data is not part of the package; it showed its
own message saying so, and that message is all that was checked.

| package | batch | machine | stack | started | note |
|---|---|---|---|---|---|
| ab3dhd | r01 | KVM | 500000 | window |  |
| alac | c09 | KVM | 40960 | rc 1 |  |
| amiberry | c09 | KVM | 2000000 | window |  |
| apotris | c07 | KVM | 100000 | window |  |
| arrakis | r05 | KVM | 40960 | window |  |
| badapple | c07 | KVM | 40960 | window |  |
| boy | s04 | KVM | 50000 | window |  |
| breakhack | c07 | KVM | 200000 | window |  |
| cabextract | c05 | KVM | 40960 | rc 20 |  |
| ccd2cue | c10 | KVM | 40960 | rc 0 |  |
| davegnukem | r01 | KVM | 40960 | window |  |
| daysleeper | r02 | KVM | 40960 | window |  |
| delugem | c04 | KVM | 400000 | window |  |
| demac | c09 | KVM | 40960 | rc 0 |  |
| demoeffects | s02 | KVM | 40960 | window |  |
| devilutionx | c09 | KVM | 1000000 | window | data message |
| diskusage | r05 | KVM | 40960 | rc 0 |  |
| dosbox | u01 | KVM | 40960 | window |  |
| dose2 | c07 | KVM | 40960 | window |  |
| fallout | s04 | KVM | 200000 | window | data message |
| fallout2 | r09 | KVM | 200000 | window | data message |
| fheroes2 | r01 | KVM | 200000 | window | data message |
| finder | r02 | KVM | 40960 | window |  |
| fireoftheapes | t02 | KVM | 40960 | window |  |
| flx_play | v01 | TCG | 40960 | rc 1 |  |
| ft2clone | c07 | KVM | 4096000 | window |  |
| gicriptofilex | r05 | KVM | 40960 | window |  |
| giddy3 | v01 | TCG | 400000 | window |  |
| giocodel15 | c09 | KVM | 40960 | window |  |
| gisplita | s04 | KVM | 40960 | rc 2 |  |
| gnurobbo | s05 | KVM | 40960 | window |  |
| hexen2 | r01 | KVM | 2000000 | window | data message |
| hivelyreplay | c04 | KVM | 40960 | rc 0 |  |
| hostile-takeover | r02 | KVM | 40960 | window |  |
| hydracastle | t03 | KVM | 102400 | window |  |
| isomaker-gui | s02 | KVM | 40960 | window |  |
| isotools-gui | r05 | KVM | 40960 | window |  |
| keen4 | c05 | KVM | 40960 | window |  |
| lbreakouthd | c04 | KVM | 500000 | window |  |
| ledblur | s04 | KVM | 40960 | window |  |
| mcamiga | c05 | KVM | 40960 | window |  |
| metemp3 | c04 | KVM | 40960 | window |  |
| minislug | s05 | KVM | 40960 | window |  |
| mplayer | r01 | KVM | 512000 | rc 0 |  |
| nano | r02 | KVM | 40960 | window |  |
| nomarch | c05 | KVM | 40960 | rc 1 |  |
| ominoarcade | c09 | KVM | 40960 | window |  |
| opentyrian | s02 | KVM | 300000 | window |  |
| openxcom | r05 | KVM | 40960 | window | data message |
| pekkakana2 | t02 | KVM | 1000000 | window |  |
| pkg-54321 | t02 | KVM | 72000 | window |  |
| pkg-7mezzoplus | c05 | KVM | 40960 | window |  |
| pkg-7zdec | s04 | KVM | 40960 | rc 0 |  |
| planethively | s05 | KVM | 40960 | window |  |
| potrace | c07 | KVM | 40960 | rc 2 |  |
| prboom-plus | r02 | KVM | 40960 | rc 0 | data message |
| protrekkr | v01 | TCG | 40960 | window |  |
| qlview | c10 | KVM | 40960 | window |  |
| residualvm | r05 | KVM | 40960 | window |  |
| rtf-riddle | c07 | KVM | 40960 | rc 22 |  |
| scopagame | w01 | KVM | 40960 | window |  |
| screentest | c04 | KVM | 40960 | window |  |
| sfsobject | c07 | KVM | 40960 | rc 10 |  |
| siddump | s05 | KVM | 40960 | rc 1 |  |
| sokobangp2x | r01 | KVM | 1000000 | window |  |
| sploiner | c09 | KVM | 40960 | rc 1 |  |
| stercus | t01 | KVM | 50000 | window |  |
| stratagus | s02 | KVM | 40960 | rc 1 | data message |
| super-haxagon | r05 | KVM | 400000 | window |  |
| super_trevor_land | u01 | KVM | 40960 | window |  |
| tbftss | r01 | KVM | 400000 | window |  |
| tiltnroll | r02 | KVM | 40960 | window |  |
| timeless | t02 | KVM | 40960 | window |  |
| tinysid | c04 | KVM | 40960 | rc 0 |  |
| tinytris | s02 | KVM | 40960 | window |  |
| tressette | r05 | KVM | 40960 | window |  |
| unrar | w01 | KVM | 800000 | rc 0 |  |
| unshield | c04 | KVM | 40960 | rc 1 |  |
| untarka | c07 | KVM | 40960 | rc 0 |  |
| wildmidi | c10 | KVM | 40960 | rc 1 |  |
| zgloom | v03 | TCG | 200000 | window |  |

Screenshots: `shots/<package>.jpg`. Logs: `logs/<batch>/`.

## Tested, not published

| package | reason |
|---|---|
| cls | installed, started and removed, but held back: clears the console; nothing a log can capture |
| ffmpegaudioextractor | installed, started and removed, but held back: frontend for an ffmpeg command that AROS One does not have |
| ffmpegvideotool | installed, started and removed, but held back: frontend for an ffmpeg command that AROS One does not have |
| flexcat-gui | installed, started and removed, but held back: frontend only; the FlexCat command it drives is not checked |
| lzop | installed, started and removed, but held back: no readable output with input from NIL: |
| playsid-gui | installed, started and removed, but held back: frontend for SYS:C/PlaySid, which AROS One does not have |
| smb2-gui | installed, started and removed, but held back: frontend for SMB2 tools; needs a share to exercise |
| timeit | installed, started and removed, but held back: prints only an argument error |
| unmount | installed, started and removed, but held back: prints only an argument error (Unmount: required argument missing) and returns 20 |
| wgetgui | installed, started and removed, but held back: frontend only; the wget command it drives is not checked |
| zunearc | installed, started and removed, but held back: frontend for archivers; ships none and names none, so what it drives is not checked |
| biniax2 | crashed at start: Privilege violation (Exec AddTail) |
| rman | crashed at start: Illegal address access |
| scummvm | crashed at start: Stack extends out of range at 40960; also crashed at 262144 bytes when run alone on a fresh guest (batch x01, `logs/x01/`, `shots/scummvm-crash-x01.jpg`) |
| arxlibertatis | apkg 0.3 cannot install it: member over size limit (arx binary) |
| augustusandjulius | apkg 0.3 cannot install it: over ~1160 files: plan exceeds 8192 JSON tokens |
| cavestory | apkg 0.3 cannot install it: over ~1160 files: plan exceeds 8192 JSON tokens |
| dethrace | apkg 0.3 cannot install it: over ~1160 files: plan exceeds 8192 JSON tokens |
| gemrb | apkg 0.3 cannot install it: over ~1160 files: plan exceeds 8192 JSON tokens |
| owb | apkg 0.3 cannot install it: over ~1160 files: plan exceeds 8192 JSON tokens |
| thextech | apkg 0.3 cannot install it: over ~1160 files: plan exceeds 8192 JSON tokens |
| vim | apkg 0.3 cannot install it: over ~1160 files: plan exceeds 8192 JSON tokens |
| dclock | apkg refused the archive: unsafe member path ram:dclock/ in the LHA |
| adoom3 | a person must look: exits rc=20 without a message |
| amigakeyremaper | a person must look: no window (keymap tool) |
| amishockolate | a person must look: black screen only |
| csid | a person must look: tries a default tune, fails, exits |
| hex2 | a person must look: no window seen |
| joytest | a person must look: no window seen |
| leu | a person must look: no window seen |
| muimapparium | a person must look: no window seen |
| muiplot | a person must look: no window seen, two runs |
| openjazz | a person must look: exits rc=-1, data missing only in log |
| reminiscence | a person must look: exits rc=-1 |
| soniccd | a person must look: exits rc=0, no window, no output |
| switch | a person must look: no window seen |
| targa_datatype | a person must look: a datatype, not a program: belongs in SYS:Classes |
| zapperng | a person must look: no window seen |

The apkg 0.3 limits are limits of this client, not faults of the packages;
they can be tried again with a client that lifts them.

## Not tested

Decided on the host from the archive and the catalogue, before any guest:

| candidate | reason |
|---|---|
| amigagpt | needs files outside its drawer |
| amissl-5 | needs files outside its drawer |
| amissl | needs files outside its drawer |
| antiword-v0.37 | newer upload of the approved antiword; to be reviewed as an update |
| aros64_extra_cores | over the client's 64 MiB download limit |
| commander_keen_4 | aarch64; not tested |
| edgesnap | the downloaded archive is not the size the catalogue gives; a person must look |
| emuv0 | ships its own copies of system libraries; a person must judge |
| exult | over the client's 64 MiB download limit |
| ffmpeg | three loose programs, no drawer |
| filesysbox | needs files outside its drawer |
| ghostscript-10.0.0 | ABIv1 build; the released client is ABIv11 |
| installerlg | no single drawer; an installer layout |
| libiffanim | ABIv1 build; the released client is ABIv11 |
| libmikmod | ABIv1 build; the released client is ABIv11 |
| libmpg123 | development files, not a program |
| libsdl2 | development files, not a program |
| libyaml | development files, not a program |
| lzo-2.03 | ABIv1 build; the released client is ABIv11 |
| ntfs3g | needs files outside its drawer |
| perl-5.7.2 | an interpreter laid out as a Development tree, not a drawer |
| pintp | aarch64; not tested |
| processicon | development files, not a program |
| python-2.5.2 | an interpreter laid out as a bin/include/lib tree, not a drawer |
| python-2.7.18 | an interpreter laid out as a Development tree, not a drawer |
| retroarch | no program at the top of its drawer; references arosc.library |
| scummvm-2026.02-sdl1 | over the client's 64 MiB download limit |
| scummvm-2026.3.0 | over the client's 64 MiB download limit |
| signus | no single drawer; files loose at the top level |
| smb2fs | needs files outside its drawer |
| sudokul | no single drawer; files loose at the top level |
| super_trevor_land | aarch64; not tested |
| telegramamiga | the downloaded archive is not the size the catalogue gives; a person must look |
| zaphod-v1.3 | newer upload of the approved zaphod; to be reviewed as an update |
| zunebrot | not a ZIP or LHA archive; the client reads ZIP and LHA |
| zunepaint | not a ZIP or LHA archive; the client reads ZIP and LHA |
| zuneview | not a ZIP or LHA archive; the client reads ZIP and LHA |

## What the runs showed about the client and the test machines

- apkg 0.3 reads its transaction plan back with an 8192-token JSON parser.
  A package with more than about 1160 files fails with "cannot write or
  re-read the plan". The unfinished transaction then stops every later
  install in the same root ("recovery stopped short") until it is resolved.
- apkg 0.3 has no network timeout. When a download stalls it waits for ever,
  and Ctrl-C does not interrupt it.
- With several guests running at once, a guest sometimes stopped in the middle
  of a transfer: the connection stayed open with nothing queued on the host
  side, and the guest stopped responding. It happened under KVM and TCG, with
  one or two virtual CPUs, and never with a single guest. Those runs were
  discarded and their packages run again; the table above uses only complete
  runs.
- On AROS One, `requires_system` must name a library as the program opens it:
  `sdl.library` does not open where `SDL.library` does.
## Index check

The final `index.json` (SHA-256 `92297eda9eac315d732b87e52a1499f5a3bf231b6d038378a693a87fdc81f94e`,
96735 bytes, 108 entries) was read by apkg 0.3 on a fresh AROS One guest:
`search` returned 0, `show` returned 0 for every entry, and `flx_play`,
the one entry whose id differs from its catalogue name, was installed and
removed, leaving `nothing installed`. Logs: `logs/index-check/`.
