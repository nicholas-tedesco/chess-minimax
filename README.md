# Chess Player

## Overview

Objective: create an intelligent agent to serve as a chess player using the minimax algorithm 

Implementation Summary: 
- if current game state is present in openings database, agent chooses move with highest weight
- else, agent uses minimax algorithm to choose move

Change Log: 
- 12/22/2023: overhaul to openings system; replaced PGN parsing with polyglot openings book consultation

## Project Details

_python-chess_

All board rendering and core game logic (e.g., legal move validation, checkmate, etc.) is handled by the python-chess library. 

<img src="/images/starting-board.png" width="250" height="250">


