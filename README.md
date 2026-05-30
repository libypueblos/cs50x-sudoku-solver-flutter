# Sudoku Solver – Flutter App
#### Video Demo:  <https://youtu.be/p1llqPhWUes>
#### Description: This project is a Sudoku Solver application built using Flutter. It supports solving puzzles of different sizes, specifically **4×4**, **6×6**, and **9×9** grids. The goal of this project is to combine a clean and interactive user interface with a reliable solving algorithm based on `Constraint Satisfaction Problems` (CSP).

The solver itself is implemented in Dart and is based on a port of an earlier Python implementation. It uses a combination of `AC-3` (Arc Consistency) and `backtracking` search, enhanced with heuristics such as `Minimum Remaining Values` (MRV) and `Least Constraining Value` (LCV) to efficiently compute solutions. The application allows users to either manually input puzzles or upload them via a text file.

Supported targets: Chrome (web), Android, iOS.

---
## Project Structure and File Overview

```
sudoku_app/
├── lib/
│   ├── main.dart                        # App entry point
│   ├── logic/
│   │   ├── csp.dart                     # AC-3 + Backtracking 
│   │   ├── sudoku_csp.dart              # Sudoku-specific CSP 
│   │   └── puzzle_parser.dart           # Validates & parses puzzle text files
│   └── ui/
│       ├── theme/
│       │   └── theme.dart               # Colors, ThemeData
│       ├── widgets/
│       │   └── sudoku_grid.dart         # Reusable grid (editable + display modes)
│       └── screens/
│           ├── home_screen.dart         # Tab controller (4×4 / 6×6 / 9×9)
│           ├── input_screen.dart        # Manual entry + file upload
│           └── result_screen.dart       # Solved grid or "No solution"
├── python_solver/                       # Original Python source (reference only)
│   ├── csp.py
│   ├── sudoku_csp.py
│   ├── sudoku_solver.py
│   └── data_io.py
└── pubspec.yaml
```

---
### Main Layer

`main.dart`
Entry point of the app. Initializes Flutter and sets the theme and home screen.

---
### Logic Layer

`csp.dart`
Contains the core CSP solver using AC-3 and backtracking with heuristics.

`sudoku_csp.dart`
Adapts the CSP solver specifically for Sudoku rules (rows, columns, boxes).

`puzzle_parser.dart`
Validates and parses puzzle input from text or files.

---
### UI Layer

`theme.dart`
Defines colors and overall UI styling.

`sudoku_grid.dart`
Reusable widget for displaying and editing the Sudoku grid.

`home_screen.dart`
Provides tab navigation for different puzzle sizes.

`input_screen.dart`
Handles puzzle input, file upload, validation, and solving trigger.

`result_screen.dart`
Displays the solved puzzle, loading state, or no-solution message.

---
## Solver Logic Overview

The core of the application is based on a Constraint Satisfaction Problem (CSP) model. In this approach, the Sudoku puzzle is treated as a set of variables, domains, and constraints.

Each cell in the grid is treated as a variable. The domain of each variable is the set of possible values it can take. For example, in a 9×9 puzzle, the domain is typically the numbers 1 through 9. If a cell is already filled, its domain is reduced to a single value.

Constraints define the rules of Sudoku. These include ensuring that no number is repeated within the same row, column, or subgrid. These constraints are used to eliminate invalid possibilities early in the solving process.

The solver first applies the AC-3 (Arc Consistency) algorithm. This step reduces the domains of variables by removing values that violate constraints. In simple terms, it narrows down the possible values for each cell before attempting to search for a complete solution.

After enforcing arc consistency, the solver uses backtracking search. This involves selecting an unassigned variable, assigning it a value, and recursively attempting to solve the rest of the puzzle. If a conflict is found, the solver backtracks and tries a different value.

Heuristics like MRV and LCV are used to improve efficiency. MRV selects the variable with the fewest remaining possible values, which helps detect conflicts earlier. LCV prioritizes values that impose the least restriction on neighboring variables, increasing the chances of finding a valid solution faster.

This combination of constraint propagation and guided search allows the solver to handle puzzles efficiently while keeping the implementation relatively understandable.

---
## Design Decisions

One key design decision was to separate the solver logic from the UI. This makes the application easier to maintain and allows the solving algorithm to be reused independently of the interface.

Another important decision was to implement the solver using a `CSP` approach instead of a simple brute-force method. While brute force is easier to implement, CSP with heuristics significantly improves performance, especially for larger grids like 9×9.

Supporting multiple grid sizes (`4×4`, `6×6`, and `9×9`) required additional flexibility in the design. Instead of hardcoding values, the application dynamically adjusts box dimensions and constraints based on the selected size.

For the user interface, the focus was on simplicity and clarity. The grid layout is clean and responsive, and error messages are shown directly where users can easily see and fix issues. The use of a reusable grid widget also helps keep the code organized and avoids duplication.

---
## Use of AI Tools

AI tools were used as a supporting aid during development, particularly for refining parts of the solver logic and improving the user interface. The original CSP implementation was adapted and translated into Dart with the help of AI, but all outputs were reviewed, modified, and integrated manually to ensure correctness and consistency within the app.

---
## Setup & Run

### Prerequisites

* Flutter SDK ≥ 3.0 — https://docs.flutter.dev/get-started/install
* VS Code with the Flutter extension

### Steps

```bash
# 1. Get dependencies
flutter pub get

# 2. Run (choose a device in VS Code status bar, then press F5 / Start Debugging)
flutter run
```

---
### Puzzle file format
Plain `.txt` file. 
Each line is one row. Blanks = `0`. No separators.

Example — 9×9:
```
530070000
600195000
098000060
800060003
400803001
700020006
060000280
000419005
000080079
```
Example — 6×6:
```
000000
015006
400000
000004
600850
000000
```
Example — 4×4:
```
1020
0301
3010
0203
```

---
## User Flow

The application follows a straightforward user flow from input to solution.

First, the user selects a puzzle size using the tabs on the home screen (4×4, 6×6, or 9×9). This determines the structure and constraints of the puzzle.

Next, the user inputs the puzzle. This can be done either manually by typing values into the grid or by uploading a `.txt` file. Empty cells can be left blank or represented using 0.

Once the puzzle is entered, the application validates the input. If there are any issues, such as invalid characters or incorrect formatting, an error message is displayed to guide the user.

After validation, the user presses the “Solve” button. The app then processes the puzzle using the CSP solver. A loading screen is briefly shown while the solution is being computed.

Finally, the result is displayed. If a valid solution is found, the completed grid is shown, with a visual distinction between the original inputs and the solved values. If no solution exists, the app informs the user and allows them to return and adjust the puzzle.

This flow ensures that the user experience remains simple, intuitive, and responsive from start to finish.

---
## Conclusion

This project demonstrates how CSP techniques can efficiently solve Sudoku while providing a clean and intuitive Flutter interface.
