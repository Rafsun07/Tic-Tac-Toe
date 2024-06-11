# Tic-Tac-Toe

This project is a simple implementation of the classic Tic-Tac-Toe game, allowing a human player to compete against a computer player. The computer player makes random moves.

## Features

- Human vs Computer gameplay
- Computer makes random moves
- Visual representation of the board in the terminal
- Detects wins, losses, and ties

## Installation

1. **Clone the repository:**

    ```
    git clone https://github.com/Rafsun07/Tic-Tac-Toe.git
    cd tic-tac-toe
    ```

2. **Run the game:**

    ```
    python game.py
    ```

## Usage

1. **Start the game:**

    Run the `game.py` script to start the tic-tac-toe game.

    ```
    python game.py
    ```

2. **Input Prompts:**

    The game will prompt the human player (playing as 'O') to input their moves. The computer (playing as 'X') will make random moves.

    ```
    0 | 1 | 2
    3 | 4 | 5
    6 | 7 | 8
    O's turn. Input move (0-8): 0
    X makes a move to square 1
    O | X |  
      |   |  
      |   |  
    ```

## How the Code Works

### Code Overview:

The code is divided into several parts:

### 1. Player Classes:

- `Player`: A base class representing a player in the game.
- `HumanPlayer`: Inherits from `Player` and handles human input for moves.
- `RandomComputerPlayer`: Inherits from `Player` and makes random moves for the computer player.

### 2. Tic-Tac-Toe Game Logic:

- `TicTacToe`: A class that represents the game board and handles game mechanics.
### 3. Main Game Loop:

- `play`: A function that runs the game loop, alternating turns between the human player and the computer player.

### Player Classes:

Player

The `Player` class is a base class that initializes a player with a given letter ('X' or 'O'). It has a method `get_move` that is meant to be overridden by subclasses.

```
class Player():
    def __init__(self, letter):
        self.letter = letter

    def get_move(self, game):
        pass
```

Human Player

The `HumanPlayer` class inherits from `Player` and overrides the `get_move` method to prompt the human player for input.

```
class HumanPlayer(Player):
    def __init__(self, letter):
        super().__init__(letter)

    def get_move(self, game):
        valid_square = False
        val = None
        while not valid_square:
            square = input(self.letter + '\'s turn. Input move (0-9): ')
            try:
                val = int(square)
                if val not in game.available_moves():
                    raise ValueError
                valid_square = True
            except ValueError:
                print('Invalid square. Try again.')
        return val

```

RandomComputerPlayer

The `RandomComputerPlayer` class inherits from `Player` and overrides the `get_move` method to select a random move from available moves.

```
class RandomComputerPlayer(Player):
    def __init__(self, letter):
        super().__init__(letter)

    def get_move(self, game):
        square = random.choice(game.available_moves())
        return square

```

### Tic-Tac-Toe Game Logic:

TicTacToe

The `TicTacToe` class handles the game board and game logic. It initializes the game board, checks for winners, and manages moves.

```
class TicTacToe():
    def __init__(self):
        self.board = self.make_board()
        self.current_winner = None

    @staticmethod
    def make_board():
        return [' ' for _ in range(9)]

    def print_board(self):
        for row in [self.board[i*3:(i+1) * 3] for i in range(3)]:
            print('| ' + ' | '.join(row) + ' |')

    @staticmethod
    def print_board_nums():
        number_board = [[str(i) for i in range(j*3, (j+1)*3)] for j in range(3)]
        for row in number_board:
            print('| ' + ' | '.join(row) + ' |')

    def make_move(self, square, letter):
        if self.board[square] == ' ':
            self.board[square] = letter
            if self.winner(square, letter):
                self.current_winner = letter
            return True
        return False

    def winner(self, square, letter):
        row_ind = math.floor(square / 3)
        row = self.board[row_ind*3:(row_ind+1)*3]
        if all([s == letter for s in row]):
            return True
        col_ind = square % 3
        column = [self.board[col_ind+i*3] for i in range(3)]
        if all([s == letter for s in column]):
            return True
        if square % 2 == 0:
            diagonal1 = [self.board[i] for i in [0, 4, 8]]
            if all([s == letter for s in diagonal1]):
                return True
            diagonal2 = [self.board[i] for i in [2, 4, 6]]
            if all([s == letter for s in diagonal2]):
                return True
        return False

    def empty_squares(self):
        return ' ' in self.board

    def num_empty_squares(self):
        return self.board.count(' ')

    def available_moves(self):
        return [i for i, x in enumerate(self.board) if x == " "]
```

### Main Game Loop:

The `play` function manages the game loop, alternating between the human player and the computer player until there is a winner or a tie.

```
def play(game, x_player, o_player, print_game=True):
    if print_game:
        game.print_board_nums()

    letter = 'X'
    while game.empty_squares():
        if letter == 'O':
            square = o_player.get_move(game)
        else:
            square = x_player.get_move(game)
        if game.make_move(square, letter):
            if print_game:
                print(letter + ' makes a move to square {}'.format(square))
                game.print_board()
                print('')

            if game.current_winner:
                if print_game:
                    print(letter + ' wins!')
                return letter  # ends the loop and exits the game
            letter = 'O' if letter == 'X' else 'X'  # switches player

        time.sleep(.8)

    if print_game:
        print('It\'s a tie!')

```

### Runinning the Game

The game is started by creating instances of the players and the `TicTacToe` game, then calling the `play` function.

```
if __name__ == '__main__':
    x_player = RandomComputerPlayer('X')
    o_player = HumanPlayer('O')
    t = TicTacToe()
    play(t, x_player, o_player, print_game=True)

```

This script sets up a Tic-Tac-Toe game where a human player ('O') plays against a computer player ('X') that makes random moves. The game prints the board state after each move and announces the winner or a tie.

## Contributing

Contributions are welcome! Please fork the repository and create a pull request with your changes. Ensure that your code follows the project's coding standards and includes appropriate tests.

## Contact

For questions, suggestions, or issues, please open an issue on GitHub or contact the project maintainer at [rafsun.eram@gmail.com](mailto:rafsun.eram@gmail.com).

## Acknowledgements

- Developed using Python.
- Inspired by classic Tic-Tac-Toe game.

---
