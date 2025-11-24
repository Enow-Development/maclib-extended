# Examples

This page provides complete, ready-to-use examples demonstrating various MacLib features and use cases.

## Table of Contents

1. [Basic Script](#basic-script)
2. [Full Section Demo](#full-section-demo)
3. [Mixed Layout](#mixed-layout)
4. [Game Script Template](#game-script-template)
5. [Settings Manager](#settings-manager)
6. [Multi-Tab Application](#multi-tab-application)

## Basic Script

A simple script demonstrating the core features of MacLib.

```lua
-- Load MacLib
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Enow-Development/maclib-extended/refs/heads/main/maclib.lua"))()

-- Create window
local window = MacLib:Window({
    Title = "Basic Script",
    Subtitle = "Version 1.0"
})

-- Create tab
local tab = window:Tab({
    Name = "Main",
    Icon = "rbxassetid://10734950309"
})

-- Create section
local section = tab:Section({ Side = "Left" })

-- Add button
section:Button({
    Name = "Click Me",
    Callback = function()
        window:Notification({
            Title = "Success",
            Description = "Button was clicked!",
            Duration = 3
        })
    end
})

-- Add toggle
section:Toggle({
    Name = "Enable Feature",
    Default = false,
    Callback = function(value)
        print("Feature enabled:", value)
    end
})

-- Add slider
section:Slider({
    Name = "Speed",
    Min = 0,
    Max = 100,
    Default = 50,
    Callback = function(value)
        print("Speed:", value)
    end
})
```

## Full Section Demo

Demonstrating the full-width section feature.

```lua
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Enow-Development/maclib-extended/refs/heads/main/maclib.lua"))()

local window = MacLib:Window({
    Title = "Full Section Demo",
    Subtitle = "Showcasing Full-Width Sections"
})

local tab = window:Tab({
    Name = "Demo",
    Icon = "rbxassetid://10734950309"
})

-- Full-width header section
local headerSection = tab:Section({ Side = "Full" })
headerSection:Label({ Text = "Welcome to Full Section Demo" })
headerSection:Button({
    Name = "Full Width Button",
    Callback = function()
        window:Notification({
            Title = "Full Section",
            Description = "This button spans the full width!",
            Duration = 3
        })
    end
})

-- Two-column sections
local leftSection = tab:Section({ Side = "Left" })
leftSection:Label({ Text = "Left Column" })
leftSection:Toggle({
    Name = "Left Toggle",
    Default = false,
    Callback = function(value)
        print("Left toggle:", value)
    end
})

local rightSection = tab:Section({ Side = "Right" })
rightSection:Label({ Text = "Right Column" })
rightSection:Slider({
    Name = "Right Slider",
    Min = 0,
    Max = 100,
    Default = 50,
    Callback = function(value)
        print("Right slider:", value)
    end
})

-- Another full-width section
local footerSection = tab:Section({ Side = "Full" })
footerSection:Label({ Text = "Full-Width Footer Section" })
footerSection:Textbox({
    Name = "Full Width Input",
    Placeholder = "Enter text here...",
    Callback = function(text)
        print("Input:", text)
    end
})
```

## Mixed Layout

A complex layout mixing different section types.

```lua
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Enow-Development/maclib-extended/refs/heads/main/maclib.lua"))()

local window = MacLib:Window({
    Title = "Mixed Layout",
    Subtitle = "Complex Section Arrangement",
    Size = UDim2.fromOffset(900, 700)
})

local tab = window:Tab({
    Name = "Dashboard",
    Icon = "rbxassetid://10734950309"
})

-- Header
local header = tab:Section({ Side = "Full" })
header:Label({ Text = "Dashboard Overview" })

-- First row of columns
local left1 = tab:Section({ Side = "Left" })
left1:Label({ Text = "Quick Actions" })
left1:Button({
    Name = "Action 1",
    Callback = function()
        window:Notification({
            Title = "Action 1",
            Description = "Executed successfully",
            Duration = 2
        })
    end
})
left1:Button({
    Name = "Action 2",
    Callback = function()
        window:Notification({
            Title = "Action 2",
            Description = "Executed successfully",
            Duration = 2
        })
    end
})

local right1 = tab:Section({ Side = "Right" })
right1:Label({ Text = "Settings" })
right1:Toggle({
    Name = "Auto Mode",
    Default = true,
    Callback = function(value)
        print("Auto mode:", value)
    end
})
right1:Toggle({
    Name = "Notifications",
    Default = true,
    Callback = function(value)
        print("Notifications:", value)
    end
})

-- Middle full section
local middle = tab:Section({ Side = "Full" })
middle:Label({ Text = "Configuration" })
middle:Dropdown({
    Name = "Select Profile",
    Options = {"Profile 1", "Profile 2", "Profile 3"},
    Default = "Profile 1",
    Callback = function(option)
        print("Profile:", option)
    end
})

-- Second row of columns
local left2 = tab:Section({ Side = "Left" })
left2:Label({ Text = "Advanced" })
left2:Slider({
    Name = "Sensitivity",
    Min = 0,
    Max = 100,
    Default = 50,
    Callback = function(value)
        print("Sensitivity:", value)
    end
})

local right2 = tab:Section({ Side = "Right" })
right2:Label({ Text = "Keybinds" })
right2:Keybind({
    Name = "Toggle UI",
    Default = Enum.KeyCode.RightShift,
    Callback = function(key)
        print("Key:", key)
    end
})
```

## Game Script Template

A template for game-specific scripts with multiple feature categories.

```lua
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Enow-Development/maclib-extended/refs/heads/main/maclib.lua"))()

local window = MacLib:Window({
    Title = "Game Script",
    Subtitle = "Universal Features",
    Size = UDim2.fromOffset(868, 650)
})

-- Combat Tab
local combatTab = window:Tab({
    Name = "Combat",
    Icon = "rbxassetid://10734950309"
})

local combatLeft = combatTab:Section({ Side = "Left" })
combatLeft:Toggle({
    Name = "Auto Attack",
    Default = false,
    Flag = "AutoAttack",
    Callback = function(value)
        print("Auto attack:", value)
    end
})
combatLeft:Slider({
    Name = "Attack Speed",
    Min = 1,
    Max = 10,
    Default = 5,
    Flag = "AttackSpeed",
    Callback = function(value)
        print("Attack speed:", value)
    end
})

local combatRight = combatTab:Section({ Side = "Right" })
combatRight:Toggle({
    Name = "Auto Heal",
    Default = false,
    Flag = "AutoHeal",
    Callback = function(value)
        print("Auto heal:", value)
    end
})
combatRight:Slider({
    Name = "Heal Threshold",
    Min = 0,
    Max = 100,
    Default = 50,
    Flag = "HealThreshold",
    Callback = function(value)
        print("Heal threshold:", value)
    end
})

-- Movement Tab
local movementTab = window:Tab({
    Name = "Movement",
    Icon = "rbxassetid://10734950309"
})

local movementSection = movementTab:Section({ Side = "Full" })
movementSection:Toggle({
    Name = "Speed Boost",
    Default = false,
    Flag = "SpeedBoost",
    Callback = function(value)
        print("Speed boost:", value)
    end
})
movementSection:Slider({
    Name = "Walk Speed",
    Min = 16,
    Max = 100,
    Default = 16,
    Flag = "WalkSpeed",
    Callback = function(value)
        print("Walk speed:", value)
    end
})
movementSection:Toggle({
    Name = "Infinite Jump",
    Default = false,
    Flag = "InfiniteJump",
    Callback = function(value)
        print("Infinite jump:", value)
    end
})

-- Visuals Tab
local visualsTab = window:Tab({
    Name = "Visuals",
    Icon = "rbxassetid://10734950309"
})

local visualsLeft = visualsTab:Section({ Side = "Left" })
visualsLeft:Toggle({
    Name = "ESP",
    Default = false,
    Flag = "ESP",
    Callback = function(value)
        print("ESP:", value)
    end
})
visualsLeft:Colorpicker({
    Name = "ESP Color",
    Default = Color3.fromRGB(255, 0, 0),
    Flag = "ESPColor",
    Callback = function(color)
        print("ESP color:", color)
    end
})

local visualsRight = visualsTab:Section({ Side = "Right" })
visualsRight:Toggle({
    Name = "Tracers",
    Default = false,
    Flag = "Tracers",
    Callback = function(value)
        print("Tracers:", value)
    end
})
visualsRight:Colorpicker({
    Name = "Tracer Color",
    Default = Color3.fromRGB(0, 255, 0),
    Flag = "TracerColor",
    Callback = function(color)
        print("Tracer color:", color)
    end
})

-- Settings Tab
local settingsTab = window:Tab({
    Name = "Settings",
    Icon = "rbxassetid://10734950309"
})

local settingsSection = settingsTab:Section({ Side = "Left" })
settingsSection:Keybind({
    Name = "Toggle UI",
    Default = Enum.KeyCode.RightShift,
    Flag = "UIToggle",
    Callback = function(key)
        print("UI toggle key:", key)
    end
})
settingsSection:Toggle({
    Name = "Save Settings",
    Default = true,
    Flag = "SaveSettings",
    Callback = function(value)
        print("Save settings:", value)
    end
})
```

## Settings Manager

A script demonstrating configuration save/load functionality.

```lua
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Enow-Development/maclib-extended/refs/heads/main/maclib.lua"))()

local window = MacLib:Window({
    Title = "Settings Manager",
    Subtitle = "With Save/Load"
})

local tab = window:Tab({
    Name = "Settings",
    Icon = "rbxassetid://10734950309"
})

-- Settings section
local settingsSection = tab:Section({ Side = "Left" })

settingsSection:Toggle({
    Name = "Feature 1",
    Default = false,
    Flag = "Feature1",
    Callback = function(value)
        print("Feature 1:", value)
    end
})

settingsSection:Toggle({
    Name = "Feature 2",
    Default = true,
    Flag = "Feature2",
    Callback = function(value)
        print("Feature 2:", value)
    end
})

settingsSection:Slider({
    Name = "Value 1",
    Min = 0,
    Max = 100,
    Default = 50,
    Flag = "Value1",
    Callback = function(value)
        print("Value 1:", value)
    end
})

settingsSection:Textbox({
    Name = "Text 1",
    Placeholder = "Enter text...",
    Flag = "Text1",
    Callback = function(text)
        print("Text 1:", text)
    end
})

-- Config section (Roblox Studio only)
tab:InsertConfigSection("Right")
```

## Multi-Tab Application

A comprehensive application with multiple tabs and features.

```lua
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Enow-Development/maclib-extended/refs/heads/main/maclib.lua"))()

local window = MacLib:Window({
    Title = "Multi-Tab Application",
    Subtitle = "Complete Example",
    Size = UDim2.fromOffset(900, 700)
})

-- Home Tab
local homeTab = window:Tab({
    Name = "Home",
    Icon = "rbxassetid://10734950309"
})

local homeSection = homeTab:Section({ Side = "Full" })
homeSection:Label({ Text = "Welcome to Multi-Tab Application" })
homeSection:Button({
    Name = "Get Started",
    Callback = function()
        window:Notification({
            Title = "Welcome",
            Description = "Explore the tabs to see all features",
            Duration = 3
        })
    end
})

-- Features Tab
local featuresTab = window:Tab({
    Name = "Features",
    Icon = "rbxassetid://10734950309"
})

local featuresLeft = featuresTab:Section({ Side = "Left" })
featuresLeft:Label({ Text = "Basic Features" })
featuresLeft:Toggle({
    Name = "Feature A",
    Default = false,
    Callback = function(value)
        print("Feature A:", value)
    end
})
featuresLeft:Toggle({
    Name = "Feature B",
    Default = false,
    Callback = function(value)
        print("Feature B:", value)
    end
})

local featuresRight = featuresTab:Section({ Side = "Right" })
featuresRight:Label({ Text = "Advanced Features" })
featuresRight:Toggle({
    Name = "Feature C",
    Default = false,
    Callback = function(value)
        print("Feature C:", value)
    end
})
featuresRight:Toggle({
    Name = "Feature D",
    Default = false,
    Callback = function(value)
        print("Feature D:", value)
    end
})

-- Configuration Tab
local configTab = window:Tab({
    Name = "Config",
    Icon = "rbxassetid://10734950309"
})

local configFull = configTab:Section({ Side = "Full" })
configFull:Label({ Text = "General Configuration" })
configFull:Dropdown({
    Name = "Theme",
    Options = {"Dark", "Light", "Auto"},
    Default = "Dark",
    Callback = function(option)
        print("Theme:", option)
    end
})

local configLeft = configTab:Section({ Side = "Left" })
configLeft:Label({ Text = "Display" })
configLeft:Slider({
    Name = "UI Scale",
    Min = 50,
    Max = 150,
    Default = 100,
    Callback = function(value)
        print("UI Scale:", value)
    end
})

local configRight = configTab:Section({ Side = "Right" })
configRight:Label({ Text = "Behavior" })
configRight:Toggle({
    Name = "Auto Save",
    Default = true,
    Callback = function(value)
        print("Auto save:", value)
    end
})

-- About Tab
local aboutTab = window:Tab({
    Name = "About",
    Icon = "rbxassetid://10734950309"
})

local aboutSection = aboutTab:Section({ Side = "Full" })
aboutSection:Label({ Text = "Multi-Tab Application v1.0" })
aboutSection:Label({ Text = "Created with MacLib" })
aboutSection:Button({
    Name = "Check for Updates",
    Callback = function()
        window:Notification({
            Title = "Up to Date",
            Description = "You are running the latest version",
            Duration = 3
        })
    end
})
```

## Tips for Using Examples

1. **Copy and Paste**: These examples are ready to use - just copy and paste into your executor
2. **Customize**: Modify the examples to fit your specific needs
3. **Combine**: Mix and match features from different examples
4. **Experiment**: Try different layouts and configurations
5. **Learn**: Study the examples to understand MacLib patterns and best practices

## Next Steps

- Review the [Components](components.md) documentation for detailed component information
- Learn about [Section Layouts](sections.md) for advanced layout techniques
- Explore [Window Configuration](window-configuration.md) for customization options
