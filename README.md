# llvm14 overlay

Personal Gentoo overlay preserving LLVM 14 toolchain packages removed
from the official Gentoo tree. Sibling of the [llvm15](../llvm15) overlay.

## Why this overlay exists

Gentoo phased out LLVM 14 from the main tree as part of regular slot
cleanup after newer LLVM versions became the default. A few packages
still pin LLVM 14 build or runtime deps (legacy compilers, older
proprietary toolchains, reproducibility for old ports), so this overlay
keeps the last known-good Gentoo ebuilds and patchsets available.

Unlike [llvm15](../llvm15), this overlay does **not** carry the legacy
Intel GPU OpenCL stack — it's just the plain LLVM 14 toolchain.

## Package groups

### LLVM 14 core toolchain (`llvm-core/`)

All at version 14.0.6, using the archived Gentoo patchsets:

- `llvm-core/llvm`
- `llvm-core/clang`, `clang-common`, `clang-runtime`, `clang-toolchain-symlinks`
- `llvm-core/lld`, `lld-toolchain-symlinks`
- `llvm-core/lldb`
- `llvm-core/libclc`
- `llvm-core/llvm-common`, `llvm-toolchain-symlinks`, `llvmgold`

### LLVM 14 runtimes (`llvm-runtimes/`)

- `compiler-rt`, `compiler-rt-sanitizers`
- `libcxx`, `libcxxabi`
- `llvm-libunwind`
- `openmp`

### Python / OCaml bindings

- `dev-python/clang-python`, `dev-python/lit`
- `dev-ml/llvm-ocaml`

## `PYTHON_COMPAT`

All ebuilds are widened to `python3_{10..14}`. See the commit history
for the analysis; Python 3.14 is supported through the archived Gentoo
patchsets plus the standard build-time-only usage.

## Upstream status

LLVM 14 is **end-of-life** upstream (final release: 14.0.6, June 2022).
No further security fixes land upstream. The archived Gentoo patchsets
continue to provide GCC / Python compatibility fixes when published by
the Gentoo LLVM team.

## Related overlays

- [**llvm15**](../llvm15) — sibling overlay preserving LLVM 15 and the
  legacy Intel GPU OpenCL stack
- [**localrepo**](../localrepo) — main personal overlay (catch-all)

## Enabling the overlay

```ini
# /etc/portage/repos.conf/llvm14.conf
[llvm14]
location = /var/db/repos/llvm14
sync-type = git
sync-uri = https://github.com/congentoo/llvm14.git
auto-sync = yes
```

Then: `emerge --sync llvm14`.

## Development

See [CONTRIBUTING.md](CONTRIBUTING.md) for the maintenance workflow and
[SECURITY.md](SECURITY.md) for the security policy.
