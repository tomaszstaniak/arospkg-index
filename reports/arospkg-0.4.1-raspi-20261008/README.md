# arospkg 0.4.1 for Raspberry Pi (aarch64-aros-raspi): built, not run, 2026-10-08

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

None of this shows that it runs. A Pi on 0.4.0 that runs `apkg
self-update` is offered this build.
