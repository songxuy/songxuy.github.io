title: Vue中使用v-viewer插件
author: coolsong
date: 2019-07-20 23:30:27
tags:
  - Vue
categories: 
  - vue
---
&nbsp;&nbsp;&nbsp;最近的一个社区项目中需要用到移动端图片预览的功能，还需要能够左右切换、缩放等功能，于是在网上找了一下。发现vue-photo-preview和v-viewer（将viewerjs、封装为vue组件来调用）都还还不错，最终还是选择了v-viewer，感觉其缩放比较人性化，不用点击按钮，并且下面的缩略图还不错。下面就介绍一下v-viewer的使用：
<!--more-->
### -安装
```
npm install v-viewer
```

### -引入

* 这里是在main.js中直接引入的，并设置了默认的选项

```Javascript
import Viewer from 'v-viewer'
import 'viewerjs/dist/viewer.css'

Vue.use(Viewer, {
  defaultOptions: {
  zIndex: 9999,
  toolbar: 0,
  keyboard: false,
  title: false,
  movable: true,
  zoomable: false,
  scalable: false
  }
})
```

* 具体参数的解释会在后边展示
* 除了在main.js中调用，还可以在我们需要的页面直接引入

```JavaScript
  import 'viewerjs/dist/viewer.css'
  import Viewer from 'v-viewer'
  import Vue from 'vue'
  Vue.use(Viewer)
  export default {
    data() {
      images: ['1.jpg', '2.jpg']
    },
    methods: {
      show () {
        const viewer = this.$el.querySelector('.images').$viewer
        viewer.show()
      }
    }
  }
```

### -使用

* 通过viewer标签包住一个图片列表就能实现
```Html
<viewer :class="$style.loadData" id="img" :images="src1" @inited="inited" ref="viewer">
    <div v-for="(item, index) in src1" :key="index+10"><img :src="require('./assets/images/'+item)" alt="" id="img" > </div>
  </viewer>
```
### -展示
![My Pic](/images/view.png)

