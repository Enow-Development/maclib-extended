# Sections

Sections are container components that organize UI elements within tabs. MacLib provides three different section layouts to give you flexibility in designing your interface.

## Section Types

### Left Section

Left sections occupy the left column of the tab, taking up 50% of the available width.

```lua
local leftSection = tab:Section({ Side = "Left" })
```

**Use Cases:**
- Organizing related controls on the left side
- Creating a two-column layout with Right sections
- Default section type when Side is not specified

### Right Section

Right sections occupy the right column of the tab, taking up 50% of the available width.

```lua
local rightSection = tab:Section({ Side = "Right" })
```

**Use Cases:**
- Organizing related controls on the right side
- Creating a two-column layout with Left sections
- Separating different categories of controls

### Full Section

Full sections span the entire width of the tab, taking up 100% of the available width.

```lua
local fullSection = tab:Section({ Side = "Full" })
```

**Use Cases:**
- Displaying wide content that needs more horizontal space
- Creating prominent sections that stand out
- Organizing content that doesn't fit well in narrow columns
- Displaying tables, lists, or other wide components

## Section Parameters

### Side (string)

Determines the position and width of the section.

**Valid Values:**
- `"Left"`: Places section in left column (50% width)
- `"Right"`: Places section in right column (50% width)
- `"Full"`: Places section across full width (100% width)

**Default Behavior:**
- If `Side` is not specified, defaults to `"Left"`
- If an invalid value is provided, defaults to `"Left"`

**Example:**
```lua
-- Explicit left section
local leftSection = tab:Section({ Side = "Left" })

-- Explicit right section
local rightSection = tab:Section({ Side = "Right" })

-- Full-width section
local fullSection = tab:Section({ Side = "Full" })

-- Default (will be left)
local defaultSection = tab:Section({})
```

## Section Layout Behavior

### Creation Order

Sections are displayed in the order they are created, regardless of their Side value. The layout system uses LayoutOrder to preserve creation order.

```lua
-- These will appear in this order:
local section1 = tab:Section({ Side = "Left" })   -- First
local section2 = tab:Section({ Side = "Full" })   -- Second
local section3 = tab:Section({ Side = "Right" })  -- Third
local section4 = tab:Section({ Side = "Left" })   -- Fourth
```

### Layout Isolation

Full sections are isolated from Left and Right sections. This means:
- Adding or removing Full sections does not affect Left/Right section layout
- Left and Right sections always appear side-by-side in their column container
- Full sections appear in sequence with the column pairs

### Styling Consistency

All section types (Left, Right, Full) share the same visual styling:
- Background transparency: 0.98
- Border stroke transparency: 0.95
- Corner radius: 8 pixels
- Padding: 22px top, 20px left/bottom, 18px right
- Internal spacing: 10px between elements

## Mixed Layout Examples

### Example 1: Two-Column Layout

```lua
local tab = window:Tab({ Name = "Settings", Icon = "rbxassetid://10734950309" })

-- Left column
local leftSection = tab:Section({ Side = "Left" })
leftSection:Toggle({ Name = "Feature 1", Default = false })
leftSection:Toggle({ Name = "Feature 2", Default = false })

-- Right column
local rightSection = tab:Section({ Side = "Right" })
rightSection:Slider({ Name = "Speed", Min = 0, Max = 100, Default = 50 })
rightSection:Slider({ Name = "Volume", Min = 0, Max = 100, Default = 75 })
```

### Example 2: Full-Width Header with Columns

```lua
local tab = window:Tab({ Name = "Dashboard", Icon = "rbxassetid://10734950309" })

-- Full-width header section
local headerSection = tab:Section({ Side = "Full" })
headerSection:Label({ Text = "Welcome to Dashboard" })
headerSection:Button({ Name = "Refresh Data", Callback = function() end })

-- Two-column content below
local leftSection = tab:Section({ Side = "Left" })
leftSection:Toggle({ Name = "Auto Update", Default = true })

local rightSection = tab:Section({ Side = "Right" })
rightSection:Textbox({ Name = "Search", Placeholder = "Search..." })
```

### Example 3: Mixed Layout

```lua
local tab = window:Tab({ Name = "Advanced", Icon = "rbxassetid://10734950309" })

-- Start with columns
local leftSection1 = tab:Section({ Side = "Left" })
leftSection1:Button({ Name = "Action 1" })

local rightSection1 = tab:Section({ Side = "Right" })
rightSection1:Button({ Name = "Action 2" })

-- Full-width section in the middle
local fullSection = tab:Section({ Side = "Full" })
fullSection:Label({ Text = "Important Settings" })
fullSection:Toggle({ Name = "Enable Advanced Mode", Default = false })

-- More columns below
local leftSection2 = tab:Section({ Side = "Left" })
leftSection2:Slider({ Name = "Setting 1", Min = 0, Max = 100 })

local rightSection2 = tab:Section({ Side = "Right" })
rightSection2:Slider({ Name = "Setting 2", Min = 0, Max = 100 })
```

## Best Practices

### When to Use Left/Right Sections

- When you have two distinct categories of controls
- When controls are narrow and can fit comfortably in 50% width
- When you want to maximize vertical space usage
- For compact, organized layouts

### When to Use Full Sections

- When displaying wide content (tables, long text, etc.)
- When you want to emphasize certain controls
- For headers or separators between groups
- When controls need more horizontal space
- For single, prominent actions

### Layout Tips

1. **Group Related Controls**: Keep related controls in the same section
2. **Balance Columns**: Try to keep left and right sections roughly equal in height
3. **Use Full Sections Sparingly**: Too many full sections can make the UI feel cramped
4. **Consider Visual Hierarchy**: Use full sections for important or primary actions
5. **Test Different Layouts**: Experiment with different combinations to find what works best

## Technical Details

### Container Structure

The section layout uses the following container structure:

```
elementsScrolling (ScrollingFrame)
├── UIListLayout (Vertical)
├── columnsContainer (Frame, 100% width)
│   ├── UIListLayout (Horizontal)
│   ├── left (Frame, 50% width)
│   └── right (Frame, 50% width)
└── full (Frame, 100% width)
```

### Automatic Sizing

All sections use `AutomaticSize = Enum.AutomaticSize.Y`, which means:
- Sections automatically grow vertically based on their content
- No need to manually calculate section heights
- Scrolling is handled automatically by the parent ScrollingFrame

### Performance

The section system is optimized for performance:
- Minimal nesting depth
- Efficient layout recalculation
- No unnecessary re-renders when adding/removing sections
