---
layout: post
title: "如何从Git永久删除一个文件"
description: ""
category: 折腾
tags: [Git]
---
{% include JB/setup %}

借助官网的[Remove sensitive data][1]这篇文章，我很快掌握了这项技术，但比较傻的是这个技术也导致刚写的博客不见了。

Git设计的初衷就是确保每一个操作是被记录在案的，所以git的直接参数只能让一个文件不被track（remove），但是在此之前对这个文件的种种操作，都仍然是记录在案，而且永远存在的。所以我们要先从本地的历史记录中除掉所有关于这个文件的记录：

    git filter-branch --force --index-filter \
        'git rm --cached --ignore-unmatch your_file' \
        --prune-empty --tag-name-filter cat -- --all

git filter-branch --index-filter 可以接受一个命令，作为对branch的操作。your_file是要处理的文件名。--prune-empty --tag-name-filter cat 忽略空提交，处理过后保留之前提交的注释信息等。会显示：
    
    Rewrite xxxxxx (xx/xx)
    Ref 'refs/heads/master' was rewritten

从磁盘删除该文件，或者加入.gitignore，提交回repo：

    git push origin master --force


如果你和我一样觉得上面的方法太反动了，那么我强烈建议你尝试如下工具[BFG][2]：

这个命令行工具的[用法][3]并不局限于删除指定文件，还可以自动删除大于多少M文件，等等。原理和filter-branch类似。这里仍以删除指定文件为例：

     git clone --mirror git://github.com/repo.git
     java -jar bfg.jar --delete-files filename repo.git
     git push


我不知道用什么命令可以追踪一个文件是否曾经出现在Repo中，但是我checkout了历史Ref，确认了在我添加那个想要删除的文件的commit附近，从未出现过那个我“永久删除”掉的文件了，就好象这个文件从来没有出现过一样，这是否意味着我成功了呢？或者谁能告诉我用某个简单的命令，就可以检查这个文件历史上究竟是否出现过呢？




[1]:http://help.github.com/articles/remove-sensitive-data
[2]:http://repo1.maven.org/maven2/com/madgag/bfg/1.11.0/bfg-1.11.0.jar (bfg-repo-cleaner)
[3]:http://rtyley.github.io/bfg-repo-cleaner/
