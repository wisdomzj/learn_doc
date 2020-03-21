# git基本使用（仅供参考）

[TOC]



### 本地库初始化

1. 命令：`git init`

2. 演示图：

![1548593963343](http://demo.bestzhangjun.com/imgs/git/1.png)

3. 注意：.git 目录中存放的是本地库相关的子目录和文件，慎重修改或删除。



### 设置签名

1. 形式：

   name：zhangjun 

   email：2556105535@qq.com

2. 作用：区分不同开发人员的身份

3. 注意：这里设置的签名和登录远程库(代码托管中心)的账号、密码没有任何关系。

4. 命令：

   + 项目级别/仓库级别：仅在当前本地库范围内有效(类似局部变量) 

     `git config` user.name zhangjun 

     `git config` user.email 2556105535@qq.com 

     信息保存位置：./.git/config 文件

     ![1548595583872](http://demo.bestzhangjun.com/imgs/git/2.png)

   + 系统用户级别：登录当前操作系统的用户范围 (类似全局变量)

     `git config --global` user.name zj 

     `git config --global` user.email 2556105535@qq.com 

     信息保存位置：~/.gitconfig 文件

     ![1548595835203](http://demo.bestzhangjun.com/imgs/git/3.png)

     ​                                 

### git命令基本操作

1. 查看状态

   `git status`  查看工作区、暂存区状态 

   ![1548597069218](http://demo.bestzhangjun.com/imgs/git/4.png)

2. 添加 

   `git add [filename]` 将工作区的“新建/修改”添加到暂存区

   ![1548654660042](http://demo.bestzhangjun.com/imgs/git/5.png)

3. 提交

   `git commit[filename] -m "commitmessage" ` 将暂存区的内容提交到本地库

   ![1548598735451](http://demo.bestzhangjun.com/imgs/git/6.png)

4. 查看历史纪录

   `git log`(多屏显示控制方式： 空格向下翻页      b 向上翻页     q 退出)

   ![1548601029601](http://demo.bestzhangjun.com/imgs/git/7.png)

   `git log --pretty=oneline` 格式化历史记录

   ![](http://demo.bestzhangjun.com/imgs/git/8.png)

   `git log --oneline` 简短格式历史记录

   ![1548600930691](http://demo.bestzhangjun.com/imgs/git/9.png)

   `git reflog`（HEAD@{需要移动多少步}）

   ![1548600952302](http://demo.bestzhangjun.com/imgs/git/10.png)

5. 前进后退

   - 使用索引值 [推荐]

     命令：`git reset --hard [版本号]`

   - 使用^符号（只能往前退 不能往前进 步，n 个表示后退 n 步）

     命令：`git reset --hard HEAD^`

   - 使用~符号（表示后退 n 步）

     命令：`git reset --hard HEAD~n`

6. reset 命令的三个参数对比

   - --soft 参数 （仅仅在本地库移动 HEAD 指针）
   - --mixed 参数（在本地库移动 HEAD 指针  重置暂存区）
   - --hard 参数 （本地库移动 HEAD 指针 重置缓存区 重置工作区）

7. 找回删除文件

   - 前提条件

     删除前，文件存在时的状态提交到了本地库，否则无法找回。

   - 删除记录已提交版本库

     操作：`git reset --hard [倒退到文件存在状态的历史版本号]`

   - 删除记录未提交版本库（在暂存区）

     操作：`git reset --hard HEAD`

8. 比较文件差异

   - 将工作区中的文件修改前后进行比较 `git diff [文件名]` 
   - 将工作区中的文件和本地库历史记录比较 `git diff HEAD [版本号] [文件名称]`
   - 不带文件名比较多个文件 `git diff`



### 分支管理

1. 分支概念

   > 在版本控制过程中，使用多条线同时推进多个任务。

2. 分支的好处

   > （1）同时并行推进多个功能开发，提高开发效率 。
   >
   > （2）各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任 何影响。失败的分支删除重新开始即可。

3. 分支操作

   - 创建分支 `git branch [分支名] `     

   - 查看分支 `git branch -v`

   - 切换分支 `git checkout [分支名]`

   - 合并分支

     第一步：`git checkout [接受修改的分支名]`

     第一步：`git merge [有新内容的分支名] ` 

4. 解决分支合并文件冲突问题

   > 第一步：编辑文件，删除特殊符号 。
   >
   > 第二步：把文件修改到满意的程度，保存退出 。
   >
   > 第三步：`git add [文件名] ` 
   >
   > 第四步：`git commit -m "日志信息"`  （注意：此时 commit 一定不能带具体文件名） 



### github远程库交互

1. 注册github账号（前提：需要一个邮箱 阿里邮箱 qq邮箱都行 ）

   ![1549030427008](http://demo.bestzhangjun.com/imgs/git/11.png)

2. 创建远程库

   ![1549030895693](http://demo.bestzhangjun.com/imgs/git/12.png)

3. 创建远程库地址别名

   命令：

   ​	`git remote -v [查看当前所有远程地址别名]  `

   ​	`git remote add [别名] [远程地址] `

   演示图：

   ![1549032926957](http://demo.bestzhangjun.com/imgs/git/13.png)

4. 推送

   命令：`git push [别名] [分支名] `

   演示图：

   ![1549033198721](http://demo.bestzhangjun.com/imgs/git/14.png)

5. 克隆

   命令：`git clone [远程库地址]` 

   演示图：

   ![1549033317873](http://demo.bestzhangjun.com/imgs/git/15.png)

6. 团队成员邀请（以组织者发出邀请链接）

![1549034471401](http://demo.bestzhangjun.com/imgs/git/16.png)

7. 加入团队（团队成员登陆个人github账号复制邀请链接点击加入团队）

   ![1549035261724](http://demo.bestzhangjun.com/imgs/git/17.png)

8. 拉取
   + `git fetch` 远程库地址别名 远程分支名 （保险起见先进行查看）
   + `git merge` 远程库地址别名/远程分支名（查看无误后在替换文件）
   + `git pull` 远程库地址别名 远程分支名（pull == fetch + merge）

9. 解决冲突

   > （1）如果不是基于 GitHub 远程库的最新版所做的修改，不能推送，必须先拉取。
   >
   > （2）拉取下来后如果进入冲突状态，则按照“分支冲突解决”操作解决即可。

10. 跨团队协作流程

    + 复制远程库（以另一个团队的账号登陆使用远程库地址进行fork操作）

      命令：git fork

      演示图：

      ![1549367549561](http://demo.bestzhangjun.com/imgs/git/18.png)

    + 远程库复制完成后复制后的远程库属于另一个团队 （本地修改，然后推送到远程 ）

    + 向对方远程库发出请求（pull requsets）

      演示图：

      ![1549368517575](http://demo.bestzhangjun.com/imgs/git/19.png)

    + 新建请求

      演示图：

      ![1549368852471](http://demo.bestzhangjun.com/imgs/git/20.png)

    + 进行对话

      演示图：

      ![1549369378909](http://demo.bestzhangjun.com/imgs/git/21.png)

    + 审核代码

      演示图：

      ![1549369485506](http://demo.bestzhangjun.com/imgs/git/22.png)

    + 合并代码

      演示图：

      ![1549369716008](http://demo.bestzhangjun.com/imgs/git/23.png)

    + 将远程库修改拉取到本地库

11. ssh免密登陆

    + 进入当前用户的家目录 `cd ~`

    + 删除.ssh目录 （有此目录进行删除操作无则忽略）`rm-rvf.ssh`

    + 运行命令生生成.ssh密钥目录（注意这里-C 这个参数是大写的 C ）

      命令： `ssh -keygen -trsa -C [邮箱地址]` 

      演示图：

      ![1549361033502](http://demo.bestzhangjun.com/imgs/git/24.png)

    + 进入.ssh 目录查看文件列表  

      命令：`cd .ssh`  `ls -ll `

      演示图：

      ![1549361479019](http://demo.bestzhangjun.com/imgs/git/25.png)

    + 查看 id_rsa.pub 文件catid_rsa.pub内容

      命令：`cat id_rsa.pub`

      演示图：

      ![1549361546085](http://demo.bestzhangjun.com/imgs/git/26.png)

    + 复制 id_rsa.pub 文件内容，登录 GitHub，点击用户头像→Settings→SSHandGPG keys→New SSHKey  输入复制的密钥信息→生成密钥信息

      ![1549361746575](http://demo.bestzhangjun.com/imgs/git/27.png)

    + 回到 Gitbash 创建远程地址别 

      命令：`git remote add [别名][ssh地址]`

      演示图：

      ![1549362646059](http://demo.bestzhangjun.com/imgs/git/28.png)

    + 推送文件进行测试

      演示图：

      ![1549362865991](http://demo.bestzhangjun.com/imgs/git/29.png)

