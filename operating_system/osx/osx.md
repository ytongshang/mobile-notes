# Mac

- [Homebrew](#homebrew)
- [Zsh](#zsh)
- [Autojump](#autojump)

## Homebrew

- 安装Homebrew,[网站](http://brew.sh/)

  ```bash
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```

- 安装 使用brew 安装git等

  ```bash
  brew install git
  brew install python@3.8
  brew install webp
  brew install mysql
  brew install yarn

  // ijkplayer
  brew install yasm

  // https://github.com/bilibili/jni4android
  brew install flex
  brew install bison
  ```

- 使用brew cask 安装java atom chrome 等等

  ```bash
  brew cask install adoptopenjdk8
  brew cask install android-sdk
  brew cask install item2
  ```

## Zsh

- 安装iterm2使用brew cask安装item2

  ```bash
  brew cask install iterm
  ```

- zsh 安装新版本的zsh

  ```bash
  brew install zsh
  ```

- 安装oh my zsh

    - [zsh文章](https://segmentfault.com/a/1190000002658335?_ea=438459)

    - 安装oh my zash

    ```bash
    git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
    ```

    - 创建一个zsh的配置文件

    ```bash
    touch ~/.zshrc
    ```

    - 如果原来使用过zsh,备份原来的zsh的配置文件

    ```bash
    cp ~/.zshrc ~/.zshrc.orig
    ```

    - 复制 zsh 模板到配置文件中

    ```bash
    cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
    ```

    - 将shell切换到zsh

    ```bash
    chsh -s /bin/zsh
    ```

    - windows使用cygwin时，在~/.bash_profile中加入zsh

    ```bash
    exec zsh
    ```

    - 修改zsh 主题 ，编辑 ~/.zshrc，主题文件在 ~/.oh-my-zsh/themes 目录

    ```bash
    ZSH_THEME=y
    ```

## Autojump

- 安装autojump

  ```bash
  git clone https://github.com/wting/autojump
  cd autojump
  python install.py
  ```

- 添加执行install.py生成的代码到~/.zshrc

  ```bash
  source .zshrc
  ```

- 重启shell

## Go2Shell

- [go2Shell](https://zipzapmac.com/Go2Shell)

## 软件

- AS
- IntelliJ IDEA
- Clion
- Visual Studio Code
- 微信开发者工具
- Zan Proxy
- ClashX
- SQLiteStudio
- Go2Shell
- iTerm
- PostMan

- WPS Office
- Chrome
- 360_zip
- 百度网盘
- 滴答清单
- 微信
- 企业微信
