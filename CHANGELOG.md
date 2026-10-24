# Changelog

All notable changes to Rust Cache are documented here, following [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

## [0.2.0] - 2026-10-25

### Added

- Add the idle pass, which removes a build folder that nothing wrote or read for 14 days (`--idle-days`).

## [0.1.0] - 2026-10-24

### Added

- Add the repository layout from the runedeck skeleton (base, shell, and tool templates).
- Add `bin/rust-cache`, which removes unused macOS debug objects, old artifacts, and old incremental caches.
