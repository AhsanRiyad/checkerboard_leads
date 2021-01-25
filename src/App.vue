 
<template>
  <v-app>
    <div class="flexContainer" v-for="i in 8" :key="i">
    <div v-for="j in 8" :key="j">
    <div 
    :style="{backgroundColor: ((i+j)%2) == 0 ? teamABoxColor : teamBBoxColor }"
    v-bind:class="[ 'box'  ]"></div>
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
    }
  }
};
</script>

<style>
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