### -参数列表
<table>
<thead><tr><th align="left">参数名称</th><th align="left">参数类型</th><th align="left">默认值</th><th align="left">说明</th></tr></thead><tbody><tr><td align="left">initialViewIndex</td><td align="left">Number</td><td align="left">0</td><td align="left">定义用于查看的图像的初始索引</td></tr><tr><td align="left">inline</td><td align="left">Boolean</td><td align="left">false</td><td align="left">支持 inline mode</td></tr><tr><td align="left">button</td><td align="left">Boolean</td><td align="left">true</td><td align="left">是否显示查看图片时右上角的关闭按钮</td></tr><tr><td align="left">navbar</td><td align="left">Boolean / Number</td><td align="left">true</td><td align="left">是否显示底部导航栏 <br><code>0</code> 或者 <code>false</code> :不显示 <br><code>1</code> 或者 <code>true</code> :显示 <br><code> 2 </code>:当屏幕宽度大于768px时显示 <br><code>3 </code>:当屏幕宽度大于992px时显示 <br><code>4 </code>:当屏幕宽度大于1200px时显示</td></tr><tr><td align="left">title</td><td align="left">Boolean / Number /<br> Function / Array</td><td align="left">true</td><td align="left"><code> 0 </code> 或者 <code>false</code> 时不显示<br><code>1</code>或者<code>true</code>或者<code>function</code>或者<code>array</code>时显示<br><code>2 </code>:当屏幕宽度大于768px时显示 <br><code>3 </code>:当屏幕宽度大于992px时显示 <br><code>4 </code>:当屏幕宽度大于1200px时显示<br><code>function</code> 在函数体内返回标题<br><code>array</code> 第一个参数表示可见性(0-4) 第二个参数就是标题</td></tr><tr><td align="left">toolbar</td><td align="left">Boolean / Number / Object</td><td align="left">true</td><td align="left">标题栏是否显示和布局 <br><code> 0 </code> 或者 <code>false</code> 时不显示<br><code>1</code>或者<code>true</code>或者时显示<br><code> 2 </code>:当屏幕宽度大于768px时显示 <br><code>3 </code>:当屏幕宽度大于992px时显示 <br><code>4 </code>:当屏幕宽度大于1200px时显示 <br><code>Object</code> : <a href="#articleHeader2">Object类型详解</a></td></tr><tr><td align="left">tooltip</td><td align="left">Boolean</td><td align="left">true</td><td align="left">放大或缩小时显示的百分比的文字提示<br><code>true</code> : 显示 <br><code>false</code> : 不显示</td></tr><tr><td align="left">movable</td><td align="left">Boolean</td><td align="left">true</td><td align="left">是否可以拖动图片</td></tr><tr><td align="left">zoomable</td><td align="left">Boolean</td><td align="left">true</td><td align="left">是否可以缩放图片</td></tr><tr><td align="left">rotatable</td><td align="left">Boolean</td><td align="left">true</td><td align="left">是否可以旋转图片</td></tr><tr><td align="left">scalable</td><td align="left">Boolean</td><td align="left">true</td><td align="left">是否可以缩放图片</td></tr><tr><td align="left">transition</td><td align="left">Boolean</td><td align="left">true</td><td align="left">为一些特殊元素启用CSS3转换。</td></tr><tr><td align="left">fullscreen</td><td align="left">Boolean</td><td align="left">true</td><td align="left">允许全屏播放</td></tr><tr><td align="left">keyboard</td><td align="left">Boolean</td><td align="left">true</td><td align="left">启用键盘支持</td</tr><tr><td align="left">backdrop</td><td align="left">Boolean / String</td><td align="left">true</td><td align="left">启用 modal 为false的时候不支持点击背景关闭</td></tr><tr><td align="left">loading</td><td align="left">Boolean</td><td align="left">true</td><td align="left">加载图片的时候的loading图标</td></tr><tr><td align="left">loop</td><td align="left">Boolean</td><td align="left">true</td><td align="left">是否可以循环查看图片</td></tr><tr><td align="left">interval</td><td align="left">Number</td><td align="left">5000</td><td align="left">定义图片查看器的最小的宽度</td></tr><tr><td align="left">minWidth</td><td align="left">Number</td><td align="left">200</td><td align="left">定义图片查看器的最小的高度</td></tr><tr><td align="left">minHeight</td><td align="left">Number</td><td align="left">100</td><td align="left">播放图片时  距离下一张图片的间隔时间</td></tr><tr><td align="left">zoomRatio</td><td align="left">Number</td><td align="left">0.1</td><td align="left">利用鼠标滚轮缩放图片时的比例</td></tr><tr><td align="left">minZoomRatio</td><td align="left">Number</td><td align="left">0.01</td><td align="left">缩小图片的最小比例</td></tr><tr><td align="left">maxZoomRatio</td><td align="left">Number</td><td align="left">100</td><td align="left">放大图片的放大比例</td></tr><tr><td align="left">zIndex</td><td align="left">Number</td><td align="left">2015</td><td align="left">定义查看器的CSS z-index值 modal 模式下</td></tr><tr><td align="left">zIndexInline</td><td align="left">Number</td><td align="left">0</td><td align="left">定义查看器的CSS z-index值 inline 模式下</td></tr><tr><td align="left">url</td><td align="left">String / Function</td><td align="left">src</td><td align="left">原始图像URL<br>如果是一个字符串，应该图像元素的属性之一<br>如果是一个函数，应该返回一个有效的图像URL</td></tr><tr><td align="left">container</td><td align="left">Element / String</td><td align="left">body</td><td align="left">将查看器置于modal模式的容器 <br> 只有在 inline为 false的时候才可以使用</td></tr><tr><td align="left">filter</td><td align="left">Function</td><td align="left">null</td><td align="left">过滤图像以便查看(如果图像是可见的，应该返回true)</td></tr><tr><td align="left">toggleOnDblclick</td><td align="left">Boolean</td><td align="left">true</td><td align="left">当你放大或者缩小图片时 双击还原</td></tr><tr><td align="left">ready</td><td align="left">Function</td><td align="left">null</td><td align="left">当查看图片时被触发的函数  只会触发一次</td></tr><tr><td align="left">show</td><td align="left">Function</td><td align="left">null</td><td align="left">当查看图片时被触发的函数 每次查看都会触发</td></tr><tr><td align="left">shown</td><td align="left">Function</td><td align="left">null</td><td align="left">当查看图片时被触发的函数 每次查看都会触发 在show之后</td></tr><tr><td align="left">hide</td><td align="left">Function</td><td align="left">null</td><td align="left">当关闭图片查看器时被触发的函数 每次关闭都会触发</td></tr><tr><td align="left">hidden</td><td align="left">Function</td><td align="left">null</td><td align="left">当关闭图片查看器时被触发的函数 每次关闭都会触发 在hide之后</td></tr><tr><td align="left">view</td><td align="left">Function</td><td align="left">null</td><td align="left">当查看图片时被触发的函数 每次查看都会触发 在shown之后</td></tr><tr><td align="left">viewed</td><td align="left">Function</td><td align="left">null</td><td align="left">当查看图片时被触发的函数 每次查看都会触发 在view之后</td></tr><tr><td align="left">zoom</td><td align="left">Function</td><td align="left">null</td><td align="left">在图片缩放时触发</td></tr><tr><td align="left">zoomed</td><td align="left">Function</td><td align="left">null</td><td align="left">在图片缩放时触发 在 zoom之后</td></tr></tbody></table>

