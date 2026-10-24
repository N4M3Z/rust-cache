# Rust Cache

> Prune stale cargo build output every night, including leftover macOS debug objects

Cargo never deletes old build output. On macOS it is worse: debug builds keep the object files of every
compile for debug info, and from Rust 1.88 each compile gives them new names ([rust#161824][RUSTOBJ]).
On one machine a single test binary had 21 compiles of 256 objects each, and its debug map named only the
newest 256. cargo-sweep cannot remove these files, because it deletes only whole artifacts.

## Install

```sh
brew install N4M3Z/tap/rust-cache
brew services start N4M3Z/tap/rust-cache
```

The service runs at 04:00. launchd runs a missed time once after the Mac wakes. The log is
`$(brew --prefix)/var/log/rust-cache.log`.

## Usage

```sh
rust-cache --help
rust-cache --dry-run              # show what each folder would free
rust-cache --days 14 ~/Projects   # other age and roots
```

Every cargo build folder under the roots (default `~/Developer` and `~/Agents`) gets four passes:

| Pass | Removes |
|------|---------|
| objects | Object files that the binary's debug map does not name and that are older than the binary. |
| toolchains | `cargo sweep --installed`: output of rustc versions that are no longer installed. |
| age | `cargo sweep --time 7`: artifacts nobody read in 7 days. |
| incremental | Incremental caches not written in 7 days. cargo-sweep never touches them. |

The objects pass keeps any object newer than its binary, so a compile in progress keeps its own. A build
folder that a link in `~/.local/bin` points into skips both sweeps, because cargo-sweep would delete the
installed release binary.

## Requirements

macOS, `cargo`, [cargo-sweep][SWEEP], `fd`, `jq`, and `nm` from the Xcode command line tools. The formula
installs cargo-sweep, fd, and jq.

## License

EUPL-1.2

[RUSTOBJ]: https://github.com/rust-lang/rust/issues/161824
[SWEEP]: https://github.com/holmgr/cargo-sweep
