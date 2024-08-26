# aura

An [aurutils](https://github.com/AladW/aurutils) wrapper script for managing a
[custom local Arch Linux repository](https://wiki.archlinux.org/title/Pacman/Tips_and_tricks#Custom_local_repository).

## Setup

This script assumes that your custom repository is hosted on the local
filesystem at `/var/cache/pacman/$DB` where `$DB` is the name of the custom
repository.

After every operation, it syncs all repository files (`.db, .pkg.tar.zst` etc.) to the remote storage
location. This script supports syncing to:

- A remote filesystem with `rsync` and SSH
- A Minio S3 bucket with `mcli`

>**Note**: This script performs `--delete` for rsync and `--remove --overwrite`
>for mcli when syncing, which can cause data loss!

Clients may download and install packages from the remote fileserver or S3
bucket (at `https://example.xyz/`) by adding the following to their
`/etc/pacman.conf`

```conf

[custom]
SigLevel = Optional TrustAll
Server = https://example.xyz/path/to/repo
```


## Usage

A custom local repository should already be setup either via a static file
server or S3 bucket. aura should only be run with this custom repository's user.

aura requires the following dependencies:

- aurutils
- rsync (by default)
- mcli and S3 alias setup for S3 syncing
- git
- paccache
- fzf, awk (only for `aura search`)

Set the following mandatory variables:

```
DB="<db-name>"
```

For non-S3 support, set the following variables:

```
SSH_USER=""
SSH_HOST=""
SSH_REMOTE_PATH=""
```

For S3 support, set the following variables:

```
S3_CONFIG_DIR=""
S3_ALIAS=""
S3_BUCKET=""
```

```bash
$ aura help

# add a package
$ aura add <package>

# remove a package
$ aura rm <package>

# list all installed packages
$ aura ls

# update one package
$ aura update <package>

# update all packages
$ aura sync

# ignore a package when syncing
$ aura ignore <package>

# check for updates
$ aura check

```
## Private Git Repository
This is for your own non-AUR packages with custom `PKGBUILD` files.

1. Sparse checkout Git repository
```bash
# sparse checkout
$ mkdir arch-repos || cd arch-repos
$ git init
$ git sparse-checkout init
$ git sparse-checkout add <path>
```

2. Set the variable `LOCAL_GIT_REPO_PATH`

3. Run `aura build <package>`

## TODO

- Prevent specific clients from adding or deleting packages (rsync auth, HTTP basic auth or
  SSH via SSH)
- Add `init` subcommand for new setup of custom repositories

# License

Copyright (c) 2023-2025 kencx

Permission to use, copy, modify, and/or distribute this software for any
purpose with or without fee is hereby granted, provided that the above
copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH
REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY
AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT,
INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM
LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR
OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR
PERFORMANCE OF THIS SOFTWARE.
