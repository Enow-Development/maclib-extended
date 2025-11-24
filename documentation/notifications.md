# Notifications

MacLib includes a built-in notification system for displaying temporary messages to users.

## Creating a Notification

```lua
window:Notification({
    Title = "Success",
    Description = "Operation completed successfully",
    Duration = 3
})
```

## Parameters

### Title (string, required)

The main title of the notification.

```lua
Title = "Success"
```

- Displayed in bold, larger text
- Should be short and descriptive
- Supports RichText formatting

### Description (string, required)

The detailed message of the notification.

```lua
Description = "Operation completed successfully"
```

- Displayed below the title
- Can be longer and more detailed
- Supports RichText formatting
- Automatically wraps to multiple lines if needed

### Duration (number, optional)

How long the notification should be displayed, in seconds.

```lua
Duration = 3
```

**Default:** `3` seconds

**Recommended Values:**
- Short messages: 2-3 seconds
- Medium messages: 3-5 seconds
- Long messages: 5-7 seconds
- Important messages: 7-10 seconds

## Notification Behavior

### Display

- Notifications appear in the bottom-right corner of the screen
- Multiple notifications stack vertically
- Newer notifications appear at the bottom
- Smooth fade-in animation

### Dismissal

- Notifications automatically dismiss after the specified duration
- Fade-out animation when dismissing
- Clicking a notification dismisses it immediately
- Dismissed notifications slide out smoothly

### Stacking

- Multiple notifications can be displayed simultaneously
- Notifications are spaced 10 pixels apart
- Older notifications move up as new ones appear
- Maximum of 5 notifications visible at once (older ones are dismissed)

## Examples

### Success Notification

```lua
window:Notification({
    Title = "Success",
    Description = "Settings saved successfully",
    Duration = 3
})
```

### Error Notification

```lua
window:Notification({
    Title = "Error",
    Description = "Failed to connect to server",
    Duration = 5
})
```

### Warning Notification

```lua
window:Notification({
    Title = "Warning",
    Description = "This action cannot be undone",
    Duration = 4
})
```

### Info Notification

```lua
window:Notification({
    Title = "Info",
    Description = "New update available",
    Duration = 3
})
```

### Long Message Notification

```lua
window:Notification({
    Title = "Important",
    Description = "Please read the terms and conditions carefully before proceeding with this action",
    Duration = 7
})
```

### RichText Notification

```lua
window:Notification({
    Title = "<b>Achievement Unlocked</b>",
    Description = "You have earned <font color='rgb(255,215,0)'>1000 coins</font>!",
    Duration = 4
})
```

## Use Cases

### Action Confirmation

```lua
section:Button({
    Name = "Save Settings",
    Callback = function()
        -- Save settings logic here
        window:Notification({
            Title = "Saved",
            Description = "Your settings have been saved",
            Duration = 2
        })
    end
})
```

### Error Handling

```lua
section:Button({
    Name = "Connect",
    Callback = function()
        local success, error = pcall(function()
            -- Connection logic here
        end)
        
        if success then
            window:Notification({
                Title = "Connected",
                Description = "Successfully connected to server",
                Duration = 3
            })
        else
            window:Notification({
                Title = "Connection Failed",
                Description = tostring(error),
                Duration = 5
            })
        end
    end
})
```

### Progress Updates

```lua
section:Button({
    Name = "Start Process",
    Callback = function()
        window:Notification({
            Title = "Started",
            Description = "Process has been started",
            Duration = 2
        })
        
        task.wait(5)
        
        window:Notification({
            Title = "Complete",
            Description = "Process completed successfully",
            Duration = 3
        })
    end
})
```

### User Guidance

```lua
section:Toggle({
    Name = "Advanced Mode",
    Default = false,
    Callback = function(value)
        if value then
            window:Notification({
                Title = "Advanced Mode",
                Description = "Advanced features are now available",
                Duration = 4
            })
        end
    end
})
```

## Best Practices

### Title

- Keep titles short (1-3 words)
- Use clear, action-oriented language
- Consider using standard titles: "Success", "Error", "Warning", "Info"
- Use title case

### Description

- Be specific and informative
- Explain what happened and why
- Keep it concise but complete
- Use proper grammar and punctuation

### Duration

- Match duration to message length
- Longer messages need more time to read
- Important messages should stay longer
- Don't make durations too long (max 10 seconds)

### Frequency

- Don't spam notifications
- Combine related notifications when possible
- Consider debouncing frequent events
- Respect user attention

### Content

- Use positive language for success messages
- Be clear and direct for errors
- Provide actionable information when possible
- Avoid technical jargon

## Styling

### Notification Appearance

- Background: Semi-transparent dark background
- Border: Subtle white stroke
- Corner radius: 8 pixels
- Padding: 15 pixels
- Shadow: Subtle drop shadow

### Text Styling

**Title:**
- Font: Inter SemiBold
- Size: 14px
- Color: White with 10% transparency

**Description:**
- Font: Inter Medium
- Size: 12px
- Color: White with 50% transparency

### Animation

**Fade In:**
- Duration: 0.3 seconds
- Easing: Sine

**Fade Out:**
- Duration: 0.3 seconds
- Easing: Sine

## Technical Details

### Container Structure

```
notifications (Frame)
├── UIListLayout (Vertical, Bottom-aligned)
├── UIPadding (10px all sides)
└── [Notification Frames]
```

### Z-Index

Notifications have a Z-Index of 2, ensuring they appear above the main window.

### Performance

- Notifications are lightweight and performant
- Automatic cleanup when dismissed
- Efficient animation system
- Minimal memory footprint

### Limitations

- Maximum of 5 visible notifications at once
- Notifications are not persistent across sessions
- No built-in notification history
- Cannot be manually dismissed by user (except by clicking)
