<script setup lang="ts">
import { Graph } from '@antv/x6'

const props = defineProps<{
    graph: Graph
}>()

const undo = () => {
    if (props.graph.canUndo()) {
        props.graph.undo()
    }
}

const redo = () => {
    if (props.graph.canRedo()) {
        props.graph.redo()
    }
}
</script>
<template>
    <div class="toolbar">
        <button @click="undo">撤销</button>
        <button @click="redo">重做</button>
    </div>
</template>

<style scoped></style>