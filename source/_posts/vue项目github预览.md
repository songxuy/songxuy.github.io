title: vue项目github预览
author: coolsong
tags:
  - 'vue '
  - 'github'
categories:
  - 经验分享
date: 2018-12-12 22:01:00
---
&nbsp;&nbsp;&nbsp;今天在掘金上看到了一片文章通过vue+echars写了一个在线简历的文章感觉效果还是不错的，自己也很快的copy了一个自己的简历，这时候就出现了一个问题。我要如何让别人也能看到我这个简历呢？由于我自己还没有服务器 所以就只能想到利用github了。之前通过github搭过自己的一个blog 于是就按照之前的步骤 把代码传到了GitHub上  在仓库的setting中GitHub Pages选项中把source改成自己的分支 这是后就会有一个链接，但是这次点开之后却啥也没有 👿 emmm
<!--MORE-->
 ![My Pic](/images/git1.jpg)
  ![My Pic](/images/git2.jpg)
然后自己在网上查了一下  原来没把自己的dist目录传上去  于是在项目的.gitignore中删除了/dist/ 这时再重新打包上传 发现项目中有了dist目录 于是再次打开网址并且在后面拼上dist 然而显示的还是一片空白 于是接着打开控制台 发现好多js的请求404了 于是又看了下引用的路径并且网上查了一下 发现时路径前面多了一个/ 于是又按照网上的解决方法在项目的config文件夹中 找到index.js 将里面build部分的assetsPublicPath的/去掉 再重新build一次
现在终于可以访问了 
 ![My Pic](/images/git3.jpg)
  ![My Pic](/images/git4.jpg)
   ![My Pic](/images/git5.jpg)
    ![My Pic](/images/git6.jpg)
以后还是考虑自己买个服务器吧 速度有时候太感人了 emmm