<template>
    <div class="item" :class="{'selected': selected, 'closed': closed}">
        {{ icon ? icon : ''}}
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
}

.item.selected {
    background-color: #00ff6655;
}

.item.closed {
    background-color: #00ff6622;
}
</style>