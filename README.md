Rationales and explanations about Xecrets Ez for flathub
========================================================

Xecrets Ez is a cross-platform desktop app to, among other things, encrypt, decrypt and
(relatively) securely delete files. It also can be used to backup passwords and seed phrases
with Shamir's Secret Sharing, splitting the secret into N parts, and only require M parts
to recover the secret. Plain text can be encrypted as well. There are many more features,
please see https://www.axantum.com/ for all details.

It runs as a single portable executable on Windows, Linux and macOS - but can also be
installed for greater ease-of-use and file type association etc via Microsoft Store,
a DMG type installer for macOS and a standalone flatpak for Linux. The next logical
step is to make it available on flathub.

History
-------

The software has a very long history, the first iteration was released to the public in 1999.
This is the third iteration, and it was released in 2024, although it in turn is largely
based on a code base that was first released in 2016 when it was called AxCrypt 2.x.

The current architecture is based on two components, a callable command line interface that
does all the heavy lifting, which is open source and available on https://github.com/xecrets/xecrets-cli.

xecrets-cli is precompiled and included in the portable distributable executable for xecrets-ez,
which in effect is mostly a graphical user interface front-end to xecrets-cli. The front-end is
proprietary and licensed according to the license included in the package.

Manifest lint warnings
----------------------

The linter gives the following output:

```
{
    "errors": [
        "finish-args-host-filesystem-access",
    ],
    "message": "See https://docs.flathub.org/linter for details and exceptions"
}
```

Host file system access is required because of the nature of the application. It is a cross-platform
application intended to encrypt and decrypt files in all locations on the system, including external
devices and network shares. It would not be natural, and it would cripple it quite severaly if file
access was sandboxed to a few specific locations. It can also be used to overwrite files, which is
more secure than just deleting them from the file system, and that is another reason why host file
system access is required.

Also some features are likely to break and make the app less useful, notably the ability to copy
and paste files to encrypt/decrypt/open them. This won't work seamlessly with portals.

Repo lint warnings
------------------

The linter gives the following output:

```
{
    "errors": [
        "finish-args-host-filesystem-access",
    ],
    "message": "See https://docs.flathub.org/linter for details and exceptions"
}
```

Host file system access is discussed above.

About building from source
--------------------------

Xecrets Ez is architected around an open source command line, Xecrets Cli, while the graphical front-end
is proprietary. In theory it would be possible to build Xecrets Cli from source, but it's non-trivial to build
Xecrets Ez. The build process is fairly complicated, and is also built around various tools and scripts
custom built for a TeamCity CI server which handles all the issues of cross-platform building as well
as various differences between a release build and a beta build. Since XecrretsEz is built as as single
file portable executable, we'd like to keep that model and distribute exactly the same binary via Flathub
as we do in the stand alone flatpak, as well as the executable-only .tar.gz tarball distribution.
