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
- `Size` (UDim2, optional): The size of the window. Default is `UDim2.fromOffset(868, 650)`
- `AcrylicBlur` (boolean, optional): Enable or disable acrylic blur effect. Default is `true`
- `DisabledWindowControls` (table, optional): Array of window controls to disable. Options: `"Exit"`, `"Minimize"`
- `DragStyle` (number, optional): Window drag style. `1` for icon-based dragging, `2` for full window dragging

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

## Next Steps

- Learn about [Window Configuration](window-configuration.md)
- Explore [Section Layouts](sections.md)
- Discover all available [Components](components.md)
- Check out more [Examples](examples.md)
