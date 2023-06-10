<template>
  <Players :tiles="tiles" :tileSize="tileSize" :playerSize="playerSize" :players="players"/>
  <div class="picSL">
    <img alt="Snakes and Ladders" src="./assets/Snakes-and-Ladders.png">
  </div>
  <Board :board="board" :rowLength="rowLength" :colLength="colLength" :tileSize="tileSize"/>
</template>

<script>
import Board from "./components/Board-Space.vue"
import Players from "./components/Player-Pieces.vue"
import { ref } from "vue"

export default {
  name: 'App',
  components: { 
    Board,
    Players
   },
  setup(){
    const tileSize = ref(64)
    const rowLength = ref(10)
    const colLength = ref(10)
    const boardLength = ref(rowLength.value*colLength.value)

    const tiles = ref(new Array(boardLength.value))
    const board = ref([])

    const num = ref(0)
    const countRight = ref(false)
    const reverseRow = ref(0)

    /* 
      <summary>
      Gets the s coordinate of a tile
      </summary
      <param name = "tile"> The tile you want the s coordinate for.</param>
    */
    const getX = (tile) => {
      const tilesOver = ref(tile%rowLength.value)

      if(tilesOver.value == 0){
        tilesOver.value = 10
      }

      tilesOver.value = rowLength.value - tilesOver.value
      return tilesOver.value * tileSize.value + 8
    }

    /* 
      <summary>
      Gets the y coordinate of a tile
      </summary
      <param name = "tile"> The tile you want the y coordinate for.</param>
    */
    const getY = (tile) => {
      const tilesDown = ref(colLength.value - Math.ceil(tile/rowLength.value))
      return tilesDown.value * tileSize.value + 60
    }

    // Constructs the board information and player piece information
    for(let i=boardLength.value; i>0; i--){
      if(i%rowLength.value == 0){
        countRight.value = !countRight.value
        reverseRow.value = rowLength.value - 1
      }
      
      if(countRight.value){
        num.value = i
      }

      else{
        num.value = i-reverseRow.value
        reverseRow.value -= 2
      }

      board.value.push({
        spaceNum: num.value,
        colorNum: i
      })
      
      tiles.value[num.value-1] = {
        x: getX(i),
        y: getY(i),
        moveTo: undefined
      }
    }

    const numOfPlayers = ref(4)
    const players = ref([])
    const playerSize = ref(tileSize.value/2)

    for(let i=1; i<=numOfPlayers.value; i++){
      players.value.push({
        num: i,
        moveToTile: -1,
        x: parseInt(tiles.value[0].x + playerSize.value*(1 - i%2)) + "px",
        y: parseInt(tiles.value[0].y + playerSize.value*(Math.floor(i/3)) + tileSize.value) + "px",
        quadrantX: playerSize.value*(1 - i%2),
        quadrantY: playerSize.value*(Math.floor(i/3))
      })
    }

    return { tiles, board, rowLength, colLength, playerSize, players, tileSize}
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.picSL{
  position: absolute;
}
</style>
