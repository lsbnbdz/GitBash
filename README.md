# GitBash
Summary of common git commands.
本文介绍一些常用的git命令，虽然IntelliJ IDEA中有非常好用的git插件，但本人在开发过程中还是习惯使用命令操作，在此做一些汇总。
# 仓库管理

## 初始化本地仓库
我们可以选择用命令在本地初始化一个Git仓库：
```shell
# 初始化本地仓库
git init
```

------------


## 克隆远程仓库
Git拥有一个中央远程版本库，每个开发者都可以把中央版本库克隆到本地进行开发，即使断网也可以进行提交代码，每一个克隆体都是一个本地仓库。
```shell
# 克隆远程仓库至本地
git clone [remote_url]
```
链接可以选择 **`HTTPS URL`** ，**`SSH URL`** ，此处以 **`HTTPS URL`** 链接 **`https://github.com/yankils/hello-world.git`** 为例：
```
$ git clone https://github.com/yankils/hello-world.git
Cloning into 'hello-world&'...
remote: Enumerating objects: 242, done.
remote: Total 242 (delta 0), reused 0 (delta 0), pack-reused 242 eceiving object
Receiving objects:  54% (131/242)
Receiving objects: 100% (242/242), 29.55 KiB | 111.00 KiB/s, done.
Resolving deltas: 100% (51/51), done.
```

------------


## 查看远程仓库
我们可以用命令查看本地关联哪些远程仓库：
```shell
# 查看仓库
git remote -v
```

------------


## 添加和移除仓库
在初次克隆远程仓库是，默认的仓库名为 **`origin`** ，我们可以选择添加其他仓库：
### 添加仓库
```shell
# 添加远程仓库
git remote add [remote_name] [remote_url]
```
### 移除仓库
```shell
# 移除仓库
git remote rm [remote_name]
```

------------


## 仓库重命名

我们可以选择重命名本地仓库：
```shell
# 仓库重命名
git remote rename [old_remote] [new_remote]
```

------------


## 仅拉取远程仓库
仅拉取远程仓库不会自动合并代码：
```shell
# 仅拉取远程仓库
git fetch [remote_name]
```

------------


## 获取仓库所有远程分支信息
有两种获取仓库所有分支的命令，一种是下面这种，仅获取远程仓库所有分支信息，包括最新提交 **`commitid`** 和远程分支名：
```shell
# 获取远程仓库所有分支信息
git ls-remote [remote_name]
```
另一种是可以获取更多信息，包括所有远程分支名，本地 **`git pull`** 和 **`git push`** 的分支信息：
```shell
# 获取仓库分支信息
git remote show [remote_name]
```

------------

# 拉取和推送
若使用 **`https`** 协议,每次在进行 **`git clone`** ，**`git fetch`** ，**`git pull`** ，**`git push`** 时，会验证用户名和密码，十分的不方便，在此推荐使用 **`ssh`** 协议连接，使用 **`ssh`** 协议，您可以连接到远程服务器和服务并进行身份验证。[配置 **`ssh`** 密钥](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh "配置 **`ssh`** 密钥")，您可以连接到GitHub，而无需在每次访问时都提供用户名和个人访问令牌。在完成 **`ssh`** 配置后就可以愉快的拉取和推送代码了。

## 拉取代码
配置完成后，在初次使用 **`git clone`** 克隆远程仓库完成初始化后就可以使用 **`git pull`** 拉取代码了，此处是远程仓库没有改动，已是最新：
```
$ git pull
Already up to date.
```
默认 **`git pull`** 是 **`git fetch`** 和 **`git merge`** 的合体，**`git fetch`** 是仅将远程代码拉取到本地仓库并未进行合并。
```shell
# 仅拉取到本地，不合并
git fetch
# 拉取到本地且合并
git pull
# 推送
```

------------


## 合并代码
合并代码使用 **`git merge [branch]`** ，在完成开发测试后，需要将开发分支 **`dev_branch`** 合并到 **`master`** 分支，此时切换到 **`master`** 分支后使用 **`git merge dev_branch`** 后即可完成合并，若其他人同时修改了相同的代码，此时会报冲突，在手动解决冲突代码文件后 **`git add [conflict_file]`** + **`git commit`** 即可完成合并。
```shell
# 合并代码
git merge [branch]
# 手动解决冲突后
git add [conflict_file]
git commit
```

------------


