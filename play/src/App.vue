<script setup lang="ts">
import { Graph } from '@antv/g6';
import { onMounted } from 'vue';
let graph: Graph;
const onClick = () => {
  const edge = graph.getEdgeData('0-1');
  graph?.updateEdgeData([
    {
      id: edge.id,
      source: edge.target,
      target: '2',
    },
  ]);
  graph.render()
}
onMounted(() => {
  graph = new Graph({
    container: 'graph',
    data: {
      nodes: [
        {
          id: '0',
          name: 'a',
        },
        {
          id: '1',
          name: 'b',
        },
        {
          id: '2',
          name: 'c',
        },
      ],
      edges: [
        {
          id: '0-1',
          source: '0',
          target: '1',
        },
      ],
    },
    node: {
      style: {
        label: true,
        labelText: (node: any) => {
          return node.id;
        },
      },
    },
    edge: {
      style: {
        endArrow: true,
        label: true,
        labelText: (edge: any) => {
          return `${edge.source} > ${edge.target}`;
        },
      },
    },
    layout: {
      type: 'force',
      linkDistance: 50,
      clustering: true,
      nodeClusterBy: 'cluster',
      clusterNodeStrength: 70,
    },
    behaviors: ['drag-element'],
  });
  graph.render()
})
</script>

<template>
  <div class="main">
    <div id="graph"></div>
    <button @click="onClick">
      更新边
    </button>
  </div>
</template>

<style>
body,
html,
#app,
.main {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
}

#graph {
  width: 100%;
  height: 500px;
}
</style>
