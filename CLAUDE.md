# Project Structure Guide

## Project Overview

> (Briefly describe this project's goals, methods, and scope here.)

## Project Status Log (≤500 words)

> (Briefly describe the current basic status of this project here.)

## Directory Structure

```
project_root/
├── papers/          # External reference papers
├── repos/           # Externally cloned codebases, logically read-only
├── weights/         # Model weights, organized into per-project subdirectories
│   └── ours/        # This project's own weights
├── data/            # Data and processing files
│   ├── datasets/    # Datasets location
│   ├── scripts/     # General data processing / preprocessing scripts
│   └── testdata/    # Small test data (usually custom test data)
├── docs/            # Unified location for documentation output
│   ├── latex/       # This project's LaTeX docs / paper writing (tex/bib/fig)
│   ├── plans/       # Development plans, with timestamps
│   └── reports/     # Development reports and analysis (create subdirectories per repo/version)
├── scripts/         # Run / test scripts (create subdirectories for classification)
├── configs/         # YAML / JSON config files
├── src/             # This project's core codebase (main development location)
│   ├── v1_baseline/ # Development version (named vN_<brief>); each carries its own CHANGELOG.md / TODO.md
│   │   ├── CHANGELOG.md  # This version's changelog (start from migrated records when creating a new version)
│   │   └── TODO.md       # This version's TODO (start from scratch when creating a new version)
│   └── current →    # symlink pointing to the currently active version
├── _tmp/            # Temporary output; temp file(s) must go in their own subdirectory, not directly under this dir; gitignored
├── _bak/            # Backup archives; gitignored
├── CLAUDE.md        # claude code configuration
├── CHANGELOG.md     # Project-level changelog (version-level: see src/<version>/)
├── README.md        # Project readme
├── TODO.md          # Project-level TODO (version-level: see src/<version>/)
└── LICENSE.md       # LICENSE
```

## Currently Active Codebase

- **`src/current` → `src/v1_baseline`**
- Switch versions by changing the symlink only: `ln -snf <version-dir> src/current`; scripts always use the stable path `src/current/...`, and update this section in sync.

## Important Rules

- **`src/` multi-version coexistence**: each development version uses a subdirectory `vN_<brief>` (e.g. `v1_baseline`, `v2_crossattn`), never overwriting one another; The versions are isolated from each other and operate independently. Tie related artifacts together with the same version key: `weights/ours/<version>/`, `configs/<version>.yaml`, `docs/reports/<version>.md`. The currently active version is designated by the `src/current` symlink (see section above).
- **`repos/`**: logically read-only. Adjustments to paths / environment / dependencies for "getting it to run" are allowed, but their run logic and method architecture must not be modified; if modification is unavoidable, confirm first and back up. Each external project uses its own subdirectory (`repos/AAA`).
- **`weights/`**: organized into per-project subdirectories (`weights/ours`, `weights/AAA`); weights produced during training are also stored in the corresponding subdirectory.
- **`data/datasets/shared_datasets`**: a symlink pointing to shared datasets. Downloading / using is allowed, but existing files must not be modified or deleted; if unavoidable, confirm first and back up.
- **`_tmp/`**: all of this project's temporary files go here; do not place them in the system root `/tmp`.
- **`_bak/`**: location for backup archives (weights, large binaries, full-project snapshots, etc.).
- **`CHANGELOG.md` / `TODO.md` (two levels)**: the root level is **project-level** (cross-version, structure / infrastructure changes); each `src/<version>/` has its own **version-level** `CHANGELOG.md` / `TODO.md` recording only that version's development. Unified format — CHANGELOG uses timestamp headers (`## 2026-05-23-17:23:57`) in descending order; TODO uses `[ ]` items with timestamps.

## Git Management Rules

- Must be tracked: `configs`, `docs`, `scripts`, `src`, `CHANGELOG.md`, `CLAUDE.md`, `LICENSE.md`, `README.md`, `TODO.md`.
- `.gitignore` explicitly excludes: `repos/`, `data/`, `weights/`, `_tmp/`, `_bak/`.
- Other files / directories that need tracking require confirmation.
