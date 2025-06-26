# KZones Layout Designer

A visual, browser-based tool for designing window tiling layouts for KZones on KDE Plasma. Create, edit, and export custom zone configurations with an intuitive drag-and-drop interface.

## Features

- **Visual Zone Designer** - Click and drag to create window zones
- **Smart Snapping** - Automatically aligns zones to grid lines, screen edges, and other zones
- **Multi-Resolution Support** - Design for any display size with preset and custom resolutions
- **Drag & Drop** - Move existing zones by clicking and dragging
- **Export/Import** - Generate JSON files compatible with KZones
- **Color-Coded Zones** - Each zone gets a unique color for easy identification
- **Real-time Coordinates** - View exact percentages for precise positioning

## Requirements

- Modern web browser (Firefox, Chrome, Safari, Edge)
- No installation required - runs entirely in the browser
- Internet connection for initial load (loads libraries from CDN)

## Quick Start

1. **Download** the `kzones-designer.html` file
2. **Open** it in your web browser
3. **Start designing** your layout immediately!

## How to Use

### Creating Zones
1. **Select Resolution** - Choose your display resolution from the dropdown or enter custom dimensions
2. **Draw Zones** - Click and drag on the canvas to create rectangular zones
3. **Move Zones** - Click and drag existing zones to reposition them
4. **Use Snapping** - Green lines appear when zones align with grid or other zones

### Managing Layouts
1. **Name Your Layout** - Enter a descriptive name in the "Layout Name" field
2. **Save Layout** - Click "Save Layout" to keep it in your session
3. **Export** - Use "Export Current" for one layout or "Export All" for multiple layouts
4. **Import** - Load existing KZones JSON files to edit them

### Settings
- **Snap Settings** - Toggle snapping on/off and adjust tolerance
- **Resolution Control** - Switch between common presets or use custom dimensions
- **Zone Management** - View, select, and delete individual zones

## Display Resolution Presets

- **1920x1080** - Full HD
- **2560x1440** - 1440p
- **3440x1440** - Ultrawide
- **3840x2160** - 4K
- **5120x1440** - Super Ultrawide
- **5120x2880** - 5K
- **Custom** - Any resolution you need

## Exporting to KZones

### For Single Layout
1. Design your zones
2. Name your layout
3. Click **"Export Current"**
4. Save the JSON file

### For Multiple Layouts
1. Create and save multiple layouts using "Save Layout"
2. Click **"Export All"**
3. Get a complete JSON file with all your layouts

### Using with KZones
1. Copy the exported JSON content
2. Open your KZones configuration
3. Paste the zones array into your KZones settings
4. Apply the configuration in KDE Plasma

## Example Export Format

```json
[
  {
    "name": "My Custom Layout",
    "zones": [
      {"x": 0, "y": 0, "width": 50, "height": 100},
      {"x": 50, "y": 0, "width": 50, "height": 50},
      {"x": 50, "y": 50, "width": 50, "height": 50}
    ]
  }
]
```

## Keyboard & Mouse Controls

| Action | Control |
|--------|---------|
| Create Zone | Click and drag on empty space |
| Move Zone | Click and drag existing zone |
| Select Zone | Click on zone |
| Delete Zone | Click trash icon in zone list |
| Toggle Snap | Use checkbox in Snap Settings |

## Tips & Tricks

- **Grid Lines** - Use the 10% grid overlay for consistent layouts
- **Snap Tolerance** - Adjust sensitivity for easier or more precise snapping
- **Aspect Ratios** - The designer shows your display's aspect ratio for reference
- **Zone Details** - Click zones to see exact coordinates in the bottom panel
- **Color Coding** - Each zone gets a unique color and number for easy identification

## Troubleshooting

### Browser Compatibility
- **Recommended**: Firefox, Chrome 80+, Safari 14+, Edge 80+
  
### Common Issues
- **Icons not showing**: Ensure internet connection for CDN resources
- **Zones not snapping**: Check that snapping is enabled in settings
- **Export not working**: Modern browsers required for file download API

### Performance
- **Large layouts**: The tool handles dozens of zones efficiently
- **High resolutions**: Canvas scales automatically to fit screen

## Contributing

This is a standalone HTML file that can be easily modified:
- Open the HTML file in a text editor
- Modify the JavaScript code in the `<script>` section
- Reload in browser to test changes

## Technical Documentation

For those interested in understanding the implementation details, see the `file-structure-overview.md` which provides:
- Complete code architecture breakdown
- JavaScript component explanations
- Canvas rendering pipeline details
- State management patterns
- Event handling system overview
- Performance optimization techniques

The technical docs are essential for anyone looking to modify, extend, or contribute to the zone designer.

## License

This tool is designed for use with KZones and KWin on KDE Plasma. Feel free to modify and distribute.

## Related Projects

- [**KZones**](https://github.com/gerritdevriese/kzones) - The KDE Plasma window tiling extension this tool generates layouts for
- [**KWin**](https://github.com/KDE/kwin) - The window manager that provides the framework for KZones
- [**KDE Plasma**](https://github.com/KDE/plasma-desktop) - The desktop environment that KZones runs on

---

**Happy tiling!** 
