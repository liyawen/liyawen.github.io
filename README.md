## hexo + yelee 搭建博客

___

#### 分支介绍

    `master` 分支为打包后的博客内容
    `source` 分支为博客源码

#### 如何搭建

    1.在github 新建一个 `[yourGithubName].github.io` 项目仓库，勾选 `public` 以及 `Initialize this repository with a README` , 点击 `create repository`。

    2.把本地电脑的 `ssh` 的 `id_rsa.pub` 里面的内容拷贝粘贴到 `github` 上（如果本地还未有ssh，请新建），路径是 `头像图标 -> Settings -> SSH and GPG keys `
    按照 hexo 教程，在本地新建项目，并搭好，加上yelee

    3.运行 `hexo d -g` 即可打包并上传到`[yourGithubName].github.io` 项目仓库

#### 本地预览

    `hexo g` 编译
    `hexo s` 开启本地服务器

#### 目前改用`yilia`主题

[Hexo-Theme-Yilia](https://github.com/litten/hexo-theme-yilia)该主题相对yelee更为简洁，个人喜欢简单的事物