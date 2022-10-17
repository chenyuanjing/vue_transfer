<template>
    <div>
        <div
        v-for="item of data"
        :key="item.id"
        :class="['list-item', item.disabled ? 'disabled' : '']"
        :draggable="!item.disabled"
        @dragstart="dragItem(item)"
    >
        <input
            :id="'__checkbox__' + item.id"
            type="checkbox"
            :disabled="item.disabled"
            @click="checkboxClick($event.target.checked, leftOrRight, item)"
        >
        <label :for="'__checkbox__' + item.id">{{ item.name }}</label>
    </div>
    </div>
</template>

<script setup>

const props = defineProps({
    data: {
        type: Array,
        default: () => []
    },
    leftOrRight: {
        type: String,
        validator(value) {
            return ['left', 'right'].includes(value);
        }
    }
})

const emit = defineEmits(['checkboxClick', 'tragItem']);

const checkboxClick = (...args) => {
    emit('checkboxClick', ...args)
}

const dragItem = (item) => {
    emit('dragItem', item);
}

</script>

<style lang="scss" scoped>

</style>