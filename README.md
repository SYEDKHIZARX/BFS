# BFS
CODE

# Breadth-First Search on a medium Pacman-style maze
# S = start, G = goal, # = wall, . = free

from collections import deque

MEDIUM_MAZE = [
    "####################",
    "#S................G#",
    "#.####.######.####.#",
    "#....#......#......#",
    "####.#.####.#.####.#",
    "#....#......#......#",
    "#.####.######.####.#",
    "#......#......#....#",
    "#.####.#.####.#.##.#",
    "#......#......#....#",
    "####################",
]

# ---------- Helper functions ----------
def parse_maze(maze):
    h, w = len(maze), len(maze[0])
    start = goal = None
    walls = set()
    for r in range(h):
        for c in range(w):
            ch = maze[r][c]
            if ch == "S":
                start = (r, c)
            elif ch == "G":
                goal = (r, c)
            elif ch == "#":
                walls.add((r, c))
    return h, w, start, goal, walls


def neighbors(pos, h, w, walls):
    r, c = pos
    for dr, dc, action in [(-1,0,"N"), (1,0,"S"), (0,-1,"W"), (0,1,"E")]:
        nr, nc = r + dr, c + dc
        if 0 <= nr < h and 0 <= nc < w and (nr, nc) not in walls:
            yield (nr, nc), action


# ---------- BFS implementation ----------
def bfs_solve(maze):
    h, w, start, goal, walls = parse_maze(maze)
    queue = deque([(start, [])])
    visited = set([start])
    expanded = 0

    while queue:
        state, path = queue.popleft()
        expanded += 1
        if state == goal:
            return path, expanded
        for nxt, act in neighbors(state, h, w, walls):
            if nxt not in visited:
                visited.add(nxt)
                queue.append((nxt, path + [act]))
    return None, expanded


# ---------- Path rendering ----------
def render_path(maze, path):
    if not path:
        return "\n".join(maze)
    h = len(maze)
    grid = [list(row) for row in maze]
    _, _, start, goal, _ = parse_maze(maze)
    moves = {"N":(-1,0), "S":(1,0), "W":(0,-1), "E":(0,1)}
    r, c = start
    for act in path:
        dr, dc = moves[act]
        r, c = r + dr, c + dc
        if (r, c) != goal and grid[r][c] == ".":
            grid[r][c] = "•"
    for i in range(h):
        for j in range(len(grid[0])):
            if grid[i][j] == ".":
                grid[i][j] = " "
    return "\n".join("".join(row) for row in grid)


# ---------- Main ----------
if __name__ == "__main__":
    path, expanded = bfs_solve(MEDIUM_MAZE)

    print("=== BFS on Medium Maze ===")
    print("Original Maze:")
    print("\n".join(MEDIUM_MAZE))
    print("\nResult:")
    if path is None:
        print("No path found.")
    else:
        print(f"Path actions: {path}")
        print(f"Path length: {len(path)}")
        print(f"Nodes expanded: {expanded}")
        print("\nDiagram:")
        print(render_path(MEDIUM_MAZE, path))
