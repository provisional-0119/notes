## git仓库和git路径

我们操作的是，git路径下的文件，而git仓库并不是我们手动操作的，而是通过提交将改变存入仓库

## 分支

1. 对于同一git路径下的不同分支，文件内容不同，选择不同分支时会动态修改当前git路径下的文件内容

2. 当同一git路径下的不同分支提交了相同文件名的添加修改操作，需要进行手动选择

3. 分支创建前的内容会保存到当前分支

## 标签

对于合并操作没有文字说明，可以右键`create tag`添加标签

## 忽略文件

比如要忽略.bak结尾的文件，在.gitignore文件中添加*.bak

## 在Git Bash Here中通过版本号解析文件信息

使用命令格式 `git cat-file -p 版本号`

#### 创建仓库

git版本号对应内容中存在格式

```git
tree 6f9509c88bed7080d496fc5e1d87a9315e30549d
author xjs <2090794185@qq.com> 1693706763 +0800
committer xjs <2090794185@qq.com> 1693706763 +0800
```

第一行中6f9509c88bed7080d496fc5e1d87a9315e30549d对应另外一个文件

文件内容为

```
100644 blob dfe0770424b2a19faf507a501ebfc23be8f54e7b    .gitattributes
```

这里面又有个版本号dfe0770424b2a19faf507a501ebfc23be8f54e7b，对应内容如下，这是git版本号对应的真正内容

```
# Auto detect text files and perform LF normalization

* text=auto
```

#### 新增和修改文件

对于新增文件的操作的版本号对应内容

```
$ git cat-file -p f75be04c168fbdf03a840cb249d5589a023d4325
tree a6003ea2b8a2682bf69e1d77cf20ab0d65e17b54
parent 26a132537e694ffc476fcafeca76619b7a252599
author xjs <2090794185@qq.com> 1693707772 +0800
committer xjs <2090794185@qq.com> 1693707772 +0800

Create a.txt
```

其中parent指向上一次的操作的版本号

tree指向当前的信息，其中的内容包括.gitattribute和a.txt的版本号

修改操作的版本号内容和新增相似

#### 删除文件

删除操作版本号tree只指向了初始提交时的.gitattribute

#### 多个文件管理

当有多个文件被管理时，修改某些文件，只会修改这些文件的版本号，其他文件的版本号保持不变，仍被tree指向。

也就是说，tree的内容为：.gitattribute和当前版本所有文件对应的版本号

#### 分支

当创建分支时，实际上是在\.git\refs\heads目录下添加文件

文件中指向分支当前的最新版本号

那么怎么才能知道当前的分支是哪个？

​	在.git\HEAD文件中指明了当前分支的文件（即上面heads中的文件）

## Git指令

#### Git版本

`git -v`

#### 初始化

`git init`

比使用工具创建少了个 `.gitattribute`

默认主分支为 `master`

工具创建的会默认进行一个Initial Commit的提交，所以会生成`.gitattribute` 和`objects`中的文件

#### 克隆命令

`git clone <路径> [仓库名称]`

####  配置名称和邮箱

局部仓库配置：`git config user.name <名称>`

​							`git config user.email <邮箱>`

仓库配置的位置在：\\.git\config

全局配置：`git config --global user.name <名称>`

​					`git config --global user.email <邮箱>`

全局配置的位置在‪C:\Users\15946998816\.gitconfig

#### 添加到暂存区

`git add <文件名>`      (文件名可以使用通配符，比如*.txt)

#### 从暂存区中删除

`git rm --cached <文件名>`

#### 提交到存储区域

`git commit -m <附带信息>`

#### 显示当前分支的提交历史

`git log [--oneline]`

添加oneline参数后就可以只显示前七位版本号

#### 恢复文件

`git restore <file>` :恢复存储空间的文件内容到工作空间

`git reset --hard <commit>` : 恢复工作空间和存储空间到指定提交的版本, 会丢弃指定提交后的提交

`git reset [--soft] <commit>` : 恢复存储空间到指定提交的版本, 会丢弃指定提交后的提交, 不修改工作空间

`git revert <commit>`: 恢复工作空间和存储空间到指定提交的版本，保留指定提交后的提交

#### 分支操作

1. 创建分支：`git branch <branch-name>`
2. 查看分支：`git branch -v`
3. 切换分支：`git checkout <branch-name>`
4. 创建并切换分支：`git checkout -b <branch-name>`
5. 删除分支：`git branch -d <branch-name>`

注意：创建分支前，主分支要先创建，删除分支前，分支不能在使用状态

#### 合并分支

`git merge <branch>`

合并出现冲突时，对合并文件进行手动处理，然后添加提交即可。

#### 增加标签

`git tag <tag-name> <commit>` 

标签可以代替版本号（比如`git log <commit>`的commit可以使用标签代替），所以标签必须唯一

#### 远程仓库指令

1. `git clone <remote-url>`：克隆远程仓库到本地。

2. `git remote add <remote-name> <remote-url>`：添加一个新的远程仓库，并为其指定一个名称。

3. `git remote rename <old-name> <new-name>`：将指定的远程仓库重命名。

4. `git remote -v`：查看当前配置的远程仓库列表及其URL。

5. `git pull <remote-name> <branch-name>`：从指定的远程仓库和分支拉取最新的提交，并将其合并到当前分支。

6. `git push <remote-name> <branch-name>`：将当前分支的提交推送到指定的远程仓库和分支。

7. `git remote remove <remote-name>`：从本地仓库中移除指定的远程仓库。

    

