# Reverie

This repository contains the QMK firmware source for a ZSA Moonlander keyboard layout named "reverie". The layout was originally created using Oryx and exported for custom compilation.

## Project Overview

- **Keyboard**: ZSA Moonlander
- **Firmware**: QMK (Quantum Mechanical Keyboard)
- **Layout Name**: reverie
- **Source Directory**: `zsa_moonlander_reverie_source/`
- **Compiled Firmware**: `zsa_moonlander_Geywg.bin`

## Build Environment Setup

This project requires QMK firmware environment. You can choose between:

1. **ZSA's QMK Fork** (recommended for compatibility)
   - Clone from: https://github.com/zsa/qmk_firmware/
   - Easier compilation, matches current firmware revision

2. **Mainline QMK** (bleeding edge features)
   - Clone from: https://github.com/qmk/qmk_firmware/
   - May require debugging compiler errors

### Setup

- Set up QMK environment (see https://docs.qmk.fm/)
- For ZSA fork, manually specify the path during setup

## Essential Commands

### Compilation
```bash
# From qmk_firmware directory
qmk compile -kb zsa/moonlander -km reverie

# Or if default keyboard/layout not set
qmk compile
```

### Flashing
```bash
# From qmk_firmware directory
qmk flash -kb zsa/moonlander -km reverie

# Then put the board into bootloader mode with the reset button
```

### File Structure for QMK
Copy the contents of `zsa_moonlander_reverie_source/` to:
```
qmk_firmware/keyboards/zsa/moonlander/keymaps/reverie/
```

## Code Organization

### Core Files
- `config.h` - Hardware configuration and feature flags
- `keymap.c` - Main keymap definitions and custom functions
- `rules.mk` - Build configuration and enabled features
- `keymap.json` - Module configuration (Oryx integration)

### Key Features Enabled
- **RGB Matrix**: Custom per-layer LED coloring
- **Tap Dance**: 22 tap dance functions for advanced key behavior
- **Auto Shift**: Automatic shift on hold
- **Dynamic Macros**: Record and playback macros
- **Oryx Integration**: Maintains compatibility with Oryx configurator

### Layer Structure
The layout uses 7 layers (0-6):
- **Layer 0**: Base typing layer
- **Layer 1**: Navigation/mouse layer
- **Layer 2**: Function keys/numpad
- **Layer 3**: Empty spacer layer
- **Layer 4**: Media controls
- **Layer 5**: Alternative base layer
- **Layer 6**: Configuration/RGB controls

## Code Patterns and Conventions

### Tap Dance Functions
- Each tap dance follows the pattern: `on_dance_X`, `dance_X_finished`, `dance_X_reset`
- Uses advanced tap dance with single/double tap and hold detection
- Functions are numbered sequentially (DANCE_0 through DANCE_21)

### RGB Matrix Implementation
- Custom per-layer LED colors defined in `ledmap` array
- `set_layer_color()` function applies colors for each layer
- `rgb_matrix_indicators_user()` handles layer-based coloring

### Dual Function Keys
- Uses `LT(layer, key)` for layer-tap combinations
- Custom `DUAL_FUNC_X` macros for tap/hold behavior with different outputs

### Naming Conventions
- Functions: `snake_case` (e.g., `set_layer_color`)
- Enums: `UPPER_CASE` (e.g., `DANCE_0`)
- Macros: `UPPER_CASE` (e.g., `DUAL_FUNC_0`)
- Variables: `snake_case` (e.g., `dance_state`)

## Important Gotchas

### Compilation Context
- Must be compiled from within the QMK firmware tree
- Source files need to be placed in the correct QMK directory structure
- Keyboard identifier: `zsa/moonlander`
- Keymap name: `reverie`

### RGB Configuration
- Most RGB matrix effects are disabled in `config.h` for custom implementation
- Custom LED colors are defined as HSV values in PROGMEM
- RGB timeout set to 900000ms (15 minutes)

### Tap Dance State Management
- Global `dance_state[22]` array tracks tap dance states
- Each function must properly reset state in `*_reset()` functions
- 10ms delay in reset functions for proper key release handling

## Oryx Integration

This layout maintains Oryx compatibility through:
- `ORYX_ENABLE = yes` in rules.mk
- Module configuration in keymap.json
- Ability to re-import to Oryx for visual editing
- Custom QMK features can be added alongside Oryx configuration