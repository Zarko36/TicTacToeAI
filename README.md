# Tic Tac Toe Game with Minimax AI
This repository contains a Python implementation of the classic game of Tic Tac Toe, with a twist: one of the players is an AI that uses the Minimax algorithm to determine its moves.

## Game Classes
The game is implemented using three classes:

* TTT_cs170_judge: This class represents the game board and the rules of the game.
* Player_1: This class represents a human player.
* Player_2: This class represents an AI player.

## Minimax Algorithm
The AI player uses the Minimax algorithm to decide its moves. The Minimax algorithm is a recursive algorithm used for decision making in game theory and artificial intelligence.

At the heart of the Minimax algorithm is a hypothetical scenario where both players play optimally. The algorithm considers all possible moves, simulates the game for each move, and calculates a score that measures the desirability of that move.

The AI player aims to maximize its score and minimize the human player's score. It does this by exploring all possible moves to a certain depth, simulating alternating turns between the AI and the human player.

## Alpha-Beta Pruning
The Minimax algorithm can be quite slow due to the number of nodes it needs to evaluate. This is where Alpha-Beta pruning comes in. Alpha-Beta pruning is an optimization technique that reduces the number of nodes that the Minimax algorithm needs to evaluate, thus speeding up the computation.

In the minimax function, a and b represent the best scores that the maximizing and minimizing player, respectively, can achieve. As the function explores further down the game tree, it updates these scores and uses them to prune branches, i.e., to stop evaluating a move when it's certain that it's worse than a previously examined move.

## How to Run the Game
To play the game, simply run the tic_tac_toe.py script. The game will randomly decide who goes first. If you're playing against the AI, it will make its move instantly. If it's your turn, you'll be asked to enter the row and column numbers of your move.

## Future Improvements

One potential area for future improvement is the addition of adjustable difficulty levels for the AI player. Currently, the AI uses the Minimax algorithm to explore all possible moves to a certain depth, simulating alternating turns between the AI and the human player. This ensures that the AI plays optimally, making it a challenging opponent.

However, not all players may want to play against an optimal AI. Some may prefer an easier game, while others may want to gradually increase the difficulty as they get better at the game. To accommodate this, we could add a difficulty setting that adjusts how deep the AI searches through the game tree.

For example, on an easy difficulty setting, the AI could only look one move ahead. On a medium difficulty setting, it could look two moves ahead, and on a hard difficulty setting, it could look three moves ahead. This would make the AI's play less optimal on lower difficulty settings, giving the human player a better chance of winning.

Implementing this feature would involve modifying the `minimax` method in the `Player_2` class to take an additional parameter representing the maximum depth to search. The method would then need to be updated to stop recursing once it reaches this maximum depth.

```
import random
import math


class TTT_cs170_judge:
    def __init__(self):
        self.board = []
        
    def create_board(self, n):
        for i in range(n):
            row = []
            for j in range(n):
                row.append('-')
            self.board.append(row)
            
    def display_board(self):
        for row in self.board:
            print(" ".join(row))
        print()
            
    def is_winner(self, player):
        # Check rows
        for row in self.board:
            if all([cell == player for cell in row]):
                return True
        
        # Check columns
        for col in range(len(self.board)):
            if all([self.board[row][col] == player for row in range(len(self.board))]):
                return True
        
        # Check diagonals
        if all([self.board[i][i] == player for i in range(len(self.board))]):
            return True
        if all([self.board[i][len(self.board) - i - 1] == player for i in range(len(self.board))]):
            return True
        
        return False
    
    def is_board_full(self):
        return all([cell in ['X', 'O'] for row in self.board for cell in row])
    

class Player_1:
    def __init__(self, judge):
        self.board = judge.board
    
    def my_play(self):
        while True:
            row, col = map(int, input("Enter the row and column numbers separated by space: ").split())
            
            if 1 <= row <= len(self.board) and 1 <= col <= len(self.board[0]):
                self.board[row-1][col-1] = 'X'
                break
            else:
                print("Wrong coordination!")


class Player_2:
    def __init__(self, judge):
        self.aiGoesFirst = True
        self.aiGoesSecond = True
        self.judge = judge
        self.board = judge.board
    
    def minimax(self, board, depth, maximizing, a, b):
        if self.judge.is_winner('O'):
            return 1
        elif self.judge.is_winner('X'):
            return -1
        elif self.judge.is_board_full():
            return 0
        if maximizing:
            bestScore = -math.inf
            for i in range(len(board)):
                for j in range(len(board[0])):
                    if board[i][j] == '-':
                        board[i][j] = 'O'
                        score = self.minimax(board, depth + 1, False, a, b)
                        board[i][j] = '-'
                        bestScore = max(score, bestScore)
                        a = max(a, bestScore)
                        if a >= b:
                            break
            return bestScore
        else:
            bestScore = math.inf
            for i in range(len(board)):
                for j in range(len(board[0])):
                    if board[i][j] == '-':
                        board[i][j] = 'X'
                        score = self.minimax(board, depth + 1, True, a, b)
                        board[i][j] = '-'
                        bestScore = min(score, bestScore)
                        b = min(b, bestScore)
                        if a >= b:
                            break
            return bestScore
        
    def my_play(self):
        if self.aiGoesFirst:
            self.aiGoesFirst = False
            self.aiGoesSecond = False
            self.board[0][0] = 'O'
            return 0
        if self.aiGoesSecond:
            if self.board[1][1] == '-':
                self.aiGoesSecond = False
                self.board[1][1] = 'O'
                return 0
            else:
                self.aiGoesSecond = False
                self.board[0][0] = 'O'
                return 0
        bestMove = None
        bestScore = -math.inf
        for i in range(len(self.board)):
            for j in range(len(self.board[0])):
                if self.board[i][j] == '-':
                    self.board[i][j] = 'O'
                    score = self.minimax(self.board, 0, False, -math.inf, math.inf)
                    self.board[i][j] = '-'
                    if score > bestScore:
                        bestScore = score
                        bestMove = (i, j)
        if bestMove:
            self.board[bestMove[0]][bestMove[1]] = 'O'
        
       
     # implment your code here
        
    

# Main Game Loop
def game_loop():
    n = 3  # Board size
    game = TTT_cs170_judge()
    game.create_board(n)
    player1 = Player_1(game)
    player2 = Player_2(game)
    starter = random.randint(0, 1)
    win = False
    if starter == 0:
        print("Player 1 starts.")
        player2.aiGoesFirst = False
        game.display_board()
        while not win:
            player1.my_play()
            win = game.is_winner('X')
            game.display_board()
            if win:
                print("Player 1 wins!")
                break
            if game.is_board_full():
                print("It's a tie!")
                break

            player2.my_play()
            win = game.is_winner('O')
            game.display_board()
            if win:
                print("Player 2 wins!")
                break
            if game.is_board_full():
                print("It's a tie!")
                break
    else:
        print("Player 2 starts.")
        game.display_board()
        while not win:
            player2.my_play()
            win = game.is_winner('O')
            game.display_board()
            if win:
                print("Player 2 wins!")
                break
            if game.is_board_full():
                print("It's a tie!")
                break
            
            player1.my_play()
            win = game.is_winner('X')
            game.display_board()
            if win:
                print("Player 1 wins!")
                break
            if game.is_board_full():
                print("It's a tie!")
                break

#game_loop() # Uncomment this line to play the game
```

