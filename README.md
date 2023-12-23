# Chess Minimax

## Overview

Objective: create an intelligent agent to serve as a chess player using the minimax algorithm 

Implementation Summary: 
- if current game state is present in openings database, agent chooses move with highest weight
- else, agent uses minimax algorithm to choose move

Change Log: 
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

This project makes use of an chess openings book, which is a collection of the "best" starting moves for a game of chess. An openings book can be used to search for a given board position in order to find the best possible continuations from the current game state. In this project, we use [The Baron's Polygot Opening Book](https://www.chessprogramming.net/new-version-of-the-baron-v3-43-plus-the-barons-polyglot-opening-book/). 

But what's the point of an openings book if we already have an algorithm to choose moves? 
1. Our recursive minimax algorithm is much more computationally expensive compared to simple position lookup
2. Opening theory is somewhat complex; often times, we are setting up our pieces to attack specific squares, or to be positioned in certain ways for later on in the game. The minimax algorithm will not have the same level of positional chess knowledge compared to a hard-coded openings book

Opening books for chess engines typically follow a polyglot file format, which basically links moves and associated weights to a hashed position. The best move is the one which maximizes the weight for a given position.

For more on polyglot file structure, please visit http://hgm.nubati.net/book_format.html.

### _Minimax_

### _Algorithm Optimizations_


