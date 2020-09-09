# Terminal

## MacOS

### Homebrew

/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)”

/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"

安装软件，如：brew install oclint

卸载软件，如：brew uninstall oclint

搜索软件，如：brew search oclint

更新软件，如：brew upgrade oclint

查看安装列表， 如：brew list

更新Homebrew，如：brew update

### NVM

curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash

wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash

https://github.com/creationix/nvm/blob/master/README.md

nvm 常用命令

\* nvm install stable ## 安装最新稳定版 node，当前是node v9.5.0 (npm v5.6.0)

\* nvm install <version> ## 安装指定版本，可模糊安装，如：安装v4.4.0，既可nvm install v4.4.0，又可nvm install 4.4

\* nvm uninstall <version> ## 删除已安装的指定版本，语法与install类似

\* nvm use <version> ## 切换使用指定的版本node

\* nvm ls ## 列出所有安装的版本

\* nvm ls-remote ## 列出所有远程服务器的版本（官方node version list）

\* nvm current ## 显示当前的版本

\* nvm alias <name> <version> ## 给不同的版本号添加别名

\* nvm unalias <name> ## 删除已定义的别名

\* nvm reinstall-packages <version> ## 在当前版本 node 环境下，重新全局安装指定版本号的 npm 包

端口

lsof -i:8095

sudo kill -9 7748

sudo npm install -g express-generator