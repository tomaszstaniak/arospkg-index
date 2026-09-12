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

## ABIv1 and ABIv11

AROS x86_64 has two incompatible ABIs, and the archives distinguish them by
filename. A `-v11` suffix means **ABIv11**, used by current distributions such
as AROS One. A name without it means **ABIv1**, which is what mainline AROS
builds. A binary for one does not run on the other, however correctly it is
installed.

Each manifest records `abi`, so a client can tell before downloading anything.
Of the archives' x86_64 entries, 171 are ABIv11 and a handful are ABIv1.
That is a count of candidates marked for an ABI, not of programs confirmed to
run.

## Dependencies and system requirements are different fields

`depends` names other packages in this index. `requires_system` names things
the machine must already provide — a library that ships with the distribution,
for example. They are separate because the failures are separate: "no package
in the index provides X" can be fixed by publishing a package, and "this
machine does not have X" cannot. The client says which of the two it hit, and
refuses before downloading anything.

`sdllopan` is the first entry with system requirements. It needs
`SDL.library`, `crt.library` and `stdlib.library`, none of which is a package
anywhere — they are parts of a distribution. AROS One 1.3 ships all three;
mainline ships none. So the same archive is installable on one machine and
not on the other, and that is a fact about the machine rather than about this
index.

A requirement the client cannot decide is reported as **undetermined**: the
install proceeds, says so, and the doubt is written into the local registry.
It is not rounded up to "satisfied".

## Installing is not running

`sdlpop` and `zaphod` **install correctly on mainline AROS and
do not run there**. The same binaries run on AROS One. They are published as
test cases for the package manager, which is a different claim from "works".

`sdllopan` is not listed as installing on mainline at all: its requirements
are not met there, so the client refuses it rather than installing something
that cannot start.

Each manifest records this explicitly — `installs_on`, `runs_on`,
`does_not_run_on` — and the generated index carries the same fields, so a
client can refuse a package on a system where it is known not to start.

Why that is stated so prominently: an index that lists a package implies you
can use it. These two were measured on our own mainline build, on the official
nightly of 2026-09-10 and on AROS One, and the boundary is real. The mechanism
is not identified and is not guessed at here.

## Verification

Every entry carries a `sha256` and a `size`, and the client refuses to install
a package whose download does not match both. An entry without a hash cannot be
installed at all; that is deliberate, because an unverified download is the
whole attack against a package manager.

## Contributing

Manifests are reviewed before they are published. Corrections to an existing
entry — a wrong hash, a moved URL, a dependency we missed — are welcome as
issues or pull requests.
