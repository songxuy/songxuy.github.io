title: vue在线翻译Demo
author: coolsong
tags:
  - 'vue '
categories:
  - vue.js
date: 2018-03-07 13:01:00
---
1.全局安装vue-cli（已安装则可跳过此步骤）
```
# 全局安装 vue-cli
$ npm install --global vue-cli
```
2.创建基于webpack的新项目
```
vue init webpack 项目名
```
安装过程中会有一些常规问题，项目名称、作者、是否需要安装其他模块

3.进入项目根目录，安装依赖 开启项目
```
#安装依赖
npm install
#运行项目
npm run dev
```
<!--more -->
 ![My Pic](/images/vue.png)
4.项目搭建 分为3个组件

&nbsp;&nbsp;&nbsp;4.1根组件（即App.vue）
```javascript
<template>
  <div id="app">
    <h1>在线翻译</h1>
    <h5 class="text-muted">简单 / 易用 / 便捷</h5>
    <Translate v-on:formSub="translateText"></Translate>
    <Output v-bind:translatedText="translatedText"></Output>
  </div>
</template>

<script>
import Translate from './components/Translate'
import Output from './components/Output'

export default {
  name: 'App',
  data:function(){
      return{
        translatedText:""
      }
  },
  components: {
    Translate,Output
  },
  methods:{
    translateText:function(text,language){
      this.$http.get('https://translate.yandex.net/api/v1.5/tr.json/translate?key=trnsl.1.1.20180305T143933Z.d87ffb49bbd9cb80.6a9c4cad5cd0e8251249146aca113cdd0b2ce7cb&lang='+language+'&text='+text)
      .then((response)=>{
        //console.log(response.body.text[0]);
        this.translatedText='Answer:'+response.body.text[0];
      })
    }
  }
}
</script>
```
其中引入了其他两个组件，并且使用了Vue-Resource模块向yandex网站请求翻译的api，注意url中的参数key是使用api的密钥，lang是要翻译为什么语言，text是翻译的文本，将结果渲染到组件中。

&nbsp;&nbsp;&nbsp;4.2Translate.vue
```javascript
<template>
  <div class="translate row">
    <form v-on:submit="formSubmit" class="well form-inline col-md-6 col-md-offset-3" id="trans">
      <input type="text" placeholder="输入要翻译的内容" v-model="textToTranslate" class="form-control" style="margin-left:120px;">
      <select v-model="language" class="form-control">
        <option value="en">English</option>
        <option value="ru">Russian</option>
        <option value="ko">Korean</option>
        <option value="fr">French</option>
      </select>
      <input type="submit" value="翻译" class="btn btn-primary"/>
    </form>
  </div>
</template>

<script>
export default {
  name: 'translate',
  data () {
    return {
      textToTranslate:"",
      language:""
    }
  },
  methods:{
    formSubmit:function(e){
      this.$emit('formSub',this.textToTranslate,this.language);
      e.preventDefault();

    }
  },
  created:function(){
    this.language="en";
  }
}
</script>
```
这个组件中定义了可以翻译语言的种类，注意option中的value值要与api中的语言代码一致，默认初始选择English。
&nbsp;&nbsp;&nbsp;4.3Output.vue
```javascript
<template>
  <div class="Output">
    <h2>{{translatedText}}</h2>
  </div>
</template>

<script>
export default {
  name: 'Output',
  props:[
    'translatedText'
  ]
  
}
</script>
```
该组件就将从App.vue中获取的值渲染到页面上。
![My Pic](/images/t1.png)
![My Pic](/images/t2.png)