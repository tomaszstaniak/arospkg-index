# Verification and review

How an entry gets into the catalogue, what is known about it once it is there, and how the client uses that.

## Listed, reviewed, runtime tested

These are three separate facts about one build and one archive.

A build is listed when its manifest is published with `status = "approved"`. Someone has measured the archive's size and SHA-256 and checked what it depends on.

A build is reviewed when the automatic review passed for exactly these bytes and this entry. The review covers the metadata, the size and SHA-256, whether the archive can be read, whether its member names are safe and cannot overwrite one another (also without regard to case), the drawer, icon and program the entry names, and the CPU of the programs. The ABI cannot be read from the files and is not guessed. Nobody installs or starts the program for the review, and nobody can declare the result: the workflow writes it into `review/`.

A build is runtime tested when `runtime-tests/` has a record that someone installed and started that archive on a named system on a given date. A changed archive starts without any.

The manifest fields `installs_on`, `runs_on` and `does_not_run_on` are older free-form claims, kept as they were. The [metadata reference](https://github.com/tomaszstaniak/arospkg/blob/main/docs/guide/metadata.md#listed-reviewed-and-runtime-tests) specifies all of these fields.

## Hashes and changed archives

Every entry carries a `sha256` and a `size`, and the client refuses to install a download that does not match both. An entry without a hash cannot be installed at all.

When a file changes under a published URL, its download no longer matches the entry and installing it fails. The new file is measured and reviewed, then published under a new `revision` (the same program version) or a new `version`. A published version and revision never get other bytes, and the old runtime tests stay with the old bytes. A file that cannot be reconciled is withdrawn with `excluded = "<reason>"`.

## Dependencies and system requirements

`depends` names other packages in this catalogue. `requires_system` names things the machine must already provide, such as a library that ships with the system. The two are separate fields because the failures differ: "no package in the catalogue provides X" can be fixed by publishing a package, and "this machine does not have X" cannot. The client reports which of the two it hit and refuses before downloading anything.

The same archive can therefore be installable on one machine and refused on another that lacks a library.

When the client cannot decide a requirement, it reports it as undetermined. The install proceeds, says so, and the doubt is written into the local registry.

## The generated catalogues

`index-v3.json` is for apkg 0.6 and later and has every package. `index-v2.json` is for apkg 0.4 and 0.5; it leaves out packages that use `depends`, `conflicts` or `replaces`, which those clients do not read and would install without what they need. `index.json` is for clients up to 0.3.2; it keeps the packages it already lists up to date, within the 8192 JSON values those clients can read.

All three are generated from the manifests, and a client must be able to rebuild its view from the manifests alone.

## How pull requests are checked

`.github/workflows/check.yml` runs on every pull request. It downloads each archive whose entry changed, within the client's limits, reviews it, and generates the catalogue files as a trial. A change to comments only reviews nothing. A pull request that edits `review/` or `history.json` fails. The check has no secrets and runs nothing from the pull request or the archive.

After a merge, `.github/workflows/publish.yml` reviews the entries changed since the last published catalogue, regenerates the catalogue and commits it with the review records and first publication dates. A failed run leaves the published files as they were. The branch `published` marks that catalogue; only the workflow moves it, together with `main`, so merges in quick succession are all reviewed. Run by hand with *review_all*, the workflow reviews every entry, and one that no longer passes stays listed.
