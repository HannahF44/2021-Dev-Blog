---
title: Battlesnake Project
date: 2021-04-19
---

## Questions and Answers

Q: How did your understanding of the problem evolve over the two weeks?
A: It's still the same but I did understand some things about the coding.

Q: What role did you play in your team? How did you contribute?
A: My role in the team is to type all the code and basically everything.

Q: What does it mean to think algorithmically?
A: It's basically like planning the turning and the where the snake is going.

Q: What did you learn about problem solving? How did you break the problem down?
A: I learned how to find the problem with a partner and together find how to solve the problem. 



## Pain

The absolute hardest part about the whole project is how to understand what the code is doing.


## Gif

This is a gif of my snake going against itself for a challenge.

![Snake going against itself](https://exporter.battlesnake.com/games/e0662291-e9b9-49c6-9c7d-cc30263741e0/gif)


## Code of Snake

This is the code for the snake.

* index.js

```javascript
const bodyParser = require('body-parser')
const express = require('express')
const routes = require('./lib/routes')

const PORT = process.env.PORT || 3000

const app = express()
app.use(bodyParser.json())

app.get('/', routes.handleIndex)
app.post('/start', routes.handleGameData)
app.post('/move', routes.handleMove)
app.post('/end', routes.handleGameData)

app.listen(PORT, () => console.log(`Battlesnake Server listening at http://127.0.0.1:${PORT}`))
```

* config.json

```javascript
{
    "apiversion": "1",
    "author": "",
    "color": "#008000",
    "head": "smile",
    "tail": "small-rattle",
    "logStartEnd": false
}
```

* .replit

```javascript
language = "nodejs"
run = "npm start"
```
* .gitignore
`
```javascript
*.DS_Store
.vscode
node_modules
```

* routes.js

```javascript
const config = require("../config.json")
const moves = require("./moves.js")

module.exports = {
  handleIndex: (request, response) => {
    var battlesnakeInfo = {
      apiversion: config.apiversion,
      author: config.author,
      color: config.color,
      head: config.head,
      tail: config.tail
    }
    response.status(200).json(battlesnakeInfo)
  },

  handleMove: (request, response) => {
    var gameData = request.body
    var height = gameData.board.height
    var width = gameData.board.width
    var head = gameData.you.head
    var neck = gameData.you.body[1]
    var body = gameData.you.body

    


    var possibleMoves = ['up', 'down', 'left', 'right']

    if (!moves.canMoveUp(height, head.y)) {
      possibleMoves = possibleMoves.filter(move => move != 'up')
    }

    if (!moves.canMoveLeft(head.x)) {
      possibleMoves = possibleMoves.filter(move => move != 'left')
    }

    if (!moves.canMoveRight(width, head.x)) {
      possibleMoves = possibleMoves.filter(move => move != 'right')
    }

    if (!moves.canMoveDown(head.y)) {
      possibleMoves = possibleMoves.filter(move => move != 'down')
    }

// Snake not crashing into its body
    if (!moves.turnLeftBody(head.x, body.x)) {
      possibleMoves = possibleMoves.filter(move => move != 'right')
    }
    
    if (!moves.turnRightBody(head.x, body.x)) {
      possibleMoves = possibleMoves.filter(move => move != 'left')
    }

    if (!moves.backwardBody(head.y, body.y)) {
      possibleMoves = possibleMoves.filter(move => move != 'up')
    }

    if (!moves.forwardBody(head.y, body.y)) {
      possibleMoves = possibleMoves.filter(move => move != 'down')
    }
// Snake not crashing into it's neck

    if (!moves.turnLeftNeck(head.x, neck.x)) {
      possibleMoves = possibleMoves.filter(move => move != 'right')
    }

    if (!moves.forwardNeck(head.y, neck.y)) {
      possibleMoves = possibleMoves.filter(move => move != 'down')
    }

    if (!moves.backwardNeck(head.y, neck.y)) {
      possibleMoves = possibleMoves.filter(move => move!= 'up')
    }
    
    if (!moves.turnRightNeck(head.x, neck.x)) {
      possibleMoves = possibleMoves.filter(move => move!= 'left')
    }

    possibleMoves = moves.checkBody(body, possibleMoves)

    console.log(possibleMoves)


    var move = possibleMoves[Math.floor(Math.random() * possibleMoves.length)]

    console.log('MOVE: ' + move)
    response.status(200).send({
      move: move
    })
  },

  handleGameData: (request, response) => {
    var gameData = request.body

    if (config.logStartEnd) {
      console.log(gameData)
    }
    response.status(200).send('ok')
  }
}
```

* moves.js

```javascript
module.exports = {
  canMoveUp: (boardHeight, snakeY) => {
    if (boardHeight - 1 == snakeY) {
      return false;
    }
    return true;
  },

  canMoveLeft: (snakeX) => {
    if (0 == snakeX) {
      return false;
    }
    return true;
  },

  canMoveRight: (boardWidth, snakeX) => {
    if (boardWidth - 1 == snakeX) {
      return false;
    }
    return true;
  },

  canMoveDown: (snakeY) => {
    if (0 == snakeY) {
      return false;
    }
    return true;
  },
//not crash into it's neck

  turnLeftNeck: (headX, neckX) => {
    if (headX < neckX) {
      return false;
    }
    return true;
  },

  forwardNeck: (headY, neckY) => {
    if (headY > neckY) {
      return false;
    }
    return true;
  },

  turnRightNeck: (headX, neckX) => {
    if (headX > neckX) {
      return false;
    }
    return true;
  },
  
  backwardNeck: (headY, neckY) => {
    if (headY < neckY) {
      return false;
    }
    return true;
  },
 
 // not crash into it's body

  turnLeftBody: (headX, bodyX) => {
    if (headX < bodyX) {
      return false;
    }
    return true;
  },

  turnRightBody: (headX, bodyX) => {
    if (headX > bodyX) {
      return false;
    }
    return true;
  },
  
  backwardBody: (headY, bodyY) => {
    if (headY < bodyY) {
      return false;
    }
    return true;
  },

  forwardBody: (headY, bodyY) => {
    if (headY > bodyY) {
      return false;
    }
    return true;
  },

  checkBody: (body, possibleMoves) => {
    var head = body[0]
    body.forEach(section => {
      if(head.x - 1 == section.x && head.y == section.y) {
        possibleMoves = possibleMoves.filter(move => move != 'left')
      }

      if(head.y - 1 == section.y && head.x == section.x) {
        possibleMoves = possibleMoves.filter(move => move != 'down')
      }

      if(head.x + 1 == section.x && head.y == section.y) {
        possibleMoves = possibleMoves.filter(move => move != 'right')
      }

      if(head.y + 1 == section.y && head.x == section.x) {
        possibleMoves = possibleMoves.filter(move => move != 'up')
      }
    })
    return possibleMoves
  }
}
```
