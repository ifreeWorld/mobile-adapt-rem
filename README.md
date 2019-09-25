# Rem布局

## flexible
```
npm i -S amfe-flexible
```

根据 document.documentElement.clientWidth 动态修改 <html> 的 font-size，页面其他元素使用 rem 作为长度单位进行布局，从而实现页面的等比缩放
```
(function flexible (window, document) {
  var docEl = document.documentElement
  var dpr = window.devicePixelRatio || 1

  // adjust body font size
  function setBodyFontSize () {
    if (document.body) {
      document.body.style.fontSize = (12 * dpr) + 'px'
    }
    else {
      document.addEventListener('DOMContentLoaded', setBodyFontSize)
    }
  }
  setBodyFontSize();

  // set 1rem = viewWidth / 10
  function setRemUnit () {
    var rem = docEl.clientWidth / 10
    docEl.style.fontSize = rem + 'px'
  }

  setRemUnit()

  // reset rem unit on page resize
  window.addEventListener('resize', setRemUnit)
  window.addEventListener('pageshow', function (e) {
    if (e.persisted) {
      setRemUnit()
    }
  })

  // detect 0.5px supports
  if (dpr >= 2) {
    var fakeBody = document.createElement('body')
    var testElement = document.createElement('div')
    testElement.style.border = '.5px solid transparent'
    fakeBody.appendChild(testElement)
    docEl.appendChild(fakeBody)
    if (testElement.offsetHeight === 1) {
      docEl.classList.add('hairlines')
    }
    docEl.removeChild(fakeBody)
  }
}(window, document))
```

## pxToRem换算
通过rem 换算，我们能够知道，如何把视觉稿（750px）中元素的 px 转换成 rem

### sass方案
```
$baseFontSize: 750;
@function pxToRem($px) {
  @return $px / $baseFontSize * 10rem;
}
#nav {
  padding: pxToRem(30); // padding: 0.4rem;
}
```

### less方案
```
@baseFontSize: 750; /* ui图的宽度 */
.pxToRem(@name, @px) {
  @{name}: unit(@px / @baseFontSize * 10, rem);
}
#nav {
  .pxToRem(padding, 60); // padding: 0.8rem;
}
```

### postcss-pxtorem方案
```
npm install postcss-pxtorem --save-dev
```

postcss.config.js配置修改
```
module.exports = {
  plugins: {
    autoprefixer: {},
    'postcss-pxtorem': {
      rootValue: 75, // 根字体大小
      unitPrecision: 5, // 精度
      propList: ['*'], // 可以将px转换成rem的属性
      selectorBlackList: [], // 选择器忽略并保留px
      replace: true, // 替换包含rems的规则
      mediaQuery: false, // 是否允许在媒体查询中转换px
      minPixelValue: 2 // 设置要替换的最小像素值
    }
  }
}
```

### retina 屏下的1px线的实现方案
设计师想要的 retina 下 **border: 1px**，其实是 1 物理像素宽，而不是 1 CSS 像素宽度，对于 CSS 而言：

- 在 dpr = 1 时，此时 1 物理像素等于 1 CSS 像素宽度
- 在 dpr = 2 时，此时 1 物理像素等于 0.5 CSS 宽度像素，可以认为 **border-width: 1px** 这里的 1px 其实是 1 CSS像素宽度，等于 2 像素物理宽度，设计师其实想要的是 **border-width: 0.5px**
- 在 dpr = 3 时，此时 1 物理像素等于 0.33 CSS 宽度像素，设计师其实想要的是 **border: 0.333px**

然而，并不是所有手机浏览器都能识别 border-width: 0.5px，在 iOS7 以下，Android 等其他系统里，小于 1px 的单位会被当成为 0px 处理，那么如何实现这 0.5px、0.33px 呢？

三种方法实现：

- 使用渐变实现，使用两种颜色填充1px宽内容
- 使用缩放实现，对 1px 高度线条进行0.5/0.33倍缩放
- base64 编码实现

[**demo**](src/views/retina.vue)



# 参考文档
- [**frozenui demo**](http://frozenui.github.io/frozenui/demo/index.html)
- [**lib-flexible**](https://github.com/amfe/lib-flexible)
- [**postcss-pxtorem**](https://github.com/cuth/postcss-pxtorem)
- [**拿到一份设计稿，我该如何进行移动端开发**](https://juejin.im/post/5d736747e51d4561ff66688c)
- [**前端基础知识概述 -- 移动端开发的屏幕、图像、字体与布局的兼容适配**](https://juejin.im/post/5d70747cf265da03e16897c8)