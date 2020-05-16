---
title: Hexo使用
date: 2020-03-29 12:04:55
tags: 
- Web
categories:
- [Web]
toc: true
---
使用Hexo建立博客网站(静态网页)
<!--more-->
## 使用

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)

## Hexo 主体配置-icarus

### 文章

```markdown
#
title: 文章名
date: XXXX-XX-XX HH:MM:SS(自动生成)
tags: 
- 标签一
- 标签二
- 标签三
categories:
- 一级分类
- 二级分类
- 三级分类
toc: true(开启目录)
description: 在扉页使用，打开后看不到
photos: 照片目录 
```

## Hexo报错

```shell
# 错误显示:
Error: Spawn failed
    at ChildProcess.task.on.code (C:\Users\20499\Documents\blog\node_modules\hexo-util\lib\spawn.js:51:21)
    at ChildProcess.emit (events.js:198:13)
    at ChildProcess.cp.emit (C:\Users\20499\Documents\blog\node_modules\cross-spawn\lib\enoent.js:34:29)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:248:12)
# 原因: 本地的博客版本与远程的版本不一致
# 解决方法: 删除博客目录下的.deploy_git文件夹，然后克隆远程(也就是将要发布的地址)的仓库到博客目录里面，然后改名字为.deploy_git
```
