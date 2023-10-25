<template>
    <div class="board">
        <BoardItem 
            v-for="(cell, index) in cells" 
            :key="index" 
            :icon="cell"
            @mousedown="startRoute(index)"
            @mouseup="finishRoute(index)"
            @mousemove="go(index)"
            :selected="checkRoute(index)"
            :closed="checkClosedRoute(index)"
        />
    </div>
</template>

<script>
import BoardItem from './BoardItem.vue';
import {ref} from 'vue';

export default {
    components: {
        BoardItem
    },
    // data() {
    //     return {
    //         cells: [3, 0, 0, 1, 0, 0, 1, 0, 2, 0, 0, 0, 0, 2, 0, 3],
    //     }
    // },
    setup() {
        const cells = ref([3, 0, 0, 1, 0, 0, 1, 0, 2, 0, 0, 0, 0, 2, 0, 3]);
        const path = ref([]);
        const size = ref(4);
        const closedPath = ref([]);

        const startRoute = (index) => {
            path.value = [];

            if (cells.value[index]) {
                path.value.push(index);
            }

        }

        const finishRoute = (index) => {
            if (index !== path.value[0] && cells.value[index] === cells.value[path.value[0]]) {
                closedPath.value = closedPath.value.concat(path.value);
            }
            
            path.value = [];
        }

        const go = (index) => {
            if(path.value.length) {
                const lastIndex = path.value[path.value.length - 1];

                if(Math.abs(lastIndex - index) === 1 || Math.abs(lastIndex - index) === size.value) {
                    path.value.push(index);
                }
            }
        }

        const checkRoute = (index) => {
            return path.value.findIndex(p => p === index) > -1;
        }

        const checkClosedRoute = (index) => {
            return closedPath.value.findIndex(p => p === index) > -1;
        }



        return {
            cells,
            startRoute,
            finishRoute,
            go,
            path,
            checkRoute,
            size,
            closedPath,
            checkClosedRoute
        }
    }
}
</script>

<style scoped>
.board {
    display: flex;
    width: 200px;
    margin: 30px auto 0;

    flex-wrap: wrap;

    /* background-color: #cdcdcd; */
}
</style>