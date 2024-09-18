<script setup lang="ts">
import { getTeleport } from '@antv/x6-vue-shape'
import { Graph } from '@antv/x6'
import { ref, onMounted } from 'vue'
import { History } from '@antv/x6-plugin-history'
import { Keyboard } from '@antv/x6-plugin-keyboard'
import ToolBar from './ToolBar.vue'

const TeleportContainer = getTeleport()
const graphRef = ref<Graph>();
onMounted(() => {
  const container = document.getElementById('container')
  if (container) {
    graphRef.value = new Graph({
      container: container,
      width: 800,
      height: 600,
      background: {
        color: '#F2F7FA',
      },
      autoResize: true,

    })
    graphRef.value.use(new History({ enabled: true }));
    graphRef.value.use(
      new Keyboard({
        enabled: true,
      }),
    )
    // 绑定ctrl+z 撤销
    graphRef.value.bindKey('ctrl+z', () => {
      if (graphRef.value!.canUndo()) {
        graphRef.value!.undo()
      }
      return false
    });
    // Add nodes
    const node1 = graphRef.value.addNode({
      shape: 'rect',
      width: 100,
      height: 40,
      x: 100,
      y: 100,
      label: 'Node 1',
    })

    const node2 = graphRef.value.addNode({
      shape: 'circle',
      width: 60,
      height: 60,
      x: 300,
      y: 200,
      label: 'Node 2',
    })

    // Add an edge between nodes
    graphRef.value.addEdge({
      source: node1,
      target: node2,
    })
  }
})

</script>

<template>
  <div class="wrapper">
    <ToolBar v-if="graphRef" :graph="graphRef" />
    <TeleportContainer />
    <div id="container"></div>
  </div>
</template>

<style scoped>
.wrapper {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
