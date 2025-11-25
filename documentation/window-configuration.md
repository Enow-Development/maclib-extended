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

**Note:** When using the scaling feature (DefaultScale, MinScale, MaxScale), the Size parameter is deprecated. Use DefaultScale instead to set the initial window size as a percentage.

### DefaultScale (number, optional)

The default scale percentage for the window (50-100).

```lua
DefaultScale = 75
```

**Default:** `75` (75% of base size 868x650)

**Valid Range:** 50-100

The window will start at this scale percentage. The base size is always 868x650, and the scale determines what percentage of this size to use. The aspect ratio (868:650) is always maintained.

**Examples:**
- `DefaultScale = 50`: Window starts at 434x325 (50% of base)
- `DefaultScale = 75`: Window starts at 651x487.5 (75% of base, default)
- `DefaultScale = 100`: Window starts at 868x650 (100% of base)

### MinScale (number, optional)

The minimum scale percentage allowed (50-100).

```lua
MinScale = 50
```

**Default:** `50`

**Valid Range:** 50-100

Prevents the window from being scaled below this percentage. The minimum is clamped at 50% to ensure usability on mobile devices.

### MaxScale (number, optional)

The maximum scale percentage allowed (50-100).

```lua
MaxScale = 100
```

**Default:** `100`

**Valid Range:** 50-100

Prevents the window from being scaled above this percentage. The maximum is also constrained by screen size - the window cannot exceed 100% of the screen dimensions.

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

### Scale Callbacks

Callbacks that are triggered during window scaling operations.

#### onScaleStart (function, optional)

Called when a scale operation begins.

```lua
onScaleStart = function(oldScale)
    print("Starting scale from " .. oldScale .. "%")
end
```

**Parameters:**
- `oldScale` (number): The scale percentage before the operation started

#### onScale (function, optional)

Called during the scale operation (during animation).

```lua
onScale = function(newScale, newSize)
    print("Scaling to " .. newScale .. "%")
    print("New size: " .. newSize.X.Offset .. "x" .. newSize.Y.Offset)
end
```

**Parameters:**
- `newScale` (number): The current scale percentage during animation
- `newSize` (UDim2): The current window size during animation

#### onScaleEnd (function, optional)

Called when a scale operation completes.

```lua
onScaleEnd = function(finalScale, finalSize)
    print("Scale complete at " .. finalScale .. "%")
    print("Final size: " .. finalSize.X.Offset .. "x" .. finalSize.Y.Offset)
end
```

**Parameters:**
- `finalScale` (number): The final scale percentage after the operation
- `finalSize` (UDim2): The final window size after the operation

**Error Handling:**
All callbacks are wrapped in pcall. If a callback throws an error, the scaling operation will continue without crashing.

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

### SetScale

Sets the window scale to a specific percentage.

```lua
window:SetScale(75)
```

**Parameters:**
- `percentage` (number): The scale percentage (50-100)

**Behavior:**
- Values below MinScale are clamped to MinScale
- Values above MaxScale are clamped to MaxScale
- Values above 100 are clamped to 100
- Values below 50 are clamped to 50
- The window animates smoothly to the new size (0.3 seconds)
- Aspect ratio (868:650) is always maintained
- Triggers onScaleStart, onScale, and onScaleEnd callbacks

**Example:**
```lua
-- Set to 80%
window:SetScale(80)

-- Set to minimum
window:SetScale(50)

-- Set to maximum
window:SetScale(100)
```

### GetScale

Returns the current scale percentage.

```lua
local scale = window:GetScale()
```

**Returns:**
- `number`: The current scale percentage (50-100)

**Example:**
```lua
local currentScale = window:GetScale()
print("Current scale: " .. currentScale .. "%")
```

### GetScreenSize

Returns the detected screen size.

```lua
local screenSize = window:GetScreenSize()
```

**Returns:**
- `Vector2`: The screen size with X (width) and Y (height) components

**Example:**
```lua
local screenSize = window:GetScreenSize()
print("Screen: " .. screenSize.X .. "x" .. screenSize.Y)
```

**Note:** Screen size is automatically detected and updated when the viewport changes (e.g., device rotation, window resize).

### ResetScale

Resets the window to its default scale.

```lua
window:ResetScale()
```

**Behavior:**
- Animates the window back to DefaultScale (or 75% if not specified)
- Uses smooth animation (0.3 seconds)
- Triggers scale callbacks
- Safe to call even if already at default scale

**Example:**
```lua
-- Reset to default scale
window:ResetScale()
```

### GetCurrentSize

Returns the current window size.

```lua
local size = window:GetCurrentSize()
```

**Returns:**
- `UDim2`: The current window size

**Example:**
```lua
local size = window:GetCurrentSize()
print("Size: " .. size.X.Offset .. "x" .. size.Y.Offset)
```

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

### Scalable Window Configuration

