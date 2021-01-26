 
<template>
  <v-app>
    <div class="teamTurnContainer" >
      <p> Turn for  </p> <div :style="{background: isTurnTeamA ? teamAPieceColor : teamBPieceColor}"></div>
    </div>

    <div class="scoreBoardContainer">
      <p>Team A Score : <span v-text="teamAScore"></span></p>
      <p>Team B Score : <span v-text="teamBScore"></span></p>
    </div>

    <div class="flexContainer" v-for="r in 8" :key="r">
    <div v-for="c in 8" :key="c">
    <div 
    :style="{backgroundColor: ((r+c)%2) == 0 ? teamABoxColor : teamBBoxColor }"
    v-bind:class="[ 'box'  ]">
      <div class="circle" :class="isMyTurn(r,c) ? 'boxShadow' : ''" :style="{ backgroundColor: checkCircleColor(r,c)  }"></div>
    </div>
    </div>
    </div>
  </v-app>
</template>

<script>
export default {
  name: 'CheckerBoard',
  data(){
    return {
    teamABoxColor: "#ECF0F1",
    teamBBoxColor: '#374360',
    teamAPieceColor: 'red',
    teamBPieceColor: 'yellow',
    isTurnTeamA: 1, 
    teamAScore: 0,
    teamBScore: 0,
    checkerBoard: [],
    }
  },
  methods:{
    checkCircleColor(r, c){
      if(this.checkerBoard.some(n=>r==n.r&&c==n.c&&n.team=='A')){
        return this.teamAPieceColor;
      }else if(this.checkerBoard.some(n=>r==n.r&&c==n.c&&n.team=='B')){
        return this.teamBPieceColor;
      }
    },
    checkPathTeamA(row,col){
      let index = this.checkerBoard.findIndex( n=> (n.r == row+1 && n.c == col-1 && n.team == null) || (n.r == row+1 && n.c == col+1 && n.team == null) )


      if(index == -1){
        return false;
      }else{
        this.checkerBoard.find(n => n.r == row && n.c == col).isInTurn = true;
        // this.checkerBoard[index].isInTurn = true;
        return true;
      }
    },
    checkPathTeamB(row,col){
      let index = this.checkerBoard.findIndex( n=> (n.r == row-1 && n.c == col+1 && n.team == null) || (n.r == row-1 && n.c == col-1 && n.team == null) )

      if(index == -1){
        return false;
      }else{
        this.checkerBoard.find(n => n.r == row && n.c == col).isInTurn = true;
        // this.checkerBoard[index].isInTurn = true;
        return true;
      }
    },
    decidePossiblePathToPlay(){
      if(this.isTurnTeamA){
         let items =  this.checkerBoard.filter(n=>n.team=='A')
         let isInTheTurn = items.filter( n => this.checkPathTeamA(n.r, n.c) )
         console.log(  isInTheTurn);
      }else{
         let items =  this.checkerBoard.filter(n=>n.team=='B')
         let isInTheTurn = items.filter( n => this.checkPathTeamB(n.r, n.c) )
         console.log(  isInTheTurn);
      }
    },
    isMyTurn(row,col){
      return this.checkerBoard.some(n=>n.r == row && n.c == col && n.isInTurn)
    }
  },
  created(){
    //setup the piece
    let r, c;
    for( r=1; r<9; r++){
      for(c=1; c<9; c++){
        if(r < 4 && ((r+c)%2 == 0) ){
          this.checkerBoard.push({
            r: r,
            c: c,
            team: 'A',
            isInTurn: false,
          });
        } else if(r > 5 && ((r+c)%2 == 0) ){
          // console.log(r)
          this.checkerBoard.push({
            r: r,
            c: c,
            team: 'B',
            isInTurn: false,
          });
        }else{
          this.checkerBoard.push({
            r: r,
            c: c,
            team: null,
            isInTurn: false,
          })
        }
      }
    }

    //decide the first turn
    this.isTurnTeamA = Math.floor(Math.random() * 2)

    //call the path 
    this.decidePossiblePathToPlay()


  },
  mounted(){}
};
</script>

<style lang="scss">
.boxShadow{
  -webkit-box-shadow: 0px 5px 42px 14px rgba(0,0,0,0.75);
-moz-box-shadow: 0px 5px 42px 14px rgba(0,0,0,0.75);
box-shadow: 0px 5px 42px 14px rgba(0,0,0,0.75);

}

.teamTurnContainer{
  display: flex;
  justify-content: center;
    margin-top: 10px;
   div{
    height: 30px;
    width: 30px;
    margin-left: 10px;
  }
}

.scoreBoardContainer{
  text-align: center;
  p{
    margin-top: 0;
    margin-bottom: 0;
  }
}

.circle{
  height: 60px;
  width: 60px;
  /* background: green; */
  border-radius: 30px;
  margin: auto;
  margin-top: 20px;
}
.box{
  height: 100px;
  width: 100px;
  border: 1px solid white;
}
.blackBox{
  background-color: black;
}
.greyBox{
  background-color: grey;
}
.flexContainer{
  display: flex;
  width: 80%;
  margin: 0 auto;
  justify-content: center;
}
</style>