<template>
  <div
    class="nodeTooltips"
    :style="{ 'top': `${y}px`, 'left': `${x}px`}">
    <el-timeline>
      <el-timeline-item>Create a services site 2015-09-01</el-timeline-item>
      <el-timeline-item>Solve initial network problems 2015-09-01</el-timeline-item>
      <el-timeline-item>Technical testing 2015-09-01</el-timeline-item>
      <el-timeline-item>Network problems being solved 2015-09-01</el-timeline-item>
    </el-timeline>
  </div>
</template>
<script>
import {Timeline, TimelineItem} from 'element-ui';

export default {
  name: 'nodeTooltip',
  components: {
    ElTimeline: Timeline,
    ElTimelineItem: TimelineItem
  },
  props: {
    x: [Number, String],
    y: [Number, String],
  },
}
</script>