## 推送代码
在完成 **`git commit`** 或 **`git merge`** 后需要将本地代码推送到远程仓库可以使用 **`git push`** ，若分支为在本地创建，第一次推送到服务器则需使用 **`git push --set-upstream origin [branch]`** 。
```shell
# 推送云端已存在分支
git push
# 初次推送本地分支
git push --set-upstream origin [branch]
```

------------


## 本地覆盖云端
若想本地代码强制覆盖远程仓库分支则需要加参数 **`-f`** -> **`git push -f`** ，需要谨慎使用，虽然服务器可以找回覆盖前代码，但是得不偿失。
```shell
# 本地强制覆盖云端
git push -f
```

------------


## 云端覆盖本地
当想使用云端代码覆盖本地时，可以使用以下组合技：
```shell
#下载云端仓库到本地,并不做合并
git fetch --all
#将本分支指向云端
git reset --hard origin/[branch]
```

------------

# 分支管理
在日常开发过程中会同时进行多个需求的开发，需要创建不同的分支，接下来介绍如何创建新分支，切换分支，查看本地或云端分支等操作。
## 创建新分支
我们以 **`master`** 为基础分支创建新分支:
```shell
# 创建开发分支 new_branch
git branch [new_branch]
```

------------


## 切换分支
切换分支比较简单，只需一个命令，切换分支前要先本地提交已修改的代码才能切换成功：
```shell
# 切换分支
git checkout [branch]
```

------------


## 本地创建跟踪分支
在本地创建跟踪分支可以重新命名，也可以选择跟云端分支名保持一致，但两者在推送到云端时略有不同。
### 分支名一致
下为检出分支名与云端一致：
```shell
# 分支名与云端保持一致,以下两条命令达到的效果相同
git checkout --track origin/[branch]
git checkout -b [branch] origin/[branch]
```
推送代码至云端只需：
```shell
# 推送代码
git push
```
### 分支名不一致
下为检出分支名与云端不一致：
```shell
# 分支名与云端不同
git checkout -b [branch_dev] origin/[branch]
```
此时将本地代码同步到云端有两种选择，推送到本地分支所跟踪的云端分支上：
```shell
# 推送代码
git push origin HEAD:[branch]		#推送到所跟踪云端分支
```
推送代码至与本地分支名相同云端分支上，若第一次推送会在云端新建分支：
```shell
# 推送代码
git push origin HEAD			#推送到与本地分支名一致到云端，第一次会在云端创建
```

------------


## 设置当前分支跟踪远程分支
以当前分支跟踪远程分支,以下两种命令都可以：
```shell
git branch -u origin/[branch]
git branch --set-upstream-to origin/[branch]
```
在对改动代码推送到云端时，同样需要指定推送到所跟踪分支还是与本地分支名相同的云端分支上。

------------


## 查看本地分支
查看本地分支跟踪哪个云端分支可以用以下命令：
```shell
# 查看跟踪分支信息
git branch -vv
```
单纯看本地分支可以用以下命令，与前者比较仅少了跟踪分支信息：
```shell
# 查看本地分支信息
git branch -v
```

------------


## 删除分支
当一个分支不需要时，我们可以选择删除分支，可以删除本地和云端分支。
### 删除本地分支
```shell
# 删除本地分支
git branch [-D|-d] [branch]
```
在删除分支时参数选择 **`-D`** 时可以强制删除修改后未合并的分支，**`-d`** 则不可以，我们在删除是优先使用后者，防止误删。

### 删除远程分支
我们也可以选择删除远程分支，但是要慎重，虽然可以找回，但还是少增加不必要的麻烦：
```shell
# 删除远程分支
git push origin --delete [branch]
```
------------

# 提交管理
在修改代码提交的过程中经过两步，第一步需要使用 **`git add`** 命令把修改的文件加入缓存区，第二步要使用 **`git commit`** 命令太提交至本地git仓库。

## 查看当前状态
我们在提交代码之前应该查看当前分支在本地的状态，为确保所有改动都已提交：
```shell
# 查看本地分支状态
git status [-s]
```
使用本命令可以查看哪些改动还未添加至缓存区，哪些改动添加至缓存区还未提交，**`-s`** 参数添加后会显示简短信息。

------------


## 加入缓存区
```shell
# 添加至缓存
git add [modify_file]
```

------------


