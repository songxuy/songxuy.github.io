title: Vue中使用无限滚动插件和拖动插件
author: coolsong
tags:
  - Vue
categories: 
  - Vue
date: 2019-07-28 22:53:00
---
&nbsp;&nbsp;&nbsp;项目中也用到了无限滚动和列表拖拽的需求。无限滚动本来自己写了一个demo，但是使用起来效果不是很好。最后在网上找了一下，最后选择了vue-infinite-scroll，效果还是不错的。除此之外，还有列表拖动的需求，也找了一下，最终选择了vuedraggable，可选的配置参数比较多，我们自己的操作性比较大。下面就介绍一下这两个插件在vue中的使用：
<!--more-->
### vue-infinite-scroll

#### -安装

* * *

```
npm install vue-infinite-scroll --save
```
#### -使用

* * *


* 在main.js中引入插件，再通过Vue.use()来引用,之后就能直接在代码中使用该组件了

```JavaScript
import infiniteScroll from 'vue-infinite-scroll'
Vue.use(infiniteScroll)
```

* 在组件中的使用

```html
<div v-infinite-scroll="loadMore" infinite-scroll-disabled="busy" infinite-scroll-distance="10">
  <div v-for="item in data" :key="item.index">{{item.name}}            </div>
</div>
```

* * *

```JavaScript
methods: {
    loadMore: function() {
      this.busy = true
      setTimeout(() => {
        for (var i = 0, j = 10; i < j; i++) {
          this.data.push({name: this.count++ })
        }
        console.log(this.data)
        this.busy = false
      }, 1000)
    }
  }
```

#### -选项参数

* * *


> v-infinite-scroll="loadMore"表示回调函数是loadMoreinfinite-scroll-disabled="busy"表示由变量busy决定是否执行loadMore，false则执行loadMore，true则不执行。
> infinite-scroll-distance="10"这里10决定了页面滚动到离页尾多少像素的时候触发回调函数，10是像素值。通常我们会在页尾做一个几十像素高的“正在加载中...”，这样的话，可以把这个div的高度设为infinite-scroll-distance的值即可。
> 其他选项：infinite-scroll-immediate-check 默认值为true，该指令意思是，应该在绑定后立即检查busy的值和是否滚动到底。如果你的初始内容高度不够高、不足以填满可滚动的容器的话，你应设为true，这样会立即执行一次loadMore，会帮你填充一些初始内容。
> infinite-scroll-listen-for-event 当事件在Vue实例中发出时，无限滚动将再次检查。
> infinite-scroll-throttle-delay 检查busy的值的时间间隔，默认值是200，因为vue-infinite-scroll的基础原理就是，vue-infinite-scroll会循环检查busy的值，以及是否滚动到底，只有当：busy为false且滚动到底，回调函数才会执行。

#### -相关链接

* [github](https://github.com/ElemeFE/vue-infinite-scroll"%3Ehttps://github.com/ElemeFE/vue-infinite-scroll%3C/a%3E)

### vuedraggable

#### - 安装
```
npm install vue-draggable --save
```

#### - 使用

* 在需要用到的页面中直接引入就ok了

```JavaScript
import draggable from 'vuedraggable'
export default {
    ...
    components: {
        draggable
    }
}
```

* * *
```Html

<draggable :options="{group:'people',animation:150,ghostClass:'sortable-ghost',chosenClass:'chosenClass',scroll:true,scrollSensitivity:200}"
      v-model="list2"
      @change="change"
      @start="start"
      @end="end"
      :move="move"
      style="display: inline-block; width:190px;height: 200px;background: #eee;overflow: auto">
      <li v-for="(item, index) in list2"
          :class="setclass(item,index)"
          :key="index">
        {{item.name}}
      </li>
</draggable>
```

#### -事件

* * *
* 常用的有以下几种
```
start, add, remove, update, end, choose, sort, filter, clone

参数带有如下属性：

add: 包含被添加到列表的元素 
        newIndex: 添加后的新索引
        element: 被添加的元素
removed: 从列表中移除的元素 
        oldIndex: 移除前的索引
        element: 被移除的元素
moved：内部移动的 
        newIndex: 改变后的索引
        oldIndex: 改变前的索引
        element: 被移动的元素

```

* * *
```JavaScript

//evt里面有两个值，一个evt.added 和evt.removed  可以分别知道移动元素的ID和删除元素的ID
    change: function (evt) {
      console.log(evt)
    },
    //start ,end ,add,update, sort, remove 得到的都差不多
    start: function (evt) {
      console.log(evt)
    },
    end: function (evt) {
      console.log(evt)
      evt.item //可以知道拖动的本身
      evt.to    // 可以知道拖动的目标列表
      evt.from  // 可以知道之前的列表
      evt.oldIndex  // 可以知道拖动前的位置
      evt.newIndex  // 可以知道拖动后的位置
    },
    move: function (evt, originalEvent) {
      console.log(evt)
      console.log(originalEvent) //鼠标位置
    }
```

#### -参数列表

* 由于vuedraggable是基于sortJS的，所以参数配置基本上一样

>group: string or array 分组用的，同一组的不同list可以相互拖动
>sort: boolean 定义是否可以拖拽
>delay:number 定义鼠标选中列表单元可以开始拖动的延迟时间
>touchStartThreshold:number px,在取消延迟拖动事件之前，点应该移动多少像素
>disabled: boolean 定义是否此sortable对象是否可用，为true时>sortable对象不能拖放排序等功能
>store: null,
>animation: umber 单位:ms 动画时间
>handle: selector 格式为简单css选择器的字符串，使列表单元中符合选择器的元素成为拖动的手柄，只有按住拖动手柄才能使列表单元进行拖动
>filter: selector 格式为简单css选择器的字符串，定义哪些列表单元不能进行拖放，可设置为多个选择器，中间用“，”分隔
>preventOnFilter: 当拖动filter时是否触发event.preventDefault()默认触发
>draggable: selector 格式为简单css选择器的字符串，定义哪些列表单元可以进行拖放
>ghostClass: selector 格式为简单css选择器的字符串，当拖动列表单元时会生成一个副本作为影子单元来模拟被拖动单元排序的情况，此配置项就是来给这个影子单元添加一个class，我们可以通过这种方式来给影子元素进行编辑样式
>chosenClass: selector 格式为简单css选择器的字符串，目标被选中时添加
>dragClass:selector 格式为简单css选择器的字符串，目标拖动过程中添加
>forceFallback: boolean 如果设置为true时，将不使用原生的html5的拖放，可以修改一些拖放中元素的样式等
>fallbackClass： string 当forceFallback设置为true时，拖放过程中鼠标附着单元的样式
>dataIdAttr： data-id
>scroll：boolean当排序的容器是个可滚动的区域，拖放可以引起区域滚动
>scrollFn：function(offsetX, offsetY, originalEvent, touchEvt, >hoverTargetEl) { … } 用于自定义滚动条的适配
>scrollSensitivity: number 就是鼠标靠近边缘多远开始滚动默认30
>scrollSpeed: number 滚动速度

* 官方建议参数配置项不写在options里，而是如下所示
```html
<draggable
        v-model="list"
        handle=".handle"
        :group="{ name: 'people', pull: 'clone', put: false }"
        ghost-class="ghost"
        :sort="false"
        @change="log"
      >
</draggable>
```

#### -相关链接

* [vuedraggable github](https://github.com/SortableJS/Vue.Draggable"%3Ehttps://github.com/SortableJS/Vue.Draggable)
* [sortJs github](https://github.com/SortableJS/Sortable"%3Ehttps://github.com/SortableJS/Sortable)