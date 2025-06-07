<script setup lang="ts">
import { Graph, treeToGraphData } from '@antv/g6';
import { onMounted } from 'vue';
let graph: Graph;

function isLeafNode(d: any) {
    return !d.children || d.children.length === 0;
}


onMounted(() => {
    fetch('https://gw.alipayobjects.com/os/antvdemo/assets/data/algorithm-category.json')
        .then((res) => res.json())
        .then((data) => {
            graph = new Graph({
                container: 'graph',
                autoFit: 'view',
                data: treeToGraphData(data),
                behaviors: ['drag-canvas', 'zoom-canvas', 'drag-element', 'collapse-expand'],
                node: {
                    style: {
                        labelText: (d) => d.id,
                        labelPlacement: (d) => (isLeafNode(d) ? 'right' : 'left'),
                        labelBackground: true,
                        ports: [{ placement: 'right' }, { placement: 'left' }],
                    },
                    animation: {
                        enter: false,
                    },
                },
                edge: {
                    type: 'cubic-horizontal',
                    animation: {
                        enter: false,
                    },
                },
                layout: {
                    type: 'compact-box',
                    direction: 'LR',
                    getHeight: function getHeight() {
                        return 32;
                    },
                    getWidth: function getWidth() {
                        return 32;
                    },
                    getVGap: function getVGap() {
                        return 10;
                    },
                    getHGap: function getHGap() {
                        return 100;
                    },
                },
                plugins: [
                    {
                        type: 'minimap', // 小地图
                        size: [142, 107],
                        delay: 0
                    },
                ]
            });

            graph.render();
        });
})
</script>

<template>
    <div class="main">
        <div id="graph"></div>
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
    height: 100%;
}
</style>
