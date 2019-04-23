<template>
  <div>
    <div class="instructions">
      <h1>How to Play ? </h1>
      <p>.....</p>
    </div>

    <div class="game-board">
      <table cellspacing="0">
        <tr class="first-row">
          <td class="first-cell" v-on:click="click(0,0)">{{ game[0][0] }}</td>
          <td class="second-cell" v-on:click="click(0,1)">{{ game[0][1] }}</td>
          <td v-on:click="click(0,2)">{{ game[0][2] }}</td>
        </tr>
        <tr class="second-row">
          <td class="first-cell" v-on:click="click(1,0)">{{ game[1][0] }}</td>
          <td class="second-cell" v-on:click="click(1,1)">{{ game[1][1] }}</td>
          <td v-on:click="click(1,2)">{{ game[1][2] }}</td>
        </tr>
        <tr>
          <td class="first-cell" v-on:click="click(2,0)">{{ game[2][0] }}</td>
          <td class="second-cell" v-on:click="click(2,1)">{{ game[2][1] }}</td>
          <td v-on:click="click(2,2)">{{ game[2][2] }}</td>
        </tr>
      </table>
      <p v-if="winner">Game Over ! Winner {{winner}} </p>
      <button class="reset-button" v-if="emptyCellCount() !== 9" v-on:click="reset">RESTART</button>
    </div>

  </div>

</template>

<script>
export default {
  name: 'Home',
  methods : {

    isDone : function () {

      for (let i=0;i<3;i++) {
        if (this.game[i][0] && this.game[i][0] === this.game[i][1] && this.game[i][0] === this.game[i][2]) {
          return true;
        }
      }
      for (let i=0;i<3;i++) {
        if (this.game[0][i] && this.game[0][i] === this.game[1][i] && this.game[0][i] === this.game[2][i]) {
          return true;
        }
      }

      if (this.game[0][0] && this.game[0][0] === this.game[1][1] && this.game[0][0] === this.game[2][2]) {
        return true;
      }

      if (this.game[0][2] && this.game[0][2] === this.game[1][1] && this.game[0][2] === this.game[2][0]) {
        return true;
      }

      return false;
    },

    reset : function () {
      this.game = [["","",""],["","",""],["","",""]];
      this.winner = null;
    },

    moveComputer : function () {
      let x,y;
      while (true) {
        const randomX = Math.floor(Math.random() * 3);
        const randomY = Math.floor(Math.random() * 3);

        if (this.game[randomX][randomY] === "") {
            this.game[randomX][randomY] = "O";
            break;
        }
      }
    },

    emptyCellCount : function () {

      let emptyCount = 0;
      for (let i=0; i<3; i++) {
        for (let k=0; k<3; k++) {
          if (this.game[i][k] === '') {
            emptyCount++;
          }
        }
      }
      return emptyCount;
    },

    click : function (x,y) {
      if (this.winner || this.game[x][y]) {
        return;
      }
      this.game[x][y] = "X";

      if (this.isDone()) {
        this.winner = "X";
      } else if (this.emptyCellCount() === 0) {
          this.winner = "Dostluk";
      } else {
        this.moveComputer();
        if (this.isDone()) {
          this.winner = "O"
        }
      }
      this.$forceUpdate();

    }
  },
  data () {
    return {
      winner : false,
      game : [
        ["","",""],
        ["","",""],
        ["","",""]
      ]
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
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
  .game-board table{
    margin: 0 auto;
  }
  .game-board table .first-row td {
    border-bottom: 1px solid #ccc;
  }
  .game-board table .second-row td {
    border-bottom: 1px solid #ccc;
  }
  .game-board table tr .first-cell,
  .game-board table tr .first-cell,
  .game-board table tr .second-cell,
  .game-board table tr .second-cell {
    border-right: 1px solid #ccc;
  }
  .game-board td {
    font-size: 30px;
    width: 60px;
    height: 60px;
    line-height: 40px;
    text-align: center;
  }
  .reset-button {
    background: coral;
    border: none;
    padding: 10px;
    margin-top: 10px ;
  }
</style>
