<template>
  <div class="hello">
     <div class="row">
      <div class="col-md-12">
        <b-container class="bv-example-row board">
          <b-row v-for="row in board">
            <b-col class="tile" v-for="element in row">{{ element==0 ? '':element}}</b-col>
          </b-row>
        </b-container>
      </div>
      <div class="col-md-12 board" v-if="isCompleted">
        <h2>You've Won the Game</h2>
      </div>
      <div class="col-md-12 board" v-if="isOver">
        <h2>You've Lost the Game</h2>
      </div>
     </div>
     <div class="row">
        <div class="col-md-6 pb-2 new-game" >
            <b-button size="lg" variant="primary" v-on:click="resetBoard()">
                New Game
            </b-button>
        </div>
        <div class="col-md-6 pb-2 score" >
            <h3>Score <b-badge>{{score}}</b-badge></h3>
        </div>
     </div>
  </div>
</template>

<script>
export default {
  name: "Game",
  data() {
    return {
      board: [],
      score:0,
      isCompleted:false,
      has2048:false,
      isOver:false,
    };
  },
  beforeMount(){
    this.resetBoard();
  },
  methods: {
    setArrowEvents(enable){
      if(enable){
        const that = this;
        document.onkeydown = function() {
          switch (window.event.keyCode) {
            case 37:
              that.updateBoard(that.board, "left"); // execute a function by passing parameter
              break;
            case 38:
              that.updateBoard(that.board, "up");
              break;
            case 39:
              that.updateBoard(that.board, "right");
              break;
            case 40:
              that.updateBoard(that.board, "down");
              break;
          }
        };
      }else{
        const that = this;
        document.onkeydown = function() {
          switch (window.event.keyCode) {
            case 37: break;
            case 38: break;
            case 39: break;
            case 40: break;
          }
        };
      }
    },
    resetBoard(){
      this.board = [[0,0,0,0],[0,0,0,0],[0,0,0,0],[0,0,0,0]];
      for(let i=0;i<3;i++){
        let { updatedBoard } = this.placeTile(this.board);
        this.board = updatedBoard;
      }
      this.score = 0;
      this.isCompleted = false;
      this.isOver = false;
      this.setArrowEvents(true);
    },
    updateBoard(board, direction) {
      let swipedBoard = this.swipeBoard(direction, board);
      if(this.board.toString()==swipedBoard.toString()){
        if(this.isGameOver()){
          this.isOver = true;
          this.setArrowEvents(false);
        }
        return false;
      }else{
        this.board = swipedBoard;
        let { updatedBoard, updatedTile } = this.placeTile(this.board);
        this.board = updatedBoard;
      }
      if(this.has2048){
        this.isCompleted = true;
        this.setArrowEvents(false)
      }
    },
    isGameOver(){
      let emptyTiles = [];
      for (let i = 0; i < this.board.length; i++) {
        for (let j = 0; j < this.board[i].length; j++) {
          if (this.board[i][j] === 0) {
            emptyTiles.push([i, j]);
          }
        }
      }
      if(emptyTiles.length==0){
        return true;
      }
      return false;
    },
    rotate2DArray(board, direction) {
      let newBoard = [];
      switch (direction) {
        case "Clockwise":
          for (let i = 0; i < board.length; i++) {
            let row = [];
            for (let j = 0; j < board[i].length; j++) {
              row.push(board[j][i]);
            }
            newBoard.push(row.reverse());
          }
          return newBoard;
        case "AntiClockwise":
          for (let i = 0; i < board.length; i++) {
            let row = [];
            for (let j = 0; j < board[i].length; j++) {
              row.push(board[j][i]);
            }
            newBoard.push(row);
          }
          return newBoard.reverse();
      }
      if ((newBoard.length = 0)) {
        return board;
      }
    },

    arrayClone(arr) {
      var i, copy;

      if (Array.isArray(arr)) {
        copy = arr.slice(0);
        for (i = 0; i < copy.length; i++) {
          copy[i] = this.arrayClone(copy[i]);
        }
        return copy;
      } else if (typeof arr === "object") {
        throw "Cannot clone array containing an object!";
      } else {
        return arr;
      }
    },

    placeTile(board) {
      let newTile;
      if (Math.random() < 0.65) {
        newTile = 2;
      } else {
        newTile = 4;
      }
      let updatedBoard = this.arrayClone(board);
      let emptyTiles = [];
      for (let i = 0; i < updatedBoard.length; i++) {
        for (let j = 0; j < updatedBoard[i].length; j++) {
          if (updatedBoard[i][j] === 0) {
            emptyTiles.push([i, j]);
          }else if(updatedBoard[i][j]==2048){
            this.has2048 = true;
          }
        }
      }
      let cordinates =
        emptyTiles[Math.floor(Math.random() * emptyTiles.length)];
      updatedBoard[cordinates[0]][cordinates[1]] = newTile;
      let updatedTile = newTile;

      return { updatedBoard, updatedTile };
    },

    updateScore(addedScore){
      this.score =  this.score + addedScore;
    },

    swipeBoard(direction, board) {
      let updatedBoard = this.arrayClone(board);
      switch (direction) {
        case "right":
          for (let i = 0; i < updatedBoard.length; i++) {
            updatedBoard[i].reverse();
            let loopLength = updatedBoard[i].length - 1;
            for (let j = 0; j < loopLength; j++) {
              if (updatedBoard[i][j] == 0) {
                updatedBoard[i].splice(j, 1);
                j--;
                updatedBoard[i].push(0);
                loopLength--;
              }
              if (updatedBoard[i][j] == updatedBoard[i][j + 1]) {
                updatedBoard[i][j] += updatedBoard[i][j + 1];
                updatedBoard[i].splice(j + 1, 1);
                updatedBoard[i].push(0);
                loopLength--;
                this.updateScore(updatedBoard[i][j]);
              }
            }
            updatedBoard[i].reverse();
          }
          return updatedBoard;
        case "down":
          updatedBoard = this.rotate2DArray(updatedBoard, "Clockwise");
          for (let i = 0; i < updatedBoard.length; i++) {
            let loopLength = updatedBoard[i].length - 1;
            for (let j = 0; j < loopLength; j++) {
              if (updatedBoard[i][j] == 0) {
                updatedBoard[i].splice(j, 1);
                j--;
                updatedBoard[i].push(0);
                loopLength--;
              }
              if (updatedBoard[i][j] == updatedBoard[i][j + 1]) {
                updatedBoard[i][j] += updatedBoard[i][j + 1];
                updatedBoard[i].splice(j + 1, 1);
                updatedBoard[i].push(0);
                loopLength--;
                this.updateScore(updatedBoard[i][j]);
              }
            }
          }
          return this.rotate2DArray(updatedBoard, "AntiClockwise");
        case "left":
          for (let i = 0; i < board.length; i++) {
            let loopLength = board[i].length - 1;
            for (let j = 0; j < loopLength; j++) {
              if (board[i][j] == 0) {
                board[i].splice(j, 1);
                j--;
                board[i].push(0);
                loopLength--;
              }
              if (board[i][j] == board[i][j + 1]) {
                board[i][j] += board[i][j + 1];
                board[i].splice(j + 1, 1);
                board[i].push(0);
                loopLength--;
                this.updateScore(updatedBoard[i][j]);
              }
            }
          }
          return board;
        case "up":
          updatedBoard = this.rotate2DArray(updatedBoard, "AntiClockwise");
          for (let i = 0; i < updatedBoard.length; i++) {
            let loopLength = updatedBoard[i].length - 1;
            for (let j = 0; j < loopLength; j++) {
              if (updatedBoard[i][j] == 0) {
                updatedBoard[i].splice(j, 1);
                j--;
                updatedBoard[i].push(0);
                loopLength--;
              }
              if (updatedBoard[i][j] == updatedBoard[i][j + 1]) {
                updatedBoard[i][j] += updatedBoard[i][j + 1];
                updatedBoard[i].splice(j + 1, 1);
                updatedBoard[i].push(0);
                loopLength--;
                this.updateScore(updatedBoard[i][j]);
              }
            }
          }
          return this.rotate2DArray(updatedBoard, "Clockwise");
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.new-game button {
  margin: 20px;
}

.new-game {
  padding-left: 500px;
}

.score h3 {
     margin-top: 24px;
    font-size: 30px;
}

.score {
  padding-right: 500px; 
}

.tile {
  min-height: 75px;
}

.board{
  background-color: #b3e6cb;
  padding:0px;
}

.tile.col {
  padding-right: 0px;
  padding-left: 0px;
  font-size: 24px;
  padding: 16px;
  font-weight: 700;
  border:2px solid rgb(96, 157, 209)
}

.bv-example-row {
  max-width: 300px;
  min-height: 300px;
  border: 5px solid black;
}

.row{
  margin-right: 0px;
  margin-left: 0px; 

}
</style>
