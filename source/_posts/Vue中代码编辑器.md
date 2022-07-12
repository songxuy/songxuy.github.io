title: Vue中代码编辑器
author: coolsong
tags:
  - 代码编辑器
  - Vue
categories:
  - Vue
date: 2022-07-12 11:25:00
cover: /images/bg7.jpg
---
<section id="nice" data-tool="mdnice编辑器" data-website="https://www.mdnice.com" style="font-size: 16px; padding: 0 10px; word-spacing: 0px; word-break: break-word; word-wrap: break-word; text-align: left; line-height: 1.25; color: #2b2b2b; letter-spacing: 2px; background-image: linear-gradient(90deg, rgba(50, 0, 0, 0.04) 3%, rgba(0, 0, 0, 0) 3%), linear-gradient(360deg, rgba(50, 0, 0, 0.04) 3%, rgba(0, 0, 0, 0) 3%); background-size: 20px 20px; background-position: center center; font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, 'PingFang SC', Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;"><p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">最近项目上有使用代码编辑器的需求，经过一番查找最后使用的是Vue-Codemirror，下面就简单介绍下这个组件的基本用法和一些坑：</p>
<h2 data-tool="mdnice编辑器" id="code1" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 22px; display: block; border-bottom: 4px solid #40B8FA;"><span class="prefix" style="display: flex; width: 20px; height: 20px; background-size: 20px 20px; background-image: url(https://files.mdnice.com/fullstack-1.png); margin-bottom: -22px;"></span><span class="content" style="display: flex; color: #40B8FA; font-size: 20px; margin-left: 25px;">基本用法</span><a href="#code1" class="headerlink" title="基本用法"></a><span class="suffix" style="display: flex; box-sizing: border-box; width: 200px; height: 10px; border-top-left-radius: 20px; background: RGBA(64, 184, 250, .5); color: rgb(255, 255, 255); font-size: 16px; letter-spacing: 0.544px; justify-content: flex-end; float: right; margin-top: -10px; box-sizing: border-box !important; overflow-wrap: break-word !important;"></span></h2>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">安装</section></li></ul>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px; border-radius: 5px; box-shadow: rgba(0, 0, 0, 0.55) 0px 2px 10px;"><span style="display: block; background: url(https://files.mdnice.com/user/3441/876cad08-0422-409d-bb5a-08afec5da8ee.svg); height: 30px; width: 100%; background-size: 40px; background-repeat: no-repeat; background-color: #fafafa; margin-bottom: -7px; border-radius: 5px; background-position: 10px 10px;"></span><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; display: -webkit-box; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px; padding-top: 15px; background: #fafafa; border-radius: 5px;">yarn&nbsp;add&nbsp;vue-codemirror<br></code></pre>
<blockquote class="multiquote-1" data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">安装这里，最新版仅支持Vue3，如果使用的是Vue2需要安装老的版本，我这里使用的是4.0.6</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">引入</section></li></ul>
<p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">main.js 引入</p>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px; border-radius: 5px; box-shadow: rgba(0, 0, 0, 0.55) 0px 2px 10px;"><span style="display: block; background: url(https://files.mdnice.com/user/3441/876cad08-0422-409d-bb5a-08afec5da8ee.svg); height: 30px; width: 100%; background-size: 40px; background-repeat: no-repeat; background-color: #fafafa; margin-bottom: -7px; border-radius: 5px; background-position: 10px 10px;"></span><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; display: -webkit-box; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px; padding-top: 15px; background: #fafafa; border-radius: 5px;"><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span>&nbsp;VueCodeMirror&nbsp;<span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">from</span>&nbsp;<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'vue-codemirror'</span>;&nbsp;<br><span class="hljs-keyword" style="color: #a626a4; line-height: 26px;">import</span>&nbsp;<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'codemirror/lib/codemirror.css'</span>;&nbsp;Vue.use(VueCodeMirror);<br></code></pre>
<p data-tool="mdnice编辑器" style="padding-top: 8px; padding-bottom: 8px; line-height: 26px; color: #2b2b2b; margin: 10px 0px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px;">组件中引入</p>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px; border-radius: 5px; box-shadow: rgba(0, 0, 0, 0.55) 0px 2px 10px;"><span style="display: block; background: url(https://files.mdnice.com/user/3441/876cad08-0422-409d-bb5a-08afec5da8ee.svg); height: 30px; width: 100%; background-size: 40px; background-repeat: no-repeat; background-color: #fafafa; margin-bottom: -7px; border-radius: 5px; background-position: 10px 10px;"></span><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; display: -webkit-box; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px; padding-top: 15px; background: #fafafa; border-radius: 5px;">&lt;template&gt;
&nbsp; &lt;div id="discode"&gt;
&nbsp; &nbsp; &lt;codemirror
&nbsp; &nbsp; &nbsp; ref="mycode" &nbsp;
&nbsp; &nbsp; &nbsp; v-model="curCode" &nbsp;
&nbsp; &nbsp; &nbsp; :options="cmOptions"
&nbsp; &nbsp; &nbsp; @ready="onCmReady"
&nbsp; &nbsp; &nbsp; class="code"&gt; &nbsp; &nbsp; &nbsp; &nbsp;
&nbsp; &nbsp; &lt;/codemirror&gt;
&nbsp; &lt;/div&gt;
&lt;/template&gt;


import { codemirror } from 'vue-codemirror';

components:{
&nbsp; &nbsp; codemirror
&nbsp; },

</code></pre>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">配置项</section></li></ul>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px; border-radius: 5px; box-shadow: rgba(0, 0, 0, 0.55) 0px 2px 10px;"><span style="display: block; background: url(https://files.mdnice.com/user/3441/876cad08-0422-409d-bb5a-08afec5da8ee.svg); height: 30px; width: 100%; background-size: 40px; background-repeat: no-repeat; background-color: #fafafa; margin-bottom: -7px; border-radius: 5px; background-position: 10px 10px;"></span><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; display: -webkit-box; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px; padding-top: 15px; background: #fafafa; border-radius: 5px;">cmOptions:{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">value</span>:<span class="hljs-string" style="color: #50a14f; line-height: 26px;">''</span>,<span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">//编辑器的起始值。可以是字符串，也可以是文档对象。</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">indentUnit</span>:&nbsp;<span class="hljs-number" style="color: #986801; line-height: 26px;">2</span>,&nbsp;<span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">//&nbsp;缩进多少个空格</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">tabSize</span>:&nbsp;<span class="hljs-number" style="color: #986801; line-height: 26px;">2</span>,&nbsp;<span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">//&nbsp;制表符宽度</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">mode</span>:<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"text/javascript"</span>,<span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">//第一个将模式名称映射到它们的构造函数，第二个将MIME类型映射到模式规范。</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">theme</span>:&nbsp;<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"liquibyte"</span>,<span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">//编辑器样式的主题</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">autocorrect</span>:&nbsp;<span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>,&nbsp;&nbsp;<span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">//&nbsp;自动更正</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">spellcheck</span>:&nbsp;<span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>,&nbsp;&nbsp;<span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">//&nbsp;拼写检查&nbsp;&nbsp;&nbsp;&nbsp;</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">indentWithTabs</span>:&nbsp;<span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>,<span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">//在缩进时，是否tabSize&nbsp;应该用N个制表符替换前N&nbsp;*个空格。默认值为false。&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">smartIndent</span>:&nbsp;<span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>,<span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">//是否使用模式提供的上下文相关缩进（或者只是缩进与之前的行相同）。默认为true。&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">lineNumbers</span>:&nbsp;<span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>,<span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">//是否在编辑器左侧显示行号。&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">matchBrackets</span>&nbsp;:&nbsp;<span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>,<span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">//括号匹配&nbsp;&nbsp;</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">line</span>:&nbsp;<span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">autoCloseBrackets</span>:<span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">styleActiveLine</span>:&nbsp;<span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">readonly</span>:&nbsp;<span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">autofocus</span>:&nbsp;<span class="hljs-literal" style="color: #0184bb; line-height: 26px;">false</span>,<span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">//可用于使CodeMirror将焦点集中在初始化上&nbsp;&nbsp;</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">extraKeys</span>:&nbsp;{<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"Ctrl-Space"</span>:&nbsp;<span class="hljs-string" style="color: #50a14f; line-height: 26px;">"autocomplete"</span>},<span class="hljs-comment" style="color: #a0a1a7; font-style: italic; line-height: 26px;">//按键配置</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">hintOptions</span>:&nbsp;{&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">tables</span>:&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">table1</span>:&nbsp;[<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'name'</span>,&nbsp;<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'score'</span>,&nbsp;<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'birthDate'</span>],<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">table2</span>:&nbsp;[<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'name'</span>,&nbsp;<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'population'</span>,&nbsp;<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'size'</span>]<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">completeSingle</span>:&nbsp;<span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">lint</span>:&nbsp;<span class="hljs-literal" style="color: #0184bb; line-height: 26px;">false</span>,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">hint</span>:&nbsp;<span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">keyMap</span>:&nbsp;<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'sublime'</span>,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">foldGutter</span>:&nbsp;<span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">lineWrapping</span>:&nbsp;<span class="hljs-literal" style="color: #0184bb; line-height: 26px;">true</span>,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="hljs-attr" style="color: #986801; line-height: 26px;">gutters</span>:&nbsp;[<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'CodeMirror-linenumbers'</span>,&nbsp;<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'CodeMirror-foldgutter'</span>,&nbsp;<span class="hljs-string" style="color: #50a14f; line-height: 26px;">'CodeMirror-lint-markers'</span>],<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},<br></code></pre>
<blockquote class="multiquote-1" data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;">这里面使用的一些东西都需要从上面引入，比如liquibyte主题，上面则需要import 'codemirror/theme/liquibyte.css'
除此之外，对于一些代码验证的功能，还需要额外引入包。比如我这里使用了JavaScript代码验证，则需要引入jshint
如果需要第一次加载数据需要添加autoRefresh: true</p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">事件</section></li></ul>
<figure data-tool="mdnice编辑器" style="margin: 0; margin-top: 10px; margin-bottom: 10px; display: flex; flex-direction: column; justify-content: center; align-items: center;"><img src="/images/codemirrot1.png" alt="Image" style="max-width: 100%; border-radius: 6px; display: block; margin: 20px auto; object-fit: contain; box-shadow: 2px 4px 7px #999;"><figcaption style="margin-top: 5px; text-align: center; display: block; font-size: 13px; color: #2b2b2b;"><span style="background-image: url(https://img.alicdn.com/tfs/TB1Yycwyrj1gK0jSZFuXXcrHpXa-32-32.png); display: inline-block; width: 18px; height: 18px; background-size: 18px; background-repeat: no-repeat; background-position: center; margin-right: 5px; margin-bottom: -5px;"></span>Image</figcaption></figure>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">完整代码</section></li></ul>
<pre class="custom" data-tool="mdnice编辑器" style="margin-top: 10px; margin-bottom: 10px; border-radius: 5px; box-shadow: rgba(0, 0, 0, 0.55) 0px 2px 10px;"><span style="display: block; background: url(https://files.mdnice.com/user/3441/876cad08-0422-409d-bb5a-08afec5da8ee.svg); height: 30px; width: 100%; background-size: 40px; background-repeat: no-repeat; background-color: #fafafa; margin-bottom: -7px; border-radius: 5px; background-position: 10px 10px;"></span><code class="hljs" style="overflow-x: auto; padding: 16px; color: #383a42; display: -webkit-box; font-family: Operator Mono, Consolas, Monaco, Menlo, monospace; font-size: 12px; -webkit-overflow-scrolling: touch; letter-spacing: 0px; padding-top: 15px; background: #fafafa; border-radius: 5px;">&lt;template&gt;
&nbsp; &lt;div class="code-editor"&gt;
&nbsp; &nbsp; &lt;codemirror
&nbsp; &nbsp; &nbsp; :style="style"
&nbsp; &nbsp; &nbsp; ref="mycode"
&nbsp; &nbsp; &nbsp; :value.sync="code"
&nbsp; &nbsp; &nbsp; :options="cmOptions"
&nbsp; &nbsp; &nbsp; @ready="onCmReady"
&nbsp; &nbsp; &nbsp; @blur="inputChange"
&nbsp; &nbsp; &nbsp; class="code"&gt;
&nbsp; &nbsp; &lt;/codemirror&gt;
&nbsp; &lt;/div&gt;
&lt;/template&gt;
&lt;script&gt;
import { codemirror } from 'vue-codemirror';
import 'codemirror/theme/liquibyte.css';//导入选中的theme主题,与初始化theme配置一致
import 'codemirror/addon/edit/closebrackets.js'
import 'codemirror/mode/javascript/javascript.js';//导入使用的语言语法定义文件，初始化mode配置一致
import 'codemirror/addon/edit/matchbrackets.js';
import 'codemirror/addon/hint/show-hint.css';//导入自动提示核心样式
import 'codemirror/addon/hint/show-hint.js';//导入自动提示核心文件
import 'codemirror/addon/hint/javascript-hint.js';//导入指定语言的提示文件
import "codemirror/addon/hint/anyword-hint.js";
import 'codemirror/addon/lint/lint.css';
import 'codemirror/addon/lint/lint.js';
import 'codemirror/addon/lint/javascript-lint.js';
import 'codemirror/mode/clike/clike.js'
import 'codemirror/addon/edit/matchbrackets.js'
import 'codemirror/addon/comment/comment.js'
import 'codemirror/addon/dialog/dialog.js'
import 'codemirror/addon/dialog/dialog.css'
import 'codemirror/addon/search/searchcursor.js'
import 'codemirror/addon/search/search.js'
import 'codemirror/keymap/sublime.js'
import 'codemirror/addon/scroll/annotatescrollbar.js'
import 'codemirror/addon/search/matchesonscrollbar.js'
import 'codemirror/addon/search/match-highlighter.js'
import 'codemirror/addon/search/jump-to-line.js'
import 'codemirror/addon/selection/active-line.js';
import 'codemirror/addon/dialog/dialog.js'
import 'codemirror/addon/dialog/dialog.css'
import 'codemirror/addon/search/searchcursor.js'
import 'codemirror/addon/search/search.js'
// 折叠
import 'codemirror/addon/fold/foldgutter.css'
import 'codemirror/addon/fold/foldcode.js'
import 'codemirror/addon/fold/foldgutter.js'
import 'codemirror/addon/fold/brace-fold.js'
import 'codemirror/addon/fold/comment-fold.js'
import 'codemirror/addon/display/autorefresh'
/* const jshint = require('./jshint.js'); // jshit.js
window.JSHINT = jshint.JSHINT; */
export default {
&nbsp; name: "codeEditor",
&nbsp; components: { codemirror },
&nbsp; props: {
&nbsp; &nbsp; code: String,
&nbsp; &nbsp; readonly: {
&nbsp; &nbsp; &nbsp; type: Boolean,
&nbsp; &nbsp; &nbsp; default: false
&nbsp; &nbsp; },
&nbsp; &nbsp; autoFocus: {
&nbsp; &nbsp; &nbsp; type: Boolean,
&nbsp; &nbsp; &nbsp; default: false
&nbsp; &nbsp; },
&nbsp; &nbsp; height: {
&nbsp; &nbsp; &nbsp; type: String,
&nbsp; &nbsp; &nbsp; default: "300px"
&nbsp; &nbsp; }
&nbsp; },
&nbsp; data() {
&nbsp; &nbsp; return {
&nbsp; &nbsp; &nbsp; loading: true,
&nbsp; &nbsp; &nbsp; cmOptions:{
&nbsp; &nbsp; &nbsp; &nbsp; autoRefresh: true,
&nbsp; &nbsp; &nbsp; &nbsp; value:'',//编辑器的起始值。可以是字符串，也可以是文档对象。
&nbsp; &nbsp; &nbsp; &nbsp; indentUnit: 2, // 缩进多少个空格
&nbsp; &nbsp; &nbsp; &nbsp; tabSize: 2, // 制表符宽度
&nbsp; &nbsp; &nbsp; &nbsp; mode:"text/javascript",//第一个将模式名称映射到它们的构造函数，第二个将MIME类型映射到模式规范。
&nbsp; &nbsp; &nbsp; &nbsp; theme: "liquibyte",//编辑器样式的主题
&nbsp; &nbsp; &nbsp; &nbsp; autocorrect: true, &nbsp;// 自动更正
&nbsp; &nbsp; &nbsp; &nbsp; spellcheck: true, &nbsp;// 拼写检查
&nbsp; &nbsp; &nbsp; &nbsp; indentWithTabs: true,//在缩进时，是否tabSize 应该用N个制表符替换前N *个空格。默认值为false。
&nbsp; &nbsp; &nbsp; &nbsp; smartIndent: true,//是否使用模式提供的上下文相关缩进（或者只是缩进与之前的行相同）。默认为true。
&nbsp; &nbsp; &nbsp; &nbsp; lineNumbers: true,//是否在编辑器左侧显示行号。
&nbsp; &nbsp; &nbsp; &nbsp; matchBrackets : true,//括号匹配
&nbsp; &nbsp; &nbsp; &nbsp; line: true,
&nbsp; &nbsp; &nbsp; &nbsp; autoCloseBrackets:true,
&nbsp; &nbsp; &nbsp; &nbsp; styleActiveLine: true,
&nbsp; &nbsp; &nbsp; &nbsp; readOnly: this.readonly,
&nbsp; &nbsp; &nbsp; &nbsp; autofocus: this.autoFocus,//可用于使CodeMirror将焦点集中在初始化上
&nbsp; &nbsp; &nbsp; &nbsp; extraKeys: {"Ctrl-Space": "autocomplete"},//按键配置
&nbsp; &nbsp; &nbsp; &nbsp; hintOptions: { tables: {
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; table1: ['name', 'score', 'birthDate'],
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; table2: ['name', 'population', 'size']
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; },
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; completeSingle: true
&nbsp; &nbsp; &nbsp; &nbsp; },
&nbsp; &nbsp; &nbsp; &nbsp; lint: false,
&nbsp; &nbsp; &nbsp; &nbsp; hint: true,
&nbsp; &nbsp; &nbsp; &nbsp; keyMap: 'sublime',
&nbsp; &nbsp; &nbsp; &nbsp; foldGutter: true,
&nbsp; &nbsp; &nbsp; &nbsp; lineWrapping: true,
&nbsp; &nbsp; &nbsp; &nbsp; gutters: ['CodeMirror-linenumbers', 'CodeMirror-foldgutter', 'CodeMirror-lint-markers'],
&nbsp; &nbsp; &nbsp; },
&nbsp; &nbsp; };
&nbsp; },
&nbsp; computed: {
&nbsp; &nbsp; style() {
&nbsp; &nbsp; &nbsp; return {
&nbsp; &nbsp; &nbsp; &nbsp; '--height': this.height
&nbsp; &nbsp; &nbsp; }
&nbsp; &nbsp; }
&nbsp; },
&nbsp; watch: {},
&nbsp; created() {},
&nbsp; mounted() {
&nbsp; },
&nbsp; methods: {
&nbsp; &nbsp; inputChange(content) {
&nbsp; &nbsp; &nbsp; this.$emit('update:code', content.getValue());
&nbsp; &nbsp; &nbsp; this.$emit('update', content.getValue());
&nbsp; &nbsp; },
&nbsp; &nbsp; onCmReady(cm) {
&nbsp; &nbsp; &nbsp; // 这里的 cm 对象就是 codemirror 对象，等于 this.$refs.myCm.codemirror
&nbsp; &nbsp; &nbsp; cm.on("inputRead", (cm, obj) =&gt; {
&nbsp; &nbsp; &nbsp; &nbsp; if (obj.text &amp;&amp; obj.text.length &gt; 0) {
&nbsp; &nbsp; &nbsp; &nbsp; let c = obj.text[0].charAt(obj.text[0].length - 1)
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; if ((c &gt;= 'a' &amp;&amp; c &lt;= 'z') || (c &gt;= 'A' &amp;&amp; c &lt;= 'Z')) {
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; cm.showHint({ completeSingle:false })
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; }
&nbsp; &nbsp; &nbsp; &nbsp; }
&nbsp; &nbsp; &nbsp; })
&nbsp; &nbsp; }
&nbsp; },
};
&lt;/script&gt;
&lt;style lang="scss" scoped&gt;
.code ::v-deep.CodeMirror {
&nbsp; height: var(--height)
}
&lt;/style&gt;
</code></pre>
<ul data-tool="mdnice编辑器" style="margin-top: 8px; margin-bottom: 8px; padding-left: 25px; font-size: 15px; color: #595959; list-style-type: circle;">
<li><section style="margin-top: 5px; margin-bottom: 5px; line-height: 26px; text-align: left; font-size: 14px; font-weight: normal; color: #595959;">效果展示</section></li></ul>
<figure data-tool="mdnice编辑器" style="margin: 0; margin-top: 10px; margin-bottom: 10px; display: flex; flex-direction: column; justify-content: center; align-items: center;"><img src="/images/codemirrot2.png" alt="Image" style="max-width: 100%; border-radius: 6px; display: block; margin: 20px auto; object-fit: contain; box-shadow: 2px 4px 7px #999;"><figcaption style="margin-top: 5px; text-align: center; display: block; font-size: 13px; color: #2b2b2b;"><span style="background-image: url(https://img.alicdn.com/tfs/TB1Yycwyrj1gK0jSZFuXXcrHpXa-32-32.png); display: inline-block; width: 18px; height: 18px; background-size: 18px; background-repeat: no-repeat; background-position: center; margin-right: 5px; margin-bottom: -5px;"></span>Image</figcaption></figure>
<h2 data-tool="mdnice编辑器" id="code2" style="margin-top: 30px; margin-bottom: 15px; padding: 0px; font-weight: bold; color: black; font-size: 22px; display: block; border-bottom: 4px solid #40B8FA;"><span class="prefix" style="display: flex; width: 20px; height: 20px; background-size: 20px 20px; background-image: url(https://files.mdnice.com/fullstack-1.png); margin-bottom: -22px;"></span><span class="content" style="display: flex; color: #40B8FA; font-size: 20px; margin-left: 25px;">参考链接</span><a href="#code2" class="headerlink" title="参考链接"></a><span class="suffix" style="display: flex; box-sizing: border-box; width: 200px; height: 10px; border-top-left-radius: 20px; background: RGBA(64, 184, 250, .5); color: rgb(255, 255, 255); font-size: 16px; letter-spacing: 0.544px; justify-content: flex-end; float: right; margin-top: -10px; box-sizing: border-box !important; overflow-wrap: break-word !important;"></span></h2>
<blockquote class="multiquote-1" data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; text-size-adjust: 100%; line-height: 1.55em; font-weight: 400; border-radius: 6px; color: #595959; font-style: normal; text-align: left; box-sizing: inherit; border-left: none; border: 1px solid RGBA(64, 184, 250, .4); background: RGBA(64, 184, 250, .1);"><span style="color: RGBA(64, 184, 250, .5); font-size: 34px; line-height: 1; font-weight: 700;">❝</span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 2px; font-size: 14px; word-spacing: 2px; margin: 0px; line-height: 26px; color: #595959;"><a href="https://github.com/surmon-china/vue-codemirror" style="text-decoration: none; word-wrap: break-word; color: #40B8FA; font-weight: normal; border-bottom: 1px solid #3BAAFA;">vue-codemirror</a></p>
<span style="float: right; color: RGBA(64, 184, 250, .5);">❞</span></blockquote>
</section>