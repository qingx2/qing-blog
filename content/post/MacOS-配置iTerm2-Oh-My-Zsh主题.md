---
title: MacOS 配置iTerm2 + Oh My Zsh主题
date: 2017-03-16 22:53:45
---

😏 想让你的终端也变成这样？Come On！跟着我左手右手一个慢动作...
![](https://cloud.githubusercontent.com/assets/1415488/13198621/f2c1320c-d848-11e5-8f22-7fac1baeec2f.jpg)


☝️ 1. Download - iTerm2

iTerm2官网下载地址 

☝️ 2. Install oh-my-zsh now

🙄 Your terminal never felt this good before! 

🙄 就像 Oh My Zsh 的主页上面说的：“当你用了这些非常酷的命令行工具后，人们来到你的电脑前，一定会对你的命令行大加称赞。迎来一片点赞。”

还有酱紫简单点带可爱风的小图标呢!
![](https://cloud.githubusercontent.com/assets/2618447/6316752/51e211a6-ba00-11e4-8f57-e82fcc4a453f.png)

<!--more-->

使用curl下载：

    curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh

安装完成后，显示如下:

    Cloning Oh My Zsh...
    Cloning into '/Users/icafe/.oh-my-zsh'...
    remote: Counting objects: 712, done.
    remote: Compressing objects: 100% (584/584), done.
    remote: Total 712 (delta 15), reused 522 (delta 4), pack-reused 0
    Receiving objects: 100% (712/712), 443.58 KiB | 27.00 KiB/s, done.
    Resolving deltas: 100% (15/15), done.
    Checking connectivity... done.
    Looking for an existing zsh config...
    Using the Oh My Zsh template file and adding it to ~/.zshrc
    Copying your current PATH and adding it to the end of ~/.zshrc for you.
    Time to change your default shell to zsh!
            __                                     __
     ____  / /_     ____ ___  __  __   ____  _____/ /_
    / __ \/ __ \   / __ `__ \/ / / /  /_  / / ___/ __ \
    / /_/ / / / /  / / / / / / /_/ /    / /_(__  ) / / /
    \____/_/ /_/  /_/ /_/ /_/\__, /    /___/____/_/ /_/
                           /____/                       ....is now installed!
    Please look over the ~/.zshrc file to select plugins, themes, and options.
    p.s. Follow us at https://twitter.com/ohmyzsh.
    p.p.s. Get stickers and t-shirts at http://shop.planetargon.com.

☝️ 3.设置主题

编辑配置文件:

    vi ~/.zshrc

修改 ZSH_THEME 为自己喜欢的主题名称:

    ZSH_THEME="robbyrussell"

☝️ 4.快捷选择目录和文件

推推眼镜 😎 虽然主题很酷，但是真正酷的地方在于它的快捷选择目录和文件的特点上。

Mac在使用普通的终端命令行时，常用的cd 命令在众多目录中切换，首先得输入这个目录下的目录前缀，按table键进行自动补全，但是这个补全功能是严格匹配大小写的。

Oh My Zsh为我们提供了更人性化的使用体验，cd一下接着table table…. 就可以查看此目录下的所有文件了： 

    cd /Applications/Calculator.app
    Alfred\ 3.app/                Dashboard.app/                MWeb.app/                     Pages.app/                    Sketch.app/                   WeChat.app/
    Android\ File\ Transfer.app/  Dictionary.app/               Mail.app/                     Photo\ Booth.app/             Steam.app/                    WebStorm.app/
    App\ Store.app/               FaceTime.app/                 Maipo.app/                    Photos.app/                   Stickies.app/                 iBooks.app/

☝️ 5.强化命令行自动提示补全功能

安装:

    git clone git://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions

打开 `zshrc` 添加配置信息:

    vi ~/.zshrc
    
    source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh



😯 over..

