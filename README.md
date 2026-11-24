# dhall-mirror

The remote Dhall that Polar imports, kept here so that losing an upstream (prelude.dhall-lang.org was down, HTTP 521, on
2026-11-23) loses nothing. Every file is as its upstream published it, at the version Polar pins by hash.

| Here | Upstream | Version | Polar imports it as |
|------|----------|---------|---------------------|
| `dhall-lang/Prelude/` | [dhall-lang/dhall-lang](https://github.com/dhall-lang/dhall-lang) `Prelude/` | tag `v23.1.0` (commit `932a4db57ed1a6de36773077ba21e7f85bc0b20d`) | `https://prelude.dhall-lang.org/package.dhall`, `.../JSON/package.dhall`, `.../List/map` |
| `dhall-kubernetes/1.35/` | [dhall-lang/dhall-kubernetes](https://github.com/dhall-lang/dhall-kubernetes) `1.35/` | `master` at commit `efdc17471d607ab796c0d1002f0b8d8e99fea615` (2026-03-29) | `https://raw.githubusercontent.com/dhall-lang/dhall-kubernetes/refs/heads/master/1.35/package.dhall` |
| `cache/dhall/` | the five imports above and nix-container-lib's prelude, resolved | as pinned | Dhall's own cache, by hash |

Each upstream's licence is beside its files (`dhall-lang/LICENSE`, `dhall-kubernetes/LICENSE`, both BSD-3-Clause).
nix-container-lib's own Dhall is not copied here: it is daveman1010221/nix-container-lib; only its resolved prelude is,
in the cache.

## The pinned hashes

| Import | sha256 |
|--------|--------|
| Prelude `package.dhall` | `931cbfae9d746c4611b07633ab1e547637ab4ba138b16bf65ef1b9ad66a60b7f` |
| Prelude `JSON/package.dhall` | `5f98b7722fd13509ef448b075e02b9ff98312ae7a406cf53ed25012dbc9990ac` |
| Prelude `List/map` | `dd845ffb4568d40327f2a817eb42d1c6138b929ca758d50bc33112ef3c885680` |
| dhall-kubernetes `1.35/package.dhall` | `2086df2178fdb83a08bd4ca89c951f12e4ce16d3cc1441005fcda943a028bb8c` |
| nix-container-lib `dhall/prelude.dhall` at `8f8be62` | `d55d44ffcfdd976aa315867138121ecf13ee1d83b7109cf27e2739881de73052` |

Checked when this was made: `dhall hash --file` of each tree's file gives its hash, with no cache; each file in
`cache/dhall/` is named `1220` and the sha256 of its own bytes; and with `XDG_CACHE_HOME` at `cache/`, Dhall resolves
the pinned imports with no network.

## Using it

Offline, with the pinned URLs unchanged: point Dhall's cache here, and every import pinned by one of these hashes is
read from it.

```sh
XDG_CACHE_HOME=$PWD/cache dhall-to-yaml --file some.dhall
```

Or import a file from this repository by its path or its raw URL; the hash stays the same, since Dhall hashes what an
expression means, not where it came from.

## Adding to it

When Polar pins a new remote import: copy the upstream's files at that version beside its licence, add the resolved
cache file (`dhall hash` names it; `dhall resolve` with `XDG_CACHE_HOME` at an empty directory writes it), and add a row
to both tables.
