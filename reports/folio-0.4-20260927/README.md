# Folio 0.4 release validation — 2026-09-27

Source: `aros-foliopdf` commit `f6294e1`, tag `v0.4`.
Architecture: x86_64, ABIv11. Compiler: GCC 10.5.0; ABIv11 SDK under
`AROS/sdk`. Runtime: AROS One 1.3, QEMU pool slot `v11-1`.

## Review and host checks

Six issues found during release review were fixed: lone-page zoom used a
scale different from its visible size; Fit Width could return without
rebuilding; equal-zoom restore could keep stale fitted geometry; fit was
limited by the manual zoom floor; horizontal panning retained an explicit
jump lock; the last state extension was skipped at the 100-document limit.

The new lone-page zoom regression was observed failing before the fix.
`scripts/test-layout.sh` and `scripts/test-queue.sh` pass.
`scripts/build-folio.sh` exits 0 with no compiler warnings.
`scripts/check-reproduction.sh` confirms both patches apply to pinned MuPDF
commit ce933bdebe05fe54ba9d69792ed268efd9e00047.

A second code review found no release blocker. Minor remaining limitation:
the existing CELL_SLACK tolerance can defer a resize of a few pixels in fit
mode. This release did not change that tolerance.

## Runtime checks on the release LHA

Installed with apkg 0.2 (a513aea7d8b066911a88466ac83d166f9c30578fb80768b0961d14df426040fc),
first from a seeded cache, then from the public GitHub release URL into a
separate empty package root. Both archive checks and installs passed.
Both installations were removed through apkg.

The installed Folio binary was launched with an 8 MiB stack and the 22-page
arXiv PDF 2505.17241v1. Observed:

- Magazine cover: whole page, horizontally centred, width about 426 px.
- First Zoom Out: width about 340 px, correctly reduced by a factor of 1.25.
- Fit Page and Next: only the complete 2–3 spread is shown.
- Shorter window: both pages refitted; Home refitted and centred the cover.
- Quit and reopen: Magazine restored and the cover fitted to the new window.
- End: page 22 alone, centred and fitted to the full reading height.

This run did not repeat selection/copy, search or link interaction from the
prior Magazine test sessions. It did not verify mainline ABIv1 or other OSes.

## Release artifacts

| Artifact | SHA-256 |
| --- | --- |
| Folio-0.4.x86_64-aros-v11.lha (29365392 bytes) | 432298b9bc4abfe92cb573c6cc157d12b54ceef43888405796a7a6e68de186a2 |
| Folio-0.4-source.zip | 9863c2608e2fb6127231e041473d6a4ef8f3e38c05fc109528dce2a16619b240 |
| Stripped installed executable | 610661bec18524d41ce943779a1b853baf2bbb329add51228e04adea0ff1189c |

The binary archive contains the application drawer and icon, embedded
arospkg manifest, README, changelog, licences and file checksums. The source
ZIP is the repository at the release commit and includes dependency pins
and patches. The Archives filename is an identical copy named
`folio.x86_64-aros-v11.lha`.

Public release: https://github.com/tomaszstaniak/aros-foliopdf/releases/tag/v0.4
AROS Archives submission is left to the user, as explicitly requested.
