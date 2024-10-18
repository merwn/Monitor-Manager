param (
    [int]$MonitorNumber,
    [string]$Resolution,
    [string]$Orientation,
    [int]$Scaling,
    [bool]$SetPrimary
)

# Load necessary assemblies
Add-Type -AssemblyName System.Windows.Forms

# Define the required Windows API functions and structures
if (-not ([System.Management.Automation.PSTypeName]'NativeMethods').Type)
{
    $pinvokeCode = @"
    using System;
    using System.Runtime.InteropServices;

    public class NativeMethods {
        [DllImport("user32.dll")]
        public static extern int EnumDisplaySettings(string deviceName, int modeNum, ref DEVMODE devMode);
        [DllImport("user32.dll")]
        public static extern int ChangeDisplaySettingsEx(string deviceName, ref DEVMODE devMode, IntPtr hwnd, uint dwflags, IntPtr lParam);

        [StructLayout(LayoutKind.Sequential)]
        public struct DEVMODE {
            [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 32)]
            public string dmDeviceName;
            public short dmSpecVersion;
            public short dmDriverVersion;
            public short dmSize;
            public short dmDriverExtra;
            public int dmFields;
            public int dmPositionX;
            public int dmPositionY;
            public int dmDisplayOrientation;
            public int dmDisplayFixedOutput;
            public short dmColor;
            public short dmDuplex;
            public short dmYResolution;
            public short dmTTOption;
            public short dmCollate;
            [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 32)]
            public string dmFormName;
            public short dmLogPixels;
            public int dmBitsPerPel;
            public int dmPelsWidth;
            public int dmPelsHeight;
            public int dmDisplayFlags;
            public int dmDisplayFrequency;
            public int dmICMMethod;
            public int dmICMIntent;
            public int dmMediaType;
            public int dmDitherType;
            public int dmReserved1;
            public int dmReserved2;
            public int dmPanningWidth;
            public int dmPanningHeight;
        }
    }
"@

    Add-Type -TypeDefinition $pinvokeCode -Language CSharp
}

function Set-ScreenResolution {
    param (
        [int]$Width,
        [int]$Height,
        [int]$MonitorIndex
    )
    
    $screens = [System.Windows.Forms.Screen]::AllScreens
    if ($MonitorIndex -ge $screens.Length) {
        Write-Error "Monitor index out of range"
        return
    }

    $deviceName = $screens[$MonitorIndex].DeviceName

    $dm = New-Object NativeMethods+DEVMODE
    $dm.dmSize = [System.Runtime.InteropServices.Marshal]::SizeOf($dm)
    $dm.dmDriverExtra = 0

    $result = [NativeMethods]::EnumDisplaySettings($deviceName, -1, [ref]$dm)
    if ($result -eq 0) {
        Write-Error "Failed to enumerate display settings"
        return
    }

    $dm.dmPelsWidth = $Width
    $dm.dmPelsHeight = $Height
    $dm.dmFields = 0x00080000 -bor 0x00100000  # DM_PELSWIDTH and DM_PELSHEIGHT

    $result = [NativeMethods]::ChangeDisplaySettingsEx($deviceName, [ref]$dm, [IntPtr]::Zero, 0, [IntPtr]::Zero)
    if ($result -ne 0) {
        Write-Error "Failed to change display settings"
    }
}

function Set-ScreenOrientation {
    param (
        [string]$Orientation,
        [int]$MonitorIndex
    )
    
    $orientationValue = switch ($Orientation.ToLower()) {
        "landscape" { 0 }
        "portrait" { 1 }
        "landscape-flipped" { 2 }
        "portrait-flipped" { 3 }
        default { 
            Write-Error "Invalid orientation. Use landscape, portrait, landscape-flipped, or portrait-flipped."
            return $null
        }
    }

    if ($null -eq $orientationValue) {
        return
    }

    $screens = [System.Windows.Forms.Screen]::AllScreens
    if ($MonitorIndex -ge $screens.Length) {
        Write-Error "Monitor index out of range"
        return
    }

    $deviceName = $screens[$MonitorIndex].DeviceName

    $dm = New-Object NativeMethods+DEVMODE
    $dm.dmSize = [System.Runtime.InteropServices.Marshal]::SizeOf($dm)
    $dm.dmDriverExtra = 0

    $result = [NativeMethods]::EnumDisplaySettings($deviceName, -1, [ref]$dm)
    if ($result -eq 0) {
        Write-Error "Failed to enumerate display settings"
        return
    }

    $dm.dmDisplayOrientation = $orientationValue
    $dm.dmFields = 0x00000080  # DM_DISPLAYORIENTATION

    $result = [NativeMethods]::ChangeDisplaySettingsEx($deviceName, [ref]$dm, [IntPtr]::Zero, 0, [IntPtr]::Zero)
    if ($result -ne 0) {
        Write-Error "Failed to change display orientation"
    }
}

