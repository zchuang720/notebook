# GIT Commands

## 全局设置
设置账号
```
git config --global user.name "zchuang720"
git config --global user.email "13392406082@163.com"
```

网络代理
```
git config --global http.proxy "http://127.0.0.1:7890"
git config --global https.proxy "http://127.0.0.1:7890"
```

取消代理
```
git config --global --unset http.proxy
git config --global --unset https.proxy
```

## 创建仓库
新建仓库
```
mkdir test
cd test
git init
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin https://gitee.com/zhichengh/test.git
git push -u origin "master"

```
上传已有仓库
```
cd existing_git_repo
git remote add origin https://github.com/zchuang720/repository.git
git push -u origin "master"
```

## 分支提交
```
git add .
```

## 冲突解决
### VSCODE
1. 在 git tree 中 staged changes 和 changes 一级菜单上点击 **stash all changes**，保存现场
2. pull 远程分支
3. pop stash 选择变更进行 merge


# 详细文档
<details>
<summary>点击查看更多内容</summary>

### 拉取仓库
克隆远程仓库，自动关联远程仓库地址，默认远程仓库简称或别名为origin：

    git clone [repository-url]

修改远程仓库别名（可选）; 先移除，重新添加：

    git remote remove [origin]
    git remote add [upstream] [repository-url]

### 新建分支

一般接到开发需求以后，基于本地master分支新建一个分支作为本次MR（Merge Request）的分支。推荐新建分支之前可以先更新本地master分支, 基于最新master分支来新建分支，更新方式：

- 拉取远程仓库的更新：

        git fetch origin

- 如果只想拉取远程指定分支(可选)：

        git fetch origin [remote-branch-name]

- 合并：

        git rebase origin/master 或者 git merge origin/master

    其中，origin为远程仓库简称，查看远程仓库信息：

        git remote -v
        git remote -a

新建分支，如果新建之前本地工作区有修改，会把修改带过去：

     git checkout -b [new-branch-name]

如果想在本地新建一个和远程某个分支一致的分支(可选)：

    git checkout -b [local-branch-name] origin/[remote-branch-name]

### 提交MR
在本次分支开发完成以后，三步提交MR到远程：

    git add . 
    git commit -m '[commit message]'
    git push origin [new-branch-name]

之后通过终端的链接到浏览器，填写相关信息后提交合并到master分支的MR（链接打不开可以直接到仓库对应分支新建MR）

第三步推送分支到远程中，命令完整写法如下，含义为把本地 [new-branch-name] 分支关联到远程仓库的 [new-branch-name] 分支，简写后默认同名：

    git push origin [new-branch-name]：[new-branch-name]

### 更新MR

在你的MR提交之后、合并之前，别人随时会更新master的代码（往master push代码或合并MR到master），之后你的MR就会落后于最新master；合并MR之前需要更新master最新commit记录到你的MR，在本地操作：

    git fetch origin
    git rebase origin/master或git merge origin/master
    git push origin [new-branch-name]

如果本地当前工作区有变更，但是需要你更新远程MR，你可以用 git stash 暂存变更，更新你的分支推到远程以后，git stash pop 放回你的变更，继续开发。

### 冲突解决

更新MR的时候，如果你当前分支和master最新代码有一起修改的文件或代码块，可能发生冲突，D当中第二步合并的时候就会合并（merge或rebase）失败。此时需要解决冲突后，再走类似C的流程（rebase方式不一样）。更新MR的时候，如果对rebase不熟悉可以使用merge；使用rebase的时候，会修改commit记录，所以如果冲突了，会基于每个commit做冲突处理；比如master最新有三个commit更新，如果存在冲突，解决冲突的时候，处理完第一个commit的冲突，用

    git rebase --continue

操作继续尝试合并，再进行后续2个commit的处理，之后的commit如果没有冲突就会合并成功。（可以了解下merge和rebase的区别）。
解决代码冲突的时候，需要具体看情况; 如有必要可以和Commiter沟通， 防止勿删除别人的代码。

### 多分支开发
在你开发一个需求的过程中，比如你当前在[new-branch-name]做开发，此时可能需求暂停这个需求开发， 先做另一个优先级高的任务，此时可以通过 
git checkout master 切换到master，基于master分支新建另一个任务的分支去做开发。
切换分支的时候，如果你当前分支还有没有提交的代码，也就是的工作区不是干净的，同时你切换的目标分支和当前分支的工作区的变更有冲突，就会切换失败（Aborting），提示你先提交或暂存后再切换分支。
这个时候有可以有几种选择：
- 如果这些修改是当前这个任务的，可以类似C中的操作，提交代码之后切换；
- 这些修改你不想要了，选择撤回，可以通过checkout（变更位于工作区）或reset（变更位于暂存区），之后工作区干净，切换成功；
- 你想把当前的变更（可能是新增的文件）带到下一个分支，可以 
git stash ,把代码暂存，之后切换到master，新建你的分支，
git stash pop 再把暂存的分支放回到工作区。

