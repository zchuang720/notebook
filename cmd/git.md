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
