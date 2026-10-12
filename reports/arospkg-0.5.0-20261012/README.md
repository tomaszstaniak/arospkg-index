# arospkg 0.5.0 through this catalogue, 2026-10-12

The release: https://github.com/tomaszstaniak/arospkg/releases/tag/v0.5.0,
archives built from arospkg commit 2d48dcb (tag v0.5.0). The assets were
downloaded on the host and checked against the release's SHA256SUMS (all
OK); the manifests carry those sizes and hashes:

| archive | bytes | SHA-256 |
|---|---|---|
| arospkg-0.5.0.x86_64-aros-v11.zip | 854414 | 4fc6b9d91b505054cc3025562e2586fb7d5714b95ec01496da85a9c7c8a5aa3c |
| arospkg-0.5.0.i386-aros-v0.zip | 320845 | cc73007ad7517240c67c40f08cfe662ee6b95da6f4e16bbe6f824a6462b92e50 |

## What else this change does

- **`[launch]` tables** (arospkg RFC 0002) for Folio x86_64/v11 (in its
  override, as its manifest is written by apkg-pack), GrafX2 and grep, and
  for arospkg x86_64/v11 (PkgManager). apkg 0.5 starts these with
  `apkg run` and PkgManager's Run; older clients ignore the key. The
  archives are unchanged: apkg writes `application.json` into the drawer
  at installation. Tested with these archives on AROS One 1.3 (pool v11-2):
  arospkg docs/evidence/2026-10-12-050 (`run1b/`, `final-v11-2/`).
- **The tools are pinned at arospkg c67ad68** (check.yml, publish.yml):
  its mkindex writes `launch` into index-v2.json only, and index.json with
  only the fields clients up to 0.3.2 read, always including arospkg, kept
  under 80% of their 8192 values (now 5723, 69%; was 7590).
- The Raspberry Pi entry stays at 0.4.0: the 0.5.0 Pi build was compiled
  and linked, not run. No mainline (x86_64/v1) entry is added: an install
  over the network was not shown on mainline in this run.

`ci_check_index.py . --base origin/main` with the new tools: all checks
pass, and the committed index files are byte-identical to what it writes.

## x86_64 ABIv11: pool v11-2, AROS One 1.3, QEMU TCG (guest-x86_64/, p.script)

- apkg 0.4.2 from its release archive (b0c107a4...): `self-update --check`
  reports 0.5.0 and says PkgManager is updated from the archive;
  `self-update` replaced it, rc 0; `--version` then shows e7432440..., the
  hash in the 0.5.0 archive's BUILD.txt.
- With that apkg and this index-v2.json: `show arospkg` 0.5.0, requirements
  satisfied; `install arospkg` from GitHub, rc 0; `verify` clean (28 files);
  `run --check arospkg` ready (Workbench, stack 65536 from PkgManager.info);
  `run arospkg` started PkgManager (`screens/pkgmanager-started-by-apkg-run.png`),
  whose About > Check for a newer version says "PkgManager 0.5.0 is the
  latest stable release" (`screens/about-check.png`).
- apkg 0.3.2 from its release archive (d2cfb1af...) with this index.json:
  `search arospkg` lists 0.5.0 x86_64; `install arospkg` fetched the
  release asset, verified, installed, rc 0; the installed apkg is 0.5.0;
  `search game` still lists the catalogue's games. The signed query of the
  GitHub redirect is removed from `p50-22-old-install.txt`.

## i386 ABIv0: pool v0-2 (guest-i386/, q.script)

apkg 0.4.2 i386 (3fe5cd4d...): `self-update` to 0.5.0, rc 0, binary
4e89d400... as in BUILD.txt; `install arospkg` through this index-v2.json,
rc 0; the installed apkg reports 0.5.0.

## Not shown here

- Eight approved third-party entries no longer match their files on AROS
  Archives (the uploads were replaced): amiberry, gicriptofilex, giocodel15,
  pkg-7mezzoplus, scopagame, tressette, xrick, zgloom (x86_64). Installing
  them fails verification, as it should. Their sums are not changed here;
  each needs its new upload reviewed.
