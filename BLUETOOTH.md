# Bluetooth Connection Guide

This guide explains how to connect your Chocofi keyboard to multiple devices and manage bluetooth profiles.

## Quick Reference

- **Access bluetooth controls**: Hold `Space` (NAV) + `Enter` (SYM) simultaneously to activate the ADJ layer
- **Switch to Mac**: Hold NAV+SYM, tap rightmost key on row 2 (automatically switches to Profile 0 and Mac layer)
- **Switch to Windows**: Hold NAV+SYM, tap ring finger key on row 2 (automatically switches to Profile 1 and Windows layer)
- **Clear current profile**: Hold NAV+SYM, tap left pinky key on row 2 (BT_CLR)

## Bluetooth Profile Layout

Your keyboard supports **5 bluetooth profiles** (0-4). The controls are on the ADJ layer (left side):

```
Row 2: [BT_CLR] [BT_SEL 2] [PC/Win] [Mac/OSX]
Row 3: [none]   [BT_SEL 4] [BT_SEL 3] [USB/BT Toggle]
```

### Pre-configured Profiles

- **Profile 0** (Mac/OSX): macOS device with CMD-based shortcuts
- **Profile 1** (PC/Win): Windows device with CTRL-based shortcuts
- **Profile 2**: Additional device (manual selection)
- **Profile 3**: Additional device (manual selection)
- **Profile 4**: Additional device (manual selection)

## Connecting to Devices

### Initial Pairing (First Time Setup)

1. **Select a profile**:
   - Hold `Space` + `Enter` (NAV + SYM)
   - Tap the desired profile key (Mac/PC for profiles 0/1, or BT_SEL 2/3/4 for others)

2. **Keyboard enters pairing mode** automatically

3. **On your device**:
   - Open Bluetooth settings
   - Search for new devices
   - Look for "Chocofi Left" and pair it
   - Look for "Chocofi Right" and pair it
   - You must pair **both halves** to the same device

4. **Repeat for other devices** using different profiles

### Quick Device Switching

The keyboard provides smart macros for seamless switching between your primary devices:

#### Switch to macOS Device
- **Keys**: Hold NAV+SYM, tap rightmost key on row 2
- **Action**: Selects Profile 0 AND switches to Mac base layer
- **Result**: All shortcuts use CMD key (copy = CMD+C, paste = CMD+V, etc.)

#### Switch to Windows Device
- **Keys**: Hold NAV+SYM, tap third key from left on row 2
- **Action**: Selects Profile 1 AND switches to Windows base layer
- **Result**: All shortcuts use CTRL key (copy = CTRL+C, paste = CTRL+V, etc.)

These macros are defined in `config/base.keymap:291-306`.

## Managing Bluetooth Profiles

### Clear and Remap a Profile

To disconnect the current device and pair a new one:

1. **Hold NAV + SYM** to access ADJ layer
2. **Tap BT_CLR** (left pinky position, row 2)
   - This clears the currently active profile
   - Keyboard automatically enters pairing mode
3. **Pair from your new device** as described above

### Switch Between Profiles

To switch to a different already-paired device:

1. **Hold NAV + SYM**
2. **Tap the desired profile**:
   - Mac/OSX key → Profile 0
   - PC/Win key → Profile 1
   - Column 2, Row 2 → Profile 2
   - Column 3, Row 3 → Profile 3
   - Column 2, Row 3 → Profile 4

### Toggle Between USB and Bluetooth

If your keyboard is connected via USB cable but you want to use bluetooth instead:

1. **Hold NAV + SYM**
2. **Tap the rightmost key on row 3** (OUT_TOG)
3. Output toggles between USB and Bluetooth

## Example Multi-Device Setup

Here's a typical configuration for managing multiple devices:

| Profile | Device | Quick Access |
|---------|--------|--------------|
| 0 | MacBook Pro | NAV+SYM → Rightmost row 2 (Mac key) |
| 1 | Windows Desktop | NAV+SYM → Third from left row 2 (PC key) |
| 2 | iPad | NAV+SYM → Column 2 row 2 |
| 3 | Android Phone | NAV+SYM → Column 3 row 3 |
| 4 | Spare Device | NAV+SYM → Column 2 row 3 |

## Hardware Notes

### Split Keyboard Bluetooth Behavior

- Each half (left and right) manages its own bluetooth connection
- When pairing, you'll see **two separate devices** in your bluetooth settings
- Both halves must be paired to the same device for the keyboard to function
- The halves communicate with each other independently (not through your computer)

### Battery and Power

- The nice!nano v2 controller has onboard battery management
- Bluetooth connection is managed per-profile, so switching profiles is instant
- The keyboard will remember paired devices even after power loss

### Connection Priority

1. If USB is connected, USB takes priority (unless toggled with OUT_TOG)
2. Bluetooth activates when no USB connection is present
3. Use OUT_TOG to force bluetooth even when USB is connected

## Troubleshooting

### Device Won't Connect
- Clear the profile with BT_CLR
- Remove "Chocofi" from your device's bluetooth settings
- Re-pair both halves

### Wrong Shortcuts After Switching Devices
- Use the Mac/OSX or PC/Win quick-switch keys instead of manual profile selection
- These automatically adjust the layer for correct modifier keys

### Only One Half Working
- Ensure both halves are paired to your device
- Check that both halves are powered on
- Try re-pairing both halves

### Forgot Which Profile is Which
- Switch to each profile and see which device responds
- Or clear unused profiles and start fresh

## Configuration Reference

The bluetooth configuration is defined in `config/base.keymap`:

- Lines 22-29: Bluetooth layer key definitions (BTL1, BTL2)
- Lines 291-306: Smart connection macros (pc_connect, osx_connect)
- Lines 487-500: ADJ layer with bluetooth controls
