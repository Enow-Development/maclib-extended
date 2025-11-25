# Window Scaling API Reference

Quick reference for the MacLib window scaling API.

## Configuration Properties

### DefaultScale
- **Type**: `number`
- **Range**: 50-100
- **Default**: `75`
- **Description**: Initial scale percentage when window is created

```lua
DefaultScale = 75
```

### MinScale
- **Type**: `number`
- **Range**: 50-100
- **Default**: `50`
- **Description**: Minimum allowed scale percentage

```lua
MinScale = 50
```

### MaxScale
- **Type**: `number`
- **Range**: 50-100
- **Default**: `100`
- **Description**: Maximum allowed scale percentage (also limited by screen size)

```lua
MaxScale = 100
```

## Callback Functions

### onScaleStart
- **Type**: `function`
- **Parameters**: `oldScale` (number)
- **Description**: Called when scaling operation begins

```lua
onScaleStart = function(oldScale)
    print("Starting from " .. oldScale .. "%")
end
```

### onScale
- **Type**: `function`
- **Parameters**: `newScale` (number), `newSize` (UDim2)
- **Description**: Called during scaling animation
- **Warning**: Fires frequently - avoid heavy operations

```lua
onScale = function(newScale, newSize)
    print("Scaling to " .. newScale .. "%")
end
```

### onScaleEnd
- **Type**: `function`
- **Parameters**: `finalScale` (number), `finalSize` (UDim2)
- **Description**: Called when scaling operation completes

```lua
onScaleEnd = function(finalScale, finalSize)
    print("Finished at " .. finalScale .. "%")
end
```

## Window Methods

### SetScale(percentage)
Sets the window scale to a specific percentage.

**Parameters:**
- `percentage` (number): Scale percentage (50-100)

**Returns:** None

**Behavior:**
- Clamps to MinScale/MaxScale bounds
- Clamps to 50-100 range
- Animates over 0.3 seconds
- Maintains aspect ratio
- Triggers callbacks
- Prevents concurrent operations

```lua
window:SetScale(75)
```

### GetScale()
Returns the current scale percentage.

**Parameters:** None

**Returns:** `number` - Current scale percentage (50-100)

```lua
local scale = window:GetScale()
print("Current scale: " .. scale .. "%")
```

### ResetScale()
Resets window to default scale.

**Parameters:** None

**Returns:** None

**Behavior:**
- Animates to DefaultScale (or 75% if not specified)
- Uses 0.3 second animation
- Triggers callbacks
- Safe to call when already at default

```lua
window:ResetScale()
```

### GetScreenSize()
Returns detected screen size.

**Parameters:** None

**Returns:** `Vector2` - Screen size (X = width, Y = height)

**Notes:**
- Auto-updates on rotation/resize
- Detected from viewport/camera

```lua
local screenSize = window:GetScreenSize()
print("Screen: " .. screenSize.X .. "x" .. screenSize.Y)
```

### GetCurrentSize()
Returns current window size.

**Parameters:** None

**Returns:** `UDim2` - Current window size

```lua
local size = window:GetCurrentSize()
print("Size: " .. size.X.Offset .. "x" .. size.Y.Offset)
```

## Complete Example

```lua
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Enow-Development/maclib-extended/refs/heads/main/maclib.lua"))()

-- Create window with all scale options
local window = MacLib:Window({
    Title = "Scale Demo",
    Subtitle = "API Reference Example",
    
    -- Scale configuration
    DefaultScale = 75,
    MinScale = 50,
    MaxScale = 100,
    
    -- Scale callbacks
    onScaleStart = function(oldScale)
        print("Starting from " .. oldScale .. "%")
    end,
    
    onScale = function(newScale, newSize)
        print("Scaling to " .. newScale .. "%")
    end,
    
    onScaleEnd = function(finalScale, finalSize)
        print("Finished at " .. finalScale .. "%")
        print("Size: " .. finalSize.X.Offset .. "x" .. finalSize.Y.Offset)
    end
})

-- Use scale methods
window:SetScale(80)              -- Set to 80%
local scale = window:GetScale()  -- Get current scale
window:ResetScale()              -- Reset to default
local screenSize = window:GetScreenSize()  -- Get screen size
local size = window:GetCurrentSize()       -- Get window size
```

## Constants

### Base Size
- **Width**: 868 pixels
- **Height**: 650 pixels
- **Aspect Ratio**: 868:650 (≈1.335:1)

### Scale Range
- **Minimum**: 50% (434x325 pixels)
- **Maximum**: 100% (868x650 pixels)
- **Default**: 75% (651x487.5 pixels)

### Animation
- **Duration**: 0.3 seconds
- **Easing**: Quad
- **FPS Target**: 60 FPS

## Scale Percentage to Size Conversion

| Scale | Width | Height |
|-------|-------|--------|
| 50%   | 434   | 325    |
| 55%   | 477.4 | 357.5  |
| 60%   | 520.8 | 390    |
| 65%   | 564.2 | 422.5  |
| 70%   | 607.6 | 455    |
| 75%   | 651   | 487.5  |
| 80%   | 694.4 | 520    |
| 85%   | 737.8 | 552.5  |
| 90%   | 781.2 | 585    |
| 95%   | 824.6 | 617.5  |
| 100%  | 868   | 650    |

## Error Handling

All callbacks are wrapped in `pcall`:
- Errors in callbacks won't crash the window
- Errors are logged to console
- Scaling operation continues even if callback fails

## Performance Notes

- **SetScale**: < 1ms calculation time
- **Element Updates**: < 10ms for all elements
- **Animation**: 60 FPS target (16ms per frame)
- **Callbacks**: < 5ms recommended execution time

## Common Patterns

### Slider Control
```lua
section:Slider({
    Name = "Window Scale",
    Min = 50,
    Max = 100,
    Default = 75,
    Callback = function(value)
        window:SetScale(value)
    end
})
```

### Preset Buttons
```lua
section:Button({
    Name = "Small (50%)",
    Callback = function() window:SetScale(50) end
})

section:Button({
    Name = "Medium (75%)",
    Callback = function() window:SetScale(75) end
})

section:Button({
    Name = "Large (100%)",
    Callback = function() window:SetScale(100) end
})
```

### Save Preference
```lua
local window = MacLib:Window({
    Title = "My Window",
    DefaultScale = _G.SavedScale or 75,
    
    onScaleEnd = function(finalScale, finalSize)
        _G.SavedScale = finalScale
    end
})
```

### Mobile Detection
```lua
local UserInputService = game:GetService("UserInputService")
local isMobile = UserInputService.TouchEnabled and not UserInputService.KeyboardEnabled

local window = MacLib:Window({
    Title = "Adaptive Window",
    DefaultScale = isMobile and 60 or 75,
    MaxScale = isMobile and 80 or 100,
    AcrylicBlur = not isMobile
})
```

### Display Current Scale
```lua
section:Button({
    Name = "Show Info",
    Callback = function()
        local scale = window:GetScale()
        local size = window:GetCurrentSize()
        local screenSize = window:GetScreenSize()
        
        print("Scale: " .. scale .. "%")
        print("Size: " .. size.X.Offset .. "x" .. size.Y.Offset)
        print("Screen: " .. screenSize.X .. "x" .. screenSize.Y)
    end
})
```

## See Also

- [Window Scaling Guide](scaling.md) - Complete scaling documentation
- [Window Configuration](window-configuration.md) - All window settings
- [Getting Started](getting-started.md) - Basic MacLib usage
- [Examples](examples.md) - More code examples
