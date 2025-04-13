# Maze-Generation-and-Recursive-Pathfinding
Maze Generation

We start by defining the start and finish points and ensure the maze has odd dimensions for proper wall placement. The maze is represented as a 2D grid, initially filled with walls ('#'). To generate paths, we dig two cells away in each direction to maintain walls between paths. Before digging, we check if the cell is valid (not already a path or border). The digging function continues recursively until no more valid paths can be created, ensuring the maze is solvable.


Maze Solver (Backtracking)
The maze solver uses backtracking to find a path from the start to the finish. It checks if the current cell is the finish and, if not, marks it as visited. It then tries moving in a random direction, recursively calling the solver from the new position. If it reaches a dead end, it backtracks, marking the cell as a path again, and tries another direction. This continues until the finish is found or all paths are exhausted.


The maze is generated with recursive digging, ensuring solvable paths and walls. The solver uses backtracking to explore possible paths and find the solution by trying all directions, backtracking when necessary.


here the code:
import random  # For randomizing directions

# Maze size (must be odd to work properly)
SIZE = 11

# Maze grid: initially full of walls '#'
maze = [['#' for _ in range(SIZE)] for _ in range(SIZE)]

# Visited cells for pathfinding
visited = [[False for _ in range(SIZE)] for _ in range(SIZE)]

# Directions for moving: up, down, left, right
dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]

# === Maze Generation ===
def make_paths(x, y):
    maze[x][y] = ' '  # Mark the current cell as a path

    # List of directions [0, 1, 2, 3] (we will use them as indices for dx, dy)
    directions = [0, 1, 2, 3]
    random.shuffle(directions)  # Shuffle to randomize path generation

    for d in directions:
        nx = x + dx[d] * 2  # Move 2 steps in chosen direction
        ny = y + dy[d] * 2

        # If next cell is within maze and is still a wall
        if 0 < nx < SIZE - 1 and 0 < ny < SIZE - 1 and maze[nx][ny] == '#':
            # Remove the wall between current cell and next cell
            maze[x + dx[d]][y + dy[d]] = ' '
            # Recurse: continue path from the next cell
            make_paths(nx, ny)

# === Pathfinding using Recursion ===
def solve(x, y):
    # Base case: Out of bounds
    if x < 0 or y < 0 or x >= SIZE or y >= SIZE:
        return False
    # Base case: Wall or already visited
    if maze[x][y] != ' ' or visited[x][y]:
        return False

    visited[x][y] = True  # Mark cell as visited

    # If we've reached the goal (bottom-right corner), mark and return success
    if x == SIZE - 2 and y == SIZE - 2:
        maze[x][y] = '.'
        return True

    # Try all 4 directions
    for i in range(4):
        if solve(x + dx[i], y + dy[i]):
            maze[x][y] = '.'  # Mark part of the correct path
            return True

    # If no path found, return False (dead end)
    return False

# === Print Maze ===
def print_maze():
    for row in maze:
        print(''.join(row))  # Join each row list into a string and print

# === Main Execution ===
random.seed()  # Initialize random generator

make_paths(1, 1)  # Start carving the maze from (1,1)
maze[1][1] = ' '  # Make sure start point is a path
maze[SIZE - 2][SIZE - 2] = ' '  # Make sure exit point is a path

solve(1, 1)  # Find path from start to exit

print_maze()  # Display the final maze with the path
