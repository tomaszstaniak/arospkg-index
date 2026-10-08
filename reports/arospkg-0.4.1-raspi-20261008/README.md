# arospkg 0.4.1 for Raspberry Pi (aarch64-aros-raspi): built, not run, withdrawn, 2026-10-08

Not submitted to the catalogue. This index publishes only builds that were
started on the system they are for, and this one was not run, on a Pi or in
an emulator. The catalogue keeps 0.4.0 for aarch64/v1.

Release asset
https://github.com/tomaszstaniak/arospkg/releases/download/v0.4.1/arospkg-0.4.1.aarch64-aros-raspi.zip,
sha256 3c15b6dc18437e29cad86acaca445d951fdd6a77a858b5927af0e30adedfd50c,
apkg ecc9372ae10589ddfbd15b7b6545ed3851b6f6a4232ca1d337a42192afccef26.
Built with tools/make-rc.sh from tag v0.4.1 (commit 4aace42), with the
raspi-aarch64 toolchain (Clang 20 over the Pi's own Developer SDK), as 0.4.0
was.

Checked on the host only:
- a second build of the same commit gives the same apkg hash;
- ELF64, AArch64, OS/ABI AROS, relocatable, as 0.4.0;
- no undefined symbols;
- the libraries named in the binary are the same 8 as in 0.4.0
  (host/libraries-0.4.*.txt), and the 12 library base symbols are the same
  (host/library-bases-0.4.*.txt);
- the version string says apkg 0.4.1, target aarch64-aros-raspi.

None of this shows that it runs.

## Withdrawn, 2026-10-08 evening

While the archive was in the release, a Pi on 0.4.0 running `apkg
self-update` would have been offered it: self-update reads the release's
SHA256SUMS, not this catalogue. A test on the pool's Pi machine (rpi-1,
native AROS raspi-aarch64 20260822 under QEMU raspi3b) was not possible:
that machine has no working network (its 2026-10-05 network trial was not
accepted), so self-update and catalogue operations cannot run there, and
its USB input has not passed acceptance.

The archive was therefore removed from the v0.4.1 release and its line from
the release's SHA256SUMS (now x86_64 and i386 only). apkg 0.4.0 looks for
`arospkg-<version>.aarch64-aros-raspi.zip` in that file; finding none, it
reports that the latest stable release has no archive for this CPU and ABI
and changes nothing (src/pkg/selfupdate.c at v0.4.0; read, not run on a
Pi). The release's asset list showed one download of the archive before
removal; the session that published it also downloaded every asset once to
check the sums.
