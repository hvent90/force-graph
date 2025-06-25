# Shortest Path Example

This example demonstrates how to add shortest path functionality to a force-directed graph.

## Features

- **Interactive Node Selection**: Select start and end nodes via dropdown menus or by clicking directly on nodes
- **Shortest Path Algorithm**: Uses BFS (Breadth-First Search) to find the shortest path between two nodes
- **Visual Highlighting**: 
  - Start node highlighted in red
  - End node highlighted in blue
  - Path nodes highlighted in green
  - Path edges highlighted in orange with animated particles
- **Path Information**: Displays path length and distance
- **Clear Functionality**: Reset the path and selections

## How to Use

1. **Select Nodes**: Choose start and end nodes from the dropdown menus, or click directly on nodes in the graph
2. **Find Path**: Click the "Find Shortest Path" button to calculate and highlight the shortest path
3. **Clear Path**: Click "Clear Path" to reset the visualization

## Technical Implementation

### Core Components

1. **Graph Data Structure**: Uses adjacency list representation for efficient pathfinding
2. **BFS Algorithm**: Implements breadth-first search to find the shortest path
3. **Visual Highlighting**: Leverages force-graph's built-in styling capabilities
4. **Interactive Controls**: HTML form elements for user interaction

### Key Functions

- `buildAdjacencyList()`: Converts graph data to adjacency list format
- `findShortestPath()`: BFS implementation for pathfinding
- `findAndHighlightPath()`: Main function to calculate and display paths
- `clearPath()`: Reset functionality

### Integration with Force Graph

The implementation uses force-graph's built-in properties:
- `nodeColor()`: Dynamic node coloring based on path status
- `linkColor()` and `linkWidth()`: Path edge highlighting
- `linkDirectionalParticles()`: Animated particles along path edges
- `onNodeClick()`: Interactive node selection

## Customization

You can easily customize:
- **Colors**: Modify the color scheme for different path elements
- **Algorithm**: Replace BFS with Dijkstra's algorithm for weighted graphs
- **Styling**: Adjust node sizes, link widths, and particle effects
- **Graph Generation**: Use your own graph data instead of the random graph

## Example Usage in Your Project

```javascript
// Basic setup
const graph = new ForceGraph()
  .graphData(yourGraphData)
  .nodeColor(node => {
    if (node === startNode) return '#ff4444';
    if (node === endNode) return '#4444ff';
    if (pathNodes.has(node)) return '#44ff44';
    return node.originalColor;
  })
  .linkColor(link => pathLinks.has(link) ? '#ffaa00' : '#999999')
  .linkWidth(link => pathLinks.has(link) ? 4 : 1)
  .linkDirectionalParticles(link => pathLinks.has(link) ? 3 : 0);

// Find shortest path
const path = findShortestPath(adjacencyList, startNodeId, endNodeId);

// Highlight the path
highlightPath(path);
```

This implementation provides a solid foundation for adding pathfinding capabilities to any force-directed graph visualization.
