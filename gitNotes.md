# git 简介

git 是一款分布式的版本控制软件。与集中式版本控制软件（SVN）的不同在于：

- 项目的历史版本库维护的位置。
- 使用 git 进行版本控制时，本地仓库不仅包含项目代码库还有历史库。在本地的环境开发中就可以记录项目的历史改变而SVN的历史库存在于中央仓库，每次对比变化及提交代码都必须连接到中央仓库才能进行。
- git 的好处就是自己可以在脱机环境下就可以查看项目开发的历史版本。多人开发时充当中央仓库的的git仓库挂了，可以随时创建新的仓库代替。

## git 常用命令

### 下载与配置

[下载](https://git-scm.com/downloads)安装之后，需要配置一下当前操作者信息，以区分多人开发。命令如下：

```sh
git config user.name "你的名字"
git config user.email "你的邮箱"
```

可以添加 `--global` 参数，设置为全局配置，之后该台机器上的所有git仓库都会使用该配置。不带该参数则表示该配置只对当前仓库有效。

### 创建版本库

初始化一个仓库，当前项目根目录下，打开 git bash 使用命令 `git init`即可。

添加文件到仓库，总共由两步：

- `git add <file>`
- `git commit -m "description"`。

这里 `git add `命令可以重复使用，添加多个文件到暂存区，而`git commit `命令可以提交暂存区内的所有文件，参数 `-m` 表示本次项目提交的说明，可以为任意内容，但应该有意义，方便自己或者他人查看。

### 查看工作区状态

如果不清楚项目文件是否已经修改，可以使用命令 `git status` 进行查看。

### 查看修改内容

```sh
### 查看工作区与暂存区区别
git diff
### 查看暂存区与最新版本库区别
git diff --cached
### 查看工作区与最新版本库区别
git diff HEAD -- <file>
### 查看总过去与某版本库区别
git diff commitID -- <file>
```

### 查看提交日志

`git log`  常用于查看项目的历史提交。如果提交日志太多查看起来很不方便，可以添加参数 `--pretty=oneline` 简化日志输出。

### 查看命令历史

`git reflog` 用于查看该仓库内使用过的git 命令。

### 版本回退

`git reset --hard HEAD^` 返回上一个项目版本。`HEAD` 表示当前版本，`HEAD^` 表示上一个版本，上上一个版本时 `HEAD^^`，往上一百个版本为`HEAD~100`。

通常我们会使用项目的版本号 `commitID`  回退到指定的版本。命令为 `git reset --hard commitID`。

### 撤销修改内容

#### 丢弃工作区的修改

使用快捷键 `ctrl + z`,No ,那样太没逼格了。使用命令 `git checkout -- <file>`。注意不要忘记参数 `--`，因为`git checkout <branchName>`是切换分支命令，会操作失败。

`git checkout -- <file>` 撤销工作区修改分两种情况：

- 文件修改后还没有放到暂存区，现在撤销修改就和最新版本库一摸一样状态。

- 文件已添加到暂存区，现在又做了修改,现在撤销修改就回到添加到暂存区后的状态

#### 丢弃暂存区的修改

分为两步：第一步，先把暂存区内的修改撤销掉，重新放回工作区，使用命令`git reset HEAD <file>`;第二步，撤销工作区修改，使用命令 `git checkout -- <file>` 。

### 删除文件

`git rm <file>`。相当于执行 `rm <file>` 和 `git add <file>`

删除文件分为两种情况：

- 误删，误删除工作区文件，使用命令 `git checkout -- <file>`；误删除暂存区文件，使用命令 `git reset HEAD <file>` 和 `git checkout -- <file>`。

- 确认删除，使用命令 `git rm <file>` 和 `git commit -m "删除了文件file"`。之后版本库就不会含有该文件。

### 远程仓库

远程仓库与本地仓库之间的传输往往需要进行用户身份验证，如 github，它为我们提供了两种连接方式：`https` 和 `SSH`。

- `https` 使用该方式之后，对远程仓库执行 git clone、git fetch、git pull 或 git push 命令时，系统将要求您输入 GitHub 用户名和密码。可以使用命令 `git config --global credential.helper wincred` 告诉 Git（版本需大于或等于 1.7.10 ） 在每次与 GitHub 会话时记住您的 GitHub 用户名和密码。

- `ssh` 对远程仓库执行 git clone、git fetch、git pull 或 git push 命令时，系统将提示您输入密码，并且必须提供您的 SSH 密钥密码。
SSH密钥生产命令 `ssh-keygen -t rsa -C "邮箱地址"`。

#### 关联远程仓库

这里我们使用 `SSh` 方式，需要创建 SSH 密钥：
`ssh-keygen -t rsa -C "email地址"` 之后在用户目录下复制 id_rsa.pub 文件的内容并添加到github。
