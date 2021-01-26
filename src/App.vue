 
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
      <div class="circle" :style="{ backgroundColor: checkCircleColor(r,c) }"></div>
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
    isTurnTeamA: true, 
    teamAScore: 0,
    teamBScore: 0,
    checkerBoard: {
      teamA:[],
      teamB:[]
    },
    }
  },
  methods:{
    checkCircleColor(r, c){
      if(this.checkerBoard.teamA.some((n)=>r==n.r&&c==n.c)){
        return this.teamAPieceColor;
      }else if(this.checkerBoard.teamB.some((n)=>r==n.r&&c==n.c)){
        return this.teamBPieceColor;
      }
    
    }
  },
  created(){
    let r, c;
    for( r=1; r<9; r++){
      for(c=1; c<9; c++){
        if(r < 4 && ((r+c)%2 == 0) ){
          this.checkerBoard.teamA.push({
            r: r,
            c: c,
            team: 'A'
          });
        } else if(r > 5 && ((r+c)%2 == 0) ){
          console.log(r)
          this.checkerBoard.teamB.push({
            r: r,
            c: c,
            team: 'B'
          });
        }
      }
    }
  },
  mounted(){}
};
</script>

<style lang="scss">
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