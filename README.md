# Chess Player

## Overview

Objective: create an intelligent agent to serve as a chess player using the minimax algorithm 

Implementation Summary: 
- if current game state is present in openings database, agent chooses move with highest weight
- else, agent uses minimax algorithm to choose move

Change Log: 
- 12/22/2023: overhaul to openings system; replaced PGN parsing with consultation of polyglot openings book

## Project Details

**_python-chess_**

All board rendering and core game logic (e.g., legal move validation, checkmate, etc.) is handled by the python-chess library. To play a game, we...
1. create on object of class "Board": ```game = chess.Board()```
2. use the "push" method to pass moves to the board: ```game.push_san('e2e4')```
   
   - different variations of the push method are available for different move input class types
   - python-chess has its own "Move" class which is used in conjuction with the "move" method; for string moves, we use the "push_san" method
  
Here's what the starting position of a chess game looks like in python-chess: 

<img src="/images/starting-board.png" width="250">

For more on python-chess, please visit [https://python-chess.readthedocs.io/en/latest/core.html]. 

_Opening Moves_

This project makes use of an chess openings book, which is a collection of the "best" starting moves. Chess openings books typically come in a polyglot format[https://www.chessprogramming.net/new-version-of-the-baron-v3-43-plus-the-barons-polyglot-opening-book/]. 

For a given g

The openings book 

_Minimax_

_Minimax Optimizations_ 


