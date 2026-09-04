# OpenFOAM-lib

## About OpenFOAM-lib

OpenFOAM-lib is a GitHub-hosted mirror of the OpenFOAM binaries that
[CakeCFD](https://github.com/CakeCFD/cake-studio) and
[cakecfd-ai](https://github.com/CakeCFD/cakecfd-ai) build and run against. It
exists because `openfoam.com`, the primary distribution point for OpenFOAM
2412, is not reachable from every environment a CakeCFD tool runs in
(notably, it is blocked from Claude chat sessions without terminal access).
Hosting the same release on GitHub gives those environments a working
install path.

This repository redistributes official OpenFOAM releases unmodified. It does
not build, patch or repackage the solver in any way.

## Copyright

OpenFOAM itself is free software: you can redistribute it and/or modify it
under the terms of the GNU General Public License as published by the Free
Software Foundation, either version 3 of the License, or (at your option) any
later version. See the file `LICENSE` in this directory, or
<https://www.gnu.org/licenses/>, for the terms under which you can copy the
files. OpenFOAM is a registered trademark of OpenCFD Limited; this repository
is an independent mirror and is not affiliated with OpenCFD Limited or the
OpenFOAM Foundation.

## Contents

| Path | Contents |
|---|---|
| `releases/2412/` | OpenFOAM 2412 (ESI/OpenCFD distribution), as published at [openfoam.com](https://www.openfoam.com/) |

Each release directory contains the same archive distributed by OpenCFD,
alongside a `SHA256SUMS` file so a download here can be verified against the
original.

**Version note:** OpenFOAM has two distributions with incompatible version
schemes. This repository mirrors the ESI/OpenCFD distribution
(`openfoam.com`), versioned `YYMM` (`2412` = December 2024) — the one
CakeCFD and cakecfd-ai are built against. It is unrelated to the OpenFOAM
Foundation's distribution (`openfoam.org`), which uses integer version
numbers (`OpenFOAM-11`, `-12`, ...) and is not compatible with CakeCFD's
dictionaries or the TENO scheme library's API.

## Usage

Download and extract a release, then source its environment before building
or running anything that depends on OpenFOAM:

```bash
curl -L -o openfoam2412.tar.gz \
    https://github.com/CakeCFD/cake-openFOAM-lib/releases/download/v2412/openfoam2412-linux-x86_64.tar.gz
tar -xzf openfoam2412.tar.gz -C /opt
source /opt/openfoam2412/etc/bashrc
```

`cakecfd-ai` reads the environment script path from the `OF_BASHRC`
environment variable, falling back to the standard apt install location if
it is unset:

```bash
export OF_BASHRC=/opt/openfoam2412/etc/bashrc
```

## Verifying a download

```bash
sha256sum -c SHA256SUMS
```

## Reporting problems

Please report defects (a corrupted archive, a checksum mismatch, a missing
release) through the GitHub issue tracker for this repository.