>toolbar Object详解
>key值列表: "zoomIn", "zoomOut", "oneToOne", "reset", "prev", "play", "next", "rotateLeft", "rotateRight", "flipHorizontal", "flipVertical"

<table><thead><tr><th align="left">key值名称</th><th align="left">说明</th></tr></thead><tbody><tr><td align="left">zoomIn</td><td align="left">放大图片的按钮</td></tr><tr><td align="left">zoomOut</td><td align="left">缩小图片的按钮</td></tr><tr><td align="left">reset</td><td align="left">重置图片大小的按钮</td></tr><tr><td align="left">prev</td><td align="left">查看上一张图片的按钮</td></tr><tr><td align="left">next</td><td align="left">查看上一张图片的按钮</td></tr><tr><td align="left">play</td><td align="left">播放图片的按钮</td></tr><tr><td align="left">rotateLeft</td><td align="left">向左旋转图片的按钮</td></tr><tr><td align="left">rotateRight</td><td align="left">向右旋转图片的按钮</td></tr><tr><td align="left">flipHorizontal</td><td align="left">图片左右翻转的按钮</td></tr><tr><td align="left">flipVertical</td><td align="left">图片上下翻转的按钮</td></tr></tbody></table>

### -参考连接

* [viewjs github](https://github.com/fengyuanchen/viewerjs"%3Ehttps://github.com/fengyuanchen/viewerjs)
* [v-view文档](https://mirari.cc/2017/08/27/Vue%E5%9B%BE%E7%89%87%E6%B5%8F%E8%A7%88%E7%BB%84%E4%BB%B6v-viewer%EF%BC%8C%E6%94%AF%E6%8C%81%E6%97%8B%E8%BD%AC%E3%80%81%E7%BC%A9%E6%94%BE%E3%80%81%E7%BF%BB%E8%BD%AC%E7%AD%89%E6%93%8D%E4%BD%9C/"%3Ehttps://mirari.cc/2017/08/27/Vue%E5%9B%BE%E7%89%87%E6%B5%8F%E8%A7%88%E7%BB%84%E4%BB%B6v-viewer%EF%BC%8C%E6%94%AF%E6%8C%81%E6%97%8B%E8%BD%AC%E3%80%81%E7%BC%A9%E6%94%BE%E3%80%81%E7%BF%BB%E8%BD%AC%E7%AD%89%E6%93%8D%E4%BD%9C/%3C/a%3E)
* [viewjs demo](https://fengyuanchen.github.io/viewerjs/"%3Ehttps://fengyuanchen.github.io/viewerjs/%3C/a%3E)
