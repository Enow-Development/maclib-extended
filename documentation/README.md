# MacLib Documentation

MacLib is a comprehensive UI library for creating script interfaces in Roblox. This documentation provides detailed information about all features and components available in MacLib.

## Table of Contents

1. [Getting Started](getting-started.md)
2. [Window Configuration](window-configuration.md)
3. [Window Scaling](scaling.md) - **New!**
4. [Tabs](tabs.md)
5. [Sections](sections.md)
6. [Components](components.md)
7. [Notifications](notifications.md)
8. [Examples](examples.md)

## Quick Start

```lua
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Enow-Development/maclib-extended/refs/heads/main/maclib.lua"))()

local window = MacLib:Window({
    Title = "My Script",
    Subtitle = "Version 1.0",
    Size = UDim2.fromOffset(868, 650)
})

local tab = window:Tab({ Name = "Main", Icon = "rbxassetid://10734950309" })
local section = tab:Section({ Side = "Left" })

section:Button({
    Name = "Click Me",
    Callback = function()
        print("Button clicked!")
    end
})
```

## Features

- Modern and clean UI design
- Acrylic blur effects
- **Dynamic window scaling (50-100%)** - **New!**
- **Responsive design for all UI elements** - **New!**
- **Mobile-optimized with touch support** - **New!**
- Resizable windows and sidebars
- Multiple tab support
- Three section layouts: Left, Right, and Full
- Rich component library
- Notification system
- Configuration saving and loading
- Customizable themes

## Installation

Load MacLib using the following code:

```lua
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Enow-Development/maclib-extended/refs/heads/main/maclib.lua"))()
```

## Support

For issues, questions, or feature requests, please visit the GitHub repository.
