# Rust Cache

> Brief: Prune stale cargo build output every night, including leftover macOS debug objects

## Conventions

- `make install` installs the repository check tools and activates the commit and push hooks. Run it once after cloning.
- Commit format: `type: description` (feat, fix, docs, chore, refactor, test), lowercase, no scope.
- Every text file ends with a newline, uses 4-space indentation, and has no tabs outside Makefiles.

## Layout

- `bin/rust-cache` is the product: one bash script. It deletes files under the user's build folders, so every
  rule in it keeps a file that a running or recent build still needs.
- The `N4M3Z/tap/rust-cache` formula installs the script and runs it as a brew service at 04:00.

## Release

- A release attaches `rust-cache-<version>.tar.gz` (`git archive --prefix=rust-cache-<version>/`) and a
  `checksums.txt` beside it. The tap's `bin/bump-release` reads the `sha256` from that file, so never publish
  a release without it.
