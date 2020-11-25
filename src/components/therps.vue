<template>
  <div class="hello">
    <div id="rounds-selection" v-if="!selected">
      <label for="roundsNumber">
        <h1>Welcome to The Rock / Paper / Scissors</h1>
        <h2>How Many Rounds do you want to play?</h2>
      </label>
      <div>
        <select name="gameNumber" v-model="bestOf" style="width:auto">
          <option >--Select Number of Rounds--</option>
          <option value="3">Best 2 out of 3</option>
          <option value="5">Best 3 out of 5</option>
          <option value="7">Best 4 out of 7</option>
        </select>

        <p style="margin:15px" v-if="bestOf">Best of: {{bestOf}}</p>

        <button @click="getNums" v-if="bestOf>0">Let's Go</button>
      </div>
    </div>
    <div id="game" v-if="selected">
      <h2>Playing the best {{bestOf/2+.5}} out of {{bestOf}}</h2>
      <div class="button-row">
        <img class="rock" src="../assets/therock.png" alt="rock" @click="rockCounter" />

        <img
          class="paper"
          src="../assets/printerpaper.png"
          alt="paper"
          width="30%"
          @click="paperCounter"
        />

        <img
          class="scissors"
          src="../assets/safetyscissors.png"
          width="40%"
          @click="scissorsCounter"
        />
      </div>
      <div>
        <h2>{{selection}}</h2>
      </div>
      <div>
        <button type="sumit" @click="onPlay" v-if="(this.pWin<this.bestOf/2 && this.cWin<this.bestOf/2)" >PLAY</button>
      </div>
      <div V-if="compSelection">
        <h2>Computer Plays:</h2>
        <h3>{{compSelection}}</h3>
        <h3>{{winner}}</h3>
      </div>
      <div></div>
      <div class="table-div">
        <table class="table">
          <tr class="thead">
            <thead>And the Winner is:</thead>
          </tr>
          <tr>
            <td>PERSON</td>
            <td>COMPY</td>
          </tr>
          <tr>
            <td>{{pWin}}</td>
            <td>{{cWin}}</td>
          </tr>
        </table>
      </div>
      <br />
      <button class="butt1" style="fontsize:58px" type="button" @click="onReset">Reset?</button>

      <div>Games: {{gameCount}} / {{bestOf}} </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "therps",

  props: {
    msg: String
  },

  data: function() {
    return {
      selection: "Pick One",
      rockCount: 0,
      paperCount: 0,
      scissorsCount: 0,
      rockGreatest: false,
      compSelection: "?",
      pWin: 0,
      cWin: 0,
      gameCount: 0,
      tieCount: 0,
      bestOf: null,
      compPlays: [],
      selected: false
    };
  },

  computed: {
    winner: function() {
      if (this.selection == "PAPER" && this.compSelection == "ROCK") {
        return `${this.selection} beats ${this.compSelection} YOU WIN!'`;
      } else if (
        this.selection == "PAPER" &&
        this.compSelection == "SCISSORS"
      ) {
        return `${this.compSelection} beats ${this.selection} COMPY WINS!`;
      } else if (this.selection == this.compSelection) {
        return "TIE!";
      } else if (this.selection == "ROCK" && this.compSelection == "SCISSORS") {
        return `${this.selection} beats ${this.compSelection} YOU WIN!'`;
      } else if (this.selection == "ROCK" && this.compSelection == "PAPER") {
        return `${this.compSelection} beats ${this.selection} COMPY WINS!`;
      } else if (
        this.selection == "SCISSORS" &&
        this.compSelection == "PAPER"
      ) {
        return `${this.selection} beats ${this.compSelection} YOU WIN!'`;
      } else if (this.selection == "SCISSORS" && this.compSelection == "ROCK") {
        return `${this.compSelection} beats ${this.selection} COMPY WINS!`;
      } else {
        return undefined;
      }
    }
  },

  methods: {
    rockCounter: function() {
      this.compSelection = "?";
      return (this.selection = "ROCK");
    },
    paperCounter: function() {
      this.compSelection = "?";
      return (this.selection = "PAPER");
    },
    scissorsCounter: function() {
      this.compSelection = "?";
      return (this.selection = "SCISSORS");
    },

    onPlay: function() {
    
      
      if (this.selection != "Pick One" ) {
        if ((this.pWin<this.bestOf/2 && this.cWin<this.bestOf/2)) {
          this.gameCount++;
          let pScore = this.pWin;
          let cScore = this.cWin;
          let tCount = this.tieCount;

          let choice = Math.floor(Math.random() * 100);
          if (choice >= 1 && choice < 33) {
            this.compSelection = "ROCK";
          } else if (choice >= 33 && choice < 66) {
            this.compSelection = "PAPER";
          } else if (choice >= 67 && choice < 101) {
            this.compSelection = "SCISSORS";
          }

          if (
            this.winner ==
            `${this.selection} beats ${this.compSelection} YOU WIN!'`
          ) {
            pScore++;
            this.pWin = pScore;
          }
          if (
            this.winner ==
            `${this.compSelection} beats ${this.selection} COMPY WINS!`
          ) {
            cScore++;
            this.cWin = cScore;
          }
          if (this.winner == "TIE!") {
            tCount++;
            this.tieCount = tCount;
          }
        }
      }
    },

    onReset: function() {
      this.gameCount = 0;
      this.tieCount = 0;
      this.pWin = 0;
      this.cWin = 0;
      this.compSelection = "?";
      this.selection = "Pick One";
      this.selected = false;
      this.bestOf = "?";
    },

    getNums: function() {
      this.selected = !this.selected;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
table{
  margin:auto
}
.button-row {
  display: inline;
  margin: auto;
  padding: 0px;
}
.rock {
  padding: 0, 6px;
  border: none;
  cursor: pointer;
  max-height: 250px;
  width: auto;
}
.paper {
  padding: 0, 6px;
  border: none;
  cursor: pointer;
  max-height: 250px;
  width: auto;
}
.scissors {
  padding: none;
  border: none;
  cursor: pointer;
  max-height: 250px;
  width: auto
}

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
table {
  padding: 5px;
  border: solid black;
}
.table-div {
  text-align: center;
}

td {
  text-align: center;
  margin: auto;
  border: 1px solid #42b983;
  border-collapse: collapse;
}
.thead {
  align-self: auto;
  text-align: center;
  background-color: rgba(120, 6, 219, 0.5);
}
button {
  font-size: 16px;
  margin: 5px;
}
</style>