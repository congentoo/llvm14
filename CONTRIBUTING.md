# Contributing to the llvm14 overlay

## Development workflow

1. Install required tools:

   ```bash
   emerge --ask dev-util/pkgcheck dev-util/pkgdev dev-vcs/pre-commit
   ```

2. Clone the repository and install pre-commit hooks:

   ```bash
   git clone https://github.com/congentoo/llvm14.git
   cd llvm14
   pre-commit install
   ```

3. Make your changes. When adding or updating packages:

   ```bash
   # After editing an ebuild, regenerate the Manifest
   pkgdev manifest

   # Or regenerate all manifests in the overlay
   pkgdev manifest -r .
   ```

4. Check for QA issues:

   ```bash
   pkgcheck scan
   ```

5. Commit with `pkgdev commit` (runs checks and regenerates manifests):

   ```bash
   pkgdev commit
   ```

## Scope guidelines

This overlay has a **narrow scope**: preserve the LLVM 14 toolchain.
Do NOT add:

- LLVM versions other than 14 (use the [llvm15](https://github.com/congentoo/llvm15)
  overlay for 15, main tree for 16+)
- Packages that happen to depend on LLVM 14 but aren't part of the
  toolchain itself (use [localrepo](https://github.com/congentoo/localrepo)
  or a dedicated overlay)
- Intel GPU OpenCL / IGC packages (use llvm15, which carries that stack)

## Ebuild guidelines

- Use the `llvm-core/` and `llvm-runtimes/` categories (not legacy
  `sys-devel/`, `sys-libs/`)
- LLVM core packages use Gentoo patchsets via `LLVM_PATCHSET=...`
- Keep `PYTHON_COMPAT=( python3_{10..14} )` consistent across all ebuilds
- SLOT conventions: LLVM core uses `SLOT="${LLVM_MAJOR}/${LLVM_SOABI}"`
  (e.g., `14/14`)

## LLVM patchset tracking

Periodically check if Gentoo has bumped the `LLVM_PATCHSET` values in
the main tree for 14.0.6 ebuilds. The Gentoo LLVM team sometimes
backports GCC/Python compatibility fixes to the archived patchsets.

```bash
# From a Gentoo tree checkout
cd /var/db/repos/gentoo
git log --oneline -- 'llvm-runtimes/compiler-rt/*14.0.6*' 'llvm-core/llvm/*14.0.6*'
```

## CI checks

Every PR runs:

- **pkgcheck**: Gentoo QA checks
- **manifest**: Verifies Manifest files are up-to-date
- **lint**: Text hygiene, shellcheck, yamllint, mdformat

Fix any issues before merging.
