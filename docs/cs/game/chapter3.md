# Maze Algorithms

## Mazes

### Classifications

- Perfect maze: Has one and only one path between any two cells -- thus plenty of branching, but no loops
    - Cell: position in the maze, the smallest geographical element of the maze
- Unicursal maze: Has no branches, so there is just a single path from beginning to end

## Maze Generation

### Binary Tree Algorithm

A wall carving algorithm. Start with a grid of unlinked cells.

- 每个单元可能和右边或者上边相连。
- 最右边一列和最上边一列分别全联通。

For each cell, randomly carve a passage either north or east.

### Sidewinder Algorithm

Top row: link all cells together.

Other rows, collect adjacent cells into "runs". Randomly add the next cell to the run, and randomly end the run.

Always end the run when there is no cell to the East.

When ending the run, link all cells in the run together, and randomly link one cell to the North.

### Aldous-Broder Random Walk

A random walk algorithm: step from cell to random neighbor to create random paths.

- Start at a random cell
- Move to a random neighbor
- If the neighbor has not been visited, link the cells and move to the neighbor
- Repeat until all cells have been visited

Inefficient, need too many steps to generate a maze.

#### Bias

<div align="center"><img src="/assets/img/CS/game/chapter3/bias.png" width="50%"></div>

A-B has no bias, then it's a unbiased random walk.

An unbiased algorithm will potentially generate any of them -- with a uniform probability distribution over them all.

- A-B 算法没有偏向性，生成各种迷宫的概率是均匀的。
- Binary Tree 和 Sidewinder 算法有偏向性，生成的迷宫有特定的特征。

### Wilson's Random Walk

Also creates mazes with no bias. Also has inefficient performance characteristics.

- Choose a random cell and mark it as visited
- Choose a random unvisited cell and start a random walk from it
- Perform a loop-erased random walk, choosing a random neighbor at each step
- If the walk reaches a visited cell, erase the loop and add the path to the maze
- Repeat until all cells have been visited

### Recursive Backtracker

DFS

- Start at a random cell
- Link and move to a random neighbor who is unvisited
- Anytime a cell has no unvisited neighbors, backtrack to the last cell with unvisited neighbors

Recursive Backtracker leads to mazes with long, twisty paths with few dead ends. And it's biased.

## Maze Solution Algorithms

### Dijkstra's Algorithm

For the maze, the weight of each edge is 1 for linked cells, and infinity for unlinked cells.

### Longest Path

Do Dijkstra's Algorithm twice:

- Pick a random cell A as the start
- Find the cell B that is farthest from A
- Find the cell C that is farthest from B
- The path from B to C is the longest path