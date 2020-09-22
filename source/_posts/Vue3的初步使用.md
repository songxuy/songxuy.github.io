title: Vue3的初步使用(-)
author: coolsong
tags:
  - Vue3
  - Composition API
categories:
  - Vue3
  - Composition API
date: 2020-09-18 14:05:00
---
<section id="nice" data-tool="mdnice编辑器" data-website="https://www.mdnice.com" style="font-size: 16px; padding: 0 10px; word-spacing: 0px; word-break: break-word; word-wrap: break-word; text-align: left; line-height: 1.25; color: #2b2b2b; font-family: Optima-Regular, Optima, PingFangTC-Light, PingFangSC-light, PingFangTC-light; letter-spacing: 2px; background-image: linear-gradient(90deg, rgba(50, 0, 0, 0.04) 3%, rgba(0, 0, 0, 0) 3%), linear-gradient(360deg, rgba(50, 0, 0, 0.04) 3%, rgba(0, 0, 0, 0) 3%); background-size: 20px 20px; background-position: center center;"><p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">之前就看过了很多关于Vue3的介绍，新特性，Composition API等特性。但是自己一直处于看文档和简单的源码上。虽然Vue3.0至今还是rc版，但是我们已经可以开始动起来了，下面就简单的介绍一下Vue3中一些新特性的使用。</p>
<!--more-->
<h4 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 18px;"><span class="prefix" style="display: none;"></span><span class="content" style="height: 16px; line-height: 16px; font-size: 16px;"><span style="background-image: url(https://my-wechat.mdnice.com/fullstack-3.png); display: inline-block; width: 16px; height: 16px; background-size: 100%; background-position: left bottom; background-repeat: no-repeat; width: 16px; height: 15px; line-height: 15px; margin-right: 6px; margin-bottom: -2px;"></span>Vue3安装</span><span class="suffix" style="display: none;"></span></h4>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">这里有两种方式，一种是通过新版的cli安装，另一种是通过vite创建。</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<h5 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 16px;"><span class="prefix" style="display: none;"></span><span class="content">Vite</span><span class="suffix" style="display: none;"></span></h5>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">Vite其实是一个前端构建工具。才出来没多久，也是尤大大搞得。1.0版本已经进入rc版本。主要是和Vue3配合使用。所以我们可以用它来搭建vue3的环境。作为构建工具它也有它很多的优势。
由于我们这里主要是通过cli去搭建的（vite并没有完善到支撑一个完整项目的地步）这里不做赘述了。
感兴趣可以看github  <a href="https://github.com/vitejs/vite" style="text-decoration: none; word-wrap: break-word; color: #40B8FA; font-weight: normal; border-bottom: 1px solid #3BAAFA;">Vite</a></p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<h5 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 16px;"><span class="prefix" style="display: none;"></span><span class="content">CLI</span><span class="suffix" style="display: none;"></span></h5>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">我这里使用的版本是4.5.4
安装过程：</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">命令行新建项目 vue create my-vue3-test</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">选择Manually select features 自定义安装</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">选择下面的这些
<img src="/images/usevue31.png" alt="Image" style="max-width: 100%; border-radius: 6px; display: block; margin: 20px auto; object-fit: contain; box-shadow: 2px 4px 7px #999;"></section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">回车选择3.x(preview)</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">Use class-style component syntax?选择n,即输入n然后回车</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">Use Babel alongside TypeScript,输入y</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">Use history mode for router输入n</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">然后css预处理可以任意选择</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">eslint选择ESLint + Prettier</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">然后是Lint on save和In dedicater config files</section></li></ul>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">安装好项目之后  ，进入项目启动就可以了</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">由于下面的有些例子会用到一些组件，这里就选择将Vue3.0继承到自家的UI库中的ant-design-vue。</section></li></ul>
<ol data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; list-style-type: decimal; font-size: 15px; color: #595959;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">安装依赖以及按需加载</section></li></ol>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;">yarn add ant-design-vue@2.0.0-beta.6 
<span/>yarn add babel-plugin-import -D
<span/></code></pre>
<ol start="2" data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; list-style-type: decimal; font-size: 15px; color: #595959;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">修改babel.config.js</section></li></ol>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-built_in" style="color: #c18401; line-height: 26px;">module</span>.exports = {
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">presets</span>: [<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"@vue/cli-plugin-babel/preset"</span>],
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">plugins</span>: [
<span/>    <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 按需加载</span>
<span/>    [
<span/>      <span class="hljs-string" style="color: #50a14f; line-height: 26px;">"import"</span>,
<span/>      <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// style 为 true 加载 less文件</span>
<span/>      { <span class="hljs-attr" style="color: #986801; line-height: 26px;">libraryName</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">"ant-design-vue"</span>, <span class="hljs-attr" style="color: #986801; line-height: 26px;">libraryDirectory</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">"es"</span>, <span class="hljs-attr" style="color: #986801; line-height: 26px;">style</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">"css"</span> }
<span/>    ]
<span/>  ]
<span/>};
<span/></code></pre>
<h4 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 18px;"><span class="prefix" style="display: none;"></span><span class="content" style="height: 16px; line-height: 16px; font-size: 16px;"><span style="background-image: url(https://my-wechat.mdnice.com/fullstack-3.png); display: inline-block; width: 16px; height: 16px; background-size: 100%; background-position: left bottom; background-repeat: no-repeat; width: 16px; height: 15px; line-height: 15px; margin-right: 6px; margin-bottom: -2px;"></span>setUp的使用</span><span class="suffix" style="display: none;"></span></h4>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">setup是Vue3.0提供的一个新的属性，可以在setup中使用Composition API。创建组件实例，然后初始化&nbsp;props&nbsp;，紧接着就调用setup&nbsp;函数。从生命周期钩子的视角来看，它会在&nbsp;beforeCreate&nbsp;钩子之前被调用。我们先来看一个例子吧：</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;">&lt;template&gt;
<span/>  <span class="xml" style="line-height: 26px;"><span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">div</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">class</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"home"</span>&gt;</span>
<span/>    <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">About</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">msg</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"hello"</span>&gt;</span><span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">About</span>&gt;</span>
<span/>    <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-form</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">layout</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"inline"</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">:model</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"state.form"</span>&gt;</span>
<span/>      <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-form-item</span>&gt;</span>
<span/>        <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-input</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">v-model:value</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"state.form.user"</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">placeholder</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"Username"</span>&gt;</span>
<span/>          <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">template</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">v-slot:prefix</span>&gt;</span><span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">UserOutlined</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">style</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"color:rgba(0,0,0,.25)"</span>/&gt;</span><span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">template</span>&gt;</span>
<span/>        <span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-input</span>&gt;</span>
<span/>      <span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-form-item</span>&gt;</span>
<span/>      <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-form-item</span>&gt;</span>
<span/>        <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-input</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">v-model:value</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"state.form.password"</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">type</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"password"</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">placeholder</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"Password"</span>&gt;</span>
<span/>          <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">template</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">v-slot:prefix</span>&gt;</span><span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">LockOutlined</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">style</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"color:rgba(0,0,0,.25)"</span>/&gt;</span><span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">template</span>&gt;</span>
<span/>        <span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-input</span>&gt;</span>
<span/>      <span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-form-item</span>&gt;</span>
<span/>      <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-form-item</span>&gt;</span>
<span/>        <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-button</span>
<span/>          <span class="hljs-attr" style="color: #986801; line-height: 26px;">type</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"primary"</span>
<span/>          <span class="hljs-attr" style="color: #986801; line-height: 26px;">:disabled</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"state.form.user === '' || state.form.password === ''"</span>
<span/>          @<span class="hljs-attr" style="color: #986801; line-height: 26px;">click</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"handleSubmit"</span>
<span/>        &gt;</span>
<span/>          登录
<span/>        <span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-button</span>&gt;</span>
<span/>      <span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-form-item</span>&gt;</span>
<span/>    <span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-form</span>&gt;</span>
<span/>  <span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">div</span>&gt;</span>
<span/><span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">template</span>&gt;</span>
<span/><span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">script</span>&gt;</span><span class="javascript" style="line-height: 26px;">
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { UserOutlined, LockOutlined } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'@ant-design/icons-vue'</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { Form, Input, Button } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'ant-design-vue'</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { reactive } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'vue'</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> About <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'@/views/About'</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">default</span> {
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">components</span>: {
<span/>    UserOutlined,
<span/>    LockOutlined,
<span/>    [Form.name]: Form,
<span/>    [Form.Item.name]: Form.Item,
<span/>    [Input.name]: Input,
<span/>    [Button.name]: Button,
<span/>    About,
<span/>  },
<span/>  setup() {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> state = reactive({
<span/>      <span class="hljs-attr" style="color: #986801; line-height: 26px;">form</span>: {
<span/>        <span class="hljs-attr" style="color: #986801; line-height: 26px;">user</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">''</span>,
<span/>        <span class="hljs-attr" style="color: #986801; line-height: 26px;">password</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">''</span>,
<span/>      },
<span/>    });
<span/>    <span class="hljs-function" style="line-height: 26px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">function</span> <span class="hljs-title" style="color: #4078f2; line-height: 26px;">handleSubmit</span>(<span class="hljs-params" style="line-height: 26px;"></span>) </span>{
<span/>      <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(state.form);
<span/>    }
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> {
<span/>      state,
<span/>      handleSubmit,
<span/>    };
<span/>  },
<span/>};
<span/></span><span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">script</span>&gt;</span>
<span/></span></code></pre>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">可以看到， 我们在setup中使用reactive创建了一个响应式对象，和一个方法，然后return出来之后就可以在template中使用了。</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<h5 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 16px;"><span class="prefix" style="display: none;"></span><span class="content">参数说明</span><span class="suffix" style="display: none;"></span></h5>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">setup有两个参数，分别是props和context</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">props</section></li></ul>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">props是setup函数的第一个参数，是组件外部传入进来的属性。可以直接在template中使用</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">default</span> {
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">props</span>: {
<span/>    <span class="hljs-attr" style="color: #986801; line-height: 26px;">name</span>: <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">String</span>,
<span/>  },
<span/>  setup(props) {
<span/>    <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(props.name)
<span/>  },}
<span/></code></pre>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">需要注意的一点是不要对props使用解构赋值，那样会使其失去响应性。</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">default</span> {
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">props</span>: {
<span/>    <span class="hljs-attr" style="color: #986801; line-height: 26px;">name</span>: <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">String</span>,
<span/>  },
<span/>  setup({ name }) {
<span/>    watchEffect(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>      <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">`name is: `</span> + name) <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// Will not be reactive!</span>
<span/>    })
<span/>  },}
<span/></code></pre>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">context</section></li></ul>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">context是setup函数的第二个参数，context是一个对象，里面包含了三个属性，分别是:</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<ol style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; list-style-type: decimal; font-size: 15px; color: #595959;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">attrs: attrs与Vue2.0的this.$attrs是一样的，即外部传入的未在props中定义的属性。对于attrs与props一样，我们不能对attrs使用es6的解构，必须使用attrs.name的写法</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">slots: slots对应的是组件的插槽，与Vue2.0的this.$slots是对应的,与props和attrs一样，slots也是不能解构的。</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">emit: emit对应的是Vue2.0的this.$emit, 即对外暴露事件。</section></li></ol>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<h5 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 16px;"><span class="prefix" style="display: none;"></span><span class="content">返回值</span><span class="suffix" style="display: none;"></span></h5>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">setup函数一般会返回一个对象，这个对象里面包含了组件模板里面要使用到的data与一些函数或者事件，但是setup也可以返回一个函数，这个函数对应的就是Vue2.0的render函数，可以在这个函数里面使用JSX，对于Vue3.0中使用JSX。</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">官网例子</section></li></ul>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { h, ref, reactive } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'vue'</span>
<span/>
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">default</span> {
<span/>  setup() {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> count = ref(<span class="hljs-number" style="color: #986801; line-height: 26px;">0</span>)
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> object = reactive({ <span class="hljs-attr" style="color: #986801; line-height: 26px;">foo</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'bar'</span> })
<span/>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> <span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> h(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'div'</span>, [count.value, object.foo])
<span/>  },}
<span/></code></pre>
<h4 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 18px;"><span class="prefix" style="display: none;"></span><span class="content" style="height: 16px; line-height: 16px; font-size: 16px;"><span style="background-image: url(https://my-wechat.mdnice.com/fullstack-3.png); display: inline-block; width: 16px; height: 16px; background-size: 100%; background-position: left bottom; background-repeat: no-repeat; width: 16px; height: 15px; line-height: 15px; margin-right: 6px; margin-bottom: -2px;"></span>Reactive和Ref</span><span class="suffix" style="display: none;"></span></h4>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">这两个新的api都是和创建响应式数据有关，但它们又有一些不同。</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<h5 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 16px;"><span class="prefix" style="display: none;"></span><span class="content">reactive</span><span class="suffix" style="display: none;"></span></h5>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">它接收一个普通对象然后返回该普通对象的响应式代理。等同于 2.x 的&nbsp;Vue.observable()。响应式转换是“深层的”：会影响对象内部所有嵌套的属性。基于 ES2015 的 Proxy 实现，返回的代理对象不等于原始对象。建议仅使用代理对象而避免依赖原始对象。</section></li></ul>
<p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">一个例子：</p>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;">&lt;template&gt;
<span/>  <span class="xml" style="line-height: 26px;"><span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">div</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">class</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"about"</span>&gt;</span>
<span/>    <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">h1</span> @<span class="hljs-attr" style="color: #986801; line-height: 26px;">click</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"changeName"</span>&gt;</span>{{ state.name }}<span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">h1</span>&gt;</span>
<span/>    <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-input</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">v-model:value</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"state.name"</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">placeholder</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"Basic usage"</span> /&gt;</span>
<span/>  <span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">div</span>&gt;</span>
<span/><span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">template</span>&gt;</span></span>
<span/><span class="xml" style="line-height: 26px;"><span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">script</span>&gt;</span><span class="javascript" style="line-height: 26px;">
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { reactive } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'vue'</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { Button, Input } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'ant-design-vue'</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">default</span> {
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">components</span>: {
<span/>    [Button.name]: Button,
<span/>    [Input.name]: Input,
<span/>  },
<span/>  setup() {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> state = reactive({
<span/>      <span class="hljs-attr" style="color: #986801; line-height: 26px;">name</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'子君'</span>,
<span/>    });
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> changeName = <span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>      state.name = <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'hello, world'</span>;
<span/>    }
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> obj = {};
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> state1 = reactive(obj);
<span/>    <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 输出false</span>
<span/>    <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(obj === state1);
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> {
<span/>      state,
<span/>      changeName
<span/>    };
<span/>  },
<span/>};
<span/></span><span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">script</span>&gt;</span></span>
<span/></code></pre>
<h5 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 16px;"><span class="prefix" style="display: none;"></span><span class="content">ref</span><span class="suffix" style="display: none;"></span></h5>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">和reactive类似，接受一个参数值并返回一个响应式且可改变的 ref 对象。ref 对象拥有一个指向内部值的单一属性&nbsp;.value。ref更像是定义对象中的单个属性。</section></li></ul>
<p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">一个例子：</p>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;">
<span/>&lt;template&gt;
<span/>  <span class="xml" style="line-height: 26px;"><span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">div</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">class</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"about"</span>&gt;</span>
<span/>    <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">h4</span>&gt;</span>{{ age }}<span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">h4</span>&gt;</span>
<span/>    <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-input</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">v-model:value</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"age"</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">placeholder</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"Basic usage"</span> /&gt;</span>
<span/>  <span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">div</span>&gt;</span>
<span/><span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">template</span>&gt;</span></span>
<span/><span class="xml" style="line-height: 26px;"><span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">script</span>&gt;</span><span class="javascript" style="line-height: 26px;">
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { ref } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'vue'</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { Button, Input } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'ant-design-vue'</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">default</span> {
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">components</span>: {
<span/>    [Button.name]: Button,
<span/>    [Input.name]: Input,
<span/>  },
<span/>  setup() {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> age = ref(<span class="hljs-number" style="color: #986801; line-height: 26px;">0</span>);
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> changeAge = <span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>      age.value++;
<span/>    };
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> {
<span/>      age,
<span/>      changeAge,
<span/>    };
<span/>  },
<span/>};
<span/></span><span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">script</span>&gt;</span></span>
<span/></code></pre>
<h5 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 16px;"><span class="prefix" style="display: none;"></span><span class="content">区别</span><span class="suffix" style="display: none;"></span></h5>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">通过上面的两个例子，我们可以总结出：</section></li></ul>
<ol data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; list-style-type: decimal; font-size: 15px; color: #595959;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">reactive传入的是一个对象，返回的是一个响应式对象，而ref传入的是一个基本数据类型（其实引用类型也可以），返回的是传入值的响应式值</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">reactive获取或修改属性可以直接通过state.prop来操作，而ref返回值需要通过name.value的方式来修改或者读取数据。但是需要注意的是，在template中并不需要通过.value来获取值，这是因为template中已经做了解套。</section></li></ol>
<h4 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 18px;"><span class="prefix" style="display: none;"></span><span class="content" style="height: 16px; line-height: 16px; font-size: 16px;"><span style="background-image: url(https://my-wechat.mdnice.com/fullstack-3.png); display: inline-block; width: 16px; height: 16px; background-size: 100%; background-position: left bottom; background-repeat: no-repeat; width: 16px; height: 15px; line-height: 15px; margin-right: 6px; margin-bottom: -2px;"></span>v-model</span><span class="suffix" style="display: none;"></span></h4>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">之前我们实现双向数据绑定有两种方式，一种是使用v-mode，而另一种是使用:value.sync+emit('update:value'))。
v-model一般用于input、select等上，绑定value并监听相应的事件实现双向绑定，在组件上使用v-mode默认会利用名为&nbsp;value&nbsp;的 prop 和名为&nbsp;input&nbsp;的事件，我们也可以使用model去自定义。但是有一个缺点是只能绑定一个。</p>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">使用:value.sync+emit('update:value'))我们就可以绑定多个数据。</p>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">在Vue3.0中为了实现统一，实现了让一个组件可以拥有多个v-model，同时删除掉了.sync。在vue3.0中，v-model后面需要跟一个modelValue，即要双向绑定的属性名，Vue3.0就是通过给不同的v-model指定不同的modelValue来实现多个v-model。</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">一个例子:
child.vue</p>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;">&lt;template&gt;
<span/>  <span class="xml" style="line-height: 26px;"><span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">div</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">class</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"child"</span>&gt;</span>
<span/>    <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">input</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">:value</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"value"</span> @<span class="hljs-attr" style="color: #986801; line-height: 26px;">input</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"_handleChangeValue"</span> /&gt;</span>
<span/>    <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">input</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">:value</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"title"</span> @<span class="hljs-attr" style="color: #986801; line-height: 26px;">input</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"_handleChangeTitle"</span> /&gt;</span>
<span/>    <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-button</span> @<span class="hljs-attr" style="color: #986801; line-height: 26px;">click</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"changeValue('xiuxiu')"</span>&gt;</span>change value<span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-button</span>&gt;</span>
<span/>    <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-button</span> @<span class="hljs-attr" style="color: #986801; line-height: 26px;">click</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"changeTitle('biubiu')"</span>&gt;</span>change title<span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">a-button</span>&gt;</span>
<span/>  <span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">div</span>&gt;</span>
<span/><span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">template</span>&gt;</span>
<span/><span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">script</span>&gt;</span><span class="javascript" style="line-height: 26px;">
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { Button } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'ant-design-vue'</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">default</span> {
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">components</span>: {
<span/>    [Button.name]: Button,
<span/>  },
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">props</span>: {
<span/>    <span class="hljs-attr" style="color: #986801; line-height: 26px;">value</span>: {
<span/>      <span class="hljs-attr" style="color: #986801; line-height: 26px;">type</span>: <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">String</span>,
<span/>      <span class="hljs-attr" style="color: #986801; line-height: 26px;">default</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">''</span>,
<span/>    },
<span/>    <span class="hljs-attr" style="color: #986801; line-height: 26px;">title</span>: {
<span/>      <span class="hljs-attr" style="color: #986801; line-height: 26px;">type</span>: <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">String</span>,
<span/>      <span class="hljs-attr" style="color: #986801; line-height: 26px;">default</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">''</span>,
<span/>    },
<span/>  },
<span/>  setup(props, { emit }) {
<span/>    <span class="hljs-function" style="line-height: 26px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">function</span> <span class="hljs-title" style="color: #4078f2; line-height: 26px;">_handleChangeValue</span>(<span class="hljs-params" style="line-height: 26px;">e</span>) </span>{
<span/>      emit(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'update:value'</span>, e.target.value);
<span/>    }
<span/>    <span class="hljs-function" style="line-height: 26px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">function</span> <span class="hljs-title" style="color: #4078f2; line-height: 26px;">_handleChangeTitle</span>(<span class="hljs-params" style="line-height: 26px;">e</span>) </span>{
<span/>      emit(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'update:title'</span>, e.target.value);
<span/>    }
<span/>    <span class="hljs-function" style="line-height: 26px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">function</span> <span class="hljs-title" style="color: #4078f2; line-height: 26px;">changeValue</span>(<span class="hljs-params" style="line-height: 26px;">str</span>) </span>{
<span/>      emit(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'update:value'</span>, str);
<span/>    }
<span/>    <span class="hljs-function" style="line-height: 26px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">function</span> <span class="hljs-title" style="color: #4078f2; line-height: 26px;">changeTitle</span>(<span class="hljs-params" style="line-height: 26px;">str</span>) </span>{
<span/>      emit(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'update:title'</span>, str);
<span/>    }
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> {
<span/>      _handleChangeValue,
<span/>      _handleChangeTitle,
<span/>      changeValue,
<span/>      changeTitle,
<span/>    };
<span/>  },
<span/>};
<span/></span><span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">script</span>&gt;</span>
<span/></span></code></pre>
<p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">parent.vue</p>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;">&lt;template&gt;
<span/>  <span class="xml" style="line-height: 26px;"><span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">div</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">class</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"demo"</span>&gt;</span>
<span/>    <span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">Child</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">v-model:value</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"state.value"</span> <span class="hljs-attr" style="color: #986801; line-height: 26px;">v-model:title</span>=<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"state.title"</span>&gt;</span><span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">Child</span>&gt;</span>
<span/>  <span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">div</span>&gt;</span></span>
<span/><span class="xml" style="line-height: 26px;"><span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">template</span>&gt;</span></span>
<span/><span class="xml" style="line-height: 26px;"><span class="hljs-tag" style="line-height: 26px;">&lt;<span class="hljs-name" style="color: #e45649; line-height: 26px;">script</span>&gt;</span><span class="javascript" style="line-height: 26px;">
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { reactive } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'vue'</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> Child <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'./child'</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">default</span> {
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">components</span>: {
<span/>    Child,
<span/>  },
<span/>  setup() {
<span/>    <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">/* const count = ref(0);
<span/>    const object = reactive({ foo: 'bar' });
<span/>    return () =&gt; h('div', [count.value, object.foo]); */</span>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> state = reactive({
<span/>      <span class="hljs-attr" style="color: #986801; line-height: 26px;">value</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'0'</span>,
<span/>      <span class="hljs-attr" style="color: #986801; line-height: 26px;">title</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'test'</span>,
<span/>    });
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> {
<span/>      state,
<span/>    };
<span/>  },
<span/>};
<span/></span><span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">script</span>&gt;</span></span>
<span/></code></pre>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">可以看到我们在v-model上绑定了两个变量</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<h4 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 18px;"><span class="prefix" style="display: none;"></span><span class="content" style="height: 16px; line-height: 16px; font-size: 16px;"><span style="background-image: url(https://my-wechat.mdnice.com/fullstack-3.png); display: inline-block; width: 16px; height: 16px; background-size: 100%; background-position: left bottom; background-repeat: no-repeat; width: 16px; height: 15px; line-height: 15px; margin-right: 6px; margin-bottom: -2px;"></span>watch的使用</span><span class="suffix" style="display: none;"></span></h4>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">在Vue3.0中，除了Vue2.0的写法之外，有两个api可以对数据变化进行监听，第一种是watch,第二种是watchEffect。对于watch,其与Vue2.0中的$watch用法基本是一模一样的，而watchEffect是Vue3.0新提供的api。</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<h5 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 16px;"><span class="prefix" style="display: none;"></span><span class="content">watch</span><span class="suffix" style="display: none;"></span></h5>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">监听单个值和函数返回</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">一个例子：</section></li></ul>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { watch, ref, reactive } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'vue'</span>
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">default</span> {
<span/>  setup() {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> title = ref(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'活着'</span>)
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> titles = reactive({
<span/>      <span class="hljs-attr" style="color: #986801; line-height: 26px;">title1</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'book1'</span>,
<span/>      <span class="hljs-attr" style="color: #986801; line-height: 26px;">title2</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'book2'</span>
<span/>    })
<span/>    watch(name, (newValue, oldValue) =&gt; {
<span/>      <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(newValue, oldValue)
<span/>    })
<span/>    watch(
<span/>      <span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>        <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> titles.title1 + titles.titles
<span/>      },
<span/>      value =&gt; {
<span/>        <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(value)
<span/>      }
<span/>    )
<span/>
<span/>    setTimeout(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>      title.value = <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'hello world'</span>
<span/>      titles.title1 = <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'miss'</span>
<span/>    }, <span class="hljs-number" style="color: #986801; line-height: 26px;">3000</span>)
<span/>  }
<span/>}
<span/></code></pre>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">watch除了可以监听单个值和函数返回的变化，还可以监听多个值得变化</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { watch, ref, reactive } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'vue'</span>
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">default</span> {
<span/>  setup() {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> title1 = ref(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'活着'</span>)
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> title2 = ref(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'活着2'</span>)
<span/>    watch([title1, title2], ([title1, title2], [pretitle1, pretitle2]) =&gt; {
<span/>      <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(title1, pretitle1)
<span/>      <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(title2, pretitle2)
<span/>    })
<span/>    setTimeout(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>      title1.value = <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'hello world'</span>
<span/>      title2.value = <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'miss you'</span>
<span/>    }, <span class="hljs-number" style="color: #986801; line-height: 26px;">3000</span>)
<span/>  }
<span/>}
<span/></code></pre>
<h5 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 16px;"><span class="prefix" style="display: none;"></span><span class="content">watchEffect</span><span class="suffix" style="display: none;"></span></h5>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">会立即执行传入的一个函数，并响应式追踪其依赖，并在其依赖变更时重新运行该函数。</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> count = ref(<span class="hljs-number" style="color: #986801; line-height: 26px;">0</span>)
<span/>
<span/>watchEffect(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(count.value))<span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// -&gt; 打印出 0</span>
<span/>
<span/>setTimeout(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>  count.value++
<span/>  <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// -&gt; 打印出 1</span>
<span/> }, <span class="hljs-number" style="color: #986801; line-height: 26px;">100</span>)
<span/></code></pre>
<h5 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 16px;"><span class="prefix" style="display: none;"></span><span class="content">停止侦听</span><span class="suffix" style="display: none;"></span></h5>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">当&nbsp;watchEffect&nbsp;在组件的&nbsp;setup()&nbsp;函数或生命周期钩子被调用时， 侦听器会被链接到该组件的生命周期，并在组件卸载时自动停止。在一些情况下，也可以显式调用返回值以停止侦听：</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> stop = watchEffect(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>  <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">/* ... */</span>
<span/> })
<span/><span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 之后</span>
<span/>stop()
<span/></code></pre>
<h5 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 16px;"><span class="prefix" style="display: none;"></span><span class="content">清除副作用</span><span class="suffix" style="display: none;"></span></h5>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">有时副作用函数会执行一些异步的副作用, 这些响应需要在其失效时清除（即完成之前状态已改变了）。所以侦听副作用传入的函数可以接收一个&nbsp;onInvalidate&nbsp;函数作入参, 用来注册清理失效时的回调。当以下情况发生时，这个失效回调会被触发:</p>
<ol style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; list-style-type: decimal; font-size: 15px; color: #595959;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">副作用即将重新执行时
2.侦听器被停止 (如果在&nbsp;setup()&nbsp;或 生命周期钩子函数中使用了&nbsp;watchEffect, 则在卸载组件时)</section></li></ol>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-function" style="line-height: 26px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">function</span> <span class="hljs-title" style="color: #4078f2; line-height: 26px;">loadData</span>(<span class="hljs-params" style="line-height: 26px;">id, cb</span>) </span>{
<span/>  <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">new</span> <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">Promise</span>(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">resolve</span> =&gt;</span> {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> timer = setTimeout(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>      resolve(
<span/>        <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">new</span> <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">Array</span>(<span class="hljs-number" style="color: #986801; line-height: 26px;">10</span>).fill(<span class="hljs-number" style="color: #986801; line-height: 26px;">0</span>).map(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>          <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> id.value + <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">Math</span>.random()
<span/>        })
<span/>      )
<span/>    }, <span class="hljs-number" style="color: #986801; line-height: 26px;">2000</span>)
<span/>    cb(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>      clearTimeout(timer)
<span/>    })
<span/>  })
<span/>}
<span/>
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">default</span> {
<span/>  setup() {
<span/>    <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 下拉框1 选中的数据</span>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> select1Id = ref(<span class="hljs-number" style="color: #986801; line-height: 26px;">0</span>)
<span/>    <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 下拉框2的数据</span>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> select2List = ref([])
<span/>    watchEffect(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">onInvalidate</span> =&gt;</span> {
<span/>      <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 模拟请求</span>
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">let</span> cancel = <span class="hljs-literal" style="color: #0184bb; line-height: 26px;">undefined</span>
<span/>      <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 第一个参数是一个回调函数，用于模拟取消请求，关于取消请求，可以了解axios的CancelToken</span>
<span/>      loadData(select1Id, cb =&gt; {
<span/>        cancel = cb
<span/>      }).then(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">data</span> =&gt;</span> {
<span/>        select2List.value = data
<span/>        <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(data)
<span/>      })
<span/>      onInvalidate(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>        cancel &amp;&amp; cancel()
<span/>      })
<span/>    })
<span/>  }
<span/>}
<span/></code></pre>
<h4 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 18px;"><span class="prefix" style="display: none;"></span><span class="content" style="height: 16px; line-height: 16px; font-size: 16px;"><span style="background-image: url(https://my-wechat.mdnice.com/fullstack-3.png); display: inline-block; width: 16px; height: 16px; background-size: 100%; background-position: left bottom; background-repeat: no-repeat; width: 16px; height: 15px; line-height: 15px; margin-right: 6px; margin-bottom: -2px;"></span>总结分享</span><span class="suffix" style="display: none;"></span></h4>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">这一篇只介绍了一部分的api和特性，下一篇会继续介绍其他的特性
参考文档：
<a href="https://composition-api.vuejs.org/" style="text-decoration: none; word-wrap: break-word; color: #40B8FA; font-weight: normal; border-bottom: 1px solid #3BAAFA;">Vue Composition API</a>
<a href="https://juejin.im/post/6869521076771094536" style="text-decoration: none; word-wrap: break-word; color: #40B8FA; font-weight: normal; border-bottom: 1px solid #3BAAFA;">Vue3.0来袭，你想学的都在这里</a></p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<p id="nice-suffix-juejin-container" class="nice-suffix-juejin-container" data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin-top: 20px !important;"></p></section>