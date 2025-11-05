# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a ZMK firmware configuration repository for a Chocofi 34/36-key split keyboard using the Canary layout. The configuration is heavily inspired by [Urob's keymap](https://github.com/urob/zmk-config) and uses a custom fork of ZMK.

## Repository Structure

- `config/` - Main configuration directory
  - `base.keymap` - Core keymap definitions with all behaviors, macros, layers, and key bindings
  - `chocofi.keymap` - Board-specific entry point that defines WIRELESS flag and includes base.keymap
  - `combos.dtsi` - Combo key definitions (key combinations that trigger specific actions)
  - `extra_keys.h` - Preprocessor definitions for different physical layouts (13332, 23332, or full 3x5 matrix)
  - `west.yml` - West manifest pointing to urob's ZMK fork
  - `boards/shields/chocofi/` - Board-specific shield definitions
- `keymap_drawer/` - Configuration files for generating visual keymap representations
  - `draw_config.yaml` - Contains custom glyphs, parsing rules, and visual styling for keymap-drawer tool
- `build.yaml` - GitHub Actions build matrix specifying which boards/shields to build (nice_nano_v2 with chocofi left/right and nice_view display)
- `build.sh` - Local build script for development

## Key Architecture Concepts

### Layer System
The keymap uses 9 layers with conditional layer activation:
- `BASE` (0) / `BASE_WIN` (1) - Mac and Windows base layers with swapped GUI/CTRL modifiers
- `NUM` (2) - Number pad and function keys
- `NAV` (3) / `NAVW` (4) - Mac and Windows navigation layers
- `SYM` (5) / `SYMW` (6) - Mac and Windows symbol layers
- `ADJ` (7) - Adjustment layer (automatically activated when NAV+SYM pressed)
- `LOCKL` (8) - Lock layer

Conditional layers automatically switch between Mac/Windows variants based on active base layer.

### Home Row Mods (HRMs)
Uses Urob's "timeless homerow mods" configuration with:
- Positional hold-tap behaviors (`hml` and `hmr`) that only activate when opposite hand keys are pressed
- `require-prior-idle-ms = <100>` to prevent accidental activation during typing flow
- `hold-trigger-on-release` for more reliable modifier timing
- GUI and CTRL swapped on Mac layer to maintain muscle memory across OS

### Combos
Key combinations defined in `combos.dtsi`:
- Missing letters (Q, Z, V, quotes) for reduced layouts
- One-handed copy/paste/cut/undo shortcuts on right hand
- Parentheses/brackets on home row
- Delete actions (backspace, delete, delete word/line)
- Lock layer activation (outer corners + inner thumbs)

### Custom Behaviors
- `lt_spc` - Space key that morphs to `. + Space + Sticky Shift` when Shift is held
- `ss_cw` - Sticky Shift that becomes Caps Word on double-tap
- `swapper` / `swapper_win` - Alt+Tab / Ctrl+Tab app switcher using tri-state behavior
- Various mod-morphs for intuitive shifted symbols (e.g., `.` → `:`, `,` → `;`)

### Build Configuration
The project uses Urob's ZMK fork (specified in `west.yml`) which includes additional features like tri-state behaviors and improved home row mod timing.

## Common Development Commands

### Local Building (Development Container)
The build script assumes you're in a development container with ZMK source at `/workspaces/zmk-urob/`:

```bash
# Build for left side (default: nice_nano_v2, chocofi_left)
./build.sh chocofi left

# Build for right side
./build.sh chocofi right

# Pristine build (clean build directory first)
./build.sh -p chocofi left

# Custom board
./build.sh -b nice_nano_v2 chocofi left
```

Output: `chocofi_left.uf2` / `chocofi_right.uf2` files

### GitHub Actions Build
Builds are automatically triggered on push/PR via `.github/workflows/build.yml`. The build matrix is defined in `build.yaml` and includes nice_view display support.

### Keymap Visualization
Keymap drawings are automatically generated on commit via `.github/workflows/draw.yml` using [keymap-drawer](https://github.com/caksoylar/keymap-drawer). The visual configuration in `keymap_drawer/draw_config.yaml` defines custom SVG glyphs and layout styling.

## Working with Layouts
The physical layout is controlled by preprocessor flags in board-specific keymap files (e.g., `chocofi.keymap`):
- `ALPHA_13332` - Reduced 30-key layout
- `ALPHA_23332` - 34-key layout (default for Chocofi)
- No flag - Full 36-key layout

These flags control which keys are active via `extra_keys.h` definitions.

## Important Notes

- This configuration uses a **custom ZMK fork** (urob's fork). Features may not work with mainline ZMK.
- The `WIRELESS` flag in board-specific keymaps enables/disables Bluetooth features
- Combos have a 25ms timeout for fast typing compatibility
- Home row mods require 200ms hold time with 125ms quick-tap and 100ms prior idle time
