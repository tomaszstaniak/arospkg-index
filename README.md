# arospkg-index

The package index for [arospkg](https://github.com/tomaszstaniak/arospkg), a
package manager for AROS.

This repository holds package metadata. Application archives remain at their
publisher's URL, such as AROS Archives or GitHub Releases; they are not mirrored here.

## What is in here

- `index-v3.json`: the generated catalogue apkg 0.6 and later download with
  `apkg update`, with every package.
- `index-v2.json`: the same for apkg 0.4 and 0.5, without the packages
  that need relations they do not read (`depends`, `conflicts`,
  `replaces`): those would install there without what they need.
- `index.json`: the same, for clients up to 0.3.2: the packages it already
  lists, kept up to date, within the 8192 JSON values those clients read.
- `manifests/`: one file per approved package build, `<id>.<arch>.<abi>.toml`
  (older ones `<id>.<arch>.toml`). This is
  the source of truth for approved manifests. The arospkg repository provides
  tools for generating and reviewing candidates locally.

- `overrides/`: a maintainer's deliberate corrections to an author's entry,
  under the manifest's file name, applied whenever the catalogue is made.
- `runtime-tests/`: who installed and started which archive, on which
  system and when, `<id>.<arch>.<abi>.toml`. Each test names the SHA-256 of
  the bytes it was made with.
- `review/`: the result of the automatic review of each entry, written by
  the publish workflow. Not edited by hand.
- `history.json`: the day each application and each build was first
  published, written by the publish workflow. Not edited by hand.

Both catalogue files are generated from the manifests, so the manifests are the source
of truth and the index is derived. A client must be able to rebuild its view
from the manifests alone.

## Listed, Reviewed, runtime tested

Three separate facts about one build and one archive:

- **Listed**: an approved manifest (`status = "approved"`) is published.
  Someone has measured the archive's size and SHA-256 and formed a view on
  its dependencies; clients check every download against both, whatever
  else is known about the entry.
- **Reviewed**: the automatic review passed for exactly these bytes and this
  entry: its metadata, size and SHA-256, a readable archive, safe member
  names that cannot overwrite one another (also without regard to case),
  the drawer, icon and program it names, and the CPU of its programs. The
  ABI cannot be read from the files and is not guessed. Nobody installs or
  starts the program for this, and nobody can declare it: the workflow
  writes the result, for those bytes, into `review/`.
- **Runtime tested**: a record in `runtime-tests/` that someone installed
  and started that archive on a named system on a given date. A changed
  archive starts without one.

An entry does not have to be started by us before it is listed. The
manifest fields `installs_on`, `runs_on` and `does_not_run_on` are older
free-form claims, kept as they were. Internal run logs are kept outside this
repository. The fields are specified in the
[metadata reference](https://github.com/tomaszstaniak/arospkg/blob/main/docs/guide/metadata.md#listed-reviewed-and-runtime-tests).

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

Manifests are read by a maintainer and reviewed automatically before they
are published. Corrections to an existing entry (a wrong hash, a moved URL,
a dependency we missed) and runtime tests are welcome as issues or pull
requests.

When an upstream file changes under a published URL, its download no longer
matches the entry and installing it fails. The new file is measured and
reviewed, and published under a new `revision` (the same program version)
or a new `version`; a published version and revision never silently get
other bytes, and the old runtime tests stay with the old bytes. A file that
cannot be reconciled is withdrawn with `excluded = "<reason>"`.

## Submitting a package

Authors pack their program with `apkg-pack`, which runs on their own
computer and writes the description into the archive as
`.arospkg/manifest.toml`. After uploading the archive, they run

    apkg-pack submit <url of the uploaded archive> --pr

which downloads the archive, measures it, and opens a pull request with the
manifest. Nobody copies fields by hand. Installing the tool, the full
example and what happens after submission are in the
[authoring guide](https://github.com/tomaszstaniak/arospkg/blob/main/docs/guide/authoring.md);
this file does not repeat them.

Every pull request is checked automatically (`.github/workflows/check.yml`):
each archive whose entry changed is downloaded within the client's limits
and reviewed, and both catalogue files are generated as a trial. A change
to comments only reviews nothing; a pull request that edits `review/` or
`history.json` fails. The check has no secrets and runs nothing from the
pull request or the archive. After a merge, `.github/workflows/publish.yml`
reviews the changed entries again, regenerates the catalogue and commits it
with the review records and first publication dates; a failed run leaves the
published files as they were. Run by hand with *review_all*, it reviews
every entry; one that no longer passes stays Listed.

Submissions are checked automatically. Maintainers review the source and
metadata before merging. Running every application on AROS is not a
requirement for catalogue inclusion; the entries a maintainer describes and
starts are the ones the paragraph above covers.
