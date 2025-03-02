# Bellman-Ford Algorithm

## Overview
The **Bellman-Ford algorithm** is a shortest path algorithm that finds the shortest paths from a single source vertex to all other vertices in a weighted graph. Unlike Dijkstra’s algorithm, Bellman-Ford works with graphs containing negative weight edges and can also detect negative weight cycles.

## Algorithm Steps
1. Initialize the distance to all vertices as **infinity** and the source vertex's distance as **0**.
2. Relax all edges **(V-1) times**, where **V** is the number of vertices.
3. Check for negative weight cycles by trying to relax the edges one more time. If any distance changes, a negative weight cycle exists.

## Code Implementation (C++)
```cpp
#include <bits/stdc++.h>
using namespace std;

struct Edge {
    int src, dest, weight;
};

void bellmanFord(int V, int E, vector<Edge>& edges, int source) {
    vector<int> distance(V, INT_MAX);
    distance[source] = 0;

    // Relax all edges V-1 times
    for (int i = 0; i < V - 1; i++) {
        for (const Edge& edge : edges) {
            if (distance[edge.src] != INT_MAX && distance[edge.src] + edge.weight < distance[edge.dest]) {
                distance[edge.dest] = distance[edge.src] + edge.weight;
            }
        }
    }

    // Check for negative weight cycles
    for (const Edge& edge : edges) {
        if (distance[edge.src] != INT_MAX && distance[edge.src] + edge.weight < distance[edge.dest]) {
            cout << "Graph contains a negative weight cycle" << endl;
            return;
        }
    }

    // Print shortest distances
    cout << "Vertex Distance from Source" << endl;
    for (int i = 0; i < V; i++) {
        cout << i << "\t" << (distance[i] == INT_MAX ? "INF" : to_string(distance[i])) << endl;
    }
}

int main() {
    int V, E, source;
    cin >> V >> E;
    vector<Edge> edges(E);
    for (int i = 0; i < E; i++) {
        cin >> edges[i].src >> edges[i].dest >> edges[i].weight;
    }
    cin >> source;

    bellmanFord(V, E, edges, source);
    return 0;
}
```

## Time and Space Complexity
- **Time Complexity**: `O(VE)`, where `V` is the number of vertices and `E` is the number of edges.
- **Space Complexity**: `O(V)`, since we use an array to store distances.

## Advantages
- Works with **negative weight edges**.
- Can detect **negative weight cycles**.
- Simpler implementation compared to Dijkstra’s algorithm.

## Disadvantages
- Slower than Dijkstra's algorithm for graphs with only positive weights.
- Not suitable for dense graphs due to its `O(VE)` complexity.

## Applications
- Network routing protocols (e.g., Distance Vector Routing Protocol).
- Arbitrage detection in currency exchange.
- Finding shortest paths in graphs with negative weights.

## How to Run
1. Compile the code using:
   ```sh
   g++ bellman_ford.cpp -o bellman_ford
   ```
2. Run the executable:
   ```sh
   ./bellman_ford
   ```
3. Input format:
   ```sh
   V E
   src1 dest1 weight1
   src2 dest2 weight2
   ...
   source
   ```

## Example Input
```
5 8
0 1 -1
0 2 4
1 2 3
1 3 2
1 4 2
3 2 5
3 1 1
4 3 -3
0
```

## Example Output
```
Vertex Distance from Source
0	0
1	-1
2	2
3	-2
4	1
```

If a negative weight cycle exists, the output will be:
```
Graph contains a negative weight cycle
```
