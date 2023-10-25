<template>
    <span class="level">Level: {{ lvl }}</span>
    <div class="board">
        <BoardItem 
            v-for="(cell, index) in cells" 
            :key="index" 
            :icon="cell"
            @mousedown="startRoute(index)"
            @mouseup="finishRoute(index)"
            @mousemove="go(index)"
            :selected="checkRoute(index)"
            :closed="isRouteClosed(index)"
        />
    </div>
    <button @click="reload" class="reload">Reset this level</button>
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
        const lvl = ref(1);

        const startRoute = (index) => {
            path.value = [];

            if (cells.value[index] && !isRouteClosed(index)) {
                path.value.push(index);
            }

        }

        const finishRoute = (index) => {
            if (index !== path.value[0] && cells.value[index] === cells.value[path.value[0]]) {
                closedPath.value = closedPath.value.concat(path.value);
            }
            
            path.value = [];

            checkLvl();
        }

        const checkLvl = () => {
            let completed = true;

            cells.value.forEach((cell, index) => {
                if (cell && !isRouteClosed(index)) {
                    completed = false
                }
            })

            if (completed) {
                alert('You won!');
            }
        }

        const go = (index) => {
            if(path.value.length) {
                const lastIndex = path.value[path.value.length - 1];

                if(Math.abs(lastIndex - index) === 1 || Math.abs(lastIndex - index) === size.value && !isRouteClosed(index)) {
                    path.value.push(index);
                }

                if (isRouteClosed(index) || (cells.value[index] && cells.value[index] !== cells.value[path.value[0]])) {
                    path.value = [];
                }
            }
        }

        const checkRoute = (index) => {
            return path.value.findIndex(p => p === index) > -1;
        }

        const isRouteClosed = (index) => {
            return closedPath.value.findIndex(p => p === index) > -1;
        }

        const startLevel = (lvl) => {
            if (lvl === 1) {
                cells.value = ref([3, 0, 0, 1, 0, 0, 1, 0, 2, 0, 0, 0, 0, 2, 0, 3]);
            }

            if (lvl === 2) {
                cells.value = ref([3, 1, 0, 1, 0, 0, 0, 0, 2, 0, 0, 0, 0, 2, 0, 3]);
            }

            size.value = 4;
            path.value = [];
            closedPath.value = [];
        }

        startLevel();

        const reload = () => {
            startLevel(lvl);
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
            isRouteClosed,
            reload,
            lvl
        }
    }
}
</script>

<style scoped>
.board {
    display: flex;
    width: 200px;
    margin: 20px auto 0;

    flex-wrap: wrap;

    /* background-color: #cdcdcd; */
}

.level {
    display: block;
    margin-top: 20px;

    font-size: 16px;
    font-weight: 600;
}

.reload {
    margin-top: 30px;
    padding: 0;
    border: none;

    background-color: transparent;

    font-size: 14px;
    text-decoration: underline;   

    cursor: pointer;
    transition: color 0.25s;
}

.reload:hover {
    color: #ff3300aa;
}
</style>