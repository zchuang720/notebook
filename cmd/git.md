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

```

## 冲突解决
```

```
