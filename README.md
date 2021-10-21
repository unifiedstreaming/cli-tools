# Unified Streaming command line tools

This repository is used to build Docker images for each of our command line
tools.

## How it works

Dockerfiles to build each command line tool are in the `docker/<tool>`
directories, the `mp4split` image is built first and the others then use it as
a base to build from. Most of them just change the `entrypoint.sh` script and
default command, but `manifest_edit` has to install Python and the Manifest
Edit packages as well.

The images get published to Docker Hub as:

* `unifiedstreaming/mp4split`
* `unifiedstreaming/cpix_verify`
* `unifiedstreaming/manifest_edit`
* `unifiedstreaming/push_input_stream`
* `unifiedstreaming/unified_capture`
* `unifiedstreaming/unified_remix`

Beta releases will be tagged as `<version>-beta`, e.g. `1.11.10-beta`.

## Shell functions

The following shell functions can be used to run specific versions of each tool
as though they were installed. 

Note: they mount the current working directory to `/data` in the container and
use that as the working directory.

By default they will run the latest GA release, but you can optionally pass
a version as the first parameter.

For example:
```
$ mp4split 1.11.9 --show_license

mp4split version=1.11.9 (25072)    Copyright 2007-2021 CodeShop B.V.

Product name: Unified Streaming Platform
License type: production
...
```

The functions can be included in your bash, zsh, etc. profile as required.

Note: if you run without an explicit version the function will first pull the
`latest` tag.

Beta releases can be specified by appending `-beta` to the version, e.g.
`1.11.10-beta`.

```bash
#!/bin/sh
# docker helper functions
mp4split() {
  BASE_IMAGE=docker.io/unifiedstreaming/mp4split
  case $1 in
    1.[0-9]*.[0-9]* ) IMAGE=${BASE_IMAGE}:$1; shift ;;
    * ) IMAGE=${BASE_IMAGE}:latest; docker pull $IMAGE ;;
  esac
  docker run -it -e UspLicenseKey -v ${PWD}:/data --workdir /data $IMAGE "$@"
}

cpix_verify() {
  BASE_IMAGE=docker.io/unifiedstreaming/cpix_verify
  case $1 in
    1.[0-9]*.[0-9]* ) IMAGE=${BASE_IMAGE}:$1; shift ;;
    * ) IMAGE=${BASE_IMAGE}:latest; docker pull $IMAGE ;;
  esac
  docker run -it -e UspLicenseKey -v ${PWD}:/data --workdir /data $IMAGE "$@"
}

manifest_edit() {
  BASE_IMAGE=docker.io/unifiedstreaming/manifest_edit
  case $1 in
    1.[0-9]*.[0-9]* ) IMAGE=${BASE_IMAGE}:$1; shift ;;
    * ) IMAGE=${BASE_IMAGE}:latest; docker pull $IMAGE ;;
  esac
  docker run -it -e UspLicenseKey -v ${PWD}:/data --workdir /data $IMAGE "$@"
}

push_input_stream() {
  BASE_IMAGE=docker.io/unifiedstreaming/push_input_stream
  case $1 in
    1.[0-9]*.[0-9]* ) IMAGE=${BASE_IMAGE}:$1; shift ;;
    * ) IMAGE=${BASE_IMAGE}:latest; docker pull $IMAGE ;;
  esac
  docker run -it -e UspLicenseKey -v ${PWD}:/data --workdir /data $IMAGE "$@"
}

unified_capture() {
  BASE_IMAGE=docker.io/unifiedstreaming/unified_capture
  case $1 in
    1.[0-9]*.[0-9]* ) IMAGE=${BASE_IMAGE}:$1; shift ;;
    * ) IMAGE=${BASE_IMAGE}:latest; docker pull $IMAGE ;;
  esac
  docker run -it -e UspLicenseKey -v ${PWD}:/data --workdir /data $IMAGE "$@"
}

unified_remix() {
  BASE_IMAGE=docker.io/unifiedstreaming/unified_remix
  case $1 in
    1.[0-9]*.[0-9]* ) IMAGE=${BASE_IMAGE}:$1; shift ;;
    * ) IMAGE=${BASE_IMAGE}:latest; docker pull $IMAGE ;;
  esac
  docker run -it -e UspLicenseKey -v ${PWD}:/data --workdir /data $IMAGE "$@"
}
```