title: BFS [Cap]
date: 2021-09-22 11:28:05 +0200
categories: [linux,idor,capabilities]
tags: [BFS]
excerpt: Cap is an easy machine in HTB that involves idor to dowload a pcap, you can find ssh creds there, then we need to use python cap_setuid to priv esc to root
---

## Building the Maze Solver with BFS, DFS, and A*

**Date**: 2025-01-29  
**Categories**: Maze Solver, Algorithms, C++  
**Tags**: BFS, DFS, A*, Maze Generation, C++  

### Project Overview

In this project, I developed a Maze Solver using various pathfinding algorithms, including **Breadth-First Search (BFS)**, **Depth-First Search (DFS)**, **A***, and more. The challenge was to not only find a solution to a generated maze but also optimize the pathfinding for different scenarios. Here's a quick overview of the implementation and some highlights from the development process.

### Algorithms Used

1. **Breadth-First Search (BFS)**  
   BFS is used to explore the maze level by level. It guarantees the shortest path, making it the go-to choice for unweighted grids.

2. **Depth-First Search (DFS)**  
   DFS is a simple algorithm that explores as deep as possible before backtracking. Although it may not always find the shortest path, it provides interesting insights into the maze structure.

3. **A* Search (Consistent)**  
   This is an optimal algorithm that combines the benefits of BFS and heuristic search. By using an admissible heuristic, A* finds the shortest path efficiently.

4. **A* Search (Weighted)**  
   A variant of A*, this algorithm introduces a weight (w=5) to test the impact of prioritizing certain paths over others.

### Time Complexity and Memory Usage

- **BFS**: Time complexity is \(O(V+E)\) where \(V\) is the number of vertices and \(E\) is the number of edges. Memory usage depends on the size of the maze and can be significant due to storing all nodes in the queue.
- **DFS**: Time complexity is \(O(V+E)\), but memory usage can be lower than BFS as it doesn't store as many nodes.
- **A***: Time complexity is \(O(E)\), where \(E\) is the number of edges explored, and memory usage depends on the number of open nodes stored in the priority queue.

### Maze Generation

For maze generation, I used **Kruskal's Algorithm** to ensure that the maze is solvable and interesting. The algorithm helps create a randomized but connected maze with many possible paths.

### Code Walkthrough

Below is a brief look at the structure of the **A* Search Algorithm** implementation:

```cpp
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

struct Node {
    int x, y;
    int g, h;  // g: cost to reach, h: heuristic
    Node* parent;
};

bool operator>(const Node& a, const Node& b) {
    return (a.g + a.h) > (b.g + b.h);
}

int heuristic(int x, int y, int goalX, int goalY) {
    return abs(x - goalX) + abs(y - goalY);
}

void solveMaze(int maze[5][5], int startX, int startY, int goalX, int goalY) {
    priority_queue<Node, vector<Node>, greater<Node>> openList;
    vector<vector<bool>> closedList(5, vector<bool>(5, false));
    Node startNode = {startX, startY, 0, heuristic(startX, startY, goalX, goalY), nullptr};
    openList.push(startNode);
    
    while (!openList.empty()) {
        Node current = openList.top();
        openList.pop();

        if (current.x == goalX && current.y == goalY) {
            // Path found, backtrack to get the solution
            Node* pathNode = &current;
            while (pathNode != nullptr) {
                cout << "(" << pathNode->x << ", " << pathNode->y << ") ";
                pathNode = pathNode->parent;
            }
            break;
        }

        // Explore neighboring nodes
        // Add your exploration logic here (e.g., check adjacent cells)
    }
}
