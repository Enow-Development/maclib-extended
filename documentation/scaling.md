# Window Scaling

MacLib supports dynamic window scaling, allowing windows to be resized from 50% to 100% of the base size (868x650). All UI elements automatically adjust to maintain proportions and functionality.

## Overview

The scaling feature provides:

- **Percentage-based scaling**: Scale from 50% to 100% of base size
- **Automatic responsive design**: All UI elements scale proportionally
- **Smooth animations**: Transitions animate smoothly over 0.3 seconds
- **Screen size detection**: Automatically detects and adapts to screen size
- **Mobile optimization**: Works great on mobile devices with touch support
- **Callback system**: React to scale changes with custom callbacks
- **Aspect ratio preservation**: Always maintains 868:650 aspect ratio

## Quick Start

### Basic Scalable Window

```lua
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Enow-Development/maclib-extended/refs/heads/main/maclib.lua"))()

local window = MacLib:Window({
    Title = "Scalable Window",
    Subtitle = "Resize me!",
    DefaultScale = 75,  -- Start at 75% (default)
    MinScale = 50,      -- Minimum 50%
    MaxScale = 100      -- Maximum 100%
})

-- Change scale programmatically
window:SetScale(80)

-- Get current scale
local scale = window:GetScale()
print("Current scale: " .. scale .. "%")

-- Reset to default
window:ResetScale()
```

### Adding Scale Controls

```lua
local tab = window:Tab({
    Name = "Controls",
    Icon = "rbxassetid://10734950309"
})

local section = tab:Section({ Side = "Left" })

-- Slider for smooth scale adjustment
section:Slider({
    Name = "Window Scale",
    Min = 50,
    Max = 100,
    Default = 75,
    Callback = function(value)
        window:SetScale(value)
    end
})

-- Preset buttons
section:Button({
    Name = "Small (50%)",
    Callback = function()
        window:SetScale(50)
    end
})

section:Button({
    Name = "Medium (75%)",
    Callback = function()
        window:SetScale(75)
    end
})

section:Button({
    Name = "Large (100%)",
    Callback = function()
        window:SetScale(100)
    end
})

section:Button({
    Name = "Reset to Default",
    Callback = function()
        window:ResetScale()
    end
})
```

## Configuration

### Scale Settings

#### DefaultScale (number, optional)

The initial scale percentage when the window is created.

```lua
DefaultScale = 75
```

- **Default**: `75` (75% of base size)
- **Range**: 50-100
- **Base size**: 868x650 pixels
- **Example**: `DefaultScale = 60` creates a window at 520x390

#### MinScale (number, optional)

The minimum allowed scale percentage.

```lua
MinScale = 50
```

- **Default**: `50`
- **Range**: 50-100
- **Purpose**: Prevents window from becoming too small to use
- **Mobile**: 50% ensures usability on small screens

#### MaxScale (number, optional)

The maximum allowed scale percentage.

```lua
MaxScale = 100
```

- **Default**: `100`
- **Range**: 50-100
- **Constraint**: Also limited by screen size
- **Mobile**: Consider using 80-90% on mobile devices

### Scale Callbacks

#### onScaleStart

Called when a scale operation begins.

```lua
onScaleStart = function(oldScale)
    print("Starting scale from " .. oldScale .. "%")
end
```

**Parameters**:
- `oldScale` (number): The scale percentage before the operation

**Use cases**:
- Logging scale changes
- Preparing UI for scale change
- Disabling certain features during scaling

#### onScale

Called during the scale animation.

```lua
onScale = function(newScale, newSize)
    print("Scaling to " .. newScale .. "%")
    print("Size: " .. newSize.X.Offset .. "x" .. newSize.Y.Offset)
end
```

**Parameters**:
- `newScale` (number): Current scale percentage during animation
- `newSize` (UDim2): Current window size during animation

**Use cases**:
- Real-time scale display
- Updating external UI elements
- Visual feedback during scaling

**Warning**: This fires frequently during animation. Avoid heavy operations.

#### onScaleEnd

Called when a scale operation completes.

