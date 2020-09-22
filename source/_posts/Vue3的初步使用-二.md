title: Vue3的初步使用(二)
author: coolsong
tags:
  - Vue
  - Composition API
categories:
  - Vue
  - Composition API
date: 2020-09-22 14:32:00
---
<section id="nice" data-tool="mdnice编辑器" data-website="https://www.mdnice.com" style="font-size: 16px; padding: 0 10px; word-spacing: 0px; word-break: break-word; word-wrap: break-word; text-align: left; line-height: 1.25; color: #2b2b2b; font-family: Optima-Regular, Optima, PingFangTC-Light, PingFangSC-light, PingFangTC-light; letter-spacing: 2px; background-image: linear-gradient(90deg, rgba(50, 0, 0, 0.04) 3%, rgba(0, 0, 0, 0) 3%), linear-gradient(360deg, rgba(50, 0, 0, 0.04) 3%, rgba(0, 0, 0, 0) 3%); background-size: 20px 20px; background-position: center center;"><p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">还是和上一篇一样，继续来介绍Vue3中一些新的特性以及其如何使用。</p>
<!--more-->
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">tips: 前几天Vue 官方团队发布了Vue3.0版本 🎉。代号为One Piece。
<a href="https://github.com/vuejs/vue-next/releases/tag/v3.0.0" style="text-decoration: none; word-wrap: break-word; color: #40B8FA; font-weight: normal; border-bottom: 1px solid #3BAAFA;">releases docs</a></p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<h4 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 18px;"><span class="prefix" style="display: none;"></span><span class="content" style="height: 16px; line-height: 16px; font-size: 16px;"><span style="background-image: url(https://my-wechat.mdnice.com/fullstack-3.png); display: inline-block; width: 16px; height: 16px; background-size: 100%; background-position: left bottom; background-repeat: no-repeat; width: 16px; height: 15px; line-height: 15px; margin-right: 6px; margin-bottom: -2px;"></span>computed的使用</span><span class="suffix" style="display: none;"></span></h4>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">计算属性一般是当我们有复杂计算且需要缓存值得时候来使用。Vue3中的使用和之前的并没有太大的区别。</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">一个例子</section></li></ul>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;">&lt;script&gt;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { computed, reactive, ref, watch } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'vue'</span>;
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
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> age = ref(<span class="hljs-number" style="color: #986801; line-height: 26px;">0</span>);
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> changeName = <span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>      state.name = <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'hello, world'</span>;
<span/>    };
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> changeAge = <span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>      age.value++;
<span/>    };
<span/>    watch(age, (newValue, oldValue) =&gt; {
<span/>      <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(newValue, oldValue);
<span/>    });
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> msg = computed(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>      <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> state.name + age.value;
<span/>    });
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> obj = {};
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> state1 = reactive(obj);
<span/>    <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 输出false</span>
<span/>    <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(obj === state1);
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> {
<span/>      state,
<span/>      changeName,
<span/>      age,
<span/>      changeAge,
<span/>      msg,
<span/>    };
<span/>  },
<span/>};
<span/><span class="xml" style="line-height: 26px;"><span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">script</span>&gt;</span></span>
<span/></code></pre>
<figure data-tool="mdnice编辑器" style="margin: 0; margin-top: 10px; margin-bottom: 10px; display: flex; flex-direction: column; justify-content: center; align-items: center;"><img src="/images/usevue32.gif" alt="Image" style="max-width: 100%; border-radius: 6px; display: block; margin: 20px auto; object-fit: contain; box-shadow: 2px 4px 7px #999;"><figcaption style="margin-top: 5px; text-align: center; display: block; font-size: 13px; color: #2b2b2b;"><span style="background-image: url(https://img.alicdn.com/tfs/TB1Yycwyrj1gK0jSZFuXXcrHpXa-32-32.png); display: inline-block; width: 18px; height: 18px; background-size: 18px; background-repeat: no-repeat; background-position: center; margin-right: 5px; margin-bottom: -5px;"></span>Image</figcaption></figure>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">当我们直接传入一个函数即默认设置的getter函数，如果要设置setter的话就需要传一个对象，这样的话创建的就是一个可手动修改的计算状态。demo如下：</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> count = ref(<span class="hljs-number" style="color: #986801; line-height: 26px;">1</span>)
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> plusOne = computed({
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">get</span>: <span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> count.value + <span class="hljs-number" style="color: #986801; line-height: 26px;">1</span>,
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">set</span>: <span class="hljs-function" style="line-height: 26px;">(<span class="hljs-params" style="line-height: 26px;">val</span>) =&gt;</span> {
<span/>    count.value = val - <span class="hljs-number" style="color: #986801; line-height: 26px;">1</span>
<span/>  },})
<span/>plusOne.value = <span class="hljs-number" style="color: #986801; line-height: 26px;">1</span>
<span/><span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(count.value) <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 0</span>
<span/></code></pre>
<h4 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 18px;"><span class="prefix" style="display: none;"></span><span class="content" style="height: 16px; line-height: 16px; font-size: 16px;"><span style="background-image: url(https://my-wechat.mdnice.com/fullstack-3.png); display: inline-block; width: 16px; height: 16px; background-size: 100%; background-position: left bottom; background-repeat: no-repeat; width: 16px; height: 15px; line-height: 15px; margin-right: 6px; margin-bottom: -2px;"></span>readonly</span><span class="suffix" style="display: none;"></span></h4>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">传入一个对象（响应式或普通）或 ref，返回一个原始对象的只读代理。一个只读的代理是“深层的”，对象内部任何嵌套的属性也都是只读的。</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">一个例子</section></li></ul>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> original = reactive({ <span class="hljs-attr" style="color: #986801; line-height: 26px;">count</span>: <span class="hljs-number" style="color: #986801; line-height: 26px;">0</span> })
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> copy = readonly(original)
<span/>
<span/>watchEffect(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>  <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 依赖追踪</span>
<span/>  <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(copy.count)
<span/>})
<span/>
<span/><span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// original 上的修改会触发 copy 上的侦听</span>
<span/>original.count++
<span/>
<span/><span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 无法修改 copy 并会被警告</span>
<span/>copy.count++ <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// warning!</span>
<span/></code></pre>
<h4 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 18px;"><span class="prefix" style="display: none;"></span><span class="content" style="height: 16px; line-height: 16px; font-size: 16px;"><span style="background-image: url(https://my-wechat.mdnice.com/fullstack-3.png); display: inline-block; width: 16px; height: 16px; background-size: 100%; background-position: left bottom; background-repeat: no-repeat; width: 16px; height: 15px; line-height: 15px; margin-right: 6px; margin-bottom: -2px;"></span>使用Vue-Router</span><span class="suffix" style="display: none;"></span></h4>
<h5 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 16px;"><span class="prefix" style="display: none;"></span><span class="content">初始化</span><span class="suffix" style="display: none;"></span></h5>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">在2.0中我们使用vue-router，是通过new VueRouter的方式去创建一个实例，在3.0中创建实例发生了一些变化。</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// router/index.ts</span>
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { createRouter, createWebHashHistory, RouteRecordRaw } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'vue-router'</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> Home <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'../views/Home.vue'</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> routes: <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">Array</span>&lt;RouteRecordRaw&gt; = [
<span/>  {
<span/>    <span class="hljs-attr" style="color: #986801; line-height: 26px;">path</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'/'</span>,
<span/>    <span class="hljs-attr" style="color: #986801; line-height: 26px;">name</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'Home'</span>,
<span/>    <span class="hljs-attr" style="color: #986801; line-height: 26px;">component</span>: Home,
<span/>  },
<span/>  {
<span/>    <span class="hljs-attr" style="color: #986801; line-height: 26px;">path</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'/about'</span>,
<span/>    <span class="hljs-attr" style="color: #986801; line-height: 26px;">name</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'About'</span>,
<span/>    <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// route level code-splitting</span>
<span/>    <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// this generates a separate chunk (about.[hash].js) for this route</span>
<span/>    <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// which is lazy-loaded when the route is visited.</span>
<span/>    component: <span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span>(<span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">/* webpackChunkName: "about" */</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'../views/About.vue'</span>),
<span/>  },
<span/>  {
<span/>    <span class="hljs-attr" style="color: #986801; line-height: 26px;">path</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'/demo'</span>,
<span/>    <span class="hljs-attr" style="color: #986801; line-height: 26px;">name</span>: <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'demo'</span>,
<span/>    <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// route level code-splitting</span>
<span/>    <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// this generates a separate chunk (about.[hash].js) for this route</span>
<span/>    <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// which is lazy-loaded when the route is visited.</span>
<span/>    component: <span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span>(<span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">/* webpackChunkName: "about" */</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'../views/demo.vue'</span>),
<span/>  },
<span/>];
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> router = createRouter({
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">history</span>: createWebHashHistory(),
<span/>  routes,
<span/>});
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">default</span> router;
<span/>
<span/><span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// main.js</span>
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { createApp } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">"vue"</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> App <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">"./App.vue"</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> router <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">"./router"</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> store <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">"./store"</span>;
<span/>createApp(App)
<span/>  .use(store)
<span/>  .use(router)
<span/>  .mount(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"#app"</span>);
<span/></code></pre>
<h5 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 16px;"><span class="prefix" style="display: none;"></span><span class="content">setup使用</span><span class="suffix" style="display: none;"></span></h5>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">同样的在setup中使用和之前route和router的使用还是有一些区别的。由于不能直接使用this，需要借助useRouter和useRoute来创建相应的实例。</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">一个例子</section></li></ul>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;">&lt;script&gt;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { reactive } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'vue'</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> Child <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'./child'</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { useRoute, useRouter, onBeforeRouteUpdate } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'vue-router'</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">default</span> {
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">components</span>: {
<span/>    Child,
<span/>  },
<span/>  beforeRouteEnter(<span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span>, to, next) {
<span/>    <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'enter'</span>);
<span/>    next();
<span/>  },
<span/>  setup() {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> route = useRoute();
<span/>    <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 获取路由实例</span>
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> router = useRouter();
<span/>    <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(route.path);
<span/>    <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(router)
<span/>    onBeforeRouteUpdate(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> { 
<span/>      <span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">// 当当前路由发生变化时，调用回调函数 </span>
<span/>    })
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> {
<span/>    };
<span/>  },
<span/>};
<span/><span class="xml" style="line-height: 26px;"><span class="hljs-tag" style="line-height: 26px;">&lt;/<span class="hljs-name" style="color: #e45649; line-height: 26px;">script</span>&gt;</span></span>
<span/></code></pre>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">除了上面的使用之外，当我们想要使用router的生命周期函数时，也提供了onBeforeRouteUpdate和onBeforeRouteLeave两个函数。如果还是想向之前那样使用，也可以像原来那样写。</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<h4 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 18px;"><span class="prefix" style="display: none;"></span><span class="content" style="height: 16px; line-height: 16px; font-size: 16px;"><span style="background-image: url(https://my-wechat.mdnice.com/fullstack-3.png); display: inline-block; width: 16px; height: 16px; background-size: 100%; background-position: left bottom; background-repeat: no-repeat; width: 16px; height: 15px; line-height: 15px; margin-right: 6px; margin-bottom: -2px;"></span>使用Vuex</span><span class="suffix" style="display: none;"></span></h4>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">vuex在3.0中的使用和之前的大体上差不太多</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<h5 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 16px;"><span class="prefix" style="display: none;"></span><span class="content">初始化</span><span class="suffix" style="display: none;"></span></h5>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">在store/index.ts中的写法为</section></li></ul>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { createStore } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">"vuex"</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">default</span> createStore({
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">state</span>: {},
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">mutations</span>: {},
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">actions</span>: {},
<span/>  <span class="hljs-attr" style="color: #986801; line-height: 26px;">modules</span>: {}
<span/>});
<span/></code></pre>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">在main.js中的写法</section></li></ul>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { createApp } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">"vue"</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> App <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">"./App.vue"</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> router <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">"./router"</span>;
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> store <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">"./store"</span>;
<span/>createApp(App)
<span/>  .use(store)
<span/>  .use(router)
<span/>  .mount(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"#app"</span>);
<span/></code></pre>
<h5 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 16px;"><span class="prefix" style="display: none;"></span><span class="content">setup中使用</span><span class="suffix" style="display: none;"></span></h5>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">同样的也是使用从vuex中引入的方法来操作vuex</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span> { useStore } <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span> <span class="hljs-string" style="color: #50a14f; line-height: 26px;">'vuex'</span>
<span/><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">export</span> <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">default</span> {
<span/>  setup() {
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> store = useStore()
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">const</span> roleMenus = store.getters[<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'roleMenus'</span>]
<span/>    <span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">return</span> {
<span/>      roleMenus
<span/>    }
<span/>  }
<span/>}
<span/></code></pre>
<h4 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 18px;"><span class="prefix" style="display: none;"></span><span class="content" style="height: 16px; line-height: 16px; font-size: 16px;"><span style="background-image: url(https://my-wechat.mdnice.com/fullstack-3.png); display: inline-block; width: 16px; height: 16px; background-size: 100%; background-position: left bottom; background-repeat: no-repeat; width: 16px; height: 15px; line-height: 15px; margin-right: 6px; margin-bottom: -2px;"></span>生命周期钩子函数</span><span class="suffix" style="display: none;"></span></h4>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">Vue3.0是兼容一部分Vue2.0的，所以对于Vue2.0的组件写法，生命周期钩子函数并未发生变化，但是如果使用的是composition api，写法上就有一些区别了：</p>
<ol style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; list-style-type: decimal; font-size: 15px; color: #595959;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">取消了beforeCreate和created，其实也就是用setup代替了这两个生命周期钩子函数</section></li><li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">将生命周期函数名称变为on+XXX。比如onMounted、onUpdated等</section></li></ol>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">setup中使用的例子</section></li></ul>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px;"><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; background: #fafafa; display: block; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; border-radius: 0px; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px;">setup() {
<span/>    onMounted(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>      <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'mounted!'</span>)
<span/>    })
<span/>    onUpdated(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>      <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'updated!'</span>)
<span/>    })
<span/>    onUnmounted(<span class="hljs-function" style="line-height: 26px;"><span class="hljs-params" style="line-height: 26px;">()</span> =&gt;</span> {
<span/>      <span class="hljs-built_in" style="color: #c18401; line-height: 26px;">console</span>.log(<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'unmounted!'</span>)
<span/>    })
<span/>  }
<span/></code></pre>
<h4 data-tool="mdnice编辑器" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 18px;"><span class="prefix" style="display: none;"></span><span class="content" style="height: 16px; line-height: 16px; font-size: 16px;"><span style="background-image: url(https://my-wechat.mdnice.com/fullstack-3.png); display: inline-block; width: 16px; height: 16px; background-size: 100%; background-position: left bottom; background-repeat: no-repeat; width: 16px; height: 15px; line-height: 15px; margin-right: 6px; margin-bottom: -2px;"></span>总结分享</span><span class="suffix" style="display: none;"></span></h4>
<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">
参考文档：
<a href="https://composition-api.vuejs.org/" style="text-decoration: none; word-wrap: break-word; color: #40B8FA; font-weight: normal; border-bottom: 1px solid #3BAAFA;">Vue Composition API</a>
<a href="https://juejin.im/post/6869521076771094536" style="text-decoration: none; word-wrap: break-word; color: #40B8FA; font-weight: normal; border-bottom: 1px solid #3BAAFA;">Vue3.0来袭，你想学的都在这里</a></p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<p id="nice-suffix-juejin-container" class="nice-suffix-juejin-container" data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin-top: 20px !important;"></p></section>

