---
title: 使用Daocloud持续构建Hexo
date: 2016-03-17T23:57:44+08:00
lastmod: 2016-04-01T12:01:40+08:00
categories: 技术
tags: [hexo]
---

本文是针对[《利用Coding和Daocloud打造全自动发布的hexo博客》](https://www.luodaoyi.com/zatan/9.html)的一些解释及补充，感谢原作者。

此方法的优点就是本地只需要git，无需nodejs环境。将源文件部署于Coding，通过Daocloud持续构建，然后发布到Github。

在Coding仓库根目录创建`daocloud.yml`，文件内容如下：<!--more-->

```yaml
image: library/node:4.4.0-onbuild
before_script:
    - npm config set user 0
    - npm config set unsafe-perm true
    # 避免出现权限问题
    - npm install hexo-cli -g
    # --registry=http://registry.npm.taobao.org 使用淘宝的npm源安装 更快捷
    - npm install
    # - git clone https://github.com/luodaoyi/hexo-theme-next.git themes/next
    # 克隆主题到主题目录 这里的主题Git地址和目录替换成你自己的主题地址和目录
    - mkdir ~/.ssh
    # 新建私钥文件夹
    - mv .daocloud/id_rsa ~/.ssh/id_rsa
    # 移动私钥到私钥文件夹
    - mv .daocloud/ssh_config ~/.ssh/config
    # 移动ssh配置文件
    - chmod 600 ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/config
    # 赋予可读权限
    - eval $(ssh-agent)
    # 启用ssh-agent进程
    - ssh-add ~/.ssh/id_rsa
    # 添加密钥
    - rm -rf .daocloud
    # 删除项目里面的私钥存放目录
    - git config --global user.name "heartnn"
    - git config --global user.email bevista@gmail.com
    # 配置git
script:
    - hexo g
    # 生成html
    - hexo d
    # 发布html代码，根据你hexo的设置可以发布到多个平台
    - rm -rf ~/.ssh/
    # 删除私钥文件夹
```

创建一组新的密钥，公钥上传到Github的一个仓库中，并在Hexo中设置Deploy到这个git仓库。私钥放在`.daocloud`目录中，并重命名为`id_rsa`。

`.daocloud`中还应该有ssh的配置文件，命名为`ssh_config`，其中至少应该有以下两行：

```plain
StrictHostKeyChecking no
UserKnownHostsFile /dev/null
```

这样部署到Github的时候可以禁用SSH远程主机的公钥检查，不会出现错误。

这样都完成后`git push`到Coding。

在Daocloud中新建一个代码构建，绑定自己的Coding，并选择Hexo源代码的仓库。

![](/uploads/2016/03/daocloud-settings.png)

可能会提示Dockerfile不存在，但是没有关系，执行环境Github的话就选国外，如果是Coding什么的就选北京(并将daocloud.yml中npm使用淘宝源)。

此后每次`git push`到Coding后，会自动触发构建，然后Deploy到Github。原作者还提供了构建Docker，避免Github的缓存问题。
