## Setup

The following sections are details on the recommended method to set up OCaml for the content and exercises within this textbook.

### Linux

On most Linux distributions, installation of the OCaml compiler and related tools is fairly straightforward using the package manager bundled with your distribution. Here are the installation commands for different distributions:

Ubuntu, Debian:
```
sudo apt install ocaml
```

Fedora:
```
dnf install ocaml
```

CentOS:
```
yum install ocaml
```

### Windows

Windows is where OCaml installation and setup is most difficult. While it is possible to install the OCaml compiler using Cygwin, it is recommended to use WSL for OCaml development due to Linux based systems having the best support when it comes to OCaml development.

For instructions on installing and setting up WSL, see: https://docs.microsoft.com/en-us/windows/wsl/install.

Once WSL is set up, installation can be done as above depending on your chosen Linux distribution.

Information on installing OCaml without WSL can be found here: https://fdopen.github.io/opam-repository-mingw/.

### MacOS

On MacOS, there are a few options for installation of OCaml, depending on the installed tooling and package managers.

Homebrew:
```
brew install ocaml
```

MacPorts:
```
port install ocaml
```

FreeBSD:
```
pkg install ocaml
```

### Building from Source

It is also possible to build from source (not recommended) using releases from here: https://github.com/ocaml/ocaml/releases.
