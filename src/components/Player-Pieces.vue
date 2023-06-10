<template>
    <div class="win" :hidden="noWinner">
        <div class="message">
            You Win
            <button @click="restart">Restart</button>
        </div>
    </div>
    <button class="dice" @click="rollDice" :hidden="needToMove">Roll</button>
    <div class="moveHere" v-if="needToMove"></div>
    <div class="playerOne" @drag="followMouseOne" @dragend="placePieceOne"></div>
    <div class="playerTwo" @drag="followMouseTwo" @dragend="placePieceTwo"></div>
    <div class="playerThree" @drag="followMouseThree" @dragend="placePieceThree"></div>
    <div class="playerFour" @drag="followMouseFour" @dragend="placePieceFour"></div>
</template>

<script>
import { ref } from 'vue';

export default {
    props: ["tiles", "tileSize", "playerSize", "players"],
    setup(props){
        const roundness = ref(parseInt(props.playerSize*3/4) + "px")
        const playerSizePx = ref(parseInt(props.playerSize) + "px")
        const tileSizePx = ref(parseInt(props.tileSize) + "px")

        const moveToTileXPx = ref(parseInt(props.tiles[0].x) + "px")
        const moveToTileYPx = ref(parseInt(props.tiles[0].y) + "px")
        const needToMove = ref(false)
        const playerTurn = ref(0)
        const noWinner = ref(true)

        const playerOneXPx = ref(props.players[0].x)
        const playerOneYPx = ref(props.players[0].y)

        /* 
        <summary>
        Gets player one to follow the mouse
        </summary
        <param name = "event">Data of the drag event.</param>
        */
        const followMouseOne = (event) => {
            playerOneXPx.value = parseInt(event.clientX) + "px"
            playerOneYPx.value = parseInt(event.clientY) + "px"
        }

        /* 
        <summary>
        Checks to see if player one is over the tile they are moving to. Places them on the tile if they
        are or returns them to their original tile after
        </summary
        <param name = "event">Data of the dragend event.</param>
        */
        const placePieceOne = (event) => {
            let player = props.players[0]
            if( player.moveToTile != -1 && 
                event.clientX - props.tiles[player.moveToTile].x <= props.tileSize &&
                event.clientX - props.tiles[player.moveToTile].x >= 0 &&
                event.clientY - props.tiles[player.moveToTile].y <= props.tileSize/2 && 
                event.clientY - props.tiles[player.moveToTile].y >= props.tileSize/-2){

                if(props.tiles[player.moveToTile].moveTo != undefined){
                    player.moveToTile = props.tiles[player.moveToTile].moveTo
                }

                player.x = parseInt(props.tiles[player.moveToTile].x + player.quadrantX) + "px" 
                player.y = parseInt(props.tiles[player.moveToTile].y + player.quadrantY) + "px" 
                playerOneXPx.value = player.x  
                playerOneYPx.value = player.y

                if(player.moveToTile == 99){
                    noWinner.value = false
                }
                else{
                    needToMove.value = false
                    updateTurn()
                }
            }
            else{
                playerOneXPx.value = player.x
                playerOneYPx.value = player.y 
            }
        }

        const playerTwoXPx = ref(props.players[1].x)
        const playerTwoYPx = ref(props.players[1].y)

        /* 
        <summary>
        Gets player two to follow the mouse
        </summary
        <param name = "event">Data of the drag event.</param>
        */
        const followMouseTwo = (event) => {
            playerTwoXPx.value = parseInt(event.clientX) + "px"
            playerTwoYPx.value = parseInt(event.clientY) + "px"
        }

        /* 
        <summary>
        Checks to see if player two is over the tile they are moving to. Places them on the tile if they
        are or returns them to their original tile after
        </summary
        <param name = "event">Data of the dragend event.</param>
        */
        const placePieceTwo = (event) => {
            let player = props.players[1]
            if( player.moveToTile != -1 && 
                event.clientX - props.tiles[player.moveToTile].x <= props.tileSize &&
                event.clientX - props.tiles[player.moveToTile].x >= 0 &&
                event.clientY - props.tiles[player.moveToTile].y <= props.tileSize/2 && 
                event.clientY - props.tiles[player.moveToTile].y >= props.tileSize/-2){ 

                if(props.tiles[player.moveToTile].moveTo != undefined){
                    player.moveToTile = props.tiles[player.moveToTile].moveTo
                }

                player.x = parseInt(props.tiles[player.moveToTile].x + player.quadrantX) + "px" 
                player.y = parseInt(props.tiles[player.moveToTile].y + player.quadrantY) + "px" 
                playerTwoXPx.value = player.x  
                playerTwoYPx.value = player.y
                needToMove.value = false
                updateTurn()
            }
            else{
                playerTwoXPx.value = player.x
                playerTwoYPx.value = player.y 
            }
        }

        const playerThreeXPx = ref(props.players[2].x)
        const playerThreeYPx = ref(props.players[2].y)

        /* 
        <summary>
        Gets player three to follow the mouse
        </summary
        <param name = "event">Data of the drag event.</param>
        */
        const followMouseThree = (event) => {
            playerThreeXPx.value = parseInt(event.clientX) + "px"
            playerThreeYPx.value = parseInt(event.clientY) + "px"
        }

        /* 
        <summary>
        Checks to see if player three is over the tile they are moving to. Places them on the tile if they
        are or returns them to their original tile after
        </summary
        <param name = "event">Data of the dragend event.</param>
        */
        const placePieceThree = (event) => {
            let player = props.players[2]
            if( player.moveToTile != -1 && 
                event.clientX - props.tiles[player.moveToTile].x <= props.tileSize &&
                event.clientX - props.tiles[player.moveToTile].x >= 0 &&
                event.clientY - props.tiles[player.moveToTile].y <= props.tileSize/2 && 
                event.clientY - props.tiles[player.moveToTile].y >= props.tileSize/-2){ 

                if(props.tiles[player.moveToTile].moveTo != undefined){
                    player.moveToTile = props.tiles[player.moveToTile].moveTo
                }

                player.x = parseInt(props.tiles[player.moveToTile].x + player.quadrantX) + "px" 
                player.y = parseInt(props.tiles[player.moveToTile].y + player.quadrantY) + "px" 
                playerThreeXPx.value = player.x  
                playerThreeYPx.value = player.y
                needToMove.value = false
                updateTurn()
            }
            else{
                playerThreeXPx.value = player.x
                playerThreeYPx.value = player.y 
            }
        }

        const playerFourXPx = ref(props.players[3].x)
        const playerFourYPx = ref(props.players[3].y)

        /* 
        <summary>
        Gets player four to follow the mouse
        </summary
        <param name = "event">Data of the drag event.</param>
        */
        const followMouseFour = (event) => {
            playerFourXPx.value = parseInt(event.clientX) + "px"
            playerFourYPx.value = parseInt(event.clientY) + "px"
        }

        /* 
        <summary>
        Checks to see if player four is over the tile they are moving to. Places them on the tile if they
        are or returns them to their original tile after
        </summary
        <param name = "event">Data of the dragend event.</param>
        */
        const placePieceFour = (event) => {
            let player = props.players[3]
            if( player.moveToTile != -1 && 
                event.clientX - props.tiles[player.moveToTile].x <= props.tileSize &&
                event.clientX - props.tiles[player.moveToTile].x >= 0 &&
                event.clientY - props.tiles[player.moveToTile].y <= props.tileSize/2 && 
                event.clientY - props.tiles[player.moveToTile].y >= props.tileSize/-2){  

                if(props.tiles[player.moveToTile].moveTo != undefined){
                    player.moveToTile = props.tiles[player.moveToTile].moveTo
                }

                player.x = parseInt(props.tiles[player.moveToTile].x + player.quadrantX) + "px" 
                player.y = parseInt(props.tiles[player.moveToTile].y + player.quadrantY) + "px" 
                playerFourXPx.value = player.x  
                playerFourYPx.value = player.y
                needToMove.value = false
                updateTurn()
            }
            else{
                playerFourXPx.value = player.x
                playerFourYPx.value = player.y 
            }
        }

        /* 
        <summary>
        Randomly generates 1-6, updates the current player to know how far it needs to move, moves the 
        moveHere square to the space where the player will be moving to, and makes the square visible.
        </summary
        */
        const rollDice = () => {
            let num = Math.floor(Math.random() * 5) + 1
            if(props.players[playerTurn.value].moveToTile + num < 99){
                props.players[playerTurn.value].moveToTile += num
            }
            else{
                props.players[playerTurn.value].moveToTile = 99
            }
            
            moveToTileXPx.value = parseInt(props.tiles[props.players[playerTurn.value].moveToTile].x) + "px"
            moveToTileYPx.value = parseInt(props.tiles[props.players[playerTurn.value].moveToTile].y) + "px"
            needToMove.value = true

            console.log(num)
        }

        /* 
        <summary>
        Changes the current turn to the next player's turn
        </summary
        */
        const updateTurn = () => {
            if(playerTurn.value + 1 < props.players.length){
                playerTurn.value++
            }
            else{
                playerTurn.value = 0
            }
        }

        /* 
        <summary>
        Changes the current turn to the next player's turn
        </summary
        */
       const restart = () => {
            location.reload()
       }

        return{roundness, playerSizePx, tileSizePx, moveToTileXPx, moveToTileYPx, playerOneXPx, playerOneYPx,
               playerTwoXPx, playerTwoYPx, playerThreeXPx, playerThreeYPx, playerFourXPx, playerFourYPx,
               needToMove, noWinner, followMouseOne, placePieceOne, followMouseTwo, placePieceTwo, 
               followMouseThree, placePieceThree, followMouseFour, placePieceFour, rollDice, restart}
    }
}
</script>

