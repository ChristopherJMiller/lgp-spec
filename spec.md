# `libmahjong` Game Playback (LGP) File Spec
*Rev 1*

## Motivation

`libmahjong` provides an effective way to run riichi mahjong matches between AI players. However, there is no way to observe matches in realtime, as this would greatly impact engine performance. Realtime viewing would require either front-end state tracking or large game event packets to always keep in sync with the engines results. Therefore, this specification presents a solution inspired by the Portable Game Notation (PGN) used in playing back chess games.

This file specification must be light weight, due to the number of events in a given match, yet still easily parsable and compatible with platforms to ensure an open ecosystem of use-cases.

## Description

`libmahjong` Game Playback (LGP) files will be used to playback Riichi Mahjong games that conform to the signal specifications of the `libmahjong` library. The format is based on a CSV plain-text presentation in order to ensure compatibility, with a custom data ordering that focuses on compactness.

# Specification

## Preamble

The preamble represents all introduction information for a given match, which includes player starting positions, player names, the base point start, and the type of game (3 player or 4 player)

Example:

```
4, # Number of Players
25000, # Number of Starting Points
Player 1 Name,Player 2 Name,Player 3 Name,Player 4 Name # Names in the order of E,S,W,N
...
```

## Match Data

Match data is stored as a list of tuples, which represents the signals originally sent to players. The shape of the entry is controlled by the type of event.

### Player Affecting Events
Events such as drawing and calling

```
15,337,0 # Event id, Piece id, Player id
```

### Game Affecting Events
Events such as doras, point diffs

```
8,337 # Dora Event (event id, piece id)
12,20,0 # Point Diff Event (event id, score / 100, player id)
88 # Game End
```