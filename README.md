# Rush Hour Solver: Search Algorithms in Python

A terminal version of the **Rush Hour** sliding-block puzzle, with a set of AI search algorithms that solve it automatically. You can play it yourself or have the program solve it, and compare how uninformed and heuristic search methods perform.

Developed for **Practical Assignment No. 1** of *Elements of Artificial Intelligence and Data Science* (EIACD), 1st year, BSc in Artificial Intelligence and Data Science, 2023/2024.

---

## Table of Contents
- [About the Puzzle](#-about-the-puzzle)
- [Features](#-features)
- [Getting Started](#-getting-started)
- [How to Play](#-how-to-play)
- [Search Problem Formulation](#-search-problem-formulation)
- [Algorithms & Heuristics](#-algorithms--heuristics)
- [Results](#-results)
- [Project Structure](#-project-structure)
- [Authors](#-authors)

---

## About the Puzzle

Rush Hour is played on a **6×6 grid** filled with cars. Each car is 2 or 3 cells long and can only slide forwards or backwards along its own direction (horizontal or vertical). The goal is to slide the other cars out of the way so the **red car (car `0`)** can reach the exit on the right edge of row 3.

```
|#|#|#|#|#|1|
|#|#|#|#|#|1|
|0|0|#|#|#|1|   ← red car (0) must reach the right edge
|#|#|#|2|2|2|
|#|#|3|3|4|4|
|#|#|#|#|#|#|
```
`#` = empty cell · numbers = cars

## Features

- **Human mode**: play any level from the terminal, one move at a time
- **Hint mode**: ask the computer to show you an optimal solution from your current position
- **Algorithm test mode**: run a solver and see:
  - the solution path, step by step
  - the number of nodes generated
  - execution time
  - maximum memory usage
- **9 built-in puzzles**: 3 difficulty levels (easy, medium, hard), 3 versions each
- **Several search strategies**: BFS, DFS, Depth-Limited Search, Greedy Best-First and A*, with two heuristics

## Getting Started

### Requirements
- Python **3.8+**
- No external packages: the program only uses the Python standard library

>  The algorithm test mode uses the `resource` module to measure memory, which is only available on **Linux and macOS**. On Windows, use WSL to run test mode. Play mode works on every system.

### Installation & Run
```bash
git clone https://github.com/luanalegi/rush-hour-solver.git
cd rush-hour-solver
python3 rush_hour.py
```

## How to Play

1. Choose a mode: `1` Play game or `2` Test game algorithms
2. Pick a difficulty (`easy`, `medium`, `hard`) and a version (`1`, `2`, `3`)
3. The board is shown in the terminal. Each turn you can:
   - Type `1` to get help (play mode) or choose a solver (test mode)
   - Type `0` to move a car yourself: enter the car number, then a direction (`up`, `down`, `left`, `right`)
4. The game ends when the red car reaches the exit.

**Example session**
```
- Choose an option -
1: Play game
2: Test game algorithms
1

- Enter the difficulty -
(easy, medium, hard): easy

- Which version do you want to play? -
(1, 2, 3): 2
```

## Search Problem Formulation

| Element | Definition |
|---|---|
| **State** | 6×6 matrix with the position of every car, plus a list of `Car` objects (id, length, start coordinate, orientation) |
| **Initial state** | One of the 9 predefined puzzles |
| **Operators** | Move a car one cell `up`/`down` (vertical cars) or `left`/`right` (horizontal cars) |
| **Preconditions** | The target cell is inside the board and empty |
| **Cost** | 1 per move |
| **Goal test** | The red car's left end is in column 4, so it occupies the two rightmost cells of row 3 |

## Algorithms & Heuristics

### Uninformed search
| Algorithm | Notes |
|---|---|
| **Breadth-First Search (BFS)** | Finds the solution with the fewest moves; used for hint mode |
| **Depth-First Search (DFS)** | Uses little memory, but its solutions are much longer |
| **Depth-Limited Search** | DFS with a maximum depth set by the user |

### Informed search
| Algorithm | Notes |
|---|---|
| **Greedy Best-First Search** | Always expands the state with the lowest heuristic value |
| **A\*** | Orders states by `f(n) = g(n) + h(n)` |

### Heuristics
- **h1, blocking cars**: the number of distinct cars between the red car and the exit. It never overestimates the remaining moves (admissible).
- **h2, Manhattan distance**: the distance from the red car's left end to its goal position (column 4).

## Results

Solution length (number of moves) and nodes generated for each puzzle:

| Level | BFS moves | BFS nodes | DFS moves | DFS nodes | Greedy (h1) nodes |
|---|---:|---:|---:|---:|---:|
| Easy 1   | 10 | 234 | 70  | 81  | 1242 |
| Easy 2   | 7  | 78  | 24  | 35  | 46   |
| Easy 3   | 7  | 428 | 49  | 51  | 365  |
| Medium 1 | 13 | 180 | 30  | 34  | 149  |
| Medium 2 | 10 | 89  | 47  | 49  | 210  |
| Medium 3 | 9  | 48  | 18  | 24  | 54   |
| Hard 1   | 20 | 927 | 158 | 167 | 555  |
| Hard 2   | 11 | 250 | 63  | 65  | 186  |
| Hard 3   | 6  | 39  | 9   | 11  | 66   |

**Key takeaways**
- **BFS** always finds the shortest solution, but on harder levels it has to generate more nodes.
- **DFS** generates few nodes, but its solutions are 2 to 8 times longer than the optimal ones.
- **Greedy** uses the heuristic to guide the search. How well it does depends on the puzzle: it beats BFS on some levels (e.g. Hard 1) and loses on others (e.g. Easy 1).

##  Project Structure

```
rush-hour-solver/
├── rush_hour.py     # Game logic, board/car classes, search algorithms, CLI
├── README.md
└── .gitignore
```

**Main components of `rush_hour.py`**
- `Car`: a single vehicle (id, length, coordinates, orientation)
- `Board`: the grid state, move generation (`next_possible_moves`), goal test, puzzle levels
- `create_board()`: turns a level matrix into `Board` and `Car` objects
- `TreeNode`: a search-tree node that tracks its parent, used to rebuild the solution path
- Search functions: `breadth_first_search`, `depth_first_search`, `depth_limited_search`, `greedy_search`, `a_star_search`
- Heuristics: `h1`, `h2`
- `play_game()`, `test_game()`, `main()`: the text interface

## Authors

- Luana Gibbels (up202208587)
- Leonardo Alzamora (up202101416)
- Diogo Luís (up201704052)


## Acknowledgements

- Puzzle concept: [Rush Hour (Wikipedia)](https://en.wikipedia.org/wiki/Rush_Hour_(puzzle))
- Course: Elements of Artificial Intelligence and Data Science, 2023/2024
