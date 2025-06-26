## File Structure Overview

```
kzones-designer.html
├── HTML Document Structure
├── External Dependencies (CDN)
├── CSS Styling
└── JavaScript Application
    ├── React Component Definition
    ├── State Management
    ├── Canvas Drawing Logic
    ├── Mouse Event Handling
    ├── Snapping System
    ├── Export/Import Functions
    └── UI Components
```

## External Dependencies

### CDN Libraries
```html
<!-- React 18 Core -->
<script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>

<!-- Babel for JSX transpilation -->
<script src="https://unpkg.com/@babel/standalone@7.23.4/babel.min.js"></script>

<!-- Tailwind CSS for styling -->
<script src="https://cdn.tailwindcss.com"></script>
```

**Why these choices:**
- **React 18**: Modern component state management and rendering
- **Babel Standalone**: Enables JSX syntax in browser without build step
- **Tailwind CDN**: Rapid styling without custom CSS compilation

## React Component Architecture

### Main Component Structure
```javascript
const ZoneDesigner = () => {
  // State declarations
  // Helper functions
  // Event handlers
  // Render JSX
};
```

## State Management

### Core State Variables
```javascript
// Zone data and layout
const [zones, setZones] = useState([]);              // Array of zone objects
const [layouts, setLayouts] = useState([]);          // Saved layouts collection
const [layoutName, setLayoutName] = useState('New Layout'); // Current layout name

// Drawing and interaction state
const [isDrawing, setIsDrawing] = useState(false);   // Zone creation mode
const [isDragging, setIsDragging] = useState(false); // Zone moving mode
const [selectedZone, setSelectedZone] = useState(null); // Currently selected zone
const [currentZone, setCurrentZone] = useState(null); // Zone being drawn

// Drag and drop state
const [draggedZone, setDraggedZone] = useState(null); // Zone being moved
const [dragOffset, setDragOffset] = useState({ x: 0, y: 0 }); // Mouse offset from zone corner
const [startPoint, setStartPoint] = useState({ x: 0, y: 0 }); // Drawing start position

// UI and interaction state
const [cursorStyle, setCursorStyle] = useState('crosshair'); // Dynamic cursor
const [snapEnabled, setSnapEnabled] = useState(true); // Snapping toggle
const [snapTolerance, setSnapTolerance] = useState(10); // Snap sensitivity in pixels
const [snapIndicators, setSnapIndicators] = useState([]); // Visual snap feedback

// Display settings
const [displayResolution, setDisplayResolution] = useState({ width: 5120, height: 1440 });
```

### Zone Data Structure
```javascript
// Each zone object contains:
{
  x: 25.0,        // X position as percentage (0-100)
  y: 15.5,        // Y position as percentage (0-100)
  width: 50.0,    // Width as percentage (0-100)
  height: 40.5    // Height as percentage (0-100)
}
```

## Canvas System

### Canvas Dimensions and Scaling
```javascript
const maxCanvasWidth = 800; // Fixed maximum width
const aspectRatio = displayResolution.width / displayResolution.height;
const canvasWidth = maxCanvasWidth;
const canvasHeight = Math.round(maxCanvasWidth / aspectRatio);
```

**Scaling Logic:**
- Canvas maintains aspect ratio of selected display resolution
- Zones stored as percentages (resolution-independent)
- Visual representation scales to fit available space

### Drawing Pipeline
```javascript
const drawCanvas = () => {
  // 1. Clear canvas with dark background
  ctx.fillStyle = '#1a1a1a';
  ctx.fillRect(0, 0, canvasWidth, canvasHeight);
  
  // 2. Draw grid lines (10% intervals)
  for (let i = 0; i <= 10; i++) {
    // Vertical and horizontal grid lines
  }
  
  // 3. Draw zones with colors and borders
  zones.forEach((zone, index) => {
    // Convert percentages to pixel coordinates
    // Apply visual states (selected, dragging)
    // Draw filled rectangle with border
    // Add zone number label
  });
  
  // 4. Draw snap indicators (green lines)
  snapIndicators.forEach(indicator => {
    // Draw vertical or horizontal snap guides
  });
};
```

## Mouse Event System

### Coordinate Conversion
```javascript
const getMousePos = (e) => {
  const canvas = canvasRef.current;
  const rect = canvas.getBoundingClientRect();
  return {
    x: e.clientX - rect.left,  // Canvas-relative X
    y: e.clientY - rect.top    // Canvas-relative Y
  };
};
```

### Event Flow
```javascript
// Mouse Down: Determine action (create zone, select zone, start drag)
const handleMouseDown = (e) => {
  // 1. Get mouse position
  // 2. Check if clicking on existing zone
  // 3. If zone clicked: start drag mode
  // 4. If empty space: start draw mode
};

// Mouse Move: Update zone position/size or show snap guides
const handleMouseMove = (e) => {
  // 1. Handle zone dragging
  // 2. Handle zone creation
  // 3. Update cursor style
  // 4. Apply snapping
};

// Mouse Up: Finalize zone creation/movement
const handleMouseUp = () => {
  // 1. Finish dragging or drawing
  // 2. Round coordinates to 0.1% precision
  // 3. Clear temporary state
};
```

## Snapping System

### Snap Point Generation
```javascript
const getSnapPoints = () => {
  const snapPoints = {
    vertical: [0, canvasWidth],    // Screen edges
    horizontal: [0, canvasHeight]  // Screen edges
  };

  // Add grid lines (10% intervals)
  for (let i = 1; i < 10; i++) {
    snapPoints.vertical.push((i * canvasWidth) / 10);
    snapPoints.horizontal.push((i * canvasHeight) / 10);
  }

  // Add existing zone edges
  zones.forEach(zone => {
    const x = (zone.x * canvasWidth) / 100;
    const y = (zone.y * canvasHeight) / 100;
    const width = (zone.width * canvasWidth) / 100;
    const height = (zone.height * canvasHeight) / 100;

    snapPoints.vertical.push(x, x + width);
    snapPoints.horizontal.push(y, y + height);
  });

  return snapPoints;
};
```

