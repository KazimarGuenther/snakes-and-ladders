<template>
    <div v-for="tile in board" :key="tile" class="container">
        <div v-if="tile.colorNum%3 == 1" class=tileOne>
            <h3>{{ tile.spaceNum}}</h3>
        </div>
        <div v-else-if="tile.colorNum%3 == 2" class=tileTwo>
            <h3>{{ tile.spaceNum }}</h3>
        </div>
        <div v-else class=tileThree>
            <h3>{{ tile.spaceNum }}</h3>
        </div>
    </div>  
</template>

<script>
import { ref } from "vue"

export default {
    props: ["board", "rowLength", "tileSize"],
    setup(props){
        const tileSizePx = ref(parseInt(props.tileSize) + "px");
        const boardWidth = ref(parseInt(props.tileSize * props.rowLength) + "px")
        return{tileSizePx, boardWidth}
    }
}
</script>

<style scoped>
    .tileOne h3, .tileTwo h3, .tileThree h3{
        position: relative;
        color: #ffffff;
    }
    .tileOne {
        height: v-bind(tileSizePx);
        width: v-bind(tileSizePx);
        background: #1100ff;
        float: left;
    }
    .tileTwo {
        height: v-bind(tileSizePx);
        width: v-bind(tileSizePx);
        background: rgb(153, 0, 255);
        float: left;
    }
    .tileThree {
        height: v-bind(tileSizePx);
        width: v-bind(tileSizePx);
        background: rgb(255, 0, 0);
        float: left;
    }
    
    .container {
        width: v-bind(boardWidth);
    }
</style>