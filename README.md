# std-filesystem-relative

## Draft Paper

To read up on the background to this work please see the current latest [Draft Paper](papers/DXXXX.md) for more details.

## Requirements

The tests build using [Scons](http://www.scons.org/) with **[cuppa](https://github.com/ja11sop/cuppa)** therefore you need to [install Scons](http://www.scons.org/download.php). **[Cuppa](https://github.com/ja11sop/cuppa)** can be easily installed using pip:

```
pip install cuppa
``` 

## Building

To build simply call:

```sh
scons
```

in the same folder as the `sconstruct` file or add `-D` (for Descend) from any folder at or below the `sconstruct` file

```sh
scons -D
```

By default this will build debug (`--dbg`) and release (`--rel`) versions using the default toolchain for your system.

If you have multiple toolchains installed, or wish to build specific variants you could type this as an example:

```sh
scons --dbg --rel --cov --toolchains=gcc48,gcc49,clang35,clang36
```

## Running the Tests

To run tests simply call scons with `--test`. For example:

```sh
scons --test
```

or

```sh
scons --dbg --rel --cov --toolchains=gcc48,gcc49,clang35,clang36 --test
```

## Generating HTML Papers

Any paper revisions under the `papers` directory are converted to HTML automatically during the build. All build output can be found under the `.build` directory by default.

## Dependencies

The tests make use of [Boost](https://www.boost.org) however **[cuppa](https://github.com/ja11sop/cuppa)** takes care of retrieving and building [Boost](https://www.boost.org) as needed.
