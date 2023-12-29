# Chess Minimax

## Overview

Objective: create an intelligent agent to serve as a chess player using the minimax algorithm 

Implementation Summary: 
- if current game state is present in openings database, agent chooses move with highest weight
- else, agent uses minimax algorithm to choose move

Change Log: 
- 12/29/2023: recoded minimax class to implement alpha-beta pruning + iterative deepening 
- 12/22/2023: overhaul to openings system; replaced PGN parsing with consultation of polyglot openings book
- 12/09/2023: added explored game state lookup to minimax algorithm using Zobrist hashing 

## Project Details

### _python-chess_

All board rendering and core game logic (e.g., legal move validation, checkmate, etc.) is handled by the python-chess library. To play a game using python-chess, we... 
1. create on object of class "Board": ```game = chess.Board()```
2. "push" moves to the board: ```game.push_san('e2e4')```
   
   - different variations of the push method are available for different classes of move inputs 
   - python-chess has its own "Move" class, which is used in conjuction with the "move" method
   - for string moves in standard algebraic notation (SAN), we use the "push_san" method
  
Here's what the starting position of a chess game looks like in python-chess: 

<img src="/images/starting-board.png" width="400">

For more on python-chess, please visit [python-chess.readthedocs.io](https://python-chess.readthedocs.io/en/latest/core.html).

### _Opening Move Selection_

This project makes use of an chess openings book, which is a collection of the "best" starting moves for a game of chess. An openings book can be used to search for a given board position in order to find the best possible continuations from the current game state. In this project, we use the [Titans Opening Book](https://chess.stackexchange.com/questions/35448/looking-for-polyglot-opening-books) from Flavio Martin. 

But what's the point of an openings book if we already have an algorithm to choose moves? 
1. Our recursive minimax algorithm is much more computationally expensive compared to simple position lookup
2. Opening theory is somewhat complex; often times, we are setting up our pieces to attack specific squares, or to be positioned in certain ways for later on in the game. The minimax algorithm will not have the same level of positional chess knowledge compared to a hard-coded openings book

Opening books for chess engines typically follow a polyglot file format, which basically links moves and associated weights to a hashed position. For example, consider the following position and it's associated moves/weights: 

<p float="middle">
  <img src="/images/test-opening-board.png" width="400" align="middle">
  <img src="/images/test-opening-moves.png" width="150" align="middle">
</p>

As you can see, our openings book contains a few possible continuations from this position. Although one move is technically the "best" in terms of weights, the other moves are also viable. In order to introduce some variability into this chess engine's openings, I decided to to use weighted random selection to choose moves from the opening book. 

For more on polyglot file structure, please visit http://hgm.nubati.net/book_format.html.

### _Minimax_

Minimax is a recursive, depth-first seach algorithm used to choose the most optimal move for a given game scenario. In order to implement minimax, we must be able to represent our game as a tree structure, where the root node is our starting game state and child nodes are subsquent game states reached by making a particular move. Minimax also relies on the use of a heuristic function, which assigns a score to a provided game state. 

The core objective behind minimax is to maximize score on our turn, and minimize score on our opponent's turn. This is completely reasonable; we should always prefer moves that yield the best possible score for our own player, whereas our opponent should always prefer moves that result in the worst possible score for our player. With this in mind, we can assign alternating "max" and "min" layers as we proceed down the game tree, starting with a max layer at the root node. We continue down the game tree in this fashion until reaching terminal nodes, which may be actual leaf nodes (i.e., representing game over), or simply nodes at the depth level specified for our search. The heuristic function is used to assign scores to the game states at the terminal nodes. Once we have the scores at the terminal nodes, we can make our way back up the tree by taking the maximum or minimum score of the child nodes (depending on the layer) until reaching the root node. 

Consider the following example (taken from [baeldung.com](https://www.baeldung.com/java-minimax-algorithm)): 

<img src="/images/minimax-demo.png" width="800" align="middle">

Once we know our scores for the bottom four terminal nodes, we can proceed back up the tree by maximizing and minimizing on the appropriate layers. In this example, node values for the second layer of the tree are assigned by taking the minimum of child node scores in the third layer. Node values for the first layer (i.e., root node) are assigned by taking the maximum of child node scores in the second layer. 

Arguably the most important aspect of the minimax algorithm is the heuristic function, considering this is what the algorithm uses to understand the favorability of a certain game state. In this project, we are using a heuristic function based on both material and position. In terms of material, we assign certain weights to each of the chess pieces (e.g., if we are white, white queen = 900, black queen = -900, white rook = 500, etc.), and calculate material score by summing the weights for the pieces on the board. In terms of position, we encourage the algorithm to place pieces on "better" squares by providing position tables for each piece type. A chess position table is simply a collection of all possible positions and associated scores for a given piece type. Consider the provided position table for a knight: 

<img src="/images/knight-position-table.png" width="400" align="middle">

We assign the most negative scores to the positions in which the knight controls the fewest squares (edges + corners of the board) and the most positive scores to the positions in which the knight controls the most squares (center of the board). As you might imagine, position tables can be a bit subjective depending on the given player's preferences for piece positioning. This project makes use of existing position tables taken from the [chess programming wiki page for evaluation functions](https://www.chessprogramming.org/Simplified_Evaluation_Function).

### _Algorithm Optimizations_

Minimax can be quite slow when we have a high branching factor or depth of search. However, there are certain optimizations that can allow us to make fewer recursive calls, thus placing less computational stress on our algorithm. Here are the optimizations that I use in my implementation of minimax: 

Transposition Tables  
- a transposition table is a dictionary of game states mapped to their associated scores   
- if the game state exists in the transposition table, simply use the score from the table instead of continuing down the more intensive recursive evaluation path  
- Zobrist hashing is a common technique used to convert a game state into an acceptable key format for transposition dictionaries; please visit the [Zobrist hashing chess programming wiki page](https://www.chessprogramming.org/Zobrist_Hashing) for more info on the topic  
   
Alpha-Beta Pruning   
- game tree pruning allows us to skip over the evaluation of certain nodes / paths while still finding the most optimal solution
- to explain alpha-beta pruning, consider the example below:
- we can maximize the amount of prune nodes by exploring the best moves first; see the iterative deepening section for more on how move sorting is implemented in this project 

Iterative Deepening  
- iterative deepening is the process of incrementing our depth of search until reaching a goal / maximum depth
   - for example, if we had a goal depth of 5, we might perform minimax search to a depth of 2 + save the move, then re-perform minimax search to a depth of 3 and save the move, and so on
   - in this way, we are "iteratively deepening" our search
- we specify a time limit for the iterative search in order to guarantee that the algorithm does not take a long time to search
   - even if we exceed the time limit, we are guaranteed to have a chosen move from a previous iteration of the algorithm (so long as our minimum search depth is not too high)
   - this also has implications for search depth at a given stage of the game:
      - for chess, we have WAY more moves to explore at the beginning, so our searches may take longer (and iterative deepening may be a bit more shallow)
      - at the end of the game, we may have less possible moves to explore, allowing us to search faster and reach a higher depth
- alpha-beta pruning can be optimized by storing the best moves from the most recent (complete) iterative deepening search, then searching over these moves first at the next depth level

### _Future Directions_ 