### 打标签（tag）
Git支持对Commit记录打标签，方便快速找到特定的提交记录；打标签相当于一个指向特定commit的指针，标签的删除、添加不影响commit记录。如果单纯为了记住特定的一次提交，可以添加轻量级tag；另外一种常见场景是使用tag来发布版本。
添加tag：
git tag -a [tag_name] -m [description]-a参数后制定本次tag的名称（版本号），-m参数后可以添加本版本的更新说明等注释信息。

查看所有存在的tag：

    git tag 
    git tag -l [pattern] 添加-l参数可以使用通配符过滤出特定的tag名称。

默认tag是打在当前最新的commit记录上，可以针对特定历史commit打tag：

    git tag -a [tag_name] [commit_id]

查看特定tag的详情：

    git show [tag_name]

默认push代码的时候不会push tag到远端，需要主动push tag：
推送特定tag ： 
    
    git push [origin] [tag_name]

推送所有tag： 
    
    git push origin --tags

删除tag：
 
    git tag --delete [tag_name]

在线上使用特定版本代码的时候，我们想切换到对应版本，执行checkout切换到tab对应的commit记录处:

    git checkout [tab_name]

此时会出现 `You are in 'detached HEAD' state` 的提示，表示你不在任何分支；如果你只是使用代码，不做任何修改，可以不做处理；如果想给予此位置处做变更，或单纯处于一个分支，可以执行:

    git checkout -b [new-branch-name] [tag_name]

tip: 详情可以参考 checking-out-git-tag-leads-to-detached-head-state

### 其他

删除本地分支：
    
    git branch -d [local-branch-name]

删除远程分支：
    
    git push origin :[remote-branch-name]

暂存代码：
    
    git stash 当前的工作区内容保存到Git栈中
    git stash list 查看暂存列表
    git stash pop 读取最近一次保存的内容
    git stash pop [stash@{1}] 恢复指定的暂存到工作区

一般开发中不会用到暂存很多工作区的场景。

回撤Commit记录：

提交一个commit记录以后，邮箱没有配置、推送失败、需要修改Commit邮箱等信息，或者想重新提交，可通过  

    git reset HEAD~ 

回撤最新一个commit后重新提交。也可以通过commit SHA值进行回撤。如图，想回撤到
fa391f26a63...这个记录，
git reset fa391f26a63...即可

已经提交到远程的commit不要回撤，会推送失败。master分支的commit肯定无法修改; 当前MR的分支的Commit，某些场景（rebase 或commit --amend）也会推送失败，可以添加 -f 参数强制推送。

### New revert代码：
场景例子：发现已经合并到master的dev分支代码有问题，需要回滚。
具体操作如下：

1. 在 GitLab 中找到对应的 Merge 申请，并点击 Revert 按钮。

    注意：以下步骤是为了避免后续出现合并丢失代码问题
2. 确保代码回滚后。在本地git pull 拉取远程最新的master代码，切换到dev分支，merge master的代码。
3. 找到 revert 的那条提交记录，注意了，revert 相关的会有两条记录，第一条是 revert，第二条是 revert 后 merge 的记录，这里取第一条。

        git revert f5c3b544164eec662ea6914d6bd19aedf46874f8 //本地恢复master分支revert掉的代码
        git push  //将dev推送到远程

### 开发规范
1. （强制）项目根目录下的.git/config配置用户名和邮箱; 配置方式见2配置文件
2. （强制）Commit Message 禁止写无意义字符
3. （推荐）一般不直接推送代码到master分支；在紧急修复线上bug或确保本次提交不引入bug的微小变动可以提master
4. （推荐）一般不要提交过多的commit记录; 可通过 git commit --amend 把本次提交附加到最新的commit当中，本次提交不增加commit记录; 如果连Commit Message也不想更新，可以 通过 git commit --amend --no-edit
5. （推荐）为方便同步进度，在没有全部开发完成的情况下，也需要提MR，同时编辑MR的Title和Description。Title前缀添加[WIP]表示你的当前MR work in process且不会被误合并；Description可以描述当前进度或其他信息

6. commit message 规范
- feat: 新功能、新特性
- fix: 修改 bug
- perf: 更改代码，以提高性能
- refactor: 代码重构（重构，在不影响代码内部行为、功能下的代码修改）
- docs: 文档修改
- style: 代码格式修改, 注意不是 css 修改（例如分号修改）
- test: 测试用例新增、修改
- build: 影响项目构建或依赖项修改
- revert: 恢复上一次提交
- ci: 持续集成相关文件修改
- chore: 其他修改（不在上述类型中的修改）
- release: 发布新版本
- workflow: 工作流相关文件修改

    More....


</details>