Modelling the graph using an adjacency list, where each node stores:

Neighbors (list of directly connected nodes)
Distance from the source node (initialized as ∞ except for the source, which is 0)
Back-pointer to track the path
The MapReduce process iterates until the shortest path to the target is found.

Steps
1. Input Format (Adjacency List)
Each node has:
node_id    [neighbor1, neighbor2, ...]    distance    backpointer

For example, in a social graph:
A    [B, C]    0    null
B    [A, D, E]    ∞    null
C    [A, F]    ∞    null
D    [B]    ∞    null
E    [B, F]    ∞    null
F    [C, E]    ∞    null


2. Mapper: Spread Distances
Each node emits:
Itself (to retain structure)
New distance for neighbors (if shorter)

For example, if A has distance = 0, it emits:
A    [B, C]    0    null   (retains original)
B    null    1    A
C    null    1    A


3. Reducer: Merge Updates
The reducer:
Keeps adjacency list
Updates shortest distance and back-pointer
Emits updated node

If B gets updates:
B    [A, D, E]    ∞    null
B    null    1    A

The reducer picks distance = 1 (shorter), updates B:
B    [A, D, E]    1    A


4. Iteration Until Target Found
The process repeats until the target node has a distance (i.e., it's reached).