### Snap Application
```javascript
const applySnapping = (pos) => {
  if (!snapEnabled) return pos;

  const snapPoints = getSnapPoints();
  const indicators = [];
  let snappedPos = { ...pos };

  // Find nearest vertical snap point within tolerance
  const nearestX = snapPoints.vertical.find(snapX => 
    Math.abs(pos.x - snapX) <= snapTolerance
  );
  
  // Apply snap and create visual indicator
  if (nearestX !== undefined) {
    snappedPos.x = nearestX;
    indicators.push({ type: 'vertical', x: nearestX });
  }

  // Same for horizontal snapping
  // Update snap indicators for visual feedback
  setSnapIndicators(indicators);
  return snappedPos;
};
```

## Export/Import System

### JSON Export Structure
```javascript
const exportLayout = () => {
  const layout = {
    name: layoutName,
    zones: zones  // Array of zone objects with x, y, width, height percentages
  };
  
  // Create downloadable blob
  const dataStr = "data:text/json;charset=utf-8," + 
    encodeURIComponent(JSON.stringify([layout], null, 2));
  
  // Trigger download
  const downloadAnchorNode = document.createElement('a');
  downloadAnchorNode.setAttribute("href", dataStr);
  downloadAnchorNode.setAttribute("download", `${layoutName.replace(/\s+/g, '_')}.json`);
  document.body.appendChild(downloadAnchorNode);
  downloadAnchorNode.click();
  downloadAnchorNode.remove();
};
```

### File Import Handling
```javascript
const importLayouts = (e) => {
  const file = e.target.files[0];
  if (file) {
    const reader = new FileReader();
    reader.onload = (e) => {
      try {
        const imported = JSON.parse(e.target.result);
        if (Array.isArray(imported) && imported.length > 0) {
          setLayouts(imported);
          // Load first layout automatically
          if (imported[0].zones) {
            setZones(imported[0].zones);
            setLayoutName(imported[0].name);
          }
        }
      } catch (error) {
        alert('Invalid JSON file');
      }
    };
    reader.readAsText(file);
  }
};
```

## UI Component Structure

### Layout Grid
```javascript
<div className="grid grid-cols-1 lg:grid-cols-4 gap-6">
  <div className="lg:col-span-3">
    {/* Canvas Area */}
    <Canvas />
  </div>
  <div className="space-y-4">
    {/* Control Panels */}
    <LayoutNameInput />
    <ResolutionControl />
    <SnapSettings />
    <ZoneList />
    <ActionButtons />
    <SavedLayouts />
  </div>
</div>
```

### Dynamic Canvas Cursor
```javascript
const updateCursor = (e) => {
  if (isDragging) {
    setCursorStyle('grabbing');
  } else {
    const overZone = zones.some(zone => {
      // Check if mouse is over any zone
    });
    setCursorStyle(overZone ? 'grab' : 'crosshair');
  }
};
```

## Color System

### Zone Color Assignment
```javascript
const colors = [
  '#FF4444', '#44FF44', '#4444FF', '#FFFF44', 
  '#FF44FF', '#44FFFF', '#FFA500', '#800080', 
  '#00FF80', '#FF8000', '#8000FF', '#00FF40'
];

// Each zone gets a color based on its index
const zoneColor = colors[index % colors.length];
```

## Resolution Management

### Preset System
```javascript
const resolutionPresets = [
  { name: "1920x1080 (Full HD)", width: 1920, height: 1080 },
  { name: "2560x1440 (1440p)", width: 2560, height: 1440 },
  { name: "3440x1440 (Ultrawide)", width: 3440, height: 1440 },
  { name: "3840x2160 (4K)", width: 3840, height: 2160 },
  { name: "5120x1440 (Super Ultrawide)", width: 5120, height: 1440 },
  { name: "5120x2880 (5K)", width: 5120, height: 2880 },
  { name: "Custom", width: 0, height: 0 }
];
```

### Dynamic Canvas Resizing
When resolution changes:
1. Canvas dimensions recalculate based on new aspect ratio
2. Existing zones remain at same percentage positions
3. Visual representation updates to match new proportions

## Performance Considerations

### Efficient Rendering
- Canvas redraws only when state changes (useEffect dependency array)
- Mouse move events throttled by browser's animation frame
- Snap calculations optimized with early returns

### Memory Management
- State updates use functional updates to prevent unnecessary re-renders
- Event listeners properly cleaned up
- No memory leaks from file operations

## Browser Compatibility

### Required Features
- **Canvas 2D API**: For zone rendering
- **File API**: For import/export functionality
- **ES6+ JavaScript**: For modern syntax
- **CSS Grid/Flexbox**: For responsive layout

### Fallback Handling
- Graceful degradation for older browsers
- Error boundaries for invalid JSON imports
- Input validation for custom resolutions

## KZones Integration

### Output Format Compatibility
The exported JSON matches KZones expected structure:
```json
[
  {
    "name": "Layout Name",
    "zones": [
      {"x": 0, "y": 0, "width": 50, "height": 100},
      {"x": 50, "y": 0, "width": 50, "height": 50}
    ]
  }
]
```

### Coordinate System
- **Input**: Mouse coordinates in pixels relative to canvas
- **Storage**: Percentages (0-100) for resolution independence  
- **Output**: Percentages compatible with KZones format
- **Display**: Scaled pixel coordinates for visual representation

This architecture provides a robust, maintainable solution for visual zone layout design while maintaining compatibility with the KZones tiling system.
