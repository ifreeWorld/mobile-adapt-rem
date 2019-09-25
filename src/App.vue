<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link> |
      <router-link to="/retina">retina 屏下的1px线的实现</router-link>
    </div>
    <a href="https://juejin.im/post/5d70747cf265da03e16897c8" target="_blank"
      >前端基础知识概述 -- 移动端开发的屏幕、图像、字体与布局的兼容适配
    </a>
    <router-view />
  </div>
</template>

<!-- less的rem转换函数 -->
<style lang="less">
@baseFontSize: 750; /* ui图的宽度 */
.pxToRem(@name, @px) {
  @{name}: unit(@px / @baseFontSize * 10, rem);
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  // .pxToRem(font-size, 37.5);
  font-size: 37.5px;
  color: #2c3e50;
}
#nav {
  // .pxToRem(padding, 60);
  padding: 60px;
  a {
    font-weight: bold;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>

<!-- sass的rem转换函数 -->
<style lang="scss">
$baseFontSize: 750;
@function pxToRem($px) {
  @return $px / $baseFontSize * 10rem;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  // font-size: pxToRem(37.5);
  font-size: 37.5px;
  color: #2c3e50;
}
#nav {
  // padding: pxToRem(30);
  padding: 30px;
  a {
    font-weight: bold;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
