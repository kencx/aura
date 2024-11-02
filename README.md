# aura

An [aurutils](https://github.com/AladW/aurutils) wrapper for managing a
custom Arch repository hosted on an Minio S3 bucket.

## Usage

A [custom
repository](https://wiki.archlinux.org/title/Pacman/Tips_and_tricks#Custom_local_repository)
should already be setup, along with the S3 bucket. aura should only be run with
this custom repository's user.

aura requires the following dependencies:

- aurutils
- mcli and S3 alias setup
- paccache

Set the following variables:

```
DB="<db-name>"
S3_ALIAS="<s3-alias>"
BUCKET="<s3-bucket"
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

# License

Copyright (c) 2023-2024 kencx

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
