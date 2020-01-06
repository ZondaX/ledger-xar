#Building

At the moment the preferred development environment is Linux.

## Get source
Apart from cloning, be sure you get all the submodules, by calling:
```
git submodule update --init --recursive
```

## Dependencies

#### Ledger Nano S

This project requires ledger firmware 1.6

The current repository keeps track of Ledger's SDK but it is posible to override it by changing the git submodule.

#### CircleCI CLI

CircleCI allows compiling `BOLOS` firmware both in Linux and MacOS. The CLI will download a docker container ready to run.

To install, follow the instructions here:

https://circleci.com/docs/2.0/local-cli/#installing-the-circleci-local-cli-on-macos-and-linux-distros

#### Docker CE

CircleCI CLI should have instructed you to install Docker. Just in case, you can find instructions here too:

https://docs.docker.com/install/

#### OS Dependencies

**Ubuntu**

Install the following packages:
```
sudo apt-get update && sudo apt-get -y install build-essential git sudo wget cmake libssl-dev libgmp-dev autoconf libtool python-pip
```

# Compiling
There are different local builds:

 - Generic C++ code and run unit tests
 - BOLOS firmware

### Generic C++ Code / Tests

This is useful when you want to make changes to libraries, run unit tests, etc. It will build all common libraries and unit tests.
```
cmake . && make
```
**Run unit tests**
```
export GTEST_COLOR=1 && ctest -VV
```

### BOLOS / Ledger firmware
In order to keep builds reproducible, a Makefile is provided.

The Makefile will build the firmware in a docker container and leave the binary in the correct directory.

**Build**

The following command will build the app firmware inside a container. All output will be available to the host.
```
make
```

**Upload the app to the device**
The following command will upload the application to the ledger. _Warning: The application will be deleted before uploading._
```
make load
```

## Continuous Integration (debugging CI issues)
This will build in a docker image identical to what CircleCI uses. This provides a clean, reproducible environment. It also can be helpful to debug CI issues.

**To build in ubuntu 18.04 and run C++ unit tests**
```
circleci build
```

**To build BOLOS firmware**
```
circleci build --job build_ledger
```
