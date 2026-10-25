# OpenFOAM-lib

## About OpenFOAM-lib

OpenFOAM-lib provides OpenFOAM 2412 as a relocatable archive for systems where
the official apt packages cannot be used. It is an unofficial mirror
maintained by the CakeCFD project for use with
[CakeCFD](https://github.com/CakeCFD/cake-studio) and
[cakecfd-ai](https://github.com/CakeCFD/cakecfd-ai). Where you can, install
OpenFOAM from [openfoam.com](https://www.openfoam.com/download) instead.

The archive is the OpenFOAM 2412 Ubuntu 24.04 package tree with one change:
WM_PROJECT_DIR in etc/bashrc detects its own location instead of assuming
/usr/lib/openfoam/openfoam2412, so the archive can be extracted anywhere.

## Copyright

OpenFOAM is free software: you can redistribute it and/or modify it under the
terms of the GNU General Public License as published by the Free Software
Foundation, either version 3 of the License, or (at your option) any later
version. See the file LICENSE in this directory, or
<https://www.gnu.org/licenses/>, for the terms under which you can copy the
files. OpenFOAM is a registered trademark of OpenCFD Limited. This repository
is an independent mirror and is not affiliated with or endorsed by OpenCFD
Limited or the OpenFOAM Foundation.

## Release assets

| Asset | Contents |
|---|---|
| openfoam2412-linux-x86_64.tar.gz | OpenFOAM 2412 (ESI/OpenCFD distribution) with the relocation change above |
| mpi-openmpi-2412-linux-x86_64.tar.gz | A minimal relocatable OpenMPI 4.1 runtime, see MPI below |
| SHA256SUMS, MPI_SHA256SUMS | SHA-256 checksums of the two archives |

The checksums confirm that a download matches what was published here.

**Version note:** OpenFOAM has two distributions with incompatible version
schemes. This repository mirrors the ESI/OpenCFD distribution
(openfoam.com), versioned YYMM (2412 is December 2024), which CakeCFD and
cakecfd-ai are built against. The OpenFOAM Foundation distribution
(openfoam.org) uses integer versions such as OpenFOAM-12 and is not compatible
with CakeCFD's dictionaries or the TENO scheme library.

## Usage

Download the archive and its checksum file, verify, then extract and source
the environment:

```bash
base=https://github.com/CakeCFD/cake-openFOAM-lib/releases/download/v2412
curl -LO $base/openfoam2412-linux-x86_64.tar.gz -LO $base/SHA256SUMS
sha256sum -c SHA256SUMS
sudo tar -xzf openfoam2412-linux-x86_64.tar.gz -C /opt
source /opt/openfoam2412/etc/bashrc
```

cakecfd-ai reads the environment script from OF_BASHRC, falling back to the
standard apt location when it is unset:

```bash
export OF_BASHRC=/opt/openfoam2412/etc/bashrc
```

## MPI

The build uses WM_MPLIB=SYSTEMOPENMPI and normally expects the distribution's
OpenMPI (sudo apt-get install openmpi-bin libopenmpi3), which is the preferred
route. Where that is not possible, the release carries a relocatable OpenMPI
4.1 runtime that needs no root access:

```bash
base=https://github.com/CakeCFD/cake-openFOAM-lib/releases/download/v2412
curl -LO $base/mpi-openmpi-2412-linux-x86_64.tar.gz -LO $base/MPI_SHA256SUMS
sha256sum -c MPI_SHA256SUMS
tar -xzf mpi-openmpi-2412-linux-x86_64.tar.gz -C "$HOME"
source "$HOME/mpi-openmpi-2412-linux-x86_64/activate.sh"
```

activate.sh only adds the bundled bin and lib directories to PATH and
LD_LIBRARY_PATH and sets OMPI_MCA_mca_base_component_path. Read it before
sourcing it. Source the OpenFOAM bashrc first, then activate.sh. The runtime
targets Ubuntu 24.04's glibc, so on other distributions install OpenMPI with
the system package manager instead.

## Reporting problems

Please report defects such as a corrupted archive, a checksum mismatch or a
missing release through the GitHub issue tracker for this repository.
