---
layout: post
title: "在 Github.Page渲染数学公式"
description: ""
category: 折腾
tags: [Github]
---
{% include JB/setup %}

这篇博客是个副产品，因为对我而言用到数学公式的场景并不多。


可是最近在和银联讨论密钥下发策略，来做这件事的算法和协议都很多，还有许多成熟的库，但银联研究院作为央企还担负着商业实现以外的一些政治责任。所以这个讨论就没法用现成的东西直接交差了，算法和协议的每一个细节实现，每一个环节强度的论证，都要经得起推敲。于是就临时抱佛脚，在做这项工作的一年多来，我也补习了一些算法理论基础，这项工作做了快一年，奈何有些内容学了又忘了，（或者说过度饮酒真的会影响记忆力啊……）于是就想着在脑海中去整理，然后以笔记的形式描述出来，形成体系，这样就涉及到了一些公式的渲染。

我用的这个博客生成引擎所默认的Markdown引擎，是支持LaTeX扩展的，但是这个实现比较粗糙：将LaTeX渲染成为PNG或者PDF，我不喜欢这个笨重而又粗糙的方案。可是我又看到老美的Blog里面提到（可能这个人认识Github的程序员？）Github不愿以用自己的CPU去渲染数学公式或者特殊字体排版。如果说此话属实，这就意味着我无法利用某些原生语法来让Github直接渲染公式系统了。

于是想到了和我的博客评论一样的思路（我的博客使用的是Duoshuo外挂评论，因为本质上Github.Page支持的都是静态页面），那么是不是有什么外挂Javascript的方案支持跨浏览器的内容渲染呢？于是再次验证了这一行的铁律：凡是你想到的都有人实现过了:[MathJax](http://www.mathjax.org/)。


他的部署方式比较简单，在你的根模板文件增加这样一句话：


    <script type="text/javascript"
    src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
    </script>

当然你可以将这段js代码host到你的Github，但是根据浏览器资源加载机制，尽量分布到不同的域名，可能更容易[异步加载](http://www.chromium.org/developers/design-documents/process-models)。

在我的Jekyll文件结构中，根模板的位置在_layout中（看过web框架代码的人应该比较好理解Jekyll的文件布局），在安装了某些模板插件的Jekyll目录下，有可能位于：


    _include/themes/yourtheme/default.html


总之认准文件名default.html，然后替换默认的Markdown引擎为kramdown：


    编辑_config.yml, markdown: kramdown
    sudo gem install kramdown


现在你可以在`$$`和`$$`之间书写数学公式的语法了。

来看几个例子，右键有惊喜：

### $$ \left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)  $$
                
                       
### $$ \mathbf{V}_1 \times \mathbf{V}_2 =  \begin{vmatrix}\mathbf{i} & \mathbf{j} & \mathbf{k} \\\frac{\partial X}{\partial u} &  \frac{\partial Y}{\partial u} & 0 \\\frac{\partial X}{\partial v} &  \frac{\partial Y}{\partial v} & 0\end{vmatrix} $$            
                
                         
### $$ \begin{aligned} \nabla \times \vec{\mathbf{B}} -\, \frac1c\, \frac{\partial\vec{\mathbf{E}}}{\partial t} & = \frac{4\pi}{c}\vec{\mathbf{j}} \\   \nabla \cdot \vec{\mathbf{E}} & = 4 \pi \rho \\\nabla \times \vec{\mathbf{E}}\, +\, \frac1c\, \frac{\partial\vec{\mathbf{B}}}{\partial t} & = \vec{\mathbf{0}} \\\nabla \cdot \vec{\mathbf{B}} & = 0 \end{aligned} $$            
                
                        
### $$ \frac{1}{\Bigl(\sqrt{\phi \sqrt{5}}-\phi\Bigr) e^{\frac25 \pi}} = 1+\frac{e^{-2\pi}} {1+\frac{e^{-4\pi}} {1+\frac{e^{-6\pi}} {1+\frac{e^{-8\pi}} {1+\ldots} } } } $$             
         
         
千万不要问我上面那几坨是什么鸟意思。
