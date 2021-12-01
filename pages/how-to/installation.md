# Installing fpm

This how-to guide covers the installation of the Fortran package manager (fpm) on various platforms.


## {fab}`windows` MSYS2 package manager

The [MSYS2 project](https://www.msys2.org>) provides a package manager and makes many common Unix tools available for Windows.

:::{note}
To install download the ``msys2-x86_64-YYYYMMDD.exe`` installer from the MSYS2 webpage and run it.
MSYS2 will create several new desktop shortcuts, like *MSYS terminal*, *MinGW64 terminal* and *UCRT64 terminal* (more information on MSYS2 terminals are available [here](https://www.msys2.org/docs/terminals/)).

The Fortran package manager is supported for the the *UCRT64*, *MinGW64*, or *MinGW32* terminal.
:::

Open a new terminal and update your installation with

```{code-block} bash
pacman -Syu
```

You might have to update MSYS2 and ``pacman`` first, restart the terminal and run the above command again to update the installed packages.

If you are using the *MinGW64 terminal* you can install the required software with

```{code-block} bash
pacman -S git mingw-w64-x86_64-gcc-fortran mingw-w64-x86_64-fpm
```

:::{tip}
Both *git* and *gfortran* are not mandatory dependencies for running fpm.
If you provide *git* and *gfortran* from outside they will get picked up as well.
:::


## {fab}`apple` Homebrew package manager

The Fortran package manager (fpm) is available for the [homebrew](https://brew.sh) package manager on MacOS via an additional tap.
To install fpm via brew, include the new tap and install it using

```{code-block} bash
brew tap awvwgk/fpm
brew install fpm
```

Binary distributions are available for MacOS 11 (Catalina) and 12 (Big Sur) for x86\_64 architectures.
For other platforms fpm will be built locally from source automatically.

Fpm should be available and functional after those steps.


## {fab}`apple` {fab}`linux` Conda package manager

Fpm is available on [conda-forge], to add conda-forge to your channels use:

```{code-block} bash
conda config --add channels conda-forge
```

Fpm can be installed with:

```{code-block} bash
conda create -n fpm fpm
conda activate fpm
```

Alternatively, if you want fpm to be always available directly install into your current environment with

```{code-block} bash
conda install fpm
```

:::{note}
The conda package manager can be installed from [miniforge](https://github.com/conda-forge/miniforge/releases)
or from [miniconda](https://docs.conda.io/en/latest/miniconda.html).
:::

[Conda]: https://conda.io
[conda-forge]: https://conda-forge.org/


## {fab}`linux` Arch Linux user repository

The Arch Linux user repository (AUR) contains two packages for the Fortran package manager (fpm).
With the [fortran-fpm-bin](https://aur.archlinux.org/packages/fortran-fpm-bin/) installs the statically linked Linux/x86\_64 binary from the release page, while the [fortran-fpm](https://aur.archlinux.org/packages/fortran-fpm/) package will bootstrap fpm from source.

Select one of the PKGBUILDs and retrieve it with

```{code-block} bash
git clone https://aur.archlinux.org/fortran-fpm.git
cd fortran-fpm
```

As usual, first inspect the PKGBUILD before building it.
After verifying the PKGBUILD is fine, build the package with

```{code-block} bash
makepkg -si
```

Once the build passed pacman will ask to install the fpm package.


## Building from source

To build fpm from source get the latest fpm source, either by cloning the repository from GitHub with

```{code-block} none
git clone https://github.com/fortran-lang/fpm
cd fpm
```

or by downloading a source tarball from the latest source

```{code-block} none
wget https://github.com/fortran-lang/fpm/archive/refs/heads/main.zip
unzip main.zip
cd fpm-main
```

The available install script allows to bootstrap fpm by using just a Fortran compiler, git and network access.
Invoke the script to start the bootstrap build

```{code-block} none
./install.sh
```

Fpm will be installed in ``~/.local/bin/fpm``.

:::{note}
Building the bootstrapper binary from the single source file version might take a few seconds, which might make the install script look like it is hanging.
:::

:::{tip}
The installation location can be adjusted by passing the ``--prefix=/path/to/install`` option.
:::

If you can't run the install script, you can perform the bootstrap procedure manually, with the following three steps:

1. Download the single source version of fpm

   :::{code-block} none
   wget https://github.com/fortran-lang/fpm/releases/download/current/fpm.F90
   :::

2. Build a bootstrap binary from the single source version

   :::{code-block} none
   mkdir -p build/bootstrap
   gfortran -J build/bootstrap -o build/bootstrap/fpm fpm.F90
   :::

3. Use the bootstrap binary to build the feature complete fpm version

   :::{code-block} none
   ./build/bootstrap/fpm install
   :::