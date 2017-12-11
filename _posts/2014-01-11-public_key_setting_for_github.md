---
layout: post
title: "Github 的免密码提交————更加方便安全的公钥认证"
description: ""
category: 折腾
tags: [Github]
---
{% include JB/setup %}

[李笑来《把时间当朋友》][1]这本书中曾讲到，学习盲打，需要集中精力一段时间，但是带来效率的提升，是伴随之后一生的。

Github提交也是同理，我一惯用http方式在每次提交的时候不厌其烦的输入用户名和密码，这样每次浪费掉的时间加起来是相当可观的。对Github.com配置公钥鉴权是一件相当容易，而且一劳永逸的事情。


#### 1、默认单用户，Repo唯一的配置

利用openssh生成公私钥对：

    ssh-keygen -t rsa

一路回车下来，会在~/.ssh 下生成id_rsa和id_rsa.pub 两个文件，RSA的公私钥对。如果在该位置已经存在同名文件，就不要一路回车，在中间有交互输入的地方另行指定文件名。

用Git的remote命令或直接编辑工程目录下的.git/config：

    url = git@ProjectName:/username/repo.git

对于单用户Repo唯一的情形，ProjectName可以任意指定，随后解释。UserName就是你在Github的用户名。repo.git是repo的全名。


#### 2、多用户，多Repo在提交时候如何区分配置

多用户指的是你在Github上面具有多个用户名，多个Repo指的是你在本地有多个Repo需要提交到远端相对应的Repo。

对于这种情况，我们首先需要生成文件名不同的公私钥对（我曾经尝试过多个Repo共用一个公钥，Github.com提示Error：公钥已被使）方法同上。

多用户多Repo的关隘在于对~/.ssh/config 的配置，这是按照ProjectName来区别的：

    Host ProjectName
        HostName Github.com
        User UserName
        IdentityFile ssh_private_key_fullpath


ProjectName可以自己指定，用来区分不同的远端Repo，UserName就是你在Github.com的用户名，IdentityFile是用来指定你的私钥文件（默认情况下是不带.pub的同名文件）。

那么Git在提交的时候是如何知道怎样去对应ProjectName的呢？在单用户的时候，我们在remote的url字段里面（.git/config）：

    url = git@ProjectName:/UserName/Repo.git

ProjectName字段此时就派上用场了，和.ssh/config中Host字段对应起来，Github就会将不同的Repo用相对应的ProjectName来区隔。




公私钥对的鉴权方式远比密码方式来的安全和方便，理论上Git的客户端可以用私钥签名，交给Github用预设的公钥来认证自己的身份，在传输的过程中，当链接建立以后，还可以用协商好的SessionKey来加密数据。这个过程涉及到公钥密码学，我计划在另一篇博客《如何在不可新环境下建立可信链接》中做进一步的描述。










[1]:http://wordpress.lixiaolai.com/time-friend.zip "李笑来，《把时间当作朋友》电子版官方下载"
