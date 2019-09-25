<!-- retina 屏下的1px线的实现 -->
<template>
  <div>
    <div id="dpr">当前设备的dpr:</div>
    <div class="border border_normal">
      正常使用1px border效果（请在移动端环境下查看）
    </div>
    <div class="border border_gradient">
      法一：使用渐变实现，使用两种颜色填充1px宽内容
    </div>
    <div class="border border_scale">
      法二：使用缩放实现，对 1px 高度线条进行0.5/0.33倍缩放
    </div>
    <div class="border_base64">法三：base64 编码实现</div>
  </div>
</template>
<script>
export default {
  name: 'retina',
  components: {},
  data() {
    return {}
  },
  methods: {},
  computed: {},
  watch: {}
}
</script>

<!-- retina屏下的1px线的实现 -->
<style lang="less" scoped>
#dpr {
  position: relative;
  display: inline-block;
  font-size: 40px;
  text-align: center;

  &::after {
    content: '1';
    color: #ff0000;
    font-size: 40px;
  }
}
.border {
  border-top: 1px solid #999;
}

// dpr为2的设备
@media only screen and (-webkit-min-device-pixel-ratio: 2) {
  .border_gradient {
    background: linear-gradient(to top, transparent 50%, #999 50%) no-repeat top
      center;
    background-size: 100% 1px;
    border-top: 0 none;
    padding-top: 1px;
    box-sizing: border-box;
  }
  .border_scale {
    position: relative;
    border-top: 0 none;
    padding-top: 1px;
  }
  .border_scale:before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    border-top: 1px solid #999;
    transform: scaleY(0.5);
    transform-origin: 0 0;
    box-sizing: border-box;
  }
  .border_base64 {
    border-top: 0 none;
    padding-top: 1px;
    background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAACCAYAAACZgbYnAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAAQSURBVBhXY5g5c+Z/BhAAABRcAsvqBShzAAAAAElFTkSuQmCC);
    background-position: 0 0;
    background-repeat: repeat-x;
    background-size: 1px 1px;
  }
}

// dpr为3的设备
@media only screen and (-webkit-min-device-pixel-ratio: 3) {
  .border_gradient {
    background: linear-gradient(to top, transparent 66.66%, #999 66.66%)
      no-repeat top center;
    background-size: 100% 1px;
    border-top: 0 none;
    padding-top: 1px;
    box-sizing: border-box;
  }
  .border_scale {
    position: relative;
    border-top: 0 none;
    padding-top: 1px;
  }
  .border_scale:before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    border-top: 1px solid #999;
    transform: scaleY(0.333);
    transform-origin: 0 0;
    box-sizing: border-box;
  }
  .border_base64 {
    padding-top: 1px;
    border-top: 0 none;
    background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAACCAYAAACZgbYnAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAAQSURBVBhXY5g5c+Z/BhAAABRcAsvqBShzAAAAAElFTkSuQmCC);
    background-position: 0 0;
    background-repeat: repeat-x;
    background-size: 1px 1px;
  }
}
</style>
