# Chess Player

## Overview

Objective: create an intelligent agent to serve as a chess player using the minimax algorithm 

Implementation Details: 
- if current game state is present in openings database, agent chooses move with highest weight
- else, agent uses minimax algorithm to choose move

Change Log: 
- 12/22/2023: overhaul to openings system; replaced PGN parsing with polyglot openings book consultation

## Introduction

Chess is a game where involving two players, each in control of their own army of pieces. 

![starting-board](/images/starting-board.png)


