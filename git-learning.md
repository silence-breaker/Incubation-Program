# 入门git基本指令
## 学习网站
[不错的git指令B站学习教程]（[15.尚硅谷_Git&GitHub_查看历史记录的几种不同方式_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1pW411A7a5?share_source=copy_web&vd_source=34ed110d766ac5910b35ccc9afedda6e&spm_id_from=333.788.player.switch&p=15)）
## 几个语句的基本关系
![test](origin/基本架构.png)
![test](origin/基本操作.png)
### 查看指令使用
`git help [语句]`

### 查看目录的方式
`git log `


最细


`git log --pretty=oneline 或者git log --oneline `


以一行的形式呈现目录


`git reflog`

 
会显示历史步骤 [索引值]HEAD@{移动到当前版本需要多少步}
![test](origin/git reflog显示.png)
### 版本控制
`git reset --hard [索引值]` 


可以直接跳转到索引值对应的状态
![test](origin/使用reset历史回退.png)
![test](origin/reset的三个参数对比.png)

## 分支管理
版本库创立好后会有main分支(默认)
![test](什么是分支.png)]
1. 查看分支 


  `git branch（带星号为当前所在分支) `
2. 创建分支 
 

  `git branch [分支名]`
3. 切换分支 
  

  `git checkout [分支名]`
### 合并分支 
1. 切换到被合并的分支上
2. `git merge [想要添加的改动内容的所在分支]`
### 处理分支冲突
当在两个分支同时修改同一个文件并进行了commit后合并分支会失败，产生冲突
![test](origin/不同分支合并时出现矛盾.png)
<<<<<<< HEAD 和 ======= 围起来的是当前分支的改动内容


剩下的是另一个分支的改动内容（和>>>>>>>main围起来）

---
1. git 拿不定主意，让你来手动修改，这时候直接对目标文件进行修改（删掉特殊字符）然后保存退出即可
2. `git add [文件名]`
3. `git commit -m "日志内容"`
（注意此处不能带文件名）*特殊之处*


至此冲突解决
## git的文件管理机制

![test](origin/git的文件管理机制.png)
## 创建远程仓库
在github上面完成操作即可，非常简单
## 将本地库上传到远程库
`git push [地址（别名）] [想推送到的远程库的分支]`
#### 地址别名的建立
1. `git remote -v`查看地址别名
2. `git remote add [地址别名] http/SSH地址`
### 克隆
`git clone [http/SSH地址]`

---

## 团队协作
如果是团队外部成员是无法将本地仓库推送到github远程仓库的


那么邀请外部成员加入团队如何操作？
1. 进入远程仓库点击settings，点击Collaborators，点击Add people
2. 接着输入对应的github账号信息 
3. 复制邀请链接，发送给他/她，那个人accept就可以了
### 拉取
pull = fetch + merge
1. `git fetch [远程仓库地址别名] [远程分支名]`
*注意fetch并不会改变本地内容，只是下载了而已，需要合并才能体现*
2. `git merge [远程仓库地址别名/远程分支名]`
fetch+merge太麻烦怎么办
3. *那么直接*
`git pull [远程仓库地址别名] [远程分支名]`
### 协同开发时冲突的解决
当多个人对同一个文件进行修改后，会导致文件冲突


不是最新版就不能push了


这时候解决冲突的方法和本地库不同分支解决commit冲突方法一样


修改完文件后直接git push即可
## 跨团队协作操作
### 对于外部人员

![test](基本架构.png)
1.  fork之后得到自己账号下所在仓库，并git clone该仓库，可在本地进行任意修改拉取
2. 更改完后点击pull request，请求合并
![test](origin/pull request界面.png)
![test](origin/create pull request.png)
3. 选择你要被合并的原远程库目标分支，与被合并的fork后的仓库分支，然后create
### 对内部人员
1. 接受到pull request后可查看具体信息
   

   conversation用于讨论
   

   commits，Files changed用于查看具体文件信息
![test](内部人员查看pull request场景.png)
2. 确认无误后点merge即可合并代码
3. 然后再pull到本地

















