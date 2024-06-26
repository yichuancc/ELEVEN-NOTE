# Git 克隆指定分支

> 参考地址：https://blog.csdn.net/qq_19922839/article/details/129325826

```shell
# 拉取指定分支代码
git clone -b dev --single-branch http://xxx地址/xx/xxx.git

# 下面是说明

# 指定克隆远程分支 `/develop/branch_1`
> git clone -b /develop/branch_1 git@www.gitee.com/ghimi/hello.git
> cd hello

# 进入仓库，通过 git branch -r 查看可以看到还是拉取到了全部远程分支
> git branch -r
origin/20200113-function-concurrency-test
origin/20210112_8457548_basic2
origin/HEAD -> origin/master
origin/acl_retrofit
origin/add_api_for_get_organizations

# 如果想要只克隆指定分支还需要搭配 --single-branch 选项，这样我们就只会拉取到我们指定的分支，而不会拉取到其他远程分支了
# 指定克隆远程分支 `/develop/branch_1`
> git clone -b /develop/branch_1 --single-branch git@www.gitee.com/ghimi/hello
> cd hello

# 进入仓库，通过 git branch -r 当前就只剩下一个分支了
> git branch -r
/origin/develop/branch_1
```

# 查看代码提交

```shell
#查看代码
git log --format='%aN' | sort -u | while read name; do echo -en "$name\t"; git log --author="$name" --pretty=tformat: --since ==2023-10-15 --until=2024-02-10 --numstat | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }' -; done
```

# GIT提交配置

```shell
#查看
git config -l |grep user

#全局级别
git config --global user.name "eleven"
git config --global user.email "274224864@qq.com"

#仓库级别
git config --local user.name "yic"
git config --local user.email "274224864@qq.com"
```

