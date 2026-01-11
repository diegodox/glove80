# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a ZMK firmware configuration repository for the MoErgo Glove80 wireless split contoured keyboard. The repository uses the official Glove80 ZMK distribution (https://github.com/moergo-sc/zmk) to build custom firmware based on user-defined keymaps.

## Build Commands

### Local builds (requires Docker)
```bash
# Build with default branch (main)
./build.sh

# Build with specific ZMK branch
./build.sh <branch-name>

# Windows equivalent
build.bat [branch-name]
```

The build scripts:
1. Build a Docker image with Nix and ZMK dependencies pre-cached
2. Run the container to build firmware for both keyboard halves
3. Output `glove80.uf2` in the project root

### CI/CD builds
GitHub Actions automatically builds firmware on push/PR. Firmware artifacts are available in the Actions tab as `glove80.uf2`.

### Nix builds (local, without Docker)
```bash
# Direct nix build (requires Nix with flakes)
nix-build config -o combined

# The firmware will be at combined/glove80.uf2
```

## Architecture

### Build System
- **Nix-based build**: Uses Nix package manager for reproducible builds
- **Docker wrapper**: Provides containerized build environment with pre-cached dependencies
- **Two-stage build**: Compiles firmware for left (`glove80_lh`) and right (`glove80_rh`) halves separately, then combines into single UF2 file
- **config/default.nix**: Orchestrates the build by overriding ZMK firmware settings with custom keymap and configuration

### Configuration Files
- **config/glove80.keymap**: Main keymap configuration in ZMK devicetree format
  - Defines layers (DEFAULT, LOWER, MAGIC, FACTORY_TEST)
  - Custom behaviors (tap-dance for layer switching, hold-tap for magic layer)
  - Macros for Bluetooth profile switching
  - 80-key layout bindings
- **config/glove80.conf**: ZMK Kconfig settings (currently empty in this repo)
- **config/keymap.json**: Alternative JSON format (for compatibility/reference)

### Keymap Layer Structure
1. **Layer 0 (DEFAULT)**: Standard QWERTY layout with F-keys and modifiers
2. **Layer 1 (LOWER)**: Media controls, navigation, numpad on right side
3. **Layer 2 (MAGIC)**: System controls (bootloader, reset), RGB controls, Bluetooth profiles
4. **Layer 3 (FACTORY_TEST)**: Hardware testing layer (number patterns)

Access layers via:
- `layer_td`: Tap-dance behavior (single tap/hold = momentary LOWER, double tap = toggle LOWER)
- `magic`: Hold-tap behavior (hold = momentary MAGIC, tap = RGB status)

### ZMK Integration
- ZMK source is checked out to `src/` directory during build (from moergo-sc/zmk)
- The build system references ZMK as a dependency via Nix
- Docker image pre-caches dependencies for main branch and latest 3 tags

## Common Modifications

### Editing the keymap
1. Modify `config/glove80.keymap`
2. Keymap uses ZMK's devicetree syntax
3. Key bindings follow format: `&kp KEY_CODE` for keypresses, `&mo LAYER` for momentary layers, etc.
4. Comments in keymap show physical key layout for reference

### Testing changes
- Push to GitHub and download artifact from Actions tab, OR
- Run local build via `./build.sh` and flash resulting `glove80.uf2`

### Flashing firmware
See official Glove80 support site (https://moergo.com/glove80-support) for flashing instructions.

## Resources
- ZMK Documentation: https://zmk.dev/docs
- Official Glove80 Support: https://moergo.com/glove80-support
- Glove80 ZMK Distribution: https://github.com/moergo-sc/zmk
