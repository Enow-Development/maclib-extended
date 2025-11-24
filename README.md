# MacLib Extended

MacLib is a comprehensive UI library for creating modern, feature-rich script interfaces in Roblox. This extended version includes support for full-width sections and enhanced layout capabilities.

## Features

- Modern and clean UI design with acrylic blur effects
- Resizable windows and sidebars
- Multiple tab support
- Three section layouts: Left (50%), Right (50%), and Full (100%)
- Rich component library (Button, Toggle, Slider, Textbox, Keybind, Dropdown, Colorpicker, Label)
- Built-in notification system
- Configuration saving and loading
- Customizable themes and styling

## Quick Start

```lua
-- Load MacLib
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Enow-Development/maclib-extended/refs/heads/main/maclib.lua"))()

-- Create window
local window = MacLib:Window({
    Title = "My Script",
    Subtitle = "Version 1.0"
})

-- Create tab
local tab = window:Tab({
    Name = "Main",
    Icon = "rbxassetid://10734950309"
})

-- Create sections
local leftSection = tab:Section({ Side = "Left" })
local rightSection = tab:Section({ Side = "Right" })
local fullSection = tab:Section({ Side = "Full" })

-- Add components
leftSection:Button({
    Name = "Click Me",
    Callback = function()
        print("Button clicked!")
    end
})

fullSection:Toggle({
    Name = "Enable Feature",
    Default = false,
    Callback = function(value)
        print("Feature:", value)
    end
})
```

## Section Layouts

MacLib supports three section layout types:

### Left Section (50% width)
```lua
local section = tab:Section({ Side = "Left" })
```
Places the section in the left column, taking up 50% of the tab width.

### Right Section (50% width)
```lua
local section = tab:Section({ Side = "Right" })
```
Places the section in the right column, taking up 50% of the tab width.

### Full Section (100% width)
```lua
local section = tab:Section({ Side = "Full" })
```
Places the section across the full width of the tab, taking up 100% of the available space.

## Documentation

Comprehensive documentation is available in the [documentation](documentation/) folder:

- [Getting Started](documentation/getting-started.md) - Installation and basic usage
- [Window Configuration](documentation/window-configuration.md) - Window setup and options
- [Tabs](documentation/tabs.md) - Creating and managing tabs
- [Sections](documentation/sections.md) - Section layouts and best practices
- [Components](documentation/components.md) - All available UI components
- [Notifications](documentation/notifications.md) - Notification system
- [Examples](documentation/examples.md) - Complete code examples

## Examples

Ready-to-use example scripts are available in the [examples](examples/) folder:

- [full_section_demo.lua](examples/full_section_demo.lua) - Demonstrates full-width sections
- [section_layouts_comparison.lua](examples/section_layouts_comparison.lua) - Compares all layout types

## Installation

Load MacLib directly from GitHub:

```lua
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Enow-Development/maclib-extended/refs/heads/main/maclib.lua"))()
```

## Key Features

### Full-Width Sections

The full-width section feature allows you to create sections that span the entire width of the tab:

- Perfect for wide content like tables or long text
- Great for headers and separators
- Maintains consistent styling with left/right sections
- Preserves creation order with other sections

### Flexible Layouts

Mix and match different section types in the same tab:

```lua
local header = tab:Section({ Side = "Full" })
local left = tab:Section({ Side = "Left" })
local right = tab:Section({ Side = "Right" })
local footer = tab:Section({ Side = "Full" })
```

### Rich Component Library

- Button - Clickable buttons with callbacks
- Toggle - On/off switches
- Slider - Numeric value selection
- Textbox - Text input fields
- Keybind - Keyboard key binding
- Dropdown - Selection from multiple options
- Colorpicker - RGB color selection
- Label - Text display

### Configuration Management

Save and load user settings:

```lua
tab:InsertConfigSection("Right")
```

## Browser-Friendly Documentation

The documentation is designed to be viewed on GitHub with proper formatting and navigation. Simply browse the [documentation](documentation/) folder on GitHub to read the guides.

## Best Practices

1. Use full sections for important or wide content
2. Balance left and right sections for visual harmony
3. Group related controls in the same section
4. Use labels to separate different groups
5. Provide sensible default values for all components

## Contributing

Contributions are welcome! Please ensure:

- Code follows the existing style
- Documentation is updated for new features
- Examples are provided for significant changes
- No emojis in documentation (for clean, professional appearance)

## License

This project is open source and available for use in your Roblox scripts.

## Credits

MacLib Extended - Enhanced version with full-width section support
Original MacLib - Base UI library framework
