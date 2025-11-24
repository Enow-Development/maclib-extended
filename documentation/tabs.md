# Tabs

Tabs allow you to organize your UI into different pages or categories. Each tab can contain multiple sections with various components.

## Creating a Tab

```lua
local tab = window:Tab({
    Name = "Main",
    Icon = "rbxassetid://10734950309"
})
```

## Tab Parameters

### Name (string, required)

The display name of the tab shown in the sidebar.

```lua
Name = "Main"
```

- Displayed in the tab switcher button
- Should be short and descriptive
- Supports RichText formatting

### Icon (string, required)

The asset ID for the tab icon.

```lua
Icon = "rbxassetid://10734950309"
```

- Must be a valid Roblox asset ID
- Format: `"rbxassetid://[ID]"`
- Icon is displayed next to the tab name
- Recommended size: 16x16 to 24x24 pixels

## Tab Methods

### Section

Creates a new section within the tab.

```lua
local section = tab:Section({
    Side = "Left"  -- "Left", "Right", or "Full"
})
```

See [Sections](sections.md) for detailed information about section layouts.

### InsertConfigSection

Creates a configuration section with save/load functionality.

```lua
tab:InsertConfigSection("Left")
```

**Parameters:**
- `Side` (string): Position of the config section - `"Left"`, `"Right"`, or `"Full"`

**Features:**
- Automatically adds configuration management UI
- Includes save and load buttons
- Manages configuration files
- Only available in Roblox Studio

## Tab Behavior

### Tab Switching

Tabs can be switched by clicking on the tab button in the sidebar.

**Features:**
- Smooth transition between tabs
- Only one tab visible at a time
- Tab state is preserved when switching
- Visual feedback on hover and selection

### Tab Order

Tabs are displayed in the order they are created.

```lua
local tab1 = window:Tab({ Name = "First", Icon = "rbxassetid://10734950309" })
local tab2 = window:Tab({ Name = "Second", Icon = "rbxassetid://10734950309" })
local tab3 = window:Tab({ Name = "Third", Icon = "rbxassetid://10734950309" })
```

### Default Tab

The first tab created is automatically selected and displayed when the window opens.

## Examples

### Basic Tab

```lua
local mainTab = window:Tab({
    Name = "Main",
    Icon = "rbxassetid://10734950309"
})

local section = mainTab:Section({ Side = "Left" })
section:Button({
    Name = "Click Me",
    Callback = function()
        print("Button clicked!")
    end
})
```

### Multiple Tabs

```lua
-- Main tab
local mainTab = window:Tab({
    Name = "Main",
    Icon = "rbxassetid://10734950309"
})

-- Settings tab
local settingsTab = window:Tab({
    Name = "Settings",
    Icon = "rbxassetid://10734950309"
})

-- Info tab
local infoTab = window:Tab({
    Name = "Info",
    Icon = "rbxassetid://10734950309"
})
```

### Tab with Mixed Sections

```lua
local tab = window:Tab({
    Name = "Dashboard",
    Icon = "rbxassetid://10734950309"
})

-- Full-width header
local headerSection = tab:Section({ Side = "Full" })
headerSection:Label({ Text = "Dashboard Overview" })

-- Two-column layout
local leftSection = tab:Section({ Side = "Left" })
leftSection:Toggle({ Name = "Feature 1", Default = false })

local rightSection = tab:Section({ Side = "Right" })
rightSection:Slider({ Name = "Setting 1", Min = 0, Max = 100 })
```

### Tab with Configuration

```lua
local settingsTab = window:Tab({
    Name = "Settings",
    Icon = "rbxassetid://10734950309"
})

-- Add regular sections
local section = settingsTab:Section({ Side = "Left" })
section:Toggle({
    Name = "Auto Save",
    Default = true,
    Flag = "AutoSave"
})

-- Add configuration section (Roblox Studio only)
settingsTab:InsertConfigSection("Right")
```

## Tab Styling

### Tab Button States

Tab buttons have three visual states:

**Default:**
- Background transparency: 1 (invisible)
- Text transparency: 0.5
- Icon transparency: 0.5

**Hover:**
- Background transparency: 0.98
- Text transparency: 0.3
- Icon transparency: 0.3

**Selected:**
- Background transparency: 0.98
- Text transparency: 0.1
- Icon transparency: 0.1

### Tab Button Layout

- Height: 40 pixels
- Padding: 10px left, 15px right
- Corner radius: 8 pixels
- Border stroke: White with 0.95 transparency
- Icon size: 16x16 pixels
- Text size: 13px Medium font

## Best Practices

### Tab Organization

1. **Logical Grouping**: Group related features in the same tab
2. **Limit Tab Count**: Keep the number of tabs reasonable (3-7 tabs is ideal)
3. **Clear Naming**: Use descriptive, concise names for tabs
4. **Consistent Icons**: Use icons that clearly represent the tab's content

### Tab Naming

- Keep names short (1-2 words)
- Use title case (e.g., "Main", "Settings", "About")
- Avoid abbreviations unless widely understood
- Be consistent with naming conventions

### Icon Selection

- Choose icons that are recognizable and relevant
- Ensure icons are visible at small sizes
- Use consistent icon style across all tabs
- Test icons with both light and dark backgrounds

### Content Organization

- Use sections to organize content within tabs
- Balance content across tabs
- Avoid overcrowding a single tab
- Consider using full-width sections for important content

## Common Tab Layouts

### Main + Settings + Info

```lua
local mainTab = window:Tab({ Name = "Main", Icon = "rbxassetid://10734950309" })
local settingsTab = window:Tab({ Name = "Settings", Icon = "rbxassetid://10734950309" })
local infoTab = window:Tab({ Name = "Info", Icon = "rbxassetid://10734950309" })
```

**Use Case:** General-purpose scripts with main functionality, configuration, and information.

### Feature-Based Tabs

```lua
local combatTab = window:Tab({ Name = "Combat", Icon = "rbxassetid://10734950309" })
local movementTab = window:Tab({ Name = "Movement", Icon = "rbxassetid://10734950309" })
local visualsTab = window:Tab({ Name = "Visuals", Icon = "rbxassetid://10734950309" })
local miscTab = window:Tab({ Name = "Misc", Icon = "rbxassetid://10734950309" })
```

**Use Case:** Game-specific scripts with distinct feature categories.

### Workflow-Based Tabs

```lua
local setupTab = window:Tab({ Name = "Setup", Icon = "rbxassetid://10734950309" })
local executeTab = window:Tab({ Name = "Execute", Icon = "rbxassetid://10734950309" })
local resultsTab = window:Tab({ Name = "Results", Icon = "rbxassetid://10734950309" })
```

**Use Case:** Scripts with a sequential workflow or process.

## Technical Details

### Tab Container Structure

```
tabSwitchers (Frame)
└── tabSwitchersScrollingFrame (ScrollingFrame)
    └── [Tab Buttons]

content (Frame)
└── [Tab Content]
```

### Tab Switching Mechanism

- Only one tab's content is visible at a time
- Switching tabs hides the current tab and shows the selected tab
- Tab content is not destroyed when switching, preserving state
- Smooth transitions using tweening

### Performance Considerations

- Tab content is created once and reused
- Invisible tabs do not consume rendering resources
- Efficient layout recalculation
- Minimal memory overhead per tab
