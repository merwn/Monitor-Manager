# PowerShell-Monitor-Manager

PowerShell-Monitor-Manager is a powerful and flexible PowerShell script for managing monitor settings on Windows systems. It allows users to easily change resolution, orientation, scaling, and set the primary monitor through command-line parameters.

## Features

- Change monitor resolution
- Adjust screen orientation (landscape, portrait, landscape-flipped, portrait-flipped)
- Set display scaling
- Designate a primary monitor
- Support for multi-monitor setups

## Requirements

- Windows operating system
- PowerShell 5.1 or later

## Usage

```powershell
.\Set-MonitorSettings.ps1 -MonitorNumber <int> -Resolution <string> -Orientation <string> -Scaling <int> -SetPrimary <bool>
```

### Parameters

- `MonitorNumber`: The index of the monitor to modify (starting from 1)
- `Resolution`: The desired resolution in the format "WidthxHeight" (e.g., "1920x1080")
- `Orientation`: The desired orientation ("landscape", "portrait", "landscape-flipped", or "portrait-flipped")
- `Scaling`: The desired scaling percentage (100-500)
- `SetPrimary`: Whether to set this monitor as the primary display ($true or $false)

### Example

```powershell
.\Set-MonitorSettings.ps1 -MonitorNumber 2 -Resolution "1920x1080" -Orientation "landscape" -Scaling 100 -SetPrimary $true
```

This command will set the second monitor to 1920x1080 resolution, landscape orientation, 100% scaling, and make it the primary display.

## Installation

1. Clone this repository or download the `Set-MonitorSettings.ps1` file.
2. Ensure you have the necessary permissions to run PowerShell scripts on your system.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Disclaimer

Use this script at your own risk. Always ensure you have a way to revert changes in case of unexpected results.

