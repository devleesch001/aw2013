# AW2013 LED Controller Script

A lightweight, POSIX-compliant shell script to control an RGB LED via the AW2013 I2C chip. This tool allows you to initialize the LED, change colors using natural names or hex codes, and configure advanced breathing effects directly from the command line.

## Features

- **Easy to use**: Control the LED with simple commands like `color red` or `breathe`.
- **Custom colors**: Set exact RGB values using hex codes.
- **Hardware Breathing Effects**: Leverage the AW2013 built-in, hardware-synchronized breathing mode without CPU overhead.
- **Command chaining**: Execute multiple commands sequentially on a single line.
- **Configurable**: Change the I2C port or device address simply by setting environment variables.

## Requirements

This script is a wrapper around common i2c utilities and requires the `i2c-tools` package to be installed on your system.

On Debian/Ubuntu-based systems:
```bash
sudo apt-get install i2c-tools
```

Ensure your user has the appropriate permissions to access the I2C bus (often by adding the user to the `i2c` group).

## Configuration

The script uses environment variables for configuration. If not set, it defaults to:
- `I2C_PORT=0` (which accesses `/dev/i2c-0`)
- `I2C_ADDR=0x45` (default AW2013 address)

To override the defaults, export the variables before running the script:
```bash
export I2C_PORT=1       # Targets /dev/i2c-1
export I2C_ADDR=0x46    # Alternative address
./aw2013 <commands>
```

## Usage

```bash
./aw2013 [OPTIONS] <Command> <Params>
```

### Options
- `-h, --help`: Show helpful usage instructions.
- `-v, --verbose`: Print detailed execution logs, including the exact `i2cset` commands being executed under the hood.

### Commands
- **`reset`**: Soft-reset the AW2013 registers to their defaults.
- **`init`**: Initialize the controller (sets up PWM mode and enables LED channels).
- **`color <color>`**: Set a predefined color.
  - *Available options:* `red`, `green`, `blue`, `yellow`, `purple`, `cyan`, `white`, `off`
- **`rgb <R> <G> <B>`**: Set a custom RGB color using hex values.
  - *Example:* `./aw2013 rgb 0x00 0x33 0x99`
- **`breathe`**: Start the hardware breathing mode.
  - `breathe` (Default: LTC0=0x34, LTC1=0x30)
  - `breathe <V>` (Sets both LTC0 and LTC1 to value V, e.g., `breathe 0x30`)
  - `breathe <V> <W>` (Sets LTC0 to V and LTC1 to W, e.g., `breathe 0x34 0x30`)

### Examples

Basic controls:
```bash
./aw2013 reset
./aw2013 color purple
./aw2013 breathe 0x11
```

Using custom RGB values and verbose output:
```bash
./aw2013 --verbose rgb 0x00 0x33 0x99
```

**Chaining Commands**
The script allows executing multiple operations in a single invocation, applied in sequence:
```bash
# Sets color to cyan and initiates breathing effect
./aw2013 color cyan breathe 0x34 0x30

# Completes a full reset, sets color to red, and turns on breathing mode
./aw2013 reset rgb 0xFF 0x00 0x00 breathe
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
Copyright (c) 2026 Alexis DEVLEESCHAUWER
