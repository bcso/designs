# TicTacToe

Features
- Allow 2 player tic tac toe on same browser
- Frontend state will update on user making moves, game will reload with most recent state for given sessionID
  - Start with hardcoded single sessionID
- User can save their game at any time
- Game can be reloaded
  - Reload hardcoded single sessionID first
- Data will be persisted in redis, S3

## Game FrontEnd

Below is how each functionality in the game will work. Front End will handle all state in `gameState` react state which will be flushed to storage by user choice. Data will be loaded into `gameState` on resume.

## Components
- TicTacToe [ct]
  - GameBoard [ct]
    - state: produces gameState (Lifted)
  - MoveHistory [ct]
    - state: consumes gameState (from GameBoard)
    - flushes gameState to storage

### Playing the Game
Front end will allow the two players to make their moves and update the `gameState.boardState` object, which is simply the react State.

### Saving the Game
Front end will allow any player to press "Save" anytime to flush the entire `gameState` React State to storage via POST call to our GameService webservice.


## GameService

gameState Recieved from Clients:
gameState = {
  "boardState": {
    "P1": {<coords>},
    "P2": {<coords>},
    "winner": <string>
  },
  "sessionID": "gameName0"
}

gameSession Stored to S3/Redis:
gameSession = {
	gameState[sessionID] : gameState
}
