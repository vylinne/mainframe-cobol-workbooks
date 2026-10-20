# mainframe-cobol-workbooks
Covering Murach's Mainframe COBOL mixed with IBM's modern z/OS training courses.

## IBM z/OS Training Modules

Fundamentals of IBM Z Path:
https://www.ibm.com/training/learning-path/foundational-concepts-of-ibm-z-904

IBM Z SysAdmin Path: 
https://www.ibm.com/training/learning-path/ibm-z-system-administrator-learning-path-903

## (Murach) Local COBOL Development with GnuCOBOL

The COBOL programs in this repository can be compiled and run locally using **GnuCOBOL**.

### Installing GnuCOBOL on MacOS

```zsh
brew install gnucobol
```

### Installing GnuCOBOL on Arch Linux

GnuCOBOL is available through the Arch User Repository (AUR). The AUR package currently provides GnuCOBOL 3.2. 【1-bc13bb】

Install the tools required to build AUR packages:

```bash
sudo pacman -S --needed base-devel git
```

Clone the GnuCOBOL AUR package:

```bash
git clone https://aur.archlinux.org/gnucobol.git
cd gnucobol
```

#### Import the GnuCOBOL Signing Key

The GnuCOBOL source archive is signed. `makepkg` verifies source signatures using the current user's GPG keyring. If the required public key is missing, the build will stop during signature verification. 【2-f7ab87】

If the build reports:

```text
FAILED (unknown public key 13E96B53C005604E)
```

import the required key:

```bash
gpg --keyserver keyserver.ubuntu.com \
    --recv-keys 13E96B53C005604E
```

The fingerprint can be inspected with:

```bash
gpg --fingerprint 13E96B53C005604E
```

#### Disable LTO if Necessary

On this system, GCC encountered an internal compiler error while building GnuCOBOL with Link Time Optimization (LTO).

The `PKGBUILD` was modified from:

```bash
options=("!libtool")
```

to:

```bash
options=("!libtool" "!lto")
```

This disables LTO for the package build.

Then build and install GnuCOBOL:

```bash
makepkg -si
```

`makepkg` builds packages from their `PKGBUILD` and associated sources. 【2-f7ab87】

After installation, the cloned AUR build directory can be removed if it is no longer needed.

### Verify the Installation

Confirm that the GnuCOBOL compiler is available:

```bash
cobc --version
```

### Compile a COBOL Program

From the repository directory, compile a `.cbl` source file using:

```bash
cobc -x -fixed FILENAME.cbl
```

The options used are:

- `-x` builds an executable program.
- `-fixed` specifies traditional fixed-format COBOL source.

GnuCOBOL's `cobc` translates COBOL and uses the system C compiler/linker to produce executable code. 【3-2fb223】【4-43b49c】

After successful compilation:

```text
FILENAME.cbl
FILENAME
```

### Run the Program

Run the resulting executable:

```bash
./FILENAME
```

For example:

```text
--------------------------------
TO END PROGRAM, ENTER 0.
TO CALCULATE SALES TAX, ENTER SALES AMOUNT.
100
SALES TAX =     7.85
--------------------------------
TO END PROGRAM, ENTER 0.
TO CALCULATE SALES TAX, ENTER SALES AMOUNT.
0
END OF SESSION
```

## Development Workflow

For the exercises in this repository, the basic local workflow is:

```text
Write/Edit COBOL source
        |
        v
    FILENAME.cbl
        |
        v
cobc -x -fixed FILENAME.cbl
        |
        v
     ./FILENAME
```

For example:

```bash
cobc -x -fixed FILENAME.cbl
./FILENAME
```

This provides a lightweight way to compile and experiment with COBOL programs locally while working through the exercises in this repository.