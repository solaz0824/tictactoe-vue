<template>
  <div>
    <div class="textStyle">{{turn}}'s turn!</div>
    <table-component>
       <tr v-for="(rowData, rowIndex) in tableData" :key="rowIndex">
        <td @click="onClickTd(rowIndex, cellIndex)" v-for="(cellData, cellIndex) in rowData" :key="cellIndex">{{cellData}}</td>
      </tr>
    </table-component>
    <div class="textStyle" v-if="winner">{{winner}} Won!</div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import store,  { CHANGE_TURN, CLICK_CELL, NO_WINNER, RESET_GAME, SET_WINNER } from "./store";
import TableComponent from "./TableComponent";

export default {
  store,
  components: {
    TableComponent
  },
  data() {
    return {
     data: 1,
    };
  },
  computed: {
    ...mapState(["winner", "turn", "tableData"])
  },
  methods: {
    onClickTd(rowIndex, cellIndex) {
        if (this.cellData) return;

        this.$store.commit(CLICK_CELL, { row: rowIndex, cell: cellIndex });
        
        let win = false;
        if (this.tableData[rowIndex][0] === this.turn && this.tableData[rowIndex][1] === this.turn && this.tableData[rowIndex][2] === this.turn) {
          win = true;
        }
        if (this.tableData[0][cellIndex] === this.turn && this.tableData[1][cellIndex] === this.turn && this.tableData[2][cellIndex] === this.turn) {
          win = true;
        }
        if (this.tableData[0][0] === this.turn && this.tableData[1][1] === this.turn && this.tableData[2][2] === this.turn) {
          win = true;
        }
        if (this.tableData[0][2] === this.turn && this.tableData[1][1] === this.turn && this.tableData[2][0] === this.turn) {
          win = true;
        }
        if (win) { 
          this.$store.commit(SET_WINNER, this.turn);
          this.$store.commit(RESET_GAME);
        } else { 
          let all = true; 
          this.tableData.forEach((row) => { 
            row.forEach((cell) => {
              if (!cell) {
                all = false;
              }
            });
          });
          if (all) { 
            this.$store.commit(NO_WINNER);
            this.$store.commit(RESET_GAME);
          } else {
            this.$store.commit(CHANGE_TURN);
          }
        }
      }
  }
};
</script>

<style>
html {
  background-color:cornflowerblue
}
body {
  margin: 10% 0;
}
.textStyle {
  text-align: center;
  margin: 3%;
  font-family: monospace;
  font-size: xx-large;
}
table {
  border-collapse: collapse;
  margin: 5% 40%;
  background: white;
}
td {
  border: 2px solid black;
  width: 100px;
  height: 100px;
  text-align: center;
  font-weight: 900;
  font-size: x-large;
  color: mediumvioletred;
}
</style>
