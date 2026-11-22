# arospkg-index

This is the software catalogue that [arospkg](https://github.com/tomaszstaniak/arospkg) reads. `apkg update` and the **Update index** button in PkgManager download it, and every program they offer to install is described here.

The repository holds descriptions only. Each archive stays where its author published it, on [AROS Archives](https://archives.arosworld.org), in a GitHub release or on another HTTPS host, and is downloaded from there at install time.

On 2026-11-22 the catalogue described 130 programs in 209 builds, and anyone who writes or ports AROS software can add theirs.

## Add your program

You do not need to be part of this project, and your source code can stay private. You keep hosting the archive yourself; the catalogue stores its URL, size and SHA-256.

`apkg-pack` does the work. It runs on macOS, Linux or Windows with Python 3.11 or later:

1. Put everything the user needs in one drawer, with the drawer icon beside it.
2. `python3 apkg-pack.pyz init build/MyApp` reads the name, version, CPU and icon from the drawer and asks for the rest: the ABI, the licence and what the program needs from the system.
3. `python3 apkg-pack.pyz build build/MyApp --output myapp.zip` writes a ZIP with the description inside it. Test that ZIP on AROS.
4. Upload the ZIP unchanged, then submit its URL:

```text
python3 apkg-pack.pyz submit <download-url> --pr
```

`submit` downloads the archive, measures it and opens a pull request here from your GitHub account, through a fork when it needs one. It uses Git and the GitHub CLI for that. You do not copy any field by hand.

If you would rather not use the GitHub CLI, run `submit` with `--index <a clone of this repository> --dry-run`. It writes the same entry without opening anything, and you can attach that file to an issue here.

An archive that is already public and has no description inside it can be listed too, with a hand-written entry in `manifests/`. The [packaging guide](https://github.com/tomaszstaniak/arospkg/blob/main/docs/guide/packaging.md#4-the-manifest) has a complete example.

The [authoring guide](https://github.com/tomaszstaniak/arospkg/blob/main/docs/guide/authoring.md) covers every step, including the download link for `apkg-pack`, the launch settings and notes shown after installation.

### What happens to a submission

The pull request is checked automatically: the archive is downloaded and compared with the entry, its member names and layout are examined, and the CPU of its programs is read. A maintainer then reads the entry and merges it. The catalogue is regenerated after the merge, and users see the program the next time they update their index.

We do not have to run your program before it is listed. If you or anyone else has installed and started it, that can be recorded separately as a runtime test.

For the next release, change `version` in the description kept beside your drawer, build, upload under a new URL and submit again. A packaging fix of the same program version gets a higher `revision`.

## Corrections and test reports

A wrong hash, a moved URL, a missing requirement or a description that no longer fits: open an issue or a pull request. The same goes for a report that you installed and started a listed archive, with the system and the date. Those reports become records in `runtime-tests/`.

## Supported systems

A binary built for one AROS ABI does not run on another, however correctly it is installed. Every entry names its CPU and ABI, so a client can tell before downloading anything whether a package fits the machine.

| `arch` | `abi` | System | Builds on 2026-11-22 |
|---|---|---|---|
| `x86_64` | `v11` | ABIv11 distributions | 130 |
| `x86_64` | `v1` | mainline AROS x86_64 | 18 |
| `aarch64` | `v1` | AROS on Raspberry Pi | 20 |
| `i386` | `v0` | 32-bit ABIv0 distributions | 41 |

AROS Archives marks ABIv11 files with a `-v11` suffix in the file name; an x86_64 file without it is built for mainline. arospkg 0.7.8 is the last client built for i386. The i386 entries stay in the catalogue.

These numbers count published entries. They say nothing about how many of the programs have been started on each system; the runtime tests do.

## What is in the repository

| Path | Contents |
|---|---|
| `manifests/` | One file per approved build, `<id>.<arch>.<abi>.toml` (older ones `<id>.<arch>.toml`). Everything else is derived from these files. |
| `overrides/` | A maintainer's deliberate corrections to an author's entry, under the manifest's file name. They are applied each time the catalogue is generated. |
| `runtime-tests/` | Who installed and started which archive, on which system and when. Each record names the SHA-256 of the archive it was made with. |
| `review/` | The result of the automatic review of each entry. The publish workflow writes it. |
| `history.json` | The day each program and each build was first published. The publish workflow writes it. |
| `index-v3.json` | The generated catalogue for apkg 0.6 and later, with every package. |
| `index-v2.json` | The catalogue for apkg 0.4 and 0.5. It leaves out packages that use `depends`, `conflicts` or `replaces`, which those clients do not read and would install without what they need. |
| `index.json` | The catalogue for clients up to 0.3.2. It keeps the packages it already lists up to date, within the 8192 JSON values those clients can read. |

The three catalogue files are generated from the manifests, and a client must be able to rebuild its view from the manifests alone.

## Listed, reviewed, runtime tested

These are three separate facts about one build and one archive.

A build is listed when its manifest is published with `status = "approved"`. Someone has measured the archive's size and SHA-256 and checked what it depends on.

A build is reviewed when the automatic review passed for exactly these bytes and this entry. The review covers the metadata, the size and SHA-256, whether the archive can be read, whether its member names are safe and cannot overwrite one another (also without regard to case), the drawer, icon and program the entry names, and the CPU of the programs. The ABI cannot be read from the files and is not guessed. Nobody installs or starts the program for the review, and nobody can declare the result: the workflow writes it into `review/`.

A build is runtime tested when `runtime-tests/` has a record that someone installed and started that archive on a named system on a given date. A changed archive starts without any.

The manifest fields `installs_on`, `runs_on` and `does_not_run_on` are older free-form claims, kept as they were. The [metadata reference](https://github.com/tomaszstaniak/arospkg/blob/main/docs/guide/metadata.md#listed-reviewed-and-runtime-tests) specifies all of these fields.

## Dependencies and system requirements

`depends` names other packages in this catalogue. `requires_system` names things the machine must already provide, such as a library that ships with the system. The two are separate fields because the failures differ: "no package in the catalogue provides X" can be fixed by publishing a package, and "this machine does not have X" cannot. The client reports which of the two it hit and refuses before downloading anything.

The same archive can therefore be installable on one machine and refused on another that lacks a library.

When the client cannot decide a requirement, it reports it as undetermined. The install proceeds, says so, and the doubt is written into the local registry.

## Verification

Every entry carries a `sha256` and a `size`, and the client refuses to install a download that does not match both. An entry without a hash cannot be installed at all.

When a file changes under a published URL, its download no longer matches the entry and installing it fails. The new file is measured and reviewed, then published under a new `revision` (the same program version) or a new `version`. A published version and revision never get other bytes, and the old runtime tests stay with the old bytes. A file that cannot be reconciled is withdrawn with `excluded = "<reason>"`.

## How pull requests are checked

`.github/workflows/check.yml` runs on every pull request. It downloads each archive whose entry changed, within the client's limits, reviews it, and generates the catalogue files as a trial. A change to comments only reviews nothing. A pull request that edits `review/` or `history.json` fails. The check has no secrets and runs nothing from the pull request or the archive.

After a merge, `.github/workflows/publish.yml` reviews the entries changed since the last published catalogue, regenerates the catalogue and commits it with the review records and first publication dates. A failed run leaves the published files as they were. The branch `published` marks that catalogue; only the workflow moves it, together with `main`, so merges in quick succession are all reviewed. Run by hand with *review_all*, the workflow reviews every entry, and one that no longer passes stays listed.