```lua
local window = MacLib:Window({
    Title = "Scalable Window",
    Subtitle = "Resize me!",
    DefaultScale = 75,
    MinScale = 50,
    MaxScale = 100,
    
    onScaleStart = function(oldScale)
        print("Starting scale from " .. oldScale .. "%")
    end,
    
    onScale = function(newScale, newSize)
        print("Scaling to " .. newScale .. "%")
    end,
    
    onScaleEnd = function(finalScale, finalSize)
        print("Scale complete at " .. finalScale .. "%")
    end
})

-- Add scale controls
local tab = window:Tab({
    Name = "Controls",
    Icon = "rbxassetid://10734950309"
})

local section = tab:Section({ Side = "Left" })

section:Slider({
    Name = "Window Scale",
    Min = 50,
    Max = 100,
    Default = 75,
    Callback = function(value)
        window:SetScale(value)
    end
})

section:Button({
    Name = "Reset Scale",
    Callback = function()
        window:ResetScale()
    end
})
```

### Mobile-Optimized Configuration

```lua
local window = MacLib:Window({
    Title = "Mobile Friendly",
    Subtitle = "Optimized for small screens",
    DefaultScale = 60,  -- Start smaller on mobile
    MinScale = 50,
    MaxScale = 80,      -- Don't allow too large on mobile
    AcrylicBlur = false,  -- Better performance
    DragStyle = 2       -- Easier to drag on mobile
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

### Window Size and Scaling

- Use DefaultScale instead of Size for scalable windows
- Set DefaultScale to 75% for a good balance on most screens
- For mobile, consider starting at 60-70% scale
- Test on different screen sizes and orientations
- Provide UI controls (slider/buttons) for users to adjust scale
- Use MinScale of 50% to ensure usability on small screens
- Don't set MaxScale above 100% - it's constrained by screen size anyway

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
- Use full window dragging (2) for easier window movement on mobile
- Consider user preference and use case

### Scale Callbacks

- Use callbacks sparingly - avoid heavy operations
- Don't perform expensive calculations in onScale (called during animation)
- Use onScaleEnd for operations that should run after scaling completes
- Always handle errors in callbacks - they're wrapped in pcall but good practice

### Mobile Considerations

- Start with smaller DefaultScale (60-70%) on mobile devices
- Use lower MaxScale (80-90%) to prevent windows too large for mobile screens
- Disable AcrylicBlur for better performance on mobile
- Use DragStyle = 2 for easier dragging on touch screens
- Test on both portrait and landscape orientations
- Consider that screen size changes on rotation

### Performance

- Avoid calling SetScale repeatedly in quick succession
- The system debounces rapid scale changes automatically
- Use callbacks efficiently - onScale fires during animation
- Disable AcrylicBlur on lower-end devices
- Consider the number of UI elements - more elements = slower scaling

## Troubleshooting

### Scaling Issues

**Window doesn't scale when calling SetScale:**
- Verify you're passing a number, not a string
- Check that the value is within MinScale and MaxScale bounds
- Look for warning messages in the console
- Ensure the window hasn't been unloaded

**Scale is clamped unexpectedly:**
- Your screen size may be limiting the maximum scale
- Call `window:GetScreenSize()` to check available space
- On mobile, screen size is often smaller than expected
- Try reducing MaxScale to a value that fits your screen

**Window is too small/large on mobile:**
- Adjust DefaultScale for mobile devices (60-70% recommended)
- Set appropriate MinScale and MaxScale values
- Test on actual mobile devices, not just desktop
- Consider device orientation (portrait vs landscape)

**Scaling animation is choppy:**
- This is normal on lower-end devices
- Animation duration is fixed at 0.3 seconds
- Disable AcrylicBlur for better performance
- Reduce the number of UI elements if possible

**Window doesn't fit on screen:**
- The maximum scale is automatically limited by screen size
- Call `window:GetScreenSize()` to see available space
- Reduce MaxScale or DefaultScale
- Test on the target device's actual screen size

### Callback Issues

**Callbacks not firing:**
- Ensure callbacks are functions, not nil or other types
- Check for syntax errors in your callback code
- Callbacks are wrapped in pcall - check console for errors
- Verify you're actually triggering a scale change

**Errors in callbacks crash the window:**
- This shouldn't happen - callbacks are wrapped in pcall
- If it does, there may be an error in the scaling system
- Check console for error messages
- Try simplifying your callback code

**onScale fires too frequently:**
- This is normal - it fires during the animation
- Use onScaleEnd instead for operations after scaling completes
- Don't perform heavy operations in onScale

### Screen Size Detection Issues

**Screen size is incorrect:**
- Screen size is detected from the viewport/camera
- On some executors, this may not work correctly
- Try calling `window:GetScreenSize()` to verify
- Screen size updates automatically on rotation/resize

**Screen size doesn't update on rotation:**
- The system should detect this automatically
- If it doesn't, there may be an executor limitation
- Try calling `window:SetScale()` again after rotation

### General Issues

**Window appears at wrong position after scaling:**
- This is normal - the window maintains its center position
- The window may move slightly to stay on screen
- This is intentional behavior to prevent off-screen windows

**UI elements don't scale proportionally:**
- All elements should scale proportionally automatically
- If they don't, this may be a bug
- Check that you're using standard MacLib components
- Custom elements may need manual scaling logic

**Performance degrades over time:**
- This shouldn't happen with normal usage
- Try reducing the number of scale operations
- Check for memory leaks in your callback code
- Consider restarting the script

### Getting Help

If you encounter issues not covered here:

1. Check the console for error messages
2. Verify your configuration is correct
3. Test with a minimal example
4. Check the examples folder for working code
5. Report bugs with reproduction steps