```lua
onScaleEnd = function(finalScale, finalSize)
    print("Scale complete at " .. finalScale .. "%")
    print("Final size: " .. finalSize.X.Offset .. "x" .. finalSize.Y.Offset)
end
```

**Parameters**:
- `finalScale` (number): Final scale percentage after operation
- `finalSize` (UDim2): Final window size after operation

**Use cases**:
- Saving user preferences
- Updating external systems
- Re-enabling features after scaling
- Performing expensive calculations

## Methods

### SetScale(percentage)

Sets the window scale to a specific percentage.

```lua
window:SetScale(75)
```

**Parameters**:
- `percentage` (number): Scale percentage (50-100)

**Behavior**:
- Values are clamped to MinScale and MaxScale
- Values below 50 are clamped to 50
- Values above 100 are clamped to 100
- Animates smoothly over 0.3 seconds
- Maintains aspect ratio (868:650)
- Triggers scale callbacks
- Prevents concurrent scaling operations

**Examples**:
```lua
-- Set to 80%
window:SetScale(80)

-- Set to minimum (clamped to MinScale)
window:SetScale(0)

-- Set to maximum (clamped to MaxScale or 100)
window:SetScale(200)
```

### GetScale()

Returns the current scale percentage.

```lua
local scale = window:GetScale()
```

**Returns**:
- `number`: Current scale percentage (50-100)

**Example**:
```lua
local currentScale = window:GetScale()
print("Window is at " .. currentScale .. "% scale")

if currentScale < 60 then
    print("Window is quite small!")
end
```

### ResetScale()

Resets the window to its default scale.

```lua
window:ResetScale()
```

**Behavior**:
- Animates to DefaultScale (or 75% if not specified)
- Uses smooth animation (0.3 seconds)
- Triggers scale callbacks
- Safe to call even if already at default scale

**Example**:
```lua
-- User can customize scale
window:SetScale(90)

-- Later, reset to default
window:ResetScale()  -- Returns to DefaultScale
```

### GetScreenSize()

Returns the detected screen size.

```lua
local screenSize = window:GetScreenSize()
```

**Returns**:
- `Vector2`: Screen size with X (width) and Y (height)

**Example**:
```lua
local screenSize = window:GetScreenSize()
print("Screen: " .. screenSize.X .. "x" .. screenSize.Y)

-- Calculate maximum possible window size
local maxWidth = screenSize.X
local maxHeight = screenSize.Y
print("Max window: " .. maxWidth .. "x" .. maxHeight)
```

**Notes**:
- Automatically updates on screen rotation/resize
- Detected from viewport/camera
- Some executors may report incorrect values

### GetCurrentSize()

Returns the current window size.

```lua
local size = window:GetCurrentSize()
```

**Returns**:
- `UDim2`: Current window size

**Example**:
```lua
local size = window:GetCurrentSize()
print("Window size: " .. size.X.Offset .. "x" .. size.Y.Offset)

-- Calculate scale from size
local baseWidth = 868
local currentWidth = size.X.Offset
local calculatedScale = (currentWidth / baseWidth) * 100
print("Calculated scale: " .. calculatedScale .. "%")
```

## Mobile Optimization

### Recommended Mobile Configuration

```lua
local UserInputService = game:GetService("UserInputService")
local isMobile = UserInputService.TouchEnabled and not UserInputService.KeyboardEnabled

local window = MacLib:Window({
    Title = "Mobile Friendly",
    Subtitle = "Optimized for touch",
    
    -- Smaller default on mobile
    DefaultScale = isMobile and 60 or 75,
    
    -- Prevent too large on mobile
    MaxScale = isMobile and 80 or 100,
    
    -- Better performance on mobile
    AcrylicBlur = not isMobile,
    
    -- Easier dragging on touch screens
    DragStyle = isMobile and 2 or 1
})
```

### Mobile Best Practices

1. **Start Smaller**: Use 60-70% DefaultScale on mobile
2. **Limit Maximum**: Set MaxScale to 80-90% on mobile
3. **Disable Blur**: Better performance without AcrylicBlur
4. **Full Drag**: Use DragStyle = 2 for easier touch dragging
5. **Test Both Orientations**: Portrait and landscape
6. **Larger Touch Targets**: Ensure buttons are usable at minimum scale
7. **Auto-Rotation**: System handles rotation automatically

