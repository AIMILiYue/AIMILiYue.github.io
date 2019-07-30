## Git基本命令

### 1.库操作命令

```	git
git init // 初始化
git pull 远程仓库地址   //拉取远程仓库（需要执行git init）
git clone 远程仓库地址   //克隆远程仓库（不需要执行git init）
git add .  [or git add 指定文件]   // 将文件或文件夹添加到暂存区
git commit -m '描述信息'   // 将暂存区文件提交到当前分支工作区（仓库）
git push 远程仓库地址   // 将仓库文件推送到远程仓库

```

!> git是修改管理机制，文件的修改、删除、新增（统称修改），git都会跟踪，但是需要手动执行git add，
	然后再commit。如果不进行git add操作，文件修改的部分将无法被提交

看下面示例:

?> ①文件readme.txt，新增一行：hello world，执行git add readme.txt，使用git status查看;

?> ②再修改readme.txt---hello world2，不执行git add，直接git commit，提交后git status 查看文件状态;

?>【得到结果】: 只有第一次的修改，没有第二次的修改;
	
?>【回顾操作过程】: 第一次修改 -> git add -> 第二次修改 -> git commit
	
?>【原因】: git commit只会把暂存区的修改进行提交，第一次修改执行了git add会把修改提交暂存区，第二次修改没有执行git add 不会把修改提交到暂存区

?>【正确使用】: 先git add，再git commit



### 2.分支操作命令

```	git
git branch  // 查看分支
git branch 分支名  // 创建本地分支
git checkout 分支名   // 切换分支
git checkout -b 分支名   // 创建并切换到该分支
git merge 分支名   // 从当前分支上合并指定分支
git branch -d 分支名   //删除指定分支
```



### 3.状态撤销命令

```git
git checkout . [指定文件]   // 恢复暂存区所有文件[指定文件]到工作区
```



### 4.版本回退

```git
git log --pertty=online  // 查看版本日志（简略信息）
// 得到结果：
/* 
 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
 e475afc93c209a690c39c13a46716e8fa000c366 add distributed
 eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file 
 */

// 说明：1094adb7b... 是版本号，通过SHA1计算出来; HEAD 表示当前版本;
// 上一个版本：HEAD^;上上个：HEAD^^;
// 如果版本比较多，比如往上100个版本，可以使用 `HEAD~100` 来表示

git reset --hard HEAD^  // 回退到上一个版本

```
!> 注意问题：回退之后最新的版本会消失（可以使用git log查看）

如果回退之后想回到最新的版本，两种办法：

?> ①如果当前命令行没有关闭，在回退之前查看版本号时会有，可以找到最新的版本号进行回退

?>②如果关闭了命令行，可以使用 git reflog来查看我们执行过的所有的命令，可以找到最新的版本号，
然后进行回退



### 5.其他

```git
git status  // 查看变更文件
git log  [--pretty=oneline]   //显示从最近到最远的提交日志 [简略查看]
git remote   // 查看当前配置的远程仓库
```



## Git进阶命令



### 1.克隆远程仓库指定分支项目

```git
git clone -b dev address    // 克隆远程仓库dev分支
```



### 2.创建远程分支

```git
git checkout -b dev    //  创建本地dev分支
git push address  // 在本地创建的dev分支上push，远程仓库会自动生成一个新的同名dev分支
```



### 3.推送代码到远程仓库指定分支

```git
git remote add origin address   // 和远程仓库地址建立关联，并器别名叫做origin
git push -u origin dev:dev    // 将本地dev分支代码推送到origin对应仓库的dev远程分支上
```



### 4.使用git的一般流程

```git
git clone -b dev address  // 克隆远程仓库dev分支项目
git branch  // 查看当前所处本地分支【可选】
git checkout dev  // 如果本地分支不在dev分支上，可以切换到dev分支上【可选】
git add .   //添加到暂存区
git commit -m 'description'  // 提交到工作区
git remote add origin address  //建立关联，起别名-origin
git push -u origin dev:dev  //推送本地dev分支项目到远程dev分支

```

[学习网站: 廖雪峰的官方网站-Git教程](https://www.liaoxuefeng.com/wiki/896043488029600 ':target=_blank')
