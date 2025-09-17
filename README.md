# DiG for Windows (unofficial)

An unofficial port of `dig` command line utility for Windows (x64) from BIND 9

## Source

The original [BIND](https://www.isc.org/bind/) 9.18.39 ([source](https://downloads.isc.org/isc/bind9/9.18.39/bind-9.18.39.tar.xz)) is licensed under the Mozilla Public License (MPL), version 2.0 (see `LICENSE.BIND9.txt`).
This repository modifies it to run on Windows (x64) with applying patches under `patches-w64/`.

For clarity, the patch files under `patches-w64/` are licensed under MPL v2.0 (NOTE: the content of `patches-w64/01-win32-resconf.patch` comes from BIND 9.16.50 [source](https://downloads.isc.org/isc/bind9/9.16.50/bind-9.16.50.tar.xz)).

## How to build

Build steps are described in GitHub Action (`.github/workflows/build.yml`).
There is a pre-built binary executable file built with the action.

## Supported platforms

The binary executable file is expected to run on Windows 10 or later (x64).

The binary is built against UCRT as a C runtime.
MSVCRT version of `strftime("%z")` does not print time zone offset, prints time zone name instead, thus not supported.

Older versions of WINE (< 4.20) has a bug in implementation of UCRT for `strftime("%z")` and may not work as expected.

## How to use

Please refer to documentation of `dig(1)`.

## Build options

### Enabled

- IPv6 support
- DNS over HTTPS (DoH)

### Disabled or Removed from the source

- DNSTAP
- libxml2
- IDN support
- jemalloc
- Regexp validation of NAPTR RR
- `.digrc`

## Difference from the original version

- Most of codes unrelated to DiG are removed
- Timestamp (`WHEN:` in output) is printed in ISO 8601 format (YYYY-mm-ddTHH:MM:SS+/-zzzz)
- Client cookie and server cookie are printed in accordance with RFC 9018

## Statically-linked libraries

The binary executable file is statically linked against the following libraries:

- OpenSSL
  - License: see `LICENSE.OpenSSL.txt`
  - Obtained from: [MSYS2 Package of mingw-w64-ucrt-x86_64-openssl](https://packages.msys2.org/packages/mingw-w64-ucrt-x86_64-openssl) as a binary package
    - [binary](https://mirror.msys2.org/mingw/ucrt64/mingw-w64-ucrt-x86_64-openssl-3.5.2-2-any.pkg.tar.zst)
    - [source](https://mirror.msys2.org/mingw/sources/mingw-w64-openssl-3.5.2-2.src.tar.zst)
- libuv
  - License: see `LICENSE.libuv.txt` and `LICENSE-extra.libuv.txt`
  - Obtained from: [MSYS2 Package of mingw-w64-ucrt-x86_64-libuv](https://packages.msys2.org/packages/mingw-w64-ucrt-x86_64-libuv) as a binary package
    - [binary](https://mirror.msys2.org/mingw/ucrt64/mingw-w64-ucrt-x86_64-libuv-1.51.0-1-any.pkg.tar.zst)
    - [source](https://mirror.msys2.org/mingw/sources/mingw-w64-libuv-1.51.0-1.src.tar.zst)
- nghttp2
  - License: see `COPYING.nghttp2.txt`
  - Obtained from: [MSYS2 Package of mingw-w64-ucrt-x86_64-nghttp2](https://packages.msys2.org/packages/mingw-w64-ucrt-x86_64-nghttp2) as a binary package
    - [binary](https://mirror.msys2.org/mingw/ucrt64/mingw-w64-ucrt-x86_64-nghttp2-1.67.0-1-any.pkg.tar.zst)
    - [source](https://mirror.msys2.org/mingw/sources/mingw-w64-nghttp2-1.67.0-1.src.tar.zst)

The binary executable file is built with [MinGW-w64](https://www.mingw-w64.org/)/[GCC](https://gcc.gnu.org/) (see `COPYING.MinGW-w64-runtime.txt`) and winpthreads (see `COPYING.winpthreads.txt`).