### Orientation Handling

```lua
local window = MacLib:Window({
    Title = "Rotation Aware",
    Subtitle = "Adapts to orientation",
    
    onScaleEnd = function(finalScale, finalSize)
        local screenSize = window:GetScreenSize()
        local isPortrait = screenSize.Y > screenSize.X
        
        if isPortrait then
            print("Portrait mode")
            -- Maybe adjust UI for portrait
        else
            print("Landscape mode")
            -- Maybe adjust UI for landscape
        end
    end
})
```

## Performance

### Optimization Features

The scaling system includes several performance optimizations:

1. **Debouncing**: Rapid scale changes are automatically debounced
2. **Batch Updates**: Multiple elements update in a single frame
3. **Lazy Calculation**: Only recalculates when necessary
4. **Selective Updates**: Only updates visible elements
5. **Efficient Animation**: Uses TweenService for smooth 60 FPS

### Performance Tips

```lua
-- ✓ Good: Single scale change
window:SetScale(80)

-- ✗ Bad: Rapid scale changes
for i = 50, 100 do
    window:SetScale(i)  -- Don't do this!
end

-- ✓ Good: Use slider (automatically debounced)
section:Slider({
    Name = "Scale",
    Min = 50,
    Max = 100,
    Callback = function(value)
        window:SetScale(value)
    end
})

-- ✓ Good: Lightweight callback
onScaleEnd = function(finalScale, finalSize)
    -- Save preference
    saveScale(finalScale)
end

-- ✗ Bad: Heavy operation in onScale
onScale = function(newScale, newSize)
    -- This fires many times during animation!
    for i = 1, 1000000 do
        -- Heavy calculation
    end
end
```

### Performance Targets

- Scale calculation: < 1ms
- Element updates: < 10ms total
- Animation: 60 FPS (16ms per frame)
- Callback execution: < 5ms

## Examples

### Complete Scale Control Panel

```lua
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Enow-Development/maclib-extended/refs/heads/main/maclib.lua"))()

local window = MacLib:Window({
    Title = "Scale Demo",
    Subtitle = "Full control example",
    DefaultScale = 75,
    MinScale = 50,
    MaxScale = 100,
    
    onScaleStart = function(oldScale)
        print("Starting from " .. oldScale .. "%")
    end,
    
    onScaleEnd = function(finalScale, finalSize)
        print("Finished at " .. finalScale .. "%")
    end
})

local tab = window:Tab({
    Name = "Scale Controls",
    Icon = "rbxassetid://10734950309"
})

local leftSection = tab:Section({ Side = "Left" })
local rightSection = tab:Section({ Side = "Right" })

-- Slider control
leftSection:Slider({
    Name = "Window Scale",
    Min = 50,
    Max = 100,
    Default = 75,
    Callback = function(value)
        window:SetScale(value)
    end
})

-- Preset buttons
leftSection:Button({
    Name = "Tiny (50%)",
    Callback = function() window:SetScale(50) end
})

leftSection:Button({
    Name = "Small (60%)",
    Callback = function() window:SetScale(60) end
})

leftSection:Button({
    Name = "Medium (75%)",
    Callback = function() window:SetScale(75) end
})

leftSection:Button({
    Name = "Large (90%)",
    Callback = function() window:SetScale(90) end
})

leftSection:Button({
    Name = "Maximum (100%)",
    Callback = function() window:SetScale(100) end
})

-- Info display
rightSection:Button({
    Name = "Show Current Scale",
    Callback = function()
        local scale = window:GetScale()
        local size = window:GetCurrentSize()
        local screenSize = window:GetScreenSize()
        
        print("=== Window Info ===")
        print("Scale: " .. scale .. "%")
        print("Size: " .. size.X.Offset .. "x" .. size.Y.Offset)
        print("Screen: " .. screenSize.X .. "x" .. screenSize.Y)
    end
})

rightSection:Button({
    Name = "Reset to Default",
    Callback = function()
        window:ResetScale()
    end
})
```