function Set-ScreenScaling {
    param (
        [int]$Scaling,
        [int]$MonitorIndex
    )
    
    $screens = [System.Windows.Forms.Screen]::AllScreens
    if ($MonitorIndex -ge $screens.Length) {
        Write-Error "Monitor index out of range"
        return
    }

    $deviceName = $screens[$MonitorIndex].DeviceName

    $dm = New-Object NativeMethods+DEVMODE
    $dm.dmSize = [System.Runtime.InteropServices.Marshal]::SizeOf($dm)
    $dm.dmDriverExtra = 0

    $result = [NativeMethods]::EnumDisplaySettings($deviceName, -1, [ref]$dm)
    if ($result -eq 0) {
        Write-Error "Failed to enumerate display settings"
        return
    }

    $dm.dmLogPixels = $Scaling
    $dm.dmFields = 0x00200000  # DM_LOGPIXELS

    $result = [NativeMethods]::ChangeDisplaySettingsEx($deviceName, [ref]$dm, [IntPtr]::Zero, 0, [IntPtr]::Zero)
    if ($result -ne 0) {
        Write-Error "Failed to change display scaling"
    }
}

function Set-PrimaryMonitor {
    param (
        [int]$MonitorIndex
    )
    
    $screens = [System.Windows.Forms.Screen]::AllScreens
    if ($MonitorIndex -ge $screens.Length) {
        Write-Error "Monitor index out of range"
        return
    }

    $deviceName = $screens[$MonitorIndex].DeviceName

    $dm = New-Object NativeMethods+DEVMODE
    $dm.dmSize = [System.Runtime.InteropServices.Marshal]::SizeOf($dm)
    $dm.dmDriverExtra = 0

    $result = [NativeMethods]::EnumDisplaySettings($deviceName, -1, [ref]$dm)
    if ($result -eq 0) {
        Write-Error "Failed to enumerate display settings"
        return
    }

    $dm.dmFields = 0x00000020  # DM_POSITION
    $dm.dmPositionX = 0
    $dm.dmPositionY = 0

    $result = [NativeMethods]::ChangeDisplaySettingsEx($deviceName, [ref]$dm, [IntPtr]::Zero, 4, [IntPtr]::Zero)
    if ($result -ne 0) {
        Write-Error "Failed to set primary monitor"
    }
}

# Validate inputs
if ($MonitorNumber -lt 1) {
    Write-Error "MonitorNumber must be 1 or greater."
    exit
}

if ($Resolution -notmatch '^\d+x\d+$') {
    Write-Error "Invalid Resolution format. Use format like '1920x1080'."
    exit
}

if ($Orientation -notmatch '^(landscape|portrait|landscape-flipped|portrait-flipped)$') {
    Write-Error "Invalid Orientation. Use landscape, portrait, landscape-flipped, or portrait-flipped."
    exit
}

if ($Scaling -lt 100 -or $Scaling -gt 500) {
    Write-Error "Scaling must be between 100 and 500."
    exit
}

# Parse resolution
$width, $height = $Resolution -split 'x'
Set-ScreenResolution -Width $width -Height $height -MonitorIndex ($MonitorNumber - 1)

# Set orientation
Set-ScreenOrientation -Orientation $Orientation -MonitorIndex ($MonitorNumber - 1)

# Set scaling
Set-ScreenScaling -Scaling $Scaling -MonitorIndex ($MonitorNumber - 1)

# Set primary monitor if requested
if ($SetPrimary) {
    Set-PrimaryMonitor -MonitorIndex ($MonitorNumber - 1)
}

Write-Host "Monitor settings updated successfully."
