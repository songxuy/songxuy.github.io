title: Hexo博客添加helper-live2d
author: coolsong
tags:
  - Hexo
categories:
  - Hexo
date: 2020-08-27 20:45:00
---
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;大家应该都看到过，有的博客右下方有一个会跟着鼠标转动的小人。当我们打开控制台又找不到找不到相应的元素。其实它是使用了helper-live2d插件，在下载相应的模型进行配置。
<!--more-->
#### Hexo-helper-live2d
>由于我的博客是使用hexo搭建的，因此所选择的插件就是hexo-helper-live2d，其实就是一个动态模型插件，[github地址](https://github.com/EYHN/hexo-helper-live2d)。
>
>除了针对于hexo的之外，还有针对vue-press的vuepress-plugin-helper-live2d插件，[github地址](https://github.com/JoeyBling/vuepress-plugin-helper-live2d)
>
>注意事项： 必须要装有Node环境，已经相应的live2d模型

#### Live2d模型
>Live2d模型对应的就是相应的小人，目前作者提供的模型如下：（[github地址](https://github.com/xiazeyu/live2d-widget-models)）

* live2d-widget-model-chitose
* live2d-widget-model-epsilon2_1
* live2d-widget-model-gf
* live2d-widget-model-haru/01 (use npm install --save live2d-widget-model-haru)
* live2d-widget-model-haru/02 (use npm install --save live2d-widget-model-haru)
* live2d-widget-model-harutolive2d-widget-model-hibiki
* live2d-widget-model-hijiki
* live2d-widget-model-izumi
* live2d-widget-model-koharu
* live2d-widget-model-miku
* live2d-widget-model-ni-j
* live2d-widget-model-nico
* live2d-widget-model-nietzsche
* live2d-widget-model-nipsilon
* live2d-widget-model-nito
* live2d-widget-model-shizuku
* live2d-widget-model-tororo
* live2d-widget-model-tsumiki
* live2d-widget-model-unitychan
* live2d-widget-model-wanko
* live2d-widget-model-z16

> 对应的截图如下，[原文链接](https://huaji8.top/post/live2d-plugin-2.0/)
> 
![Image](/images/live2d1.png)

#### 具体安装

* 安装模块（在hexo根目录安装）

```
//安装lived2d插件
cnpm install --save hexo-helper-live2d

// 安装2d模型 我这里装的是live2d-widget-model-haruto
cnpm install --save live2d-widget-model-haruto
```

* 在_config.yml中配置
```Yml

live2d:
  enable: true
  #enable: false
  scriptFrom: local # 默认
  pluginRootPath: live2dw/ # 插件在站点上的根目录(相对路径)
  pluginJsPath: lib/ # 脚本文件相对与插件根目录路径
  pluginModelPath: assets/ # 模型文件相对与插件根目录路径
  # scriptFrom: jsdelivr # jsdelivr CDN
  # scriptFrom: unpkg # unpkg CDN
  # scriptFrom: https://cdn.jsdelivr.net/npm/live2d-widget@3.x/lib/L2Dwidget.min.js # 你的自定义 url
  tagMode: false # 标签模式, 是否仅替换 live2d tag标签而非插入到所有页面中
  debug: false # 调试, 是否在控制台输出日志
  model:
    use: live2d-widget-model-haruto
    # use: live2d-widget-model-wanko # npm-module package name
    # use: wanko # 博客根目录/live2d_models/ 下的目录名
    # use: ./wives/wanko # 相对于博客根目录的路径
    # use: https://cdn.jsdelivr.net/npm/live2d-widget-model-wanko@1.0.5/assets/wanko.model.json # 你的自定义 url
  display:
    position: right
    width: 145
    height: 315
    vOffset: -60 #竖直偏移量
  mobile:
    show: true # 是否在移动设备上显示
    scale: 0.5 # 移动设备上的缩放
  react:
    opacityDefault: 0.7
    opacityOnHover: 0.8
```

#### 最终效果

![Image](/images/live2d2.png)
