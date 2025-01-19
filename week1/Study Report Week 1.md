[toc]

# 2025大一上寒假学习报告
---
## 第一周

### 模拟电子技术基础网课学习
本周学习了1-7小节。
本周的重点内容如下：
~~~
PN结的形成和性质
二极管的伏安特性以及运用
    -伏安特性折线化
    -微变等效
    -稳压二极管
双极晶体管的性质和运用
    -BJT共射特性曲线
~~~

### 线性代数网课学习
本周学习了1.1-1.6小节
本周重点内容如下：
~~~
行列式的展开方式
行列式的性质
    -转置
    -交换
    -提公因子
    -拆项
    -把一行/列*k +到另一行/列 行列式值不变（最常用）
行列式的计算
    -行列式按一行（列）展开
    -行列式按多行（列）展开
~~~

### WSL安装
将windows系统升至专业版，根据微软教程，发现直接通过命令安装失败之后，选择通过[旧版 WSL 的手动安装步骤](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual)一步一步操作，尤其是使用发行版下载，最终完成了WSL安装。

**WSL的迁移**
使用move命令执行迁移：
~~~
wsl --manage Ubuntu-24.04 --move D:\WSL\Ubuntu
~~~

第二天发现WSL磁盘损坏无法打开，选择重装WSL。可能是move命令迁移会导致磁盘损坏，因此改用用export和import操作，尚未发现故障：
~~~
wsl --export <发行版名称> <导出文件路径>

wsl --unregister <发行版名称>

wsl --import <发行版名称> <目标文件夹> <导出文件路径>
~~~

**用户登录**
默认登录为root用户，权限最高。
通过以下指令登录指定用户：
~~~
sudo su - <用户名>
~~~


### Git学习
**本地库操作**
创建本地库
~~~
git init
~~~
设置用户签名
~~~
git config user.name <name>
git config user.email <email>
~~~
查看本地库状态
~~~
git status
~~~
添加文件到暂存区
~~~
git add <文件名>
~~~
将文件移出暂存区
~~~
git rm --cached <文件名>
~~~
提交更改后的文件到暂存区
~~~
git commit -m "日志内容" <文件名>
~~~
查看更新日志
~~~
git log 
//最详细
git reflog
//最常用
git log --pretty=oneline
//单行显示
git log --oneline
//更简略的单行显示
~~~
跳到某历史版本
~~~
git reset --hard <哈希值>
~~~
比较文件差异
~~~
git diff <文件名>
//比较工作区和暂存区文件差异
git diff [历史版本] <文件名>
//比较工作区和某历史版本差异
~~~

**分支管理**
查看所有分支
~~~
git branch -v
~~~
添加分支
~~~
git branch <分支名>
~~~
切换当前分支
~~~
git checkout <分支名>
~~~
合并分支
~~~
//必须切换到接受修改的分支上（增加新内容的分支上）
git merge <另一分支名>
~~~
- **手动处理合并冲突**：通过文件编辑器处理冲突，使用 git add 提交，再用git commit -m "日志内容"（注意：不加文件名！）

**文件查看编辑操作**
查看文件夹内所有文件
~~~
ll
~~~
在终端查看文件
~~~
cat <文件名>
~~~
用vim创建/编辑文件
~~~
vim <文件名>
~~~
用vscode打开并编辑文件
~~~
code <文件名>
//注：此时终端必须在vscode中打开
~~~
删除文件
~~~
rm <文件名>
~~~

**远程库修改的拉取**
pull = fetch + merge
简单，不易出错的拉取：直接用pull
易发生冲突：fetch + merge

**跨团队协作远程库提交操作**
- fork到本地库
~~~
git clone <ssh码>
~~~
注意给予当前文件夹足够的权限，使用sudo会使得环境ssh出错而失败。
方法如下：
~~~
sudo chmod 777 /home/user/workspace/project
~~~

- 查看远程分支：
首先，你可以使用 git remote show origin 命令查看远程仓库的详细信息，包括远程分支的信息：
~~~
git remote show origin
~~~
该命令会显示远程仓库的 URL、远程分支、本地分支跟踪的远程分支等信息，你可以确认远程仓库确实存在多个分支。

- 获取远程分支到本地：
要将远程分支获取到本地并创建相应的本地分支，可以使用 git fetch 命令：

~~~
git fetch origin
~~~
git fetch 会从远程仓库拉取最新的提交和分支信息，但不会自动合并到当前分支。

- 查看本地分支列表（包括远程跟踪分支）：
执行 git branch -av 命令可以查看本地分支和远程跟踪分支的信息：
~~~
git branch -av
~~~
这个命令会列出本地分支和远程跟踪分支，你可以看到以 remotes/origin/ 开头的远程跟踪分支，这些分支是对远程分支的引用。

- 创建本地分支并跟踪远程分支：
如果你想创建一个本地分支来跟踪某个远程分支，可以使用 git checkout 命令，例如，如果你想跟踪 origin/feature-branch 远程分支：
~~~
git checkout -b feature-branch origin/feature-branch
~~~
这里 git checkout -b feature-branch origin/feature-branch 的意思是创建一个名为 feature-branch 的本地分支，并使其跟踪 origin/feature-branch 远程分支。

- 切换到该本地分支并pull
~~~
git pull origin <feature-branch>
~~~

- 在github上提交pull request等待审核



### STM32学习
没什么时间学 TT 下周补上
现在计划快速过一遍江科大的理论介绍然后转入hal库学习
本周学习了DMA相关知识

以上