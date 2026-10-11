# arospkg 0.4.2: self-update and install through this catalogue, 2026-10-11

The release: https://github.com/tomaszstaniak/arospkg/releases/tag/v0.4.2,
built from arospkg commit bbefb2f. The release assets were downloaded on the
host and checked against the release's SHA256SUMS (all OK); the manifests
carry those sizes and hashes:

| archive | bytes | SHA-256 |
|---|---|---|
| arospkg-0.4.2.x86_64-aros-v11.zip | 741579 | 5536075b79e568776ff33979b17f0cbdc29a838ff341d400dd56aba420d0aa12 |
| arospkg-0.4.2.i386-aros-v0.zip | 311192 | a50c121af16aabfd07436ce011b1e4a393c978dca2d4d76375243ae6b2c3bbe8 |

The library names referenced by apkg and PkgManager are the same as in
0.4.1, so `requires_system` is unchanged. `ci_check_index.py . --base
origin/main` (tools at the pinned c8b4eff): all checks pass.

**index.json.** arospkg is added to index.json too (x86_64 ABIv11 and i386
ABIv0, one entry per architecture as before), at 7590 of the 8192 values
clients up to 0.3.2 read. Those clients have no `self-update`; with the
entry they can find and install 0.4.2 and then copy its apkg to C:, as the
release README says. Checked with apkg 0.3.2 below.

Test catalogues: index-v2.json and index.json of this branch (SHA-256
e4d544f2... and f02e84f7...), passed with `apkg --index`. The archives came
from GitHub over the network in every install.

## x86_64 ABIv11: pool v11-1, AROS One 1.3, QEMU TCG (guest-x86_64/, p42.script)

- apkg 0.4.1 from the 0.4.1 release archive (SHA-256 14f076ca..., as in its
  SHA256SUMS), in RAM:su:
  - `self-update --check`: "Latest stable release: 0.4.2 (x86_64-aros-v11)";
  - `self-update`: "Updated apkg to 0.4.2", previous kept as apkg.old,
    rc 0;
  - `--version` then shows binary b0c107a4..., the hash in the archive's
    BUILD.txt.
- With that apkg and this index-v2.json, in the new root RAM:cat:
  - `show arospkg`: 0.4.2, requirements satisfied;
  - `install arospkg`: installed into RAM:cat/arospkg, rc 0, and `verify`
    clean;
  - `outdated --all`: up to date;
  - `remove`: removed.
- Installed again, then started (`p42-17`, `p42-18`):
  - RAM:cat/arospkg/apkg `--version` printed 0.4.2;
  - RAM:cat/arospkg/PkgManager opened its window (`screens/q6.png`);
  - About > Check for a newer version said "PkgManager 0.4.2 is the latest
    stable release" (`screens/q8.png`).
- Steps 08 and 15 of the script name a wrong path
  (`arospkg-0.4.2.x86_64-aros-v11/`, the archive's top drawer, which apkg
  strips) and say "object not found"; the start was repeated by hand with
  the right path, as above.
- apkg 0.3.2 from its release archive (SHA-256 66fa3c4a..., as in its
  SHA256SUMS), with this index.json:
  - `search arospkg` lists 0.4.2 x86_64 and hides the i386 entry;
  - `install arospkg` fetched the release asset, "verified and cached",
    installed, rc 0;
  - `remove` worked.

  The signed query of the GitHub redirect is removed from
  `p42-14-old-install.txt`.

## i386 ABIv0: pool v0-2, QEMU TCG (guest-i386/, p42i.script)

- apkg 0.4.1 from the 0.4.1 i386 archive (SHA-256 f6ad30fe..., as in its
  SHA256SUMS):
  - `self-update --check` reports 0.4.2 (i386-aros-v0);
  - `self-update` replaced it, rc 0;
  - `--version` shows binary 3fe5cd4d..., the hash in the archive's
    BUILD.txt.
- With that apkg and this index-v2.json:
  - `install arospkg` into RAM:cat/arospkg, rc 0;
  - RAM:cat/arospkg/apkg `--version` started and printed 0.4.2;
  - `verify` clean;
  - `remove` worked.

## Not shown here

- The Raspberry Pi entry stays 0.4.0; there is no 0.4.2 Pi archive.
- Release testing of the features themselves (updates suite, Ctrl-C, GUI)
  is in arospkg `docs/evidence/2026-10-11-updates/`.
