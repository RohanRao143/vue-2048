<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      For guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint" target="_blank" rel="noopener">eslint</a></li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
    </ul>
     <div class="row">
        <div class="col-md-6 pb-2 new-game" >
            <b-button size="lg" variant="primary">
                New Game
            </b-button>
        </div>
        <div class="col-md-6 pb-2 score" >
            <h3>Score <b-badge>3078</b-badge></h3>
        </div>
     </div>
     <div class="row">
      <div class="col-md-12 board">
        <b-container class="bv-example-row">
          <b-row v-for="row in board">
            <b-col class="tile" v-for="element in row">{{element}}</b-col>
          </b-row>
        </b-container>
      </div>
     </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      board: [[0, 2, 0, 4], [0, 2, 0, 4], [0, 2, 0, 4], [0, 2, 0, 4]]
    };
  },
  props: {
    msg: String
  },
  mounted() {
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
  },
  methods: {
    abc() {
      console.log("reached");
    },
    disp(str) {
      alert(str);
    },
    updateBoard(board, direction) {
      this.board = this.swipeBoard(direction, board);
      let { updatedBoard, updatedTile } = this.placeTile(this.board);
      this.board = updatedBoard;
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
      if (Math.random() < 0.7) {
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
          }
        }
      }
      let cordinates =
        emptyTiles[Math.floor(Math.random() * emptyTiles.length)];
      updatedBoard[cordinates[0]][cordinates[1]] = newTile;
      let updatedTile = newTile;

      return { updatedBoard, updatedTile };
    },

    swipeBoard(direction, board) {
      console.log(this);
      let updatedBoard = this.arrayClone(board);
      switch (direction) {
        case "right":
          for (let i = 0; i < board.length; i++) {
            board[i].reverse();
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
              }
            }
            board[i].reverse();
          }
          return board;
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
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.new-game button {
  margin: 20px;
}

.new-game {
  padding-left: 300px;
}

.score h3 {
  margin: 22px;
}

.score {
  padding-right: 300px;
}

.board {
}

.tile {
  min-height: 75px;
}

.tile.col {
  padding-right: 0px;
  padding-left: 0px;
  padding: 25px;
  border:2px solid rgb(96, 157, 209)
}

.bv-example-row {
  max-width: 300px;
  min-height: 300px;
  border: 5px solid black;
}
</style>
