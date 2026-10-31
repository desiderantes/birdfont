![Birdfont logo][birdfont]

# Birdfont - Font Editor

Birdfont is a font editor which can create vector graphics and
export SVG, EOT and TTF fonts.

    Author: Johan Mattsson and others see AUTHORS for full attribution.
    License: GNU GPL v3
    Webpage: https://birdfont.org

## Building from Source

Install `valac`, `meson`, `ninja`, and all required libraries, which are most
likely in packages with a `-dev` or `-devel` affix:

    valac
    meson
    ninja-build
    libxmlbird-dev (or built automatically as a Meson subproject)
    libgee-0.8-dev
    libglib2.0-dev
    libgtk-3-dev
    libwebkit2gtk-4.1-dev
    libnotify-dev
    libsqlite3-dev

XML Bird is available from [birdfont.org][xmlbird] and is automatically fetched
as a Meson wrap subproject if not installed on the system.

Configure and build:

    meson setup build
    meson compile -C build

The default prefix is `/usr/local`. On some systems `/usr` is the right prefix:

    meson setup build --prefix=/usr
    meson compile -C build

Run tests and benchmarks:

    meson test -C build
    meson test -C build --benchmark

Install:

    sudo meson install -C build

All patches must be compiled with support for Vala's null pointer checks.
Configure the project with `-Dexperimental_non_null=true`:

    meson setup build -Dexperimental_non_null=true

## Hacking

Run the test suite and benchmarks:

    meson test -C build
    meson test -C build --benchmark

To enable the fuzzing targets and run the fuzz seed tests:

    meson setup build -Dfuzzing=true
    meson test -C build --suite fuzz

To run continuous coverage-guided fuzzing with AFL++ (`afl-fuzz`):

    CC=afl-clang-fast meson setup build -Dfuzzing=true
    ninja -C build fuzz-svg
    ninja -C build fuzz-bf

## Packages

Windows and Mac binaries can be downloaded from
https://birdfont.org Many Linux distributions have packages of
Birdfont in their repositories. There are BSD packages in OpenBSD and FreeBSD.

## Logo
The logo was created by Marko Jovanovac. You can find it here:
resources/birdfont-logo.svg

[birdfont]: https://birdfont.org/images/birdfont_logo2.png "Birdfont logo"
[xmlbird]: https://birdfont.org/xmlbird.php "XML Bird – XML Parser for programs written in VALA"
