<a name="top"> </a>

[This document is formatted with GitHub-Flavored Markdown.               ]:#
[For better viewing, including hyperlinks, read it online at             ]:#
[https://github.com/sourceryinstitute/OpenCoarrays/blob/master/INSTALL.md]:#

# Installing OpenCoarrays #

[![GitHub release](https://img.shields.io/github/release/sourceryinstitute/OpenCoarrays.svg?style=flat-square)](https://github.com/sourceryinstitute/OpenCoarrays/releases/latest)
[![Github All Releases](https://img.shields.io/github/downloads/sourceryinstitute/OpenCoarrays/total.svg?style=flat-square)](https://github.com/sourceryinstitute/OpenCoarrays/releases/latest)
[![Download as PDF][pdf img]][INSTALL.pdf]

Download this file as a PDF document
[here][INSTALL.pdf].

* [End-User Installation]
  * [macOS]
  * [Windows]
  * [Linux]
  * [FreeBSD]
  * [Virtual machine]
  * [Installation Script]
* [Advanced Installation from Source]
  * [Prerequisites]
  * [CMake scripts]
  * [Make]

## End-User Installation ##

Most users will find it easiest and fastest to use package management
software to install OpenCoarrays.  Package management options for
macOS, Windows, and Linux are described first below. Also described 
below are options for installing via the Sourcery Institute virtual 
machine or via the bash and/or CMake scripts included that are in the 
OpenCoarrays source.

[top]

### macOS ###

[![homebrew](https://img.shields.io/homebrew/v/opencoarrays.svg?style=flat-square)](http://braumeister.org/formula/opencoarrays)

* [Homebrew]: This is the recommend OpenCoarrays installation method on macOS.

Basic Homebrew installation steps:

```bash
brew update
brew install opencoarrays
```

OpenCoarrays also ships with a
[`Brewfile`][Brewfile]
that will make it easier to install opencoarrays using MPICH built
with the GNU Compiler Collection ([GCC]). To install using the
[`Brewfile`][Brewfile]
with MPICH wrapping GCC, follow these steps:

```bash
brew tap homebrew/bundle
brew update
brew bundle
```

* [MacPorts]: An unmaintained [OpenCoarrays Portfile] exists for the [MacPorts] package
manager.  Although the current OpenCoarrays contributors have no plans to 
update the portfile, new contributors are welcome to asssume the port 
maintainer role and to submit a pull request to update this [INSTALL.md] file.

[top]

### Windows ###

Windows users may run the [install.sh] script inside the Windows Subsystem 
for Linux ([WSL]). The script uses Ubuntu's [APT] package manager to build 
[GCC] 5.4.0, [CMake], and [MPICH].  Windows users who desire a newer version 
of GCC are welcome to submit a request via our [Issues] page and suggest a 
method for updating. Previously attempted upgrade methods are described in 
the discussion thread starting with [commit comment 20539810].

[top]

### Linux ###

Access OpenCoarrays on Linux via any of the following package managers
or pre-installed copies:

* The [linuxbrew] package manager installs OpenCoarrays on all Linux distributions.
* Debian-based distributions such as Ubuntu provide an "open-coarrays" [APT package].
* [Arch Linux] provides an [aur package].
* An [HPCLinux] installation script is in the [developer-scripts] subdirectory (available via git clone only).
* [EasyBuild] can install OpenCoarrays on Linux distributions
* [Spack], a multiplatform package manager, can also install OpenCoarrays on Linux distributions

[linuxbrew] does not require `sudo` privileges and will generally
provide the most up-to-date OpenCoarrays release because linxubrew
pulls directly from macOS homebrew, which updates automatically.


<a name="easybuild"></a>
With [EasyBuild], the following bash commands install OpenCoarrays:
```bash
# Search available specification files (also known as easyconfigs) for OpenCoarrays
eb --search OpenCoarrays

# Automatically download prerequisites (with the --robot flag) and install OpenCoarrays
# with the desired easyconfig, e.g., OpenCoarrays-1.9.0-gompi-2017a.eb
eb OpenCoarrays-1.9.0-gompi-2017a.eb --robot
```
Once installed, use OpenCoarrays by loading the newly created environment
module `OpenCoarrays/1.9.0-gompi-2017a`:
```bash
module load OpenCoarrays/1.9.0-gompi-2017a
```

<a name="spack"></a>
With [Spack], the following commands install OpenCoarrays in a bash shell:
```bash
# Check build information for OpenCoarrays in the default specification file
spack spec opencoarrays

# To automatically download prerequisites and install OpenCoarrays with the default specification.
# (Note: In addition to its own prerequisites, Spack requires gfortran compiler
# to be installed to compile OpenMPI)
spack install opencoarrays

# Or, To install with customisations (e.g., to install OpenCoarrays [version 1.9.0]
# with MPICH [version default] and GCC [version 7.1.0]).
spack install opencoarrays@1.9.0 ^mpich %gcc@7.1.0
```
In the previous example, it was assumed that GCC [version 7.1.0] is already installed, and is available as
a compiler to Spack. Otherwise, [add a new compiler to Spack].
Once installed, OpenCoarrays can be used by [loading the environment modules with Spack], e.g.
```bash
spack module loads --dependencies opencoarrays
```

[top]

### FreeBSD ###

Use the OpenCoarrays FreeBSD, Port to install OpenCoarrays by executing the following commands as root:
```bash
pkg install opencoarrays
```
For more information, please review the [FreeBSD ports/packages installation information](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/ports.html).

[top]

## Virtual machine ##

Users of macOS, Windows, or Linux have the option to use OpenCoarrays
by installing the Lubuntu Linux virtual machine from the
[Sourcery Institute Store].  The virtual machine boots inside the
open-source [VirtualBox] virtualization package.  In addition to
containing [GCC], [MPICH], and OpenCoarrays, the virtual machine
contains dozens of other open-source software packages that support
modern Fortran software development.  See the
[download and installation instructions] for a partial list of the
included packages.

[top]

## Installation Script ##

If the above package management or virtualization options are
infeasible or unavailable, Linux, macOS, and [WSL] users may also install
OpenCoarrays by downloading and uncompressing our [latest release] and
running our installation script in the top-level OpenCoarrays source
directory (see above for the corresponding [Windows] script):

```bash
tar xvzf OpenCoarrays-x.y.z.tar.gz
cd OpenCoarrays-x.y.z
./install.sh
```

where `x.y.z` should be replaced with the appropriate version numbers.
A complete installation should result in the creation of the following
directories inside the installation path (.e.g, inside `build` in the
above example):

* `bin`: contains the compiler wrapper (`caf`) and program launcher
  (`cafun`).
* `mod`: contains the `opencoarrays.mod` module file for use with
  non-OpenCoarrays-aware compilers
* `lib`: contains the `libcaf_mpi.a` static library to which codes
  link for CAF support


### Example script invocations ###
Execute `./install.sh --help` or `./install.sh -h` to see a list of flags
that can be passed to the installer.  Below are examples of useful combinations
of flags. Each flag also has a single-character version not shown here.

1. Install after building any missing prerequisites -- all source, build,
and installation files will be inside the OpenCoarrays source tree under
prerequisites/installations:
```bash
$ ./install.sh
```

2. Install non-interactively by assuming a "yes" answer to all questions
```bash
$ ./install.sh --yes-to-all
```

3. Install with the specified compilers, overriding the default compilers:
```bash
$ ./install.sh --with-fortran <path-to-gcc-bin>/gfortran \
               --with-cxx <path-to-gcc-bin>/g++ \
               --with-c <path-to-gcc-bin>/gcc 
```
Without the latter arguments, [install.sh] will attempt to install the
default GCC version even if a newer version is available.  This happens
to protect users from instability in cases when known one or more
known regressions exist in the newer compiler.

4. Install only a specific prerequisite package (the default version):
```bash
$ ./install.sh --package mpich
```

5. Install a specific version of a prerequisite:
```bash
$ ./install.sh --package cmake --install-version 3.7.0
```

6. Download a prerequisite package (e.g., gcc/gfortran/g++ below) but
don't build or install it:
```bash
$ ./install.sh --only-download gcc
```

7. Print the default URL, version, or download mechanism that the
script will use for a given prerequisite package (e.g., mpich below)
on this system:
```bash
$ ./install.sh --print-url mpich
$ ./install.sh --print-version mpich
$ ./install.sh --print-downloader mpich
```

8. Install a prerelease branch (e.g., trunk below) of the GCC repository:
```bash
$ ./install.sh --package gcc --install-branch trunk
```

9. Install to a specific location:
```bash
$ ./install.sh --install-prefix /opt/gnu/
```
If the path provided in the install prefix requires sudo privileges,
the user will be prompted for a password after the package download
and build complete and just before installing to the specified path.

10. Install a prerequisite package from a non-default URL:
```bash
$ ./install.sh --package gcc \ 
  --from-url https://github.com/sourceryinstitute/gcc/archive/teams-20170919.tar.gz \
  --install-version teams-20170919
```
The latter command will install the Sourcery Institute GCC fork that provides 
experimental support for the Fortran 2015 teams feature.

11. Speed up a GCC build at a higher risk of a faild build:
```bash
./install.sh --package gcc --disable-bootstrap
```
If the latter command works, it could reduce GCC's build time from hours down to
minutes.

12. Speed up a GCC build with multithreading at a risk of a failed build:
```bash
./install.sh --package gcc --num-threads 4
```
The latter command sometimes fails if the GCC build system has not fully
specified dependencies between source files.


[top]

## Advanced Installation from Source ##

### Prerequisites ###

Package managers and the [install.sh] attempt to handle the installation
of all OpenCoarrays prerequisites automatically.  Installing with CMake
or the provided, static Makefile burdens the person installing with the
need to ensure that all prerequisites have been built and are in the
expected or specified locations prior to building OpenCoarrays. The 
prerquisite package/version dependency tree is as follows:

```text
opencoarrays
├── cmake-3.4.0
└── mpich-3.2
    └── gcc-6.1.0
        ├── flex-2.6.0
        │   └── bison-3.0.4
        │       └── m4-1.4.17
        ├── gmp
        ├── mpc
        └── mpfr
```

[top]

### CMake scripts ###

On most platforms, the [install.sh] script ultimately invokes [CMake] after performing
numerous checks, customizations, and installations of any missing prerequisites.  
Advanced users who prefer to invoke CMake directly may do so as described here.
CMake is a cross-platform Makefile generator that includes the testing tool CTest.  
To avoid cluttering or clobbering the source tree, our CMake setup requires that
your build directory be any directory other than the top-level OpenCoarays
source directory.  In a bash shell, the following steps should build OpenCoarrays,
build the tests, run the tests, and report the test results:

```bash
tar xvzf opencoarrays.tar.gz
cd opencoarrays
mkdir opencoarrays-build
cd opencoarrays-build
CC=gcc FC=gfortran cmake .. -DCMAKE_INSTALL_PREFIX=${HOME}/packages/
make
ctest
make install
```

where the the first part of the cmake line sets the CC and FC
environment variables and the final part of the same line defines the
installation path as the `packages` directory in the current user's
`$HOME` directory.  Please report any test failures via the
OpenCoarrays [Issues] page. Please note that you need a recent
GCC/GFortran, and a recent MPI-3 implementation. If CMake is having
trouble finding the MPI implementation, or is finding the wrong MPI
implementation, you can try setting the `MPI_HOME` environment
variable to point to the installation you wish to use. If that fails,
you can also try passing the
`-DMPI_Fortran_COMPILER=/path/to/mpi/fortran/wrapper/script` and
`-DMP_C_COMPILER=/path/to/mpi/c/wrapper/script` options to CMake.

Advanced options (most users should not use these):

```CMake
-DMPI_HOME=/path/to/mpi/dir  # try to force CMake to find your preferred MPI implementation
        # OR
-DMPI_C_COMPILER=/path/to/c/wrapper
-DMPI_Fortran_COMPILER=/path/to/fortran/wrapper

-DHIGH_RESOLUTION_TIMER=ON   # enables timers that tick once per clock cycle
-DCOMPILER_PROVIDES_MPI      # is set automatically when building with-the Cray
                             # Compiler Environment
```

[top]

### Make ###

Unlike the Makefiles that CMake generates automatically for the chosen
platform, static Makefiles require a great deal more maintenance and are
less portable.  Also, the static Makefiles provided in [src] lack several
several important capabilities.  In particular, they will not build the tests;
they will not generate the `caf` compiler wrapper that ensures correct linking
and `cafrun` program launcher that ensures support for advanced features such
as Fortran 2015 failed images; they will not build the [opencoarrays] module 
that can be used to provide some Fortran 2015 features with non-Fortran-2015
compilers; nor do the static Makefiles provide a `make install` option so you 
will need to manually move the resultant library from the build location to your chosen
installation location.

If none of the installation methods mentioned higher in this document are 
work on your platform and if CMake is unavailable, build and install the 
OpenCoarrays parallel runtime library as follows:

```bash
tar xvzf opencoarrays.tar.gz
cd opencoarray/src
make
mv mpi/libcaf_mpi.a <insert-install-path>
```
replacing the angular-bracketed text with your desired install path.

For the above steps to succeed, you might need to edit the [make.inc]
file to match your system settings.  For example, you might need to
remove the `-Werror` option from the compiler flags or name a
different compiler.  In order to activate efficient strided-array
transfer support, uncomment the `-DSTRIDED` flag inside the [make.inc]
file.

[top]

---

<div align="center">

[![GitHub forks](https://img.shields.io/github/forks/sourceryinstitute/OpenCoarrays.svg?style=social&label=Fork)](https://github.com/sourceryinstitute/OpenCoarrays/fork)
[![GitHub stars](https://img.shields.io/github/stars/sourceryinstitute/OpenCoarrays.svg?style=social&label=Star)](https://github.com/sourceryinstitute/OpenCoarrays)
[![GitHub watchers](https://img.shields.io/github/watchers/sourceryinstitute/OpenCoarrays.svg?style=social&label=Watch)](https://github.com/sourceryinstitute/OpenCoarrays)
[![Twitter URL](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?hashtags=HPC,Fortran,PGAS&related=zbeekman,gnutools,HPCwire,HPC_Guru,hpcprogrammer,SciNetHPC,DegenerateConic,jeffdotscience,travisci&text=Stop%20programming%20w%2F%20the%20%23MPI%20docs%20in%20your%20lap%2C%20try%20Coarray%20Fortran%20w%2F%20OpenCoarrays%20%26%20GFortran!&url=https%3A//github.com/sourceryinstitute/OpenCoarrays)

</div>

[Internal document links]: #

[top]: #top
[End-User Installation]: #end-user-installation
[macOS]: #macos
[Windows]: #windows
[Linux]: #linux
[FreeBSD]: #freebsd
[Virtual machine]: #virtual-machine
[Installation Script]: #installation-script

[Advanced Installation from Source]: #advanced-installation-from-source
[Prerequisites]: #prerequisites
[CMake scripts]: #cmake-scripts
[Make]: #make

[Links to source]: #

[install.sh]: ./install.sh

[URLs]: #

[linuxbrew]: http://linuxbrew.sh
[APT package]: https://qa.debian.org/popcon.php?package=open-coarrays
[APT]: https://en.wikipedia.org/wiki/APT_(Debian)
[HPCLinux]: http://www.paratools.com/hpclinux/
[Brewfile]: https://github.com/sourceryinstitute/OpenCoarrays/blob/master/Brewfile
[INSTALL.pdf]: https://md2pdf.herokuapp.com/sourceryinstitute/OpenCoarrays/blob/master/INSTALL.pdf
[INSTALL.md]: https://github.com/sourceryinstitute/OpenCoarrays/blob/master/INSTALL.md
[CMake]: https://cmake.org
[Sourcery Institute Store]: http://www.sourceryinstitute.org/store/c1/Featured_Products.html
[VirtualBox]: https://www.virtualbox.org
[download and installation instructions]: http://www.sourceryinstitute.org/uploads/4/9/9/6/49967347/overview.pdf
[yum]: http://yum.baseurl.org
[apt-get]: https://en.wikipedia.org/wiki/Advanced_Packaging_Tool
[Issues]: https://github.com/sourceryinstitute/OpenCoarrays/issues
[make.inc]: ./src/make.inc
[opencoarrays]: ./src/extensions/opencoarrays.F90
[prerequisites]: #prerequisites
[MPICH]: http://www.mpich.org
[MVAPICH]:http://mvapich.cse.ohio-state.edu
[MacPorts]: https://www.macports.org
[GCC]: http://gcc.gnu.org
[TS18508 Additional Parallel Features in Fortran]: http://isotc.iso.org/livelink/livelink/nfetch/-8919044/8919782/8919787/17001078/ISO%2DIECJTC1%2DSC22%2DWG5_N2056_Draft_TS_18508_Additional_Paralle.pdf?nodeid=17181227&vernum=0
[GFortran Binaries]:  https://gcc.gnu.org/wiki/GFortranBinaries#FromSource
[Installing GCC]: https://gcc.gnu.org/install/
[Arch Linux]: https://www.archlinux.org
[aur package]: https://aur.archlinux.org/packages/opencoarrays/
[latest release]: https://github.com/sourceryinstitute/OpenCoarrays/releases/latest
[pdf img]: https://img.shields.io/badge/PDF-INSTALL.md-6C2DC7.svg?style=flat-square "Download as PDF"
[commit comment 20539810]: https://github.com/sourceryinstitute/OpenCoarrays/commit/26e99919fe732576f7277a0e1b83f43cc7c9d749#commitcomment-20539810
[Homebrew]: https://brew.sh
[dnf]: https://github.com/rpm-software-management/dnf
[port details]: http://www.freshports.org/lang/opencoarrays
[port search]: https://www.freebsd.org/cgi/ports.cgi?query=opencoarrays
[EasyBuild]: https://github.com/easybuilders/easybuild
[Spack]: https://github.com/LLNL/spack
[add a new compiler to Spack]: http://spack.readthedocs.io/en/latest/tutorial_modules.html#add-a-new-compiler
[loading the environment modules with Spack]: http://spack.readthedocs.io/en/latest/module_file_support.html#cmd-spack-module-loads
[OpenCoarrays Portfile]: https://www.macports.org/ports.php?by=name&substr=opencoarrays
[WSL]: https://blogs.msdn.microsoft.com/commandline/2017/07/10/ubuntu-now-available-from-the-windows-store/
[developer-scripts]: https://github.com/sourceryinstitute/OpenCoarrays/tree/master/developer-scripts 
[src]: https://github.com/sourceryinstitute/OpenCoarrays/tree/master/src
