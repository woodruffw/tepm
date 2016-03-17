`tepm`
======

`tepm` is a Tiny Executable Package Manager.

`tepm` is only one file, a `bash` script that lists "packages" available on a
remote `ssh` server and fetches them using `scp` and `sftp`.

`tepm` "packages" are really just directories containing two subdirectories (bin/
and man/) and a checksum file for validating the files within the
subdirectories.

There are no central packages or servers, just whatever you
choose to host on your own. I do **not** suggest using it for complex packages,
but it's ideal for the sort of scripts/minimally linked binaries that need to
be installed on all of your machines but aren't worth integrating into the
system package manager.

## Usage

`tepm` installs packages on a per-user basis, so you should add `~/bin` and
`~/man` to your `$PATH` and `$MANPATH` respectively.

Then:

```bash
$ tepm config # follow the instructions
$ tepm list
$ tepm install <package>
```

## Package structure

The *package tree* is stored under `/home/${TEPM_USER}/tepm` on the remote
system:

```
tepm
└── Linux
    └── x86_64
        └── xnelson
            ├── bin
            │   └── xnelson
            ├── checksums
            └── man
                └── xnelson.1
```

The tree above only shows one architecture on one system, but `tepm` can be used
with any number of combinations (the trees are just `uname -s`/`uname -m`).

Under each architecture is the package directories, which contain `bin/` and
`man/` folders as well as SHA256 checksums in the `checksums` file. The
`tepm pkg <directory>` command can be used to quickly generate the `checksums`
file and ensure that only `bin/` and `man/` are present.
