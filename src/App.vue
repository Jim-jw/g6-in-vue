<template>
  <div>
    <div ref="graph">
      <transition
        name="el-fade-in-linear">
        <NodeTooltip
          v-show="showNodeTooltip"
          :x="nodeTooltipX"
          :y="nodeTooltipY"/>
      </transition>
      <transition
        name="el-fade-in-linear">
        <EdgTooltip
          v-show="showEdgeTooltip"
          :x="edgeTooltipX"
          :y="edgeTooltipY"/>
      </transition>
      <transition
        name="el-fade-in-linear">
        <ContextMenu
          v-show="showNodeContextMenu"
          :x="nodeContextMenuX"
          :y="nodeContextMenuY"/>
      </transition>
    </div>
  </div>
</template>
<script>
  import G6 from '@antv/g6';
  import { data } from './plugins/data'; // 渲染Graph的数据
  import './plugins/registerShape'; // 自定义节点；自定义边；
  import './style/index.css';
  import EdgTooltip from './components/edgTooltip';
  import NodeTooltip from './components/nodeTooltip';
  import ContextMenu from './components/contextMenu';

  export default {
    components: {
      NodeTooltip,
      EdgTooltip,
      ContextMenu,
    },
    data() {
      return {
        graph: null,
        showNodeTooltip: false,
        nodeTooltipX: 0,
        nodeTooltipY: 0,
        showEdgeTooltip: false,
        edgeTooltipX: 0,
        edgeTooltipY: 0,
        showNodeContextMenu: false,
        nodeContextMenuX: 0,
        nodeContextMenuY: 0,
      }
    },
    mounted() {
      this.$nextTick(() => {
        const miniMap = new G6.Minimap()
        this.graph = new G6.Graph({
          container: this.$refs['graph'],
          width: 1200,
          height: 800,
          modes: {
            default: ['drag-canvas', 'drag-node']
          },
          defaultNode: {
            type: 'node',
            // 节点文本样式
            labelCfg: {
              style: {
                fill: '#000000A6',
                fontSize: 10
              }
            },
            // 节点默认样式
            style: {
              stroke: '#72CC4A',
              width: 150
            }
          },
          defaultEdge: {
            type: 'polyline'
          },
          // 节点交互状态配置
          nodeStateStyles: {
            hover: {
              stroke: 'red',
              lineWidth: 3
            }
          },
          edgeStateStyles: {
            hover: {
              stroke: 'blue',
              lineWidth: 3
            }
          },
          layout: {
            type: 'dagre',
            rankdir: 'LR',
            nodesep: 30,
            ranksep: 100
          },
          plugins: [miniMap]
        })
        this.graph.data(data)

        this.graph.render()

        const edges = this.graph.getEdges()
        edges.forEach(edge => {
          const line = edge.getKeyShape()
          const stroke = line.attr('stroke')
          const targetNode = edge.getTarget()
          targetNode.update({
            style: { stroke }
          })
        })
        this.graph.paint()
        this.bindEvents()
      })
    },
    methods: {
      bindEvents() {
        this.graph.on('edge:mouseenter', evt => {
          const { item, target } = evt

          const type = target.get('type')
          if(type !== 'text') {
            return
          }
          const model = item.getModel()
          const { endPoint } = model
          // y=endPoint.y - height / 2，在同一水平线上，x值=endPoint.x - width - 10
          const y = endPoint.y - 35
          const x = endPoint.x - 150 - 10
          const point = this.graph.getCanvasByPoint(x, y)
          this.edgeTooltipX = point.x
          this.edgeTooltipY = point.y
          this.showEdgeTooltip = true
        })

        this.graph.on('edge:mouseleave', () => {
          this.showEdgeTooltip = false
        })

        // 监听node上面mouse事件
        this.graph.on('node:mouseenter', evt => {
          const { item } = evt
          const model = item.getModel()
          const { x, y } = model
          const point = this.graph.getCanvasByPoint(x, y)
          this.nodeTooltipX = point.x - 70
          this.nodeTooltipY = point.y + 23
          this.showNodeTooltip = true
        })

        // 节点上面触发mouseleave事件后隐藏tooltip和ContextMenu
        this.graph.on('node:mouseleave', () => {
          this.showNodeTooltip = false
          this.showNodeContextMenu = false
        })

        this.graph.on('node:contextmenu', evt => {
          const { item } = evt
          const model = item.getModel()
          const { x, y } = model
          const point = this.graph.getCanvasByPoint(x, y)
          this.nodeContextMenuX = point.x
          this.nodeContextMenuY = point.y + 15
          this.showNodeContextMenu = true
        })
      },
    },
  }
</script>