<style scoped>
.playerOne {
    position: absolute;
    left: v-bind(playerOneXPx);
    top: v-bind(playerOneYPx);
    z-index: 2;
    height: v-bind(playerSizePx);
    width: v-bind(playerSizePx);
    border-radius: v-bind(roundness);
    background: #ff5100;
}
.playerTwo {
    position: absolute;
    left: v-bind(playerTwoXPx);
    top: v-bind(playerTwoYPx);
    z-index: 2;
    height: v-bind(playerSizePx);
    width: v-bind(playerSizePx);
    border-radius: v-bind(roundness);
    background: #73ff00;
}
.playerThree {
    position: absolute;
    left: v-bind(playerThreeXPx);
    top: v-bind(playerThreeYPx);
    z-index: 2;
    height: v-bind(playerSizePx);
    width: v-bind(playerSizePx);
    border-radius: v-bind(roundness);
    background: #fbff00;
}
.playerFour {
    position: absolute;
    left: v-bind(playerFourXPx);
    top: v-bind(playerFourYPx);
    z-index: 2;
    height: v-bind(playerSizePx);
    width: v-bind(playerSizePx);
    border-radius: v-bind(roundness);
    background: #ff009d;
}
.moveHere {
    position: absolute;
    left: v-bind(moveToTileXPx);
    top: v-bind(moveToTileYPx);
    z-index: 2;
    height: v-bind(tileSizePx);
    width: v-bind(tileSizePx);
    background: #ffffff88;
}
.dice {
    position: absolute;
    left: 8px;
    top: 30px;
}
.win {
    position: absolute;
    width: 1000px;
    height: 1000px;
    background: #00000044;
}
.message {
    position: absolute;
    width: 100px;
    height: 100px;
    z-index: 5;
    background: #ffffff;
}
</style>