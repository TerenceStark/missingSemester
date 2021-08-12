# HW6Solution

## Author:金之航

## StuID:2183411101



## Problem

### 1.Git  Basic	s	

#### (1)(2) basics and fork

```shell
git status
git add <file>
git commit
git cat-file -p SHA-1

git config --global user.name "TerenceStark"
git config --global user.email "xiashulang@gmail.com"

git diff <hash> hello.txt

git clone https://hub.fastgit.org/pcottle/learnGitBranching.git
```

![image-20210722155901608](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210722155901608.png)

![image-20210722175855407](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210722175855407.png)

![image-20210722175709985](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210722175709985.png)

#### (3) git graph

```shell
git log --all --graph --decorate #查看所有分支图形化的commit历史
```

![image-20210722215310176](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210722215310176.png)

#### (4)(5)

```shell
git log README.md
git blame _config.yml | grep collections
git show a88b4eac
```



### 2.Git Exercise

#### (1) git stash

```shell
git status
git stash
#git stash 会把所有未提交的修改（包括暂存的和非暂存的）都保存起来，用于后续恢复当前工作目录。
#比如下面的中间状态，通过 git stash 命令推送一个新的储藏，当前的工作目录就干净了。
#stash是本地的，不会通过 git push 命令上传到 git server 上。
```

![image-20210723003322237](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210723003322237.png)

```shell
git log –all –oneline #查看所有分支的commit历史,oneline 一条提交信息用一行展示
```

![image-20210723003655684](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210723003655684.png)

```shell
git status
git stash pop #可以通过git stash pop命令恢复之前缓存的工作目录
git stash list #查看现有stash
git stash drop #移除stash
git stash show -p #查看指定stash的diff
```

![image-20210723004234301](C:\Users\jin0805\AppData\Roaming\Typora\typora-user-images\image-20210723004234301.png)

#### (2)(3) dotfile

```shell
[alias]
  co = checkout
  ci = commit
  st = status
  br = branch
  hist = log --pretty=format:\"%h %ad | %s%d [%an]\" --graph --date=short
  type = cat-file -t
  dump = cat-file -p
  graph = log --all --graph --decorate --oneline
  
git config --global core.excludesfile ~/.gitignore .DS_Store
```

