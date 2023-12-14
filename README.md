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

Enjoy the game!
