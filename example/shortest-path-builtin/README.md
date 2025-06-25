# Shortest Path with Built-in Methods

This example demonstrates how to use the force-graph library's built-in shortest path methods.

## New Library Methods

The force-graph library now includes these pathfinding methods:

```javascript
// Find shortest path between two nodes
const path = graph.findShortestPath(startNodeId, endNodeId);

// Highlight a specific path
graph.highlightPath(path);

// Clear path highlighting
graph.clearPathHighlight();

// Find and highlight in one call (convenience method)
const path = graph.findAndHighlightPath(startNodeId, endNodeId);

// Get current path highlight information
const pathInfo = graph.getPathHighlight();
```

## Key Differences from Manual Implementation

### Before (Manual Implementation)
```javascript
// You had to implement all this yourself:
function buildAdjacencyList(graphData) { /* ... */ }
function findShortestPath(adjacencyList, start, end) { /* ... */ }
function highlightPath(path) { /* ... */ }

// And manage highlighting state manually:
let pathNodes = new Set();
let pathLinks = new Set();

// And modify node/link colors in the graph setup:
.nodeColor(node => {
  if (pathNodes.has(node)) return '#44ff44';
  return node.color;
})
.linkColor(link => {
  if (pathLinks.has(link)) return '#ffaa00';
  return '#999999';
})
```

### After (Built-in Methods)
```javascript
// Simple one-liner:
const path = graph.findAndHighlightPath(startNodeId, endNodeId);

// Or separate calls for more control:
const path = graph.findShortestPath(startNodeId, endNodeId);
if (path) {
  graph.highlightPath(path);
  console.log(`Path: ${path.join(' → ')}`);
}

// Clear when done:
graph.clearPathHighlight();
```

## Automatic Visual Styling

The built-in methods automatically apply these visual styles:

- **Start Node**: Red (`#ff4444`)
- **End Node**: Blue (`#4444ff`) 
- **Path Nodes**: Green (`#44ff44`)
- **Path Links**: Orange (`#ffaa00`) with increased thickness

No manual color management required!

## Method Details

### `findShortestPath(startNodeId, endNodeId)`
- **Returns**: Array of node IDs representing the shortest path, or `null` if no path exists
- **Algorithm**: Breadth-First Search (BFS) for unweighted graphs
- **Time Complexity**: O(V + E) where V = nodes, E = edges

### `highlightPath(path)`
- **Parameters**: Array of node IDs, or `null`/`undefined` to clear highlighting
- **Returns**: The graph instance (chainable)
- **Effect**: Visually highlights the specified path with automatic coloring

### `clearPathHighlight()`
- **Returns**: The graph instance (chainable)
- **Effect**: Removes all path highlighting

### `findAndHighlightPath(startNodeId, endNodeId)`
- **Returns**: Array of node IDs representing the path, or `null` if no path exists
- **Effect**: Combines `findShortestPath()` and `highlightPath()` in one call
- **Convenience**: Perfect for interactive applications

### `getPathHighlight()`
- **Returns**: Object with path information, or `null` if no path is highlighted
- **Properties**:
  - `path`: Array of node IDs
  - `nodes`: Set of node IDs in the path
  - `links`: Set of link objects in the path
  - `nodeObjects`: Array of node objects
  - `linkObjects`: Array of link objects
  - `startNodeId`: ID of the start node
  - `endNodeId`: ID of the end node

## TypeScript Support

All methods are fully typed for TypeScript users:

```typescript
interface ForceGraphInstance {
  findShortestPath(startNodeId: string | number, endNodeId: string | number): (string | number)[] | null;
  highlightPath(path: (string | number)[] | null): ForceGraphInstance;
  clearPathHighlight(): ForceGraphInstance;
  findAndHighlightPath(startNodeId: string | number, endNodeId: string | number): (string | number)[] | null;
  getPathHighlight(): PathHighlightInfo | null;
}
```

## Integration with Existing Features

The pathfinding methods work seamlessly with all existing force-graph features:

- **Custom Node Colors**: Path highlighting overrides base colors but preserves your color scheme for non-path nodes
- **Custom Link Styles**: Path links get enhanced styling while maintaining your base link appearance
- **Node/Link Interactions**: Click handlers, hover effects, and tooltips continue to work normally
- **Performance**: Built-in caching and optimizations for smooth interaction

This built-in approach significantly reduces boilerplate code while providing a more robust and feature-complete pathfinding solution.
