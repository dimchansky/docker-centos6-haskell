Haskell for CentOS 6
--------------------

## Contents

This image ships a minimal Haskell toolchain with the following packages installed to
`/opt/{name}/{version}` and added to the `PATH`:

| package         | version    |
|-----------------|------------|
| `alex`          | `3.1.4`    |
| `cabal-install` | `1.22.2.0` |
| `happy`         | `1.19.5`   |
| `c2hs`          | `0.25.1`   |
| `ghc`           | `7.8.4`    |

## Usage

* Start an interactive interpreter session with `ghci`:

```
    $ docker run -it --rm dimchansky/centos6-haskell:7.8.4
    GHCi, version 7.8.4: http://www.haskell.org/ghc/  :? for help
    Loading package ghc-prim ... linking ... done.
    Loading package integer-gmp ... linking ... done.
    Loading package base ... linking ... done.
    Prelude>
```