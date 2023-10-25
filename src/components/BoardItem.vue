<template>
    <div class="item" :class="{'selected': selected, 'closed': closed}">
        <i v-if="icon" class="icon" :class="'icon-' + icon"></i>
    </div>
</template>

<script>
export default {
    props: {
        icon: {
            type: Number,
            required: false,
            default: 0
        },
        selected: {
            type: Boolean,
            required: false,
            default: false
        },
        closed: {
            type: Boolean,
            required: false,
            default: false
        }
    }
}
</script>

<style scoped>
.item {
    width: 50px;
    height: 50px;
    border: 1px solid #2c3e50;

    position: relative;
}

.item.selected {
    background-color: #00ff6655;
}

.item.closed {
    background-color: #00ff6622;
}

.icon {
    display: block;
    width: 100%;
    height: 100%;

    position: absolute;
    top: 0;
    left: 0;
    
}

.icon-1,
.icon-2,
.icon-3 {
    background: url('../assets/icons.png');
    background-size: 150px 150px;
}

.icon-1 {
    background-position: 0 0;
}

.icon-2 {
    background-position: 50px 0;
}

.icon-3 {
    background-position: 100px 0;
}
</style>