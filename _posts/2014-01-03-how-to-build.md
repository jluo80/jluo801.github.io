---
layout: post
title: '在 Github 上搭建自己的博客系统'
category: 文档
tags: 折腾
---


本来是准备用wordpress写一点读书笔记的，结果在找可以将wordpress备份到Github插件或脚本的时候，才发现早就有成熟的方案可以在Github直接搭建自己的博客系统了。

对于程序员来说，这样做的好处是显而易见的：

1、发布博客的过程就是git push的过程，于是天然具备了博客的版本管理和分布式备份。

2、整个博客完全由代码所生成，内容和展现完全分离，可以比较容易的更换主题，以及升级维护、增加功能插件……各种折腾，完了一键编译就好。

3、服务器的维护成本摊给了Github，如果愿意使用username.github.io的话，连域名都省下了。

4、逼格略高。

我在搭建这个系统的过程中，阅读了不少指导性的文章，其中最有帮助的要属[Github](http://help.github.com/categories/20/articles/)和[Jekyll的官方文档](http://jekyllrb.com/docs/home/)，但是如果你觉得中文更加亲近一些，加上我走过的弯路兴许能够对你有帮助，你就可以考虑读下去。



一、基本原理

1、我们之所以有机会能够在Github上图文并茂，这要得益于Github的Page.Github（现入口更名为username.github.io）。这个功能可以让开发者或者企业host一些静态页面在指定的地点，他的工作原理是通过Jekyll（下面介绍）来编译放在指定repo的模板，从而生成静态的html页面再展现在（和指定repo位置）对应的url位置。

2、Jekyll是一个建站工具（Transform your pain text into static websites and blogs），其本质是一个模板引擎，可以根据模板和配置文件，生成博客或是网站。
    
Jekyll的开发环境是Ruby，这就意味着高度建议你在Linux或是Mac系统下完成环境的搭建工作。


二、环境搭建

1、安装ruby和ruby-dev。ruby-dev中存在一些gem install所需要的依赖。

2、gem install jekyll

3、为了维持ruby jekyll等一系列工具链的版本一致性，请键入：gem install bundler安装bundler，之后建立一个如下的Gemfile供bundle update的时候读取。


    source 'https://rubygems.org'
    gem 'github-pages'


4、mkdir my_blog，jekyll new my_blog - 在目录my_blog下生成博客框架。

5、_post 目录下的以.md为后缀的文件就是markdown语法的一篇文章。

6、在_config.yml的同级目录执行jekyll build，然后预览：jekyll serve。预览的时候会在本机起一个监听4000端口的web server，可以在浏览器中查看127.0.0.1:4000。

7、build的结果会放在_site目录中。但是该目录无需添加入下面的git repo中，因为Github会根据源码自行编译，所以有的jekyll主题会将_site目录放入.gitignore文件中。

（以下为可选）

1、gem install jekyll-bootstrap，安装jekyll辅助，可以用命令行来帮助生成post，about等页面。

2、gem install rdiscount, 并且将_config.yml中默认的markdown渲染引擎更改为rdiscount。因为坊间传闻默认的markdown渲染引擎非常不好用。


三、Github部署 

博客生成系统好了以后，我们来进行部署：在Github的static page hosting中，存在着两种形态：

1、通过username.github.io 可以直接访问得到的————目前我也是这样做的————如果希望Github在此编译渲染你的页面，你需要将你的repo命名为username.github.io，*并且提交到master默认主分支*。

2、通过username.github.io/projectname来访问的，这需要将你的代码环境（上文中的my_blog目录）*提交至projectname下面的gh-pages分支*。


四、模板系统

这套Github博客生成框架内含着一个和我喜欢Linux同样的一个原因：极简主义。

核心框架只是一个模板引擎而已，基于这个模板引擎，你可以open source自己的风格布局，也可以加上自己的功能（Javascript），甚至可以加上三方的评论系统，计数器，等等……


本来我打算推荐一些模板，但是想到我之蜜糖，彼之砒霜，您不妨自己去搜索jekyll theme。


我这套风格使用的是（http://yonsm.net）还有一个修改版（http://webforgs.me）,作者欢迎fork，但是显然，如果想要加入自己喜欢的功能，还需要阅读一下模板代码，更多的了解工作原理以后进行修改。虽然风格极简，但是已经在模板引擎中内置了对站长统计和多说评论插件的支持，只需要简单配置你在站长统计和多说评论的id即可使用上述功能。


