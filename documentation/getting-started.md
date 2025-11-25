# Getting Started with MacLib

This guide will help you get started with MacLib and create your first UI.

## Installation

MacLib can be loaded directly from GitHub using HttpGet:

```lua
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Enow-Development/maclib-extended/refs/heads/main/maclib.lua"))()
```

## Creating Your First Window

To create a window, use the `MacLib:Window()` function:

```lua
local window = MacLib:Window({
    Title = "My Script",
    Subtitle = "Version 1.0",
    Size = UDim2.fromOffset(868, 650),
    AcrylicBlur = true
})
```

### Window Parameters

- `Title` (string): The main title displayed in the window
- `Subtitle` (string): The subtitle text below the title
- `Size` (UDim2, optional): The size of the window. Default is `UDim2.fromOffset(868, 650)`. **Note:** Deprecated when using scaling features.
- `DefaultScale` (number, optional): Default scale percentage (50-100). Default is `75`. Use this instead of Size for scalable windows.
- `MinScale` (number, optional): Minimum scale percentage (50-100). Default is `50`.
- `MaxScale` (number, optional): Maximum scale percentage (50-100). Default is `100`.
- `AcrylicBlur` (boolean, optional): Enable or disable acrylic blur effect. Default is `true`
- `DisabledWindowControls` (table, optional): Array of window controls to disable. Options: `"Exit"`, `"Minimize"`
- `DragStyle` (number, optional): Window drag style. `1` for icon-based dragging, `2` for full window dragging
- `onScaleStart` (function, optional): Callback when scaling starts. Receives `oldScale` parameter.
- `onScale` (function, optional): Callback during scaling. Receives `newScale` and `newSize` parameters.
- `onScaleEnd` (function, optional): Callback when scaling completes. Receives `finalScale` and `finalSize` parameters.

## Creating Tabs

Tabs organize your UI into different sections:

```lua
local mainTab = window:Tab({
    Name = "Main",
    Icon = "rbxassetid://10734950309"
})

local settingsTab = window:Tab({
    Name = "Settings",
    Icon = "rbxassetid://10734950309"
})
```

### Tab Parameters

- `Name` (string): The name of the tab
- `Icon` (string): Asset ID for the tab icon

## Creating Sections

Sections are containers within tabs that hold UI components. MacLib supports three section layouts:

```lua
-- Left section (50% width, left side)
local leftSection = mainTab:Section({ Side = "Left" })

-- Right section (50% width, right side)
local rightSection = mainTab:Section({ Side = "Right" })

-- Full section (100% width)
local fullSection = mainTab:Section({ Side = "Full" })
```

### Section Parameters

- `Side` (string): Position of the section
  - `"Left"`: Places section in the left column (50% width)
  - `"Right"`: Places section in the right column (50% width)
  - `"Full"`: Places section across full width (100% width)
  - If not specified or invalid, defaults to `"Left"`

## Adding Components

Once you have a section, you can add various UI components:

```lua
leftSection:Button({
    Name = "Click Me",
    Callback = function()
        print("Button clicked!")
    end
})

leftSection:Toggle({
    Name = "Enable Feature",
    Default = false,
    Callback = function(value)
        print("Toggle is now:", value)
    end
})

leftSection:Slider({
    Name = "Speed",
    Min = 0,
    Max = 100,
    Default = 50,
    Callback = function(value)
        print("Slider value:", value)
    end
})
```

## Complete Example

Here's a complete example that puts everything together:

```lua
-- Load MacLib
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Enow-Development/maclib-extended/refs/heads/main/maclib.lua"))()

-- Create window
local window = MacLib:Window({
    Title = "My First Script",
    Subtitle = "Learning MacLib",
    Size = UDim2.fromOffset(868, 650)
})

-- Create tab
local tab = window:Tab({
    Name = "Main",
    Icon = "rbxassetid://10734950309"
})

-- Create sections
local leftSection = tab:Section({ Side = "Left" })
local rightSection = tab:Section({ Side = "Right" })

-- Add components to left section
leftSection:Button({
    Name = "Test Button",
    Callback = function()
        window:Notification({
            Title = "Success",
            Description = "Button was clicked!",
            Duration = 3
        })
    end
})

leftSection:Toggle({
    Name = "Enable Feature",
    Default = false,
    Callback = function(value)
        print("Feature enabled:", value)
    end
})

-- Add components to right section
rightSection:Slider({
    Name = "Speed",
    Min = 0,
    Max = 100,
    Default = 50,
    Callback = function(value)
        print("Speed set to:", value)
    end
})

rightSection:Textbox({
    Name = "Username",
    Placeholder = "Enter username...",
    Callback = function(text)
        print("Username:", text)
    end
})
```

## Window Scaling

MacLib supports dynamic window scaling, allowing users to resize windows from 50% to 100% of the base size (868x650).

### Basic Scaling Example

```lua
-- Create a scalable window
local window = MacLib:Window({
    Title = "Scalable Window",
    Subtitle = "Resize me!",
    DefaultScale = 75,  -- Start at 75%
    MinScale = 50,      -- Minimum 50%
    MaxScale = 100      -- Maximum 100%
})

-- Create a tab with scale controls
local tab = window:Tab({
    Name = "Controls",
    Icon = "rbxassetid://10734950309"
})

local section = tab:Section({ Side = "Left" })

-- Add slider to control scale
section:Slider({
    Name = "Window Scale",
    Min = 50,
    Max = 100,
    Default = 75,
    Callback = function(value)
        window:SetScale(value)
    end
})

-- Add button to reset scale
section:Button({
    Name = "Reset Scale",
    Callback = function()
        window:ResetScale()
    end
})

-- Add button to show current scale
section:Button({
    Name = "Show Current Scale",
    Callback = function()
        local scale = window:GetScale()
        print("Current scale: " .. scale .. "%")
    end
})
```

### Scaling Methods

- `window:SetScale(percentage)` - Set window scale (50-100)
- `window:GetScale()` - Get current scale percentage
- `window:ResetScale()` - Reset to default scale
- `window:GetScreenSize()` - Get detected screen size
- `window:GetCurrentSize()` - Get current window size

### Mobile Considerations

For mobile devices, consider using smaller default scales:

```lua
local window = MacLib:Window({
    Title = "Mobile Friendly",
    Subtitle = "Optimized for mobile",
    DefaultScale = 60,  -- Smaller default for mobile
    MinScale = 50,
    MaxScale = 80,      -- Don't allow too large
    AcrylicBlur = false,  -- Better performance
    DragStyle = 2       -- Easier to drag on touch screens
})
```

## Next Steps

- Learn about [Window Configuration](window-configuration.md)
- Explore [Section Layouts](sections.md)
- Discover all available [Components](components.md)
- Check out more [Examples](examples.md)
