# FV8

FV8 is a custom variant of the V8 JavaScript engine that applies systematic forced execution inside V8 in order to increase code coverage, unravel more code, detect code evasions and provide better access to malicious code.

This repository provides the patches and build tools (with some tests) for turning Chromium into FV8.

The core patches are architecture and platform agnostic, but some of the logging code currently has implementation-detail dependencies on Linux.
The [optional] build system is definitely Linux-specific.

## Building FV8

(These instructions are for building FV8 on Chromium 110. 

* Make sure you have [Docker](https://docs.docker.com/install/) and [Python 3](https://www.python.org/downloads/) and a lot of free disk space (e.g., 50GiB) for downloading and building Chromium
* Clone this repository *(we will call the cloned working directory **$FV8**)*
* Create an empty working directory on a device with enough space to check out and build Chromium *(we will call this directory **$WD**)*
* Run `$FV8/builder/tool.py -d $WD checkout diff_hash`
    * This will take a while: it has to check out all the code and run initial software installation steps
    * All tool installation will be captured in a Docker container image that can be reused for all future builds of this version of Chromium
* Run `patch -p1 <$FV8/patches/110.XX/chromiumpatch.diff` from *inside* `$WD/src/v8` 
* Run `$FV8/builder/tool.py -d $WD build @std`
    * This will *really* take a while: it has to build all of Chromium and FV8, as well as unittests

