# Components

MacLib provides a rich set of UI components that can be added to sections. This guide covers all available components and their usage.

## Component List

- [Button](#button)
- [Toggle](#toggle)
- [Slider](#slider)
- [Textbox](#textbox)
- [Keybind](#keybind)
- [Dropdown](#dropdown)
- [Colorpicker](#colorpicker)
- [Label](#label)

## Button

A clickable button that executes a callback function.

### Usage

```lua
section:Button({
    Name = "Click Me",
    Callback = function()
        print("Button clicked!")
    end
})
```

### Parameters

- `Name` (string, required): The text displayed on the button
- `Callback` (function, required): Function called when the button is clicked

### Methods

```lua
local button = section:Button({ Name = "Test", Callback = function() end })

-- Update button text
button:Set("New Text")
```

## Toggle

A switch that can be turned on or off.

### Usage

```lua
section:Toggle({
    Name = "Enable Feature",
    Default = false,
    Flag = "FeatureEnabled",
    Callback = function(value)
        print("Toggle is now:", value)
    end
})
```

### Parameters

- `Name` (string, required): The label for the toggle
- `Default` (boolean, optional): Initial state of the toggle. Default is `false`
- `Flag` (string, optional): Unique identifier for saving/loading configuration
- `Callback` (function, required): Function called when toggle state changes, receives boolean value

### Methods

```lua
local toggle = section:Toggle({ Name = "Test", Default = false })

-- Set toggle state
toggle:Set(true)

-- Get current state
local state = toggle:Get()
```

## Slider

A slider for selecting numeric values within a range.

### Usage

```lua
section:Slider({
    Name = "Speed",
    Min = 0,
    Max = 100,
    Default = 50,
    Flag = "SpeedValue",
    Callback = function(value)
        print("Slider value:", value)
    end
})
```

### Parameters

- `Name` (string, required): The label for the slider
- `Min` (number, required): Minimum value
- `Max` (number, required): Maximum value
- `Default` (number, optional): Initial value. Default is `Min`
- `Flag` (string, optional): Unique identifier for saving/loading configuration
- `Callback` (function, required): Function called when value changes, receives number value

### Methods

```lua
local slider = section:Slider({ Name = "Test", Min = 0, Max = 100, Default = 50 })

-- Set slider value
slider:Set(75)

-- Get current value
local value = slider:Get()
```

## Textbox

An input field for entering text.

### Usage

```lua
section:Textbox({
    Name = "Username",
    Placeholder = "Enter username...",
    Default = "",
    Flag = "Username",
    Callback = function(text)
        print("Text entered:", text)
    end
})
```

### Parameters

- `Name` (string, required): The label for the textbox
- `Placeholder` (string, optional): Placeholder text shown when empty
- `Default` (string, optional): Initial text value. Default is `""`
- `Flag` (string, optional): Unique identifier for saving/loading configuration
- `Callback` (function, required): Function called when text changes, receives string value

### Methods

```lua
local textbox = section:Textbox({ Name = "Test", Placeholder = "Enter text..." })

-- Set textbox value
textbox:Set("New text")

-- Get current value
local text = textbox:Get()
```

## Keybind

A component for binding keyboard keys to actions.

### Usage

```lua
section:Keybind({
    Name = "Toggle UI",
    Default = Enum.KeyCode.RightShift,
    Flag = "UIToggleKey",
    Callback = function(key)
        print("Key bound:", key)
    end
})
```

### Parameters

- `Name` (string, required): The label for the keybind
- `Default` (Enum.KeyCode, optional): Initial key binding. Default is `Enum.KeyCode.RightShift`
- `Flag` (string, optional): Unique identifier for saving/loading configuration
- `Callback` (function, required): Function called when key is pressed, receives KeyCode value

### Methods

```lua
local keybind = section:Keybind({ Name = "Test", Default = Enum.KeyCode.E })

-- Set keybind
keybind:Set(Enum.KeyCode.Q)

-- Get current keybind
local key = keybind:Get()
```

## Dropdown

A dropdown menu for selecting from multiple options.

### Usage

```lua
section:Dropdown({
    Name = "Select Option",
    Options = {"Option 1", "Option 2", "Option 3"},
    Default = "Option 1",
    Flag = "SelectedOption",
    Callback = function(option)
        print("Selected:", option)
    end
})
```

### Parameters

- `Name` (string, required): The label for the dropdown
- `Options` (table, required): Array of string options to choose from
- `Default` (string, optional): Initially selected option. Default is first option
- `Flag` (string, optional): Unique identifier for saving/loading configuration
- `Callback` (function, required): Function called when selection changes, receives string value

### Methods

```lua
local dropdown = section:Dropdown({
    Name = "Test",
    Options = {"A", "B", "C"},
    Default = "A"
})

-- Set selected option
dropdown:Set("B")

-- Get current selection
local selected = dropdown:Get()

-- Update options list
dropdown:Refresh({"X", "Y", "Z"})
```

## Colorpicker

A color picker for selecting RGB colors.

### Usage

```lua
section:Colorpicker({
    Name = "Theme Color",
    Default = Color3.fromRGB(255, 0, 0),
    Flag = "ThemeColor",
    Callback = function(color)
        print("Color selected:", color)
    end
})
```

### Parameters

- `Name` (string, required): The label for the colorpicker
- `Default` (Color3, optional): Initial color. Default is `Color3.fromRGB(255, 255, 255)`
- `Flag` (string, optional): Unique identifier for saving/loading configuration
- `Callback` (function, required): Function called when color changes, receives Color3 value

### Methods

```lua
local colorpicker = section:Colorpicker({
    Name = "Test",
    Default = Color3.fromRGB(255, 0, 0)
})

-- Set color
colorpicker:Set(Color3.fromRGB(0, 255, 0))

-- Get current color
local color = colorpicker:Get()
```

## Label

A text label for displaying information.

### Usage

```lua
section:Label({
    Text = "This is a label"
})
```

### Parameters

- `Text` (string, required): The text to display

### Methods

```lua
local label = section:Label({ Text = "Initial text" })

-- Update label text
label:Set("New text")
```

## Component Best Practices

### Naming

- Use clear, descriptive names
- Keep names concise
- Use title case for consistency
- Avoid technical jargon when possible

### Callbacks

- Keep callback functions lightweight
- Avoid blocking operations in callbacks
- Handle errors gracefully
- Consider debouncing for frequently-called callbacks

### Flags

- Use unique, descriptive flag names
- Follow a consistent naming convention
- Use flags for all settings you want to save
- Avoid special characters in flag names

### Default Values

- Always provide sensible default values
- Consider the most common use case
- Test default values thoroughly
- Document default behavior

### Organization

- Group related components in the same section
- Order components logically
- Use labels to separate groups
- Consider visual hierarchy

## Complete Example

```lua
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Enow-Development/maclib-extended/refs/heads/main/maclib.lua"))()

local window = MacLib:Window({
    Title = "Component Demo",
    Subtitle = "All Components"
})

local tab = window:Tab({
    Name = "Components",
    Icon = "rbxassetid://10734950309"
})

local section = tab:Section({ Side = "Full" })

-- Label
section:Label({ Text = "Basic Components" })

-- Button
section:Button({
    Name = "Test Button",
    Callback = function()
        window:Notification({
            Title = "Button",
            Description = "Button was clicked!",
            Duration = 2
        })
    end
})

-- Toggle
section:Toggle({
    Name = "Enable Feature",
    Default = false,
    Flag = "Feature",
    Callback = function(value)
        print("Feature:", value)
    end
})

-- Slider
section:Slider({
    Name = "Speed",
    Min = 0,
    Max = 100,
    Default = 50,
    Flag = "Speed",
    Callback = function(value)
        print("Speed:", value)
    end
})

-- Textbox
section:Textbox({
    Name = "Username",
    Placeholder = "Enter username...",
    Flag = "Username",
    Callback = function(text)
        print("Username:", text)
    end
})

-- Keybind
section:Keybind({
    Name = "Toggle UI",
    Default = Enum.KeyCode.RightShift,
    Flag = "UIKey",
    Callback = function(key)
        print("Key:", key)
    end
})

-- Dropdown
section:Dropdown({
    Name = "Select Mode",
    Options = {"Mode 1", "Mode 2", "Mode 3"},
    Default = "Mode 1",
    Flag = "Mode",
    Callback = function(option)
        print("Mode:", option)
    end
})

-- Colorpicker
section:Colorpicker({
    Name = "Theme Color",
    Default = Color3.fromRGB(255, 0, 0),
    Flag = "Color",
    Callback = function(color)
        print("Color:", color)
    end
})
```