## 提交本地仓库
### 新增提交
当我们将改动添加至缓存区时，之后就可以提交本地仓库了：
```shell
# 提交仓库
git commit -m ''
```
提交仓库时可以选择在两个单引号 **`'`** 之间添加备注信息。
### 补充提交
当我们做了小改动不想再新增一次提交记录时，我们可以选择使用以下命令做补充提交，不会新增提交记录：
```shell
# 补充提交
git commit --amend
```
### 添加并提交
正常提交需要两个步骤，先add到缓存区，然后commit提交，使用以下命令可以合并两步：
```shell
# 添加并提交
git commit -a -m ''
```
### 删除命令
#### 从工作目录删除
在使用 **`git`** 开头的删除命令时，后接不同的参数会有不一样的效果，下列命令会直接删除工作目录中的文件：
```shell
# 删除工作目录中文件
git rm [rm_file]
# 组合技，同上条命令
rm [rm_file]
git add/rm [rm_file]
```
删除后我们在未提交之前可以使用以下命令恢复文件至缓存区：
```shell
# 恢复文件
git restore --staged [rm_file]
```
恢复至缓存区后，我们可以使用以下命令撤销删除：
```shell
git restore [rm_file]
```
#### 从缓存区删除
从缓存区删除，不删除工作目录，相当于撤销跟踪：
```shell
# 撤销跟踪
git rm --cached [untracked_file]
```
### 移动文件
移动文件同样有两组命令，下俩两组可以达到同样的效果：
```shell
# 移动文件
git mv [old_file] [new_file]
# 下列组合技可达到同样效果
mv [old_file] [new_file]
git rm [old_name]
git add [new_file]
```

------------


## 放弃本地修改
在修改代码时我们可以随时放弃整个文件的修改，放弃本地文件修改有三种，一种是添加到了缓存区，另一种是还未添加至缓存区，第三种则是放弃整个提交。
### 添加至缓存区
```shell
# 放弃缓存区修改
git checkout -- [modify_file]
```

### 未添加至缓存区
```shell
# 放弃未添加至缓存区修改,两者等价
git reset HEAD [modify_file]
git reset HEAD -- [modify_file]
```

### 放弃提交记录
git支持我们向前跳转至某个提交记录节点，每一次提交会有一个唯一的提交ID，根据commitid我们可以随时向前跳转，放弃提交记录。
#### 查看commitid
```shell
# 查看提交记录
git log [-p]
```
参数 **`-p`** 可选，加上则会显示提交前后修改明细，提交记录中可看到commitid。

#### 回滚提交记录
```shell
# 回滚提交记录
git reset --hard [commitid]
```

------------

## 暂时存储（stash命令）
在日常开发过程中，我们会经常切换分支，如果我们在切换分支时不想进行提交，我们就可以使用 **`stash`** 命令，待切回分支后再使用命令恢复，注意执行 **`git stash`** 与 **`git add/rm`** 命令所处的状态不同。
### 执行存储
只有被添加到暂存区文件的修改才会在执行暂存命令时生效，新增还处于 **`untracked`** 状态的文件是不会生效的。执行存储命令如下：
```shell
# 暂时存储命令
git stash {save ''}
```
大括号内的内容可以省略，但我们不建议省略，如果不加备注描述的话，在我们添加了多条暂存时，我们可能会我们分不清每条记录都修改了什么。
### 查看存储记录
查看存储记录有两种，一种是之查看最近一条暂存，另一种是查看所有记录：
```shell
# 查看最近一条
git stash show {-p}
# 查看所有记录
git stash list {-p}
```
大括号中的参数可以省略，加上此参数会显示具体暂存修改的明细。
### 应用存储
当我们想恢复存储时，可以用以下命令执行恢复，在执行恢复命令时有两种方式，一种是清除暂存记录，另一种不清除：
```shell
# 不清除记录
git stash apply stash@{$num}
# 清除记录
git stash pop stash@{$num}
```
**`$num`** 为从0开始的整数，在我们查看存储记录是可以看到的。
### 丢弃记录
我们可以选择丢弃某一个记录：
```shell
# 丢弃某一记录
git stash drop stash@{$num}
```
也可以选择清除所有记录：
```shell
# 丢弃所有记录
git stash clear
```

------------
>**本文作者**：[lsbnbdz](http://www.lsbnbdz.top "lsbnbdz")
>**版权声明**：本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/ "CC BY-NC-SA 4.0") 许可协议。转载请注明出处！
