[![Build Status](https://travis-ci.org/Mizux/Cbc.svg?branch=master)](https://travis-ci.org/Mizux/Cbc)
[![Build status](https://ci.appveyor.com/api/projects/status/rylt2b8cap2ai0ow?svg=true)](https://ci.appveyor.com/project/Mizux/cbc)

# Introduction

This is test of Coin-or Cbc port to [CMake](https://cmake.org/) C++ Project.

This project should run on Linux, Mac and Windows.

# CMake Dependencies Tree
This CMake project is composed of several targets with the following dependencies:  
```
CoinUtils:
Osi: CoinUils
Clp: CoinUtils Osi
OsiClp: Osi Clp
ClpSolver: Clp
Cgl: Osi
Cbc: Cgl Clp
OsiCbc: Osi Cbc
CbcSolver: Cbc
```
All dependencies are built in static to have one standalone executable.
## Project directory layout
Thus the project layout is as follow:
```
.
├── CMakeLists.txt // meta CMake doing the orchestration
├── cmake
├── CoinUtils
│   └── CMakeLists.txt
├── Osi
│   └── CMakeLists.txt
├── Clp
│   └── CMakeLists.txt
├── Cgl
│   └── CMakeLists.txt
└── Cbc
    └── CMakeLists.txt
```

# C++ Project Build
To build the C++ project, as usual:
```sh
cmake -H. -Bbuild
cmake --build build
```

# Contributing

The [CONTRIBUTING.md](./CONTRIBUTING.md) file contains instructions on how to
file the Contributor License Agreement before sending any pull requests (PRs).
Of course, if you're new to the project, it's usually best to discuss any
proposals and reach consensus before sending your first PR.

# License

Apache 2 for all patches otherwise see the LICENSE file for details.

# Disclaimer

This is not an official Google product, it is just patches that happens to be
owned by Google.

