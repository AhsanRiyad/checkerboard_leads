 
<template>
  <v-app>
    <div class="teamTurnContainer">
      <p>Turn for</p>
      <div
        :style="{ background: isTurnTeamA ? teamAPieceColor : teamBPieceColor }"
      ></div>
    </div>

    <div class="scoreBoardContainer">
      <p>Team A Score : <span v-text="teamAScore"></span></p>
      <p>Team B Score : <span v-text="teamBScore"></span></p>
    </div>
    
    <div class="selectColorContainer">
      <!-- <v-btn color="red" class="white--text">Team A piece color</v-btn>
      <v-btn color="red" class="white--text">Team A block color</v-btn>
      <v-btn color="red" class="white--text">Team B piece color</v-btn>
      <v-btn color="red" class="white--text">Team B block color</v-btn> -->

    </div>


    <div class="flexContainer" v-for="(r, i) in 8" :key="i">
      <div v-for="(c, ci) in 8" :key="ci">
        <div
          :style="{
            backgroundColor: checkerBoard[(i*8)+ci].isPossiblePath ? 'green' : (r + c) % 2 == 0  ? teamABoxColor : teamBBoxColor,
          }"
          v-bind:class="['box']"
        >
          <div
            class="circle"
            :class="isMyTurn(r, c) ? 'boxShadow' : ''"
            :style="{ backgroundColor: checkCircleColor(r, c) }"
            v-on:click="() => selectToCheckPossiblePath(r, c)"
          >
          </div>
        </div>
      </div>
    </div>
  </v-app>
</template>

