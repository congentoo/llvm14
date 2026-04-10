# Security Policy

## Scope

This is a **personal Gentoo overlay** preserving the LLVM 14 toolchain
after its removal from the official Gentoo tree. Sibling of the
[llvm15](https://github.com/congentoo/llvm15) overlay.

## Supported Versions

Only the **latest version of each package** in this overlay receives
updates. Older versions are kept for rollback but not patched.

**LLVM 14 is end-of-life upstream** (final release: 14.0.6, June 2022).
No new security fixes are available from the LLVM project. Gentoo
patchsets (`LLVM_PATCHSET=14.0.6-rN`) continue to provide GCC/Python
compatibility fixes but are not a security channel.

For security issues in LLVM itself there is no upstream support. If
the threat model matters to you, migrate to a supported LLVM version.

## Reporting Issues With the Overlay

For issues specific to the **ebuilds in this overlay** (build failures,
incorrect dependencies, QA issues, wrong patchset versions, etc.):

- Open a GitHub issue on this repository
- For sensitive issues, use GitHub's private vulnerability reporting feature

## Integrity Guarantees

- All distfiles are verified via BLAKE2B and SHA512 hashes in Manifest files
- LLVM patchsets are fetched from `dev.gentoo.org` with SHA512 verification
  via the `llvm.org` eclass
- CI runs `pkgdev manifest` verification on every PR

## Supply Chain

- All GitHub Actions are pinned by SHA (via Dependabot updates)
- OSSF Scorecard runs weekly
- CODEOWNERS enforces review for all changes

## Threat Model

This overlay is intended for **personal use on trusted systems**. LLVM
is a compiler toolchain, not a network-facing service. Security
vulnerabilities in LLVM typically require processing malicious input
(bitcode, IR, debug info), which is not an attack vector in normal
compilation workflows.

If you are compiling untrusted code, the EOL status of LLVM 14 is a
concern and you should migrate away from this overlay.
