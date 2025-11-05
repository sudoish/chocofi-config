# Flashing Guide for Chocofi Keyboard

This guide covers how to build and flash the ZMK firmware to your Chocofi keyboard using both GitHub Actions (automated) and local builds.

## Prerequisites

- Chocofi keyboard with nice!nano v2 controllers
- USB-C cable for connecting to your computer
- Git installed on your system

## Method 1: Using GitHub Actions (Recommended for Quick Updates)

This is the easiest method if you've pushed your changes to GitHub.

### Step 1: Trigger a Build

1. Push your configuration changes to GitHub:
   ```bash
   git add .
   git commit -m "Update keymap configuration"
   git push
   ```

2. The GitHub Actions workflow will automatically start building the firmware.

### Step 2: Download the Firmware

1. Go to your repository on GitHub
2. Click on the **Actions** tab
3. Click on the most recent workflow run (should show a green checkmark when complete)
4. Scroll down to the **Artifacts** section
5. Download the `firmware` artifact (it will be a .zip file)
6. Extract the .zip file - you should see:
   - `chocofi_left-nice_nano_v2-zmk.uf2`
   - `chocofi_right-nice_nano_v2-zmk.uf2`
   - Settings reset files (if needed)

### Step 3: Flash the Keyboard

**For the LEFT half:**

1. Connect the LEFT half of your keyboard to your computer via USB-C
2. Double-tap the **reset button** on the nice!nano v2 controller
   - The reset button is the small button on the controller (usually near the USB port)
   - You should see the nice!nano appear as a USB drive named **NICENANO** or **DKBOOT**
3. Drag and drop (or copy) the `chocofi_left-nice_nano_v2-zmk.uf2` file onto the mounted drive
4. The keyboard will automatically flash and reboot (the drive will disappear)
5. Wait for the LED to stop blinking (indicates flashing is complete)

**For the RIGHT half:**

1. Disconnect the left half
2. Connect the RIGHT half of your keyboard to your computer via USB-C
3. Double-tap the **reset button** on the nice!nano v2 controller
4. Drag and drop the `chocofi_right-nice_nano_v2-zmk.uf2` file onto the mounted drive
5. The keyboard will automatically flash and reboot

### Step 4: Verify

1. Disconnect and reconnect both halves (or power cycle)
2. The two halves should automatically pair via Bluetooth
3. Test your keymap to ensure everything works as expected

---

## Method 2: Building Locally

Building locally is useful for rapid iteration during development without pushing to GitHub.

### Prerequisites for Local Building

You need a development environment with ZMK. The recommended approach is using the development container.

#### Option A: Using Development Container (Recommended)

1. Install [Docker Desktop](https://www.docker.com/products/docker-desktop)
2. Install [VS Code](https://code.visualstudio.com/) with the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
3. Clone the ZMK repository (Urob's fork):
   ```bash
   cd /workspaces
   git clone https://github.com/urob/zmk zmk-urob
   cd zmk-urob
   ```
4. Open the ZMK folder in VS Code and reopen in the development container when prompted

#### Option B: Native Linux/WSL Setup

If you prefer not to use Docker, you can set up ZMK natively on Linux or WSL. Follow the [ZMK Development Setup guide](https://zmk.dev/docs/development/setup).

### Step 1: Build the Firmware Locally

Once you have the development environment ready:

1. Navigate to your config repository:
   ```bash
   cd /path/to/chocofi-config
   ```

2. Build the LEFT half:
   ```bash
   ./build.sh chocofi left
   ```
   - Output: `chocofi_left.uf2`
   - Use `-p` flag for pristine build: `./build.sh -p chocofi left`

3. Build the RIGHT half:
   ```bash
   ./build.sh chocofi right
   ```
   - Output: `chocofi_right.uf2`

**Note:** The build script assumes:
- You're in a development container
- ZMK source is located at `/workspaces/zmk-urob/`
- Board is `nice_nano_v2` (can be overridden with `-b` flag)

### Step 2: Flash the Firmware

Follow the same flashing steps as in Method 1, Step 3:

1. Connect the keyboard half via USB-C
2. Double-tap the reset button to enter bootloader mode
3. Copy the corresponding `.uf2` file to the mounted drive
4. Wait for automatic flashing and reboot
5. Repeat for the other half

---

## Troubleshooting

### Keyboard doesn't appear as USB drive

- Make sure you're **double-tapping** the reset button (not single tap)
- Try a different USB cable (some cables are charge-only)
- Check if the nice!nano has power (LED should light up)

### Halves won't pair

1. Reset the Bluetooth bonds:
   - Use the ADJ layer (press NAV + SYM together)
   - Press the `BT_CLR` key (top-left position on left half)
2. Power cycle both halves
3. They should automatically pair

### Build script fails

- Ensure you're in the development container
- Verify ZMK source is at `/workspaces/zmk-urob/`
- Try a pristine build: `./build.sh -p chocofi left`
- Check that `west.yml` points to the correct ZMK fork

### Changes don't take effect

- Verify you flashed the correct half (left vs right)
- Check if you need to clear settings (download settings reset from GitHub artifacts)
- Ensure your keymap syntax is correct (check GitHub Actions build logs)

---

## Quick Reference

### File Locations

- **Keymap**: `config/base.keymap`
- **Combos**: `config/combos.dtsi`
- **Board config**: `config/chocofi.keymap`
- **Build script**: `build.sh`

### Build Commands

```bash
# Basic build
./build.sh chocofi left
./build.sh chocofi right

# Pristine build (clean first)
./build.sh -p chocofi left

# Custom board
./build.sh -b nice_nano_v2 chocofi left
```

### Flashing Checklist

- [ ] Build or download firmware files
- [ ] Connect keyboard half via USB-C
- [ ] Double-tap reset button
- [ ] Copy .uf2 file to mounted drive
- [ ] Wait for automatic reboot
- [ ] Repeat for other half
- [ ] Test functionality

---

## Additional Resources

- [ZMK Documentation](https://zmk.dev/docs)
- [Urob's ZMK Config](https://github.com/urob/zmk-config)
- [nice!nano Documentation](https://nicekeyboards.com/docs/nice-nano/)
- [Keymap Drawer](https://github.com/caksoylar/keymap-drawer) - for visual keymap generation
