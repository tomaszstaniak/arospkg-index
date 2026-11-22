# arospkg-index

Package index for [arospkg](https://github.com/tomaszstaniak/arospkg), the AROS package manager.

This repository contains package manifests and the indexes generated from them. It doesn't host any software. Each archive is downloaded from where its author published it, usually [AROS Archives](https://archives.arosworld.org/) or a GitHub release.

The index lists 130 programs (2026-11-22). AROS Archives holds far more than that, and the index grows as packages are submitted. If you write or port AROS software, you can add yours.

## Submitting a package

You don't need to be part of this project, and your source code can stay private. Pack your program with `apkg-pack` (Python 3.11 or later, on macOS, Linux or Windows), upload the ZIP wherever you publish your releases, then run:

```sh
python3 apkg-pack.pyz submit <archive-url> --pr
```

This downloads the archive, measures its size and SHA-256 and opens a pull request here from your GitHub account. It needs Git and the GitHub CLI. If you don't use the GitHub CLI, `submit --index <clone of this repository> --dry-run` writes the same entry to a file, which you can attach to an issue.

The [authoring guide](https://github.com/tomaszstaniak/arospkg/blob/main/docs/guide/authoring.md) explains how to install `apkg-pack` and describe a package.

Pull requests are checked automatically. The CI downloads the archive, verifies its size and SHA-256, validates the manifest and the archive layout, and checks that the indexes can be regenerated. A maintainer reviews the submission before merging, and users see the package the next time they update their index.

We don't have to run your program before it is listed.

Corrections to existing packages are welcome too. If you find a broken URL, an incorrect dependency or a compatibility problem, open an issue or a pull request. So are reports that you installed and started a listed package, with the system and the date.

## The package description

`apkg-pack init` writes the description for you and asks for what it cannot read from the drawer. This is a complete one, for a Shell program:

```toml
id              = "fastfetch"
version         = "2.70.0"
revision        = 4
summary         = "System information in the Shell, with the AROS logo"
category        = "utility/misc"
license         = "MIT"
kind            = "app"
arch            = "aarch64"
abi             = "v1"
subdir          = "Fastfetch"
depends         = []
depends_checked = true

[[requires_system]]
type = "library"
id   = "stdc.library"

[launch]
executable = "fastfetch"
mode       = "shell"
```

`id`, `version`, `summary`, `kind`, `arch`, `abi` and `subdir` are always needed, and so is an answer about `depends` and `requires_system`, even when it is "none". `revision`, `category`, `license`, `icon`, `post_install_notes` and `[launch]` are optional. `submit` adds `url`, `size`, `sha256` and `status` when it measures the uploaded archive, and the result is the file in `manifests/`.

The [metadata reference](https://github.com/tomaszstaniak/arospkg/blob/main/docs/guide/metadata.md#the-fields-at-a-glance) describes every field and its limits. An archive that is already public and has no description inside can be listed with a hand-written manifest; the [packaging guide](https://github.com/tomaszstaniak/arospkg/blob/main/docs/guide/packaging.md#4-the-manifest) has a complete example.

## Architecture and ABI support

AROS has several incompatible binary targets:

| `arch` | `abi` | System | Builds listed |
|---|---|---|---|
| `x86_64` | `v11` | ABIv11 distributions | 130 |
| `x86_64` | `v1` | Mainline AROS | 18 |
| `aarch64` | `v1` | AROS on Raspberry Pi | 20 |
| `i386` | `v0` | 32-bit ABIv0 distributions | 41 |

Every manifest names its target, so the package manager can check it before installation. An ABIv11 binary won't work on ABIv1, even if the archive extracts correctly.

arospkg 0.7.8 is the last client built for i386. The i386 entries stay in the index.

## Repository structure

- `manifests/`: reviewed package manifests, one per package build.
- `overrides/`: maintainer corrections to package metadata, where needed.
- `runtime-tests/`: records of who installed and started which archive, on which system and when.
- `review/` and `history.json`: results of the automatic review and first publication dates, written by the publish workflow.
- `index-v3.json`: generated catalogue for apkg 0.6 and later.
- `index-v2.json` and `index.json`: generated catalogues for older clients.

The indexes are generated from the manifests. Don't edit them, `review/` or `history.json` manually.

## Package verification

Before a package is added, its archive is downloaded and inspected: contents, dependencies, file size and SHA-256. The client checks every download against the recorded size and hash and refuses a file that doesn't match.

Being listed doesn't mean a package has been run. Where someone has installed and started an archive, `runtime-tests/` says on which system and when.

Some packages need libraries that come with the system and not from another package. These are listed in `requires_system`; package dependencies use `depends`.

The details are in [docs/verification.md](docs/verification.md).
