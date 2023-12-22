# Chess Player

## Overview

Objective: create an intelligent agent to serve as a chess player using the minimax algorithm 

Implementation Details: 
- if current game state is present in openings database, agent chooses move with highest weight
- else, agent uses minimax algorithm to choose move

Change Log: 
- 12/22/2023: overhaul to openings system; replaced PGN parsing with polyglot openings book consultation

## Project Specifics

#### Board Rendering

This project makes use of python-chess, which is a chess library for python that handles the game rules and general user interface. 

<img src="/images/starting-board.png" width="500" height="500">


