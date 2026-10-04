# Folio 0.4.1 package check — 2026-10-04

Source commit: f9d1e49, tag v0.4.1 in aros-foliopdf.
Target: x86_64 ABIv11. GCC 10.5.0, matching ABIv11 SDK (AROS/sdk).
Runtime: AROS One 1.3 under QEMU, reserved pool slot v11-2.

Host layout, render-queue and page-input tests passed. MuPDF patch reproduction
passed against ce933bdebe05fe54ba9d69792ed268efd9e00047. The front end compiled
and linked without warnings. The release executable was stripped with
--strip-unneeded --remove-section .comment. Every packaged file checksum
passed after extracting the LHA; the embedded manifest names version 0.4.1.

## Package and runtime checks

C:apkg installed the release LHA from a seeded cache into RAM:FP using a
single-entry test index with the release URL, size and hash. Installation
returned 0. The installed Folio opened tests/fixtures/pages3.pdf:

- All three thumbnails and the first page rendered.
- Amiga+2 displayed pages 1–2 as a spread.
- Amiga+J, 3, Return displayed page 3 alone at full fitted width.
- Amiga+Q exited with 0; apkg removal returned 0 and left no installed files
  or registry entry. Only package-manager directories and archive cache remained.

The guest Version command reports only major/minor (0.4); the packaged ELF's
raw $VER string and About text contain 0.4.1. No files from a different build
were substituted. Logs beside this report record the command results.

Dependency review: deps.py scanned the packaged ELF; the mandatory system
requirements remain crt.library, m.library and stdlib.library. MuPDF and its
bundled dependencies are static; openurl.library is opened only for external
URLs. No dependency code changed in this release.

## Artifacts

- LHA: 29367583 bytes, SHA-256 c78eddde9a5e34eda2de1dc51eb16ebf810aea5b5feb9579562232d3d2589626
- Source ZIP: SHA-256 55fc77b8618554b4e8cc40a8963c3e10322ff330d0ebf189fc2c4f1c59d48fdb
- Packaged executable: SHA-256 b3c6b188f3dfb21a84e334be6cb1bcec117927308a71e6e4e0f3d38e34d65ec2

## What these checks did not show

The guest install used a seeded cache, not a live network download. Public
release download integrity is checked separately on the host. Clipboard,
annotation saving, external links and rendering performance were not retested.
i386 ABIv0 and Raspberry Pi aarch64 were compiled and linked, not run; they
are release assets, not newly approved entries in this index. Mainline x86_64
runtime compatibility is not claimed.
