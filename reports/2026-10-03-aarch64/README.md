# aarch64 test of the AROS Archives aarch64 uploads, 2026-10-03

The four `aarch64-aros` uploads on AROS Archives were installed, started and
removed on AROS for the Raspberry Pi under QEMU. None is published: two
crashed at start, one needs a network stack the test machine does not have,
and one ended without a window.

## Setup

- Client: `apkg` 0.3.1 for aarch64 (`arospkg-0.3.1.aarch64-aros.zip`, checked
  against the release's SHA256SUMS). It reports "built for this machine:
  aarch64, ABIv1".
- System: the AROS nightly `AROS-20261002-raspi-aarch64-system.tar.bz2`
  (md5 `f5fea7461a1c73cda8a746e889d2b26e`, as published beside it), on a
  FAT32 SD image.
- Machine: `qemu-system-aarch64 -M raspi3b -m 1G` with the kernel, device
  tree and BSP ROM from that archive (`-initrd` for the ROM), usb-kbd and
  usb-tablet. QEMU's raspi3b has no network device.
- Without a network, each archive was placed in the client's cache before
  the run; `apkg` checks the cached file against the index's SHA-256 before
  it installs it, as it does after a download. Downloading was therefore not
  tested.
- One fresh boot per package: install, `info`/`verify`, start with Run for
  15 s (stack 262144), Ctrl-C, remove, `list`.

## Results

| package | installed | started | removed |
|---|---|---|---|
| commander_keen_4 | yes, 22 files verified | Software Failure within about 4 s: 0x80000002 hardware bus fault, PC 0xF81CA4CC (in the Kickstart range) | rc 0, nothing installed; an empty drawer remained |
| super_trevor_land | yes | Software Failure after about 25 s, same error and PC | rc 0, nothing installed; an empty drawer remained |
| pintp | yes | requester "Unable to open bsdsocket.library version 4 or later"; rc 20 after Exit | rc 0, nothing installed; an empty drawer remained |
| telegramamiga | yes, 10 files verified | printed its start-up lines and returned 0 within 15 s, no window | rc 0, drawer gone |

The two crashes fault at the same address inside the system, not in the
programs, so they may come from the emulated board rather than from the
packages; that is not decided. pintp needs a network stack. telegramamiga
probably needs one too, or a first-run step; it was not looked into.

Screenshots: `shots/<package>.jpg`. The guest's log for each package:
`logs/<package>.txt` (the guest has no clock, so its dates read 1978).

## What the runs showed about the client

- After `install`, `apkg list` printed nothing, although `info` and `verify`
  showed the package installed; after `remove` it printed "nothing
  installed".
- The confirmation after `install` and `remove` printed only the package id,
  without the word the x86_64 client prints before it ("installed").
- For three packages `remove` left the package's empty drawer behind.

These were seen with the aarch64 client only and are recorded, not
investigated.
