# Shortest Path Integration Guide

This guide shows how to add shortest path functionality to your force-graph implementation.

## Quick Start

See the complete examples in:
- `example/shortest-path/` - Full-featured implementation with UI controls
- `example/shortest-path-simple/` - Minimal click-to-select implementation

## Core Implementation

### 1. Build Adjacency List

```javascript
function buildAdjacencyList(graphData) {
  const adjacencyList = {};
  
  // Initialize adjacency list
  graphData.nodes.forEach(node => {
    adjacencyList[node.id] = [];
  });
  
  // Add edges (undirected graph)
  graphData.links.forEach(link => {
    const sourceId = typeof link.source === 'object' ? link.source.id : link.source;
    const targetId = typeof link.target === 'object' ? link.target.id : link.target;
    
    adjacencyList[sourceId].push(targetId);
    adjacencyList[targetId].push(sourceId);
  });
  
  return adjacencyList;
}
```

### 2. Shortest Path Algorithm (BFS)

```javascript
function findShortestPath(adjacencyList, startId, endId) {
  if (startId === endId) return [startId];
  
  const queue = [startId];
  const visited = new Set([startId]);
  const parent = { [startId]: null };
  
  while (queue.length > 0) {
    const current = queue.shift();
    
    if (current === endId) {
      // Reconstruct path
      const path = [];
      let node = endId;
      while (node !== null) {
        path.unshift(node);
        node = parent[node];
      }
      return path;
    }
    
    for (const neighbor of adjacencyList[current] || []) {
      if (!visited.has(neighbor)) {
        visited.add(neighbor);
        parent[neighbor] = current;
        queue.push(neighbor);
      }
    }
  }
  
  return null; // No path found
}
```

### 3. Visual Highlighting

```javascript
// State management
let pathNodes = new Set();
let pathLinks = new Set();
let startNode = null;
let endNode = null;

// Graph configuration with highlighting
const graph = new ForceGraph()
  .graphData(graphData)
  .nodeColor(node => {
    if (node === startNode) return '#ff4444';      // Red for start
    if (node === endNode) return '#4444ff';        // Blue for end
    if (pathNodes.has(node)) return '#44ff44';     // Green for path
    return '#69b3a2';                              // Default color
  })
  .linkColor(link => pathLinks.has(link) ? '#ffaa00' : '#999999')
  .linkWidth(link => pathLinks.has(link) ? 4 : 1)
  .linkDirectionalParticles(link => pathLinks.has(link) ? 3 : 0)
  .linkDirectionalParticleSpeed(0.01)
  .linkDirectionalParticleWidth(3)
  .linkDirectionalParticleColor('#ffaa00')
  .autoPauseRedraw(false); // Keep redrawing for updates
```

### 4. Path Highlighting Function

```javascript
function highlightPath(pathArray) {
  // Clear previous highlights
  pathNodes.clear();
  pathLinks.clear();
  
  if (!pathArray || pathArray.length === 0) return;
  
  // Set start/end nodes
  startNode = graphData.nodes.find(n => n.id === pathArray[0]);
  endNode = graphData.nodes.find(n => n.id === pathArray[pathArray.length - 1]);
  
  // Highlight path nodes
  pathArray.forEach(nodeId => {
    const node = graphData.nodes.find(n => n.id === nodeId);
    if (node) pathNodes.add(node);
  });
  
  // Highlight path links
  for (let i = 0; i < pathArray.length - 1; i++) {
    const sourceId = pathArray[i];
    const targetId = pathArray[i + 1];
    
    const link = graphData.links.find(l => {
      const lSourceId = typeof l.source === 'object' ? l.source.id : l.source;
      const lTargetId = typeof l.target === 'object' ? l.target.id : l.target;
      return (lSourceId === sourceId && lTargetId === targetId) ||
             (lSourceId === targetId && lTargetId === sourceId);
    });
    
    if (link) pathLinks.add(link);
  }
}
```

## Usage Examples

### Basic Usage

```javascript
// Find and highlight shortest path
const adjacencyList = buildAdjacencyList(graphData);
const path = findShortestPath(adjacencyList, startNodeId, endNodeId);
if (path) {
  highlightPath(path);
  console.log(`Path found: ${path.length} nodes, ${path.length - 1} edges`);
} else {
  console.log('No path found');
}
```

### Interactive Node Selection

```javascript
let selectedNodes = [];

graph.onNodeClick(node => {
  selectedNodes.push(node.id);
  
  if (selectedNodes.length === 2) {
    const path = findShortestPath(adjacencyList, selectedNodes[0], selectedNodes[1]);
    if (path) {
      highlightPath(path);
    }
    selectedNodes = []; // Reset for next selection
  }
});
```

## Advanced Features

### Weighted Graphs (Dijkstra's Algorithm)

For graphs with weighted edges, replace the BFS implementation with Dijkstra's algorithm:

```javascript
function dijkstraShortestPath(adjacencyList, weights, startId, endId) {
  // Implementation left as exercise - see Dijkstra's algorithm
  // Use a priority queue for optimal performance
}
```

### Multiple Path Algorithms

- **BFS**: Shortest path by number of edges (unweighted)
- **Dijkstra**: Shortest path by total weight
- **A***: Shortest path with heuristic (requires node coordinates)
- **Bidirectional BFS**: Faster for large graphs

### Customization Options

- **Colors**: Modify node/link colors for different path states
- **Animation**: Add smooth transitions when paths change
- **Path Info**: Display distance, number of hops, total weight
- **Multiple Paths**: Show alternative routes
- **Path History**: Keep track of previously found paths

## Performance Considerations

- **Large Graphs**: Consider using Web Workers for pathfinding
- **Real-time Updates**: Debounce path calculations during user interaction
- **Memory**: Clear path highlights when not needed
- **Rendering**: Use `autoPauseRedraw(false)` only when actively showing paths

This implementation provides a solid foundation for pathfinding in force-directed graphs and can be extended based on your specific requirements.