<script>
import './styles/checkers.scss';
export default {
  name: "CheckerBoard",
  data() {
    return {
      teamABoxColor: "#ECF0F1",
      teamBBoxColor: "#374360",
      teamAPieceColor: "red",
      teamBPieceColor: "yellow",
      isTurnTeamA: 1,
      teamAScore: 0,
      teamBScore: 0,
      checkerBoard: [],
      selectedRow: null,
      selectedColumn: null,
      opponentTeamIndex: null,
    };
  },
  methods: {
    resetPossiblePath(){
      this.checkerBoard.map((n)=>{
        return {
          ...n, ...n.isPossiblePath = false
        }
      })
    },
    checkCircleColor(r, c) {
      if(r==this.selectedRow && c == this.selectedColumn){
        return 'green'
      }
      else if (
        this.checkerBoard.some((n) => r == n.r && c == n.c && n.team == "A")
      ) {
        return this.teamAPieceColor;
      } else if (
        this.checkerBoard.some((n) => r == n.r && c == n.c && n.team == "B")
      ) {
        return this.teamBPieceColor;
      }
    },
    checkPathTeamA(row, col) {
      const index = this.checkerBoard.findIndex(
        (n) =>
          (n.r == row + 1 && n.c == col - 1 && n.team == null) ||
          (n.r == row + 1 && n.c == col + 1 && n.team == null)
      );
      const opp1 = this.checkerBoard.findIndex(
        (n) =>
          (n.r == row + 1 && n.c == col - 1 && n.team == 'B')
      );

      if(opp1 !=-1){
        const opp11 = this.checkerBoard.findIndex(
        (n) =>
          (n.r == row + 2 && n.c == col - 2 && n.team == null)
          );
        if(opp11 != -1){
          this.checkerBoard.find((n) => n.r == row && n.c == col).isInTurn = true;
          return true;
        }
      }
      const opp2 = this.checkerBoard.findIndex(
        (n) =>
          (n.r == row + 1 && n.c == col + 1 && n.team == 'B')
      );

      if(opp2 !=-1){
        
         const opp22 = this.checkerBoard.findIndex(
        (n) =>
          (n.r == row + 2 && n.c == col + 2 && n.team == null)
      );
        if(opp22 != -1){
          this.checkerBoard.find((n) => n.r == row && n.c == col).isInTurn = true;
          return true;
        }
      }

     

      if (index == -1) {
        return false;
      } else {
        this.checkerBoard.find((n) => n.r == row && n.c == col).isInTurn = true;
        // this.checkerBoard[index].isInTurn = true;
        return true;
      }
    },
    resetTurn(){
       this.checkerBoard.map((n)=>{
        return {
          ...n, ...n.isInTurn = false
        }
      })
      
      this.isTurnTeamA = !this.isTurnTeamA;
      this.decidePossiblePathToPlay();
    },
    selectToCheckPossiblePath(row, col) {

      if(this.checkerBoard[((row-1)*8)+(col-1)].isPossiblePath){
        console.log('yes this is possible path')
        

          this.checkerBoard[((this.selectedRow-1)*8)+(this.selectedColumn-1)].team = null; 
        this.checkerBoard[((row-1)*8)+(col-1)].team = this.isTurnTeamA ? 'A' : 'B'; 

        if(this.opponentTeamIndex != null && this.opponentTeamIndex != -1){

            console.log('selected row' , this.selectedRow)
            console.log('this row' , row)
            console.log('selected col' , this.selectedColumn)
            console.log('this col' , col)


          if(this.checkerBoard[this.opponentTeamIndex].r != row){
            if(this.isTurnTeamA){
              this.teamAScore = this.teamAScore + 1;
            }else{
              this.teamBScore = this.teamBScore + 1;
            }
            this.checkerBoard[this.opponentTeamIndex].team = null;
          }
 
        }


        this.resetTurn();
      }
      
      this.resetPossiblePath();
      this.selectedRow = null;
      this.selectedColumn = null;
      this.opponentTeamIndex = null;
      if(!this.checkerBoard[((row-1)*8)+(col-1)].isInTurn) return; 

      this.selectedRow = row;
      this.selectedColumn = col;

      // console.log(row, col)
      if (this.isTurnTeamA) {
       /*  const a  = this.checkerBoard.filter( n => (n.r == row+1 && n.c == col-1 && n.team == null) || (n.r == row+1 && n.c == col+1 && n.team == null)  );
         a.map((n)=>{return {
         ...n, ...n.isPossiblePath = true
         }}) */


         const index1 =  this.checkerBoard.findIndex(n => (n.r == row+1 && n.c == col-1 && n.team == null))
         const index2 =  this.checkerBoard.findIndex(n => (n.r == row+1 && n.c == col+1 && n.team == null))
         
         const opponentTeamIndex1 =  this.checkerBoard.findIndex(n => (n.r == row+1 && n.c == col-1 && n.team == 'B'))
         const opponentTeamIndex2 =  this.checkerBoard.findIndex(n => (n.r == row+1 && n.c == col+1 && n.team == 'B'))
          console.log('opponent team' ,  this.opponentTeamIndex1)
          if(opponentTeamIndex1 != -1 ){
            this.opponentTeamIndex = opponentTeamIndex1;
            console.log('got inside ')
            const indexOfNextMove = this.checkerBoard.findIndex(n => (n.r == row+2 && n.c == col-2 && n.team == null))
            console.log('next move index ', indexOfNextMove)
             if(indexOfNextMove != -1){
               this.checkerBoard[indexOfNextMove].isPossiblePath = true;
          }
          }
          if(opponentTeamIndex2 != -1 ){
            this.opponentTeamIndex = opponentTeamIndex2;
            console.log('got inside ')
            const indexOfNextMove = this.checkerBoard.findIndex(n => (n.r == row+2 && n.c == col+2 && n.team == null))
            console.log('next move index ', indexOfNextMove)
             if(indexOfNextMove != -1){
            this.checkerBoard[indexOfNextMove].isPossiblePath = true;
          }
          }

          

          if(index1 != -1){
            this.checkerBoard[index1].isPossiblePath = true;
          }
          if(index2 != -1){
            this.checkerBoard[index2].isPossiblePath = true;
          }

          console.log(index1)
          console.log(index2)
          // console.log(this.checkerBoard)

        /* const a = this.checkerBoard.map((n) => {
          if (
            (n.r == row + 1 && n.c == col - 1 && n.team == null) ||
            (n.r == row + 1 && n.c == col + 1 && n.team == null)
          ) {
            return {
              ...n,
              ...(n.isPossiblePath = true),
            };
          }else{
            return n;
          }
        });
        this.checkerBoard = [...a]
        console.log(a); */
      }else{

        const index1 =  this.checkerBoard.findIndex(n => (n.r == row-1 && n.c == col+1 && n.team == null))
         const index2 =  this.checkerBoard.findIndex(n => (n.r == row-1 && n.c == col-1 && n.team == null))


            const opponentTeamIndex1 =  this.checkerBoard.findIndex(n => (n.r == row-1 && n.c == col+1 && n.team == 'A'))
         const opponentTeamIndex2 =  this.checkerBoard.findIndex(n => (n.r == row-1 && n.c == col-1 && n.team == 'A'))
          console.log('opponent team' ,  this.opponentTeamIndex1)
          if(opponentTeamIndex1 != -1 ){
            this.opponentTeamIndex = opponentTeamIndex1;
            console.log('got inside ')
            const indexOfNextMove = this.checkerBoard.findIndex(n => (n.r == row-1-1 && n.c == col+1+1 && n.team == null))
            console.log('next move index ', indexOfNextMove)
             if(indexOfNextMove != -1){
               this.checkerBoard[indexOfNextMove].isPossiblePath = true;
          }
          }
          if(opponentTeamIndex2 != -1 ){
            this.opponentTeamIndex = opponentTeamIndex2;
            console.log('got inside ')
            const indexOfNextMove = this.checkerBoard.findIndex(n => (n.r == row-1-1 && n.c == col-1-1 && n.team == null))
            console.log('next move index ', indexOfNextMove)
             if(indexOfNextMove != -1){
            this.checkerBoard[indexOfNextMove].isPossiblePath = true;
          }
          }


          if(index1 != -1){
            this.checkerBoard[index1].isPossiblePath = true;
          }
          if(index2 != -1){
            this.checkerBoard[index2].isPossiblePath = true;
          }
      }
    },
    checkPathTeamB(row, col) {
      const index = this.checkerBoard.findIndex(
        (n) =>
          (n.r == row - 1 && n.c == col + 1 && n.team == null) ||
          (n.r == row - 1 && n.c == col - 1 && n.team == null)
      );

         const opp1 = this.checkerBoard.findIndex(
        (n) =>
          (n.r == row - 1 && n.c == col + 1 && n.team == 'A')
      );

      if(opp1 !=-1){
        const opp11 = this.checkerBoard.findIndex(
        (n) =>
          (n.r == row - 2 && n.c == col + 2 && n.team == null)
          );
        if(opp11 != -1){
          this.checkerBoard.find((n) => n.r == row && n.c == col).isInTurn = true;
          return true;
        }
      }
      const opp2 = this.checkerBoard.findIndex(
        (n) =>
          (n.r == row - 1 && n.c == col - 1 && n.team == 'A')
      );

      if(opp2 !=-1){
        
         const opp22 = this.checkerBoard.findIndex(
        (n) =>
          (n.r == row - 2 && n.c == col - 2 && n.team == null)
      );
        if(opp22 != -1){
          this.checkerBoard.find((n) => n.r == row && n.c == col).isInTurn = true;
          return true;
        }
      }


      if (index == -1) {
        return false;
      } else {
        this.checkerBoard.find((n) => n.r == row && n.c == col).isInTurn = true;
        // this.checkerBoard[index].isInTurn = true;
        return true;
      }
    },
    decidePossiblePathToPlay() {
      if (this.isTurnTeamA) {
        const items = this.checkerBoard.filter((n) => n.team == "A");
        const isInTheTurn = items.filter((n) => this.checkPathTeamA(n.r, n.c));
        console.log(isInTheTurn);
      } else {
        const items = this.checkerBoard.filter((n) => n.team == "B");
        const isInTheTurn = items.filter((n) => this.checkPathTeamB(n.r, n.c));
        console.log(isInTheTurn);
      }
    },
    isMyTurn(row, col) {
      return this.checkerBoard.some(
        (n) => n.r == row && n.c == col && n.isInTurn
      );
    },
    isPossiblePath(row, col) {
      console.log('is possible path')
      const a = this.checkerBoard.some(
        (n) => n.row == row && n.col == col && n.isPossiblePath
      );
      console.log(a)
      return a;
    },
  },
  created() {
    //setup the piece
    let r, c;
    for (r = 1; r < 9; r++) {
      for (c = 1; c < 9; c++) {
        if (r < 4 && (r + c) % 2 == 0) {
          this.checkerBoard.push({
            r: r,
            c: c,
            team: "A",
            isInTurn: false,
            isPossiblePath: false,
          });
        } else if (r > 5 && (r + c) % 2 == 0) {
          // console.log(r)
          this.checkerBoard.push({
            r: r,
            c: c,
            team: "B",
            isInTurn: false,
            isPossiblePath: false,
          });
        } else {
          this.checkerBoard.push({
            r: r,
            c: c,
            team: null,
            isInTurn: false,
            isPossiblePath: false,
          });
        }
      }
    }

    //decide the first turn
    this.isTurnTeamA = Math.floor(Math.random() * 2);

    //call the path
    this.decidePossiblePathToPlay();
  },
  mounted() {},
};
</script>
