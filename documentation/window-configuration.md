# Window Configuration

The window is the main container for your MacLib UI. This guide covers all available configuration options.

## Creating a Window

```lua
local window = MacLib:Window({
    Title = "My Script",
    Subtitle = "Version 1.0",
    Size = UDim2.fromOffset(868, 650),
    AcrylicBlur = true,
    DisabledWindowControls = {},
    DragStyle = 1
})
```

## Configuration Parameters

### Title (string, required)

The main title displayed at the top of the sidebar.

```lua
Title = "My Script"
```

- Supports RichText formatting
- Displayed in 18px SemiBold font
- Truncates with ellipsis if too long

### Subtitle (string, required)

The subtitle text displayed below the title.

```lua
Subtitle = "Version 1.0"
```

- Supports RichText formatting
- Displayed in 12px Medium font
- Truncates with ellipsis if too long
- Useful for version numbers, descriptions, or status

### Size (UDim2, optional)

The size of the window.

```lua
Size = UDim2.fromOffset(868, 650)
```

**Default:** `UDim2.fromOffset(868, 650)`

**Recommended Sizes:**
- Small: `UDim2.fromOffset(700, 500)`
- Medium: `UDim2.fromOffset(868, 650)` (default)
- Large: `UDim2.fromOffset(1000, 750)`

### AcrylicBlur (boolean, optional)

Enables or disables the acrylic blur effect on the window background.

```lua
AcrylicBlur = true
```

**Default:** `true`

**When enabled:**
- Window background has a blur effect
- Background transparency is set to 0.05
- Creates a modern, translucent appearance

**When disabled:**
- Window background is solid
- Background transparency is set to 0
- Better performance on lower-end devices

### DisabledWindowControls (table, optional)

An array of window control buttons to disable.

```lua
DisabledWindowControls = {"Exit", "Minimize"}
```

**Default:** `{}` (all controls enabled)

**Available Controls:**
- `"Exit"`: Red button that closes the window
- `"Minimize"`: Yellow button that minimizes the window

**Example - Disable Exit:**
```lua
DisabledWindowControls = {"Exit"}
```

**Example - Disable Both:**
```lua
DisabledWindowControls = {"Exit", "Minimize"}
```

### DragStyle (number, optional)

Determines how the window can be dragged.

```lua
DragStyle = 1
```

**Default:** `1`

**Options:**
- `1`: Icon-based dragging - Window can only be dragged by clicking the move icon in the top-right
- `2`: Full window dragging - Window can be dragged by clicking anywhere on the topbar

## Window Methods

### Tab

Creates a new tab in the window.

```lua
local tab = window:Tab({
    Name = "Main",
    Icon = "rbxassetid://10734950309"
})
```

See [Tabs](tabs.md) for more information.

### Notification

Displays a notification popup.

```lua
window:Notification({
    Title = "Success",
    Description = "Operation completed successfully",
    Duration = 3
})
```

See [Notifications](notifications.md) for more information.

### Unload

Destroys the window and cleans up resources.

```lua
window:Unload()
```

**Note:** After calling Unload, the window object should not be used.

## Window Features

### Resizable Sidebar

The sidebar can be resized by dragging the divider between the sidebar and content area.

**Features:**
- Drag the divider to resize
- Snaps back to default width when close to it
- Minimum and maximum width constraints
- Visual feedback on hover

**Default Width:** Approximately 282px (32.5% of 868px)

### Window Controls

The window includes macOS-style control buttons:

**Exit (Red):**
- Closes and destroys the window
- Can be disabled via `DisabledWindowControls`

**Minimize (Yellow):**
- Minimizes the window to a small icon
- Click again to restore
- Can be disabled via `DisabledWindowControls`

**Maximize (Green):**
- Currently disabled by default
- Reserved for future functionality

### User Information

The sidebar displays the current user's information:
- Avatar thumbnail
- Display name
- Username (with @ prefix)

This information is automatically populated from the LocalPlayer.

### Global Settings

The globe icon in the information section opens global settings (if configured).

## Examples

### Minimal Configuration

```lua
local window = MacLib:Window({
    Title = "Simple Script",
    Subtitle = "v1.0"
})
```

### Full Configuration

```lua
local window = MacLib:Window({
    Title = "Advanced Script",
    Subtitle = "Version 2.0 - Premium Edition",
    Size = UDim2.fromOffset(1000, 750),
    AcrylicBlur = true,
    DisabledWindowControls = {},
    DragStyle = 2
})
```

### Performance-Optimized Configuration

```lua
local window = MacLib:Window({
    Title = "Lightweight Script",
    Subtitle = "Optimized for Performance",
    Size = UDim2.fromOffset(700, 500),
    AcrylicBlur = false,  -- Disable blur for better performance
    DragStyle = 1
})
```

### Locked Window (No Close Button)

```lua
local window = MacLib:Window({
    Title = "Required Setup",
    Subtitle = "Complete setup to continue",
    DisabledWindowControls = {"Exit", "Minimize"}
})
```

## Best Practices

### Title and Subtitle

- Keep titles short and descriptive
- Use subtitles for version numbers or brief descriptions
- Avoid using special characters that might not render correctly
- Consider using RichText for colored or styled text

### Window Size

- Choose a size appropriate for your content
- Consider the target audience's screen resolution
- Test on different screen sizes
- Larger windows provide more space but may not fit on smaller screens

### Acrylic Blur

- Enable for modern, aesthetic appearance
- Disable for better performance on lower-end devices
- Disable if the background is distracting

### Window Controls

- Only disable controls if absolutely necessary
- Provide alternative ways to close the window if Exit is disabled
- Consider user experience when disabling controls

### Drag Style

- Use icon-based dragging (1) for a cleaner look
- Use full window dragging (2) for easier window movement
- Consider user preference and use case
