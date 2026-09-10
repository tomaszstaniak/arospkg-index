# arospkg-index

The package index for [arospkg](https://github.com/tomaszstaniak/arospkg), a
package manager for AROS.

This repository holds **metadata only**. It contains no binaries and never
will: packages are downloaded from [AROS
Archives](https://archives.arosworld.org), which remains the only place the
software itself lives. Nothing here is mirrored from there.

## What is in here

- `index.json` — the generated index the client downloads with `pkg update`.
- `manifests/` — one file per approved package, `<id>.<arch>.toml`.

`index.json` is generated from the manifests, so the manifests are the source
of truth and the index is derived. A client must be able to rebuild its view
from the manifests alone.

## What "approved" means

A manifest is published only after someone has downloaded the archive,
recorded its SHA-256, looked inside it to establish what the package actually
installs, and formed a view on its dependencies. An entry that has not had that
done to it is not here.

That is why this index is small. The AROS Archives catalogue holds 1895
entries, of which 168 are built for x86_64 or aarch64 — but a candidate is not
an installable package, and publishing a list of things that fail to install
would be worse than publishing a short list that works.

## Verification

Every entry carries a `sha256` and a `size`, and the client refuses to install
a package whose download does not match both. An entry without a hash cannot be
installed at all; that is deliberate, because an unverified download is the
whole attack against a package manager.

## Contributing

Manifests are reviewed before they are published. Corrections to an existing
entry — a wrong hash, a moved URL, a dependency we missed — are welcome as
issues or pull requests.