### Adaptive Scaling Based on Content

```lua
local window = MacLib:Window({
    Title = "Adaptive Window",
    Subtitle = "Scales based on content",
    DefaultScale = 75
})

local tab = window:Tab({
    Name = "Content",
    Icon = "rbxassetid://10734950309"
})

local section = tab:Section({ Side = "Full" })

-- Toggle that adjusts window size
section:Toggle({
    Name = "Show Detailed View",
    Default = false,
    Callback = function(enabled)
        if enabled then
            -- Scale up for detailed view
            window:SetScale(90)
        else
            -- Scale down for compact view
            window:SetScale(60)
        end
    end
})
```

### Save User Preference

```lua
-- Simple preference saving (using _G for demo)
_G.SavedScale = _G.SavedScale or 75

local window = MacLib:Window({
    Title = "Persistent Scale",
    Subtitle = "Remembers your preference",
    DefaultScale = _G.SavedScale,
    
    onScaleEnd = function(finalScale, finalSize)
        -- Save preference
        _G.SavedScale = finalScale
        print("Saved scale preference: " .. finalScale .. "%")
    end
})
```

## Troubleshooting

### Window doesn't scale

**Symptoms**: Calling SetScale has no effect

**Solutions**:
- Verify you're passing a number, not a string
- Check MinScale and MaxScale bounds
- Look for console warnings
- Ensure window isn't unloaded
- Try GetScale() to see current value

### Scale is clamped

**Symptoms**: Scale stops at a value lower than expected

**Solutions**:
- Screen size may be limiting maximum
- Call GetScreenSize() to check available space
- Mobile screens are often smaller than expected
- Reduce MaxScale to fit screen
- Test on actual target device

### Choppy animation

**Symptoms**: Scaling animation is not smooth

**Solutions**:
- Normal on lower-end devices
- Disable AcrylicBlur for better performance
- Reduce number of UI elements
- Test on better hardware
- Some executors have performance limitations

### Callbacks not firing

**Symptoms**: Scale callbacks don't execute

**Solutions**:
- Ensure callbacks are functions, not nil
- Check for syntax errors in callback code
- Check console for errors (callbacks use pcall)
- Verify scale is actually changing
- Try simple callback like print()

### Window too small/large on mobile

**Symptoms**: Window doesn't fit mobile screen well

**Solutions**:
- Adjust DefaultScale (60-70% for mobile)
- Set appropriate MinScale and MaxScale
- Test on actual mobile devices
- Consider orientation (portrait vs landscape)
- Use device detection for different defaults

## Technical Details

### Aspect Ratio

Windows always maintain an aspect ratio of **868:650** (≈1.335:1).

### Base Size

The base size is **868x650 pixels**. Scale percentages are relative to this:

| Scale | Width | Height |
|-------|-------|--------|
| 50%   | 434   | 325    |
| 60%   | 520.8 | 390    |
| 75%   | 651   | 487.5  |
| 90%   | 781.2 | 585    |
| 100%  | 868   | 650    |

### Animation

- **Duration**: 0.3 seconds (fixed)
- **Easing**: Quad (smooth acceleration/deceleration)
- **Service**: TweenService
- **FPS Target**: 60 FPS
- **Concurrent**: Prevented (new calls ignored during animation)

### Screen Size Detection

- **Method**: Viewport/Camera size
- **Updates**: Automatic on rotation/resize
- **Fallback**: Reasonable defaults if detection fails
- **Frequency**: Checked on each scale operation

### Responsive Elements

All UI elements scale proportionally:

- **Sidebar**: Width ratio maintained
- **Content**: Fills remaining space
- **Fonts**: Scale with window
- **Spacing**: Padding/margins scale proportionally
- **Sections**: All types (Left, Right, Full) scale correctly
- **Components**: Buttons, sliders, toggles, etc. all scale

## See Also

- [Window Configuration](window-configuration.md) - Complete window settings
- [Getting Started](getting-started.md) - Basic MacLib usage
- [Examples](examples.md) - More code examples
- `examples/scale_*.lua` - Scale example files
- `SCALING_USAGE_GUIDE.md` - Additional scaling information
