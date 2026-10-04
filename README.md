# arospkg-index

The package index for [arospkg](https://github.com/tomaszstaniak/arospkg), a
package manager for AROS.

This repository holds **metadata only**. It contains no binaries and never
will: packages are downloaded from [AROS
Archives](https://archives.arosworld.org), which remains the only place the
software itself lives. Nothing here is mirrored from there.

## What is in here

- `index-v2.json`: the generated catalogue clients after 0.3.2 download with
  `apkg update`, with every package.
- `index.json`: the same, for clients up to 0.3.2: the packages it already
  lists, kept up to date, within the 8192 JSON values those clients read.
- `manifests/`: one file per approved package build, `<id>.<arch>.<abi>.toml`
  (older ones `<id>.<arch>.toml`). This is
  the only copy of the approved manifests; the arospkg repository keeps the
  tools and the candidates that have not been reviewed yet.

- `overrides/`: a maintainer's deliberate corrections to an author's entry,
  under the manifest's file name, applied whenever the catalogue is made.

Both catalogue files are generated from the manifests, so the manifests are the source
of truth and the index is derived. A client must be able to rebuild its view
from the manifests alone.

## What "approved" means

A manifest is published only after someone has downloaded the archive,
recorded its SHA-256, looked inside it to establish what the package actually
installs, and formed a view on its dependencies. An entry that has not had that
done to it is not here.

**Since 2026-09-20, approval also requires that the package has been started.**
Every entry here was installed by `pkg`, started, and removed again on the
system it is built for: AROS One 1.3 x86_64 for the x86_64 entries, AROS One
2.9 32-bit for the i386 ones. A hash and a clean unpack do not qualify a
package: they say the archive arrived intact, not that the program runs. "Started" means it opened
its window or printed its output and was still running; it is not a claim that
every feature works.

That is why this index is small. The AROS Archives catalogue holds 1895
entries, of which 168 are built for x86_64 or aarch64. But a candidate is not
an installable package, and publishing a list of things that fail to install
would be worse than publishing a short list that works.

## ABIv1 and ABIv11

AROS x86_64 has two incompatible ABIs, and the archives distinguish them by
filename. A `-v11` suffix means **ABIv11**, used by current distributions such
as AROS One. A name without it means **ABIv1**, which is what mainline AROS
builds. A binary for one does not run on the other, however correctly it is
installed.

i386 AROS is a third line, **ABIv0**: the archives' `i386-aros` uploads, run by
32-bit distributions such as AROS One 2.9. Its entries carry `arch = "i386"`
and `abi = "v0"`.

Each manifest records `abi`, so a client can tell before downloading anything.
Of the archives' x86_64 entries, 171 are ABIv11 and a handful are ABIv1.
That is a count of candidates marked for an ABI, not of programs confirmed to
run.

## Dependencies and system requirements are different fields

`depends` names other packages in this index. `requires_system` names things
the machine must already provide: a library that ships with the distribution,
for example. They are separate because the failures are separate: "no package
in the index provides X" can be fixed by publishing a package, and "this
machine does not have X" cannot. The client says which of the two it hit, and
refuses before downloading anything.

`sdllopan` is the first entry with system requirements. It needs
`SDL.library`, `crt.library` and `stdlib.library`, none of which is a package
anywhere; they are parts of a distribution. AROS One 1.3 ships all three;
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

`sdllopan` is not listed as installing on mainline at all. Two separate reasons
would each refuse it there, and the ABI gate is the one that fires first,
because it is the earlier check: the package is ABIv11 and mainline is ABIv1.
Its system requirements are also unmet on mainline, but a client never gets
that far. Both facts are recorded; only one of them is what a user would see.

Each manifest records this explicitly (`installs_on`, `runs_on`,
`does_not_run_on`), and the generated index carries the same fields, so a
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
entry (a wrong hash, a moved URL, a dependency we missed) are welcome as
issues or pull requests.

## Submitting a package

An author whose archive carries `.arospkg/manifest.toml` (written by
`apkg-pack init` in the arospkg repository) runs

    apkg-pack submit <url of the uploaded archive> --pr

which downloads the archive, measures it, and opens a pull request with the
manifest. Nobody copies fields by hand. The steps are in the
[authoring guide](https://github.com/tomaszstaniak/arospkg/blob/main/docs/guide/authoring.md).

Every pull request is checked automatically (`.github/workflows/check.yml`):
each changed archive is downloaded within the client's limits, its size and
SHA-256 must match, its description and layout must pass the catalogue's
rules, and both catalogue files are generated as a trial. The check has no
secrets and runs nothing from the pull request or the archive. After a merge,
`.github/workflows/publish.yml` regenerates the catalogue and commits it; a
failed run leaves the published files as they were.

Submissions are checked automatically. Maintainers review the source and
metadata before merging. Running every application on AROS is not a
requirement for catalogue inclusion; the entries a maintainer describes and
starts are the ones the paragraph above covers.
