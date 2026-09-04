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

This repository redistributes official OpenFOAM releases with one deliberate
patch: `etc/bashrc`'s `WM_PROJECT_DIR` is changed from a path hardcoded to the
apt install location (`/usr/lib/openfoam/openfoam2412`, which Debian's own
packaging bakes in) to self-detect its location at source time, so the
archive can be extracted anywhere. The solver itself is untouched: no
source, binaries, or numerics are built, patched or repackaged.

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
| `releases/2412/` | OpenFOAM 2412 (ESI/OpenCFD distribution), as published at [openfoam.com](https://www.openfoam.com/), with the `WM_PROJECT_DIR` relocation patch described above |
| `releases/2412/` (MPI bundle) | A minimal, relocatable OpenMPI 4.1 runtime (`mpirun`, `libmpi`, and the MCA plugin set); see MPI below |

Each release directory contains the same archive distributed by OpenCFD,
alongside a `SHA256SUMS` file so a download here can be verified against the
original.

**Version note:** OpenFOAM has two distributions with incompatible version
schemes. This repository mirrors the ESI/OpenCFD distribution
(`openfoam.com`), versioned `YYMM` (`2412` = December 2024), the one
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

## MPI

OpenFOAM's `WM_MPLIB=SYSTEMOPENMPI` setting normally expects the distro's own
OpenMPI install (`apt-get install openmpi-bin libopenmpi3`). For environments
where that apt install isn't practical either, this release also carries a
self-contained, relocatable OpenMPI 4.1 runtime, no root and no system
package manager needed:

```bash
curl -L -o mpi.tar.gz \
    https://github.com/CakeCFD/cake-openFOAM-lib/releases/download/v2412/mpi-openmpi-2412-linux-x86_64.tar.gz
tar -xzf mpi.tar.gz -C /opt
source /opt/mpi-openmpi-2412-linux-x86_64/activate.sh
```

`activate.sh` sets `PATH`, `LD_LIBRARY_PATH` and
`OMPI_MCA_mca_base_component_path` relative to wherever you extracted it.
Source the OpenFOAM `bashrc` above first, then this, and `mpirun` /
`decomposePar`-based parallel runs work without any system OpenMPI install.
This bundle is tied to glibc/Ubuntu 24.04-era ABI, matching the OpenFOAM
build above; on a materially different distro, install OpenMPI via that
distro's own package manager instead.

## Verifying a download

```bash
sha256sum -c SHA256SUMS
sha256sum -c MPI_SHA256SUMS
```

## Reporting problems

Please report defects (a corrupted archive, a checksum mismatch, a missing
release) through the GitHub issue tracker for this repository.
