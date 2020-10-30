<template>
  <div
    class="edgeTooltips"
    :style="{ 'top': `${y}px`, 'left': `${x}px`}">
    <div class="edgeTitle">
      <p class="tooltipsCommon">凭证开立</p>
      <p class="tooltipsCommon tooltipsMoney">1000,000,000元</p>
      <p class="tooltipsCommon tooltipsDate">2019-09-10</p>
    </div>
    <div class="edgeDetail">
      <div class="detailContent">
        <p class="edgeCode">交易编码：</p>
        <span class="edgeValue">1000190203455</span>
      </div>
      <div class="detailContent">
        <p class="edgeCode">交易编码：</p>
        <span class="edgeValue">1000190203455</span>
      </div>
      <div class="detailContent">
        <p class="edgeCode">交易编码：</p>
        <span class="edgeValue">1000190203455</span>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'edgTooltip',
  props: {
    x: Number,
    y: Number,
  },
}
</script>