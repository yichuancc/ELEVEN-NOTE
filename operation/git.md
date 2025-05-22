# Git克隆仓库

**基础克隆**

```shell
git clone <远程仓库URL>
```

将远程仓库**所有分支**（默认只检出 `master/main` 分支）、提交历史、标签克隆到本地

**克隆指定分支**

```shell
git clone -b <分支名> --single-branch <远程仓库URL>
```

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

# 复制到新仓库并保留提交记录

要将一个GitLab仓库的指定分支完整复制到新仓库（包括所有提交历史），按照以下步骤操作：

## 方法一：使用git命令克隆并推送到新仓库

1. **克隆原仓库**：

   ```bash
   git clone --branch <分支名> --single-branch <原仓库URL>
   cd <仓库目录>
   ```

2. **添加新仓库为远程**：

   ```bash
   git remote add new-origin <新仓库URL>
   ```

3. **推送分支到新仓库**：

   ```bash
   git push new-origin <分支名>
   ```

## 方法二：完整复制所有分支

1. **克隆原仓库（所有分支）**：

   ```bash
   git clone --mirror <原仓库URL>
   cd <仓库目录>
   ```

2. **修改远程地址为新仓库**：

   ```bash
   git remote set-url origin <新仓库URL>
   ```

3. **推送所有内容**：

   ```bash
   git push --mirror
   ```

## 两种方式区别

* **方法一：`git remote add new-origin <URL>` + `git push new-origin <分支名>`**

**特点：**

1. **仅推送指定分支**
   - 只将你明确指定的分支推送到新仓库，其他分支不会被复制。
2. **保留原仓库的远程配置**
   - 原仓库的远程（通常叫 `origin`）保持不变，新增一个远程（如 `new-origin`）指向新仓库。
3. **非镜像推送**
   - 不会影响新仓库的其他分支或标签，仅更新你指定的分支。
4. **适用于**
   - 只需要复制单个分支到新仓库，且希望保留与原仓库的关联。

**示例流程：**

```bash
git clone --branch <分支名> --single-branch <原仓库URL>
cd <仓库目录>
git remote add new-origin <新仓库URL>
git push new-origin <分支名>
```

------

* **方法二：`git clone --mirror` + `git push --mirror`**

**特点：**

1. **完全镜像克隆和推送**
   - 复制**所有分支、标签、提交历史、引用（包括 notes 和 reflogs）**，完全一致地迁移到新仓库。
2. **覆盖新仓库的所有内容**
   - `--mirror` 推送会强制使新仓库和原仓库完全一致（包括删除新仓库中不存在于原仓库的分支）。
3. **不保留工作目录**
   - `git clone --mirror` 创建的是**裸仓库**（没有工作区文件，只有 `.git` 目录内容），适合服务器端操作。
4. **适用于**
   - 需要完整复制整个仓库（包括所有分支和标签），或迁移到新 GitLab 实例。

**示例流程：**

```bash
git clone --mirror <原仓库URL>
cd <仓库目录.git>  # 注意是裸仓库
git remote set-url origin <新仓库URL>
git push --mirror
```

------

### **总结**

|     **对比项**     |    方法一（普通推送）     |        方法二（镜像推送）         |
| :----------------: | :-----------------------: | :-------------------------------: |
|    **复制内容**    |        仅指定分支         |       所有分支、标签、引用        |
| **是否覆盖新仓库** |      只更新指定分支       | 强制完全覆盖（删除多余分支/标签） |
|    **远程配置**    | 保留原 `origin`，新增远程 |   直接修改 `origin` 指向新仓库    |
|    **仓库类型**    |   普通仓库（有工作区）    |        裸仓库（无工作区）         |
|    **适用场景**    |      选择性迁移分支       |        完整仓库迁移或备份         |

------

### **如何选择？**

- 如果只需要**复制特定分支**且**保留原仓库关联** → **用方法一**。
- 如果需要**完全一致地迁移整个仓库**（包括所有分支和标签）→ **用方法二**。