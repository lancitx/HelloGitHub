# Hello GitHub

本文内容：

- **Git安装**
- **Git常用命令**
- **GitHub进阶用法**
- **日常debug总结**

---

## 1. git与github

### 1.1 git的安装

下载：[Git Download](https://git-scm.com/)

安装：[Git 详细安装教程](https://blog.csdn.net/mukes/article/details/115693833)

---

### 1.2 github的使用

注册：[GitHub](https://github.com/)

创建仓库：[New repository](https://github.com/new)

---

### 1.3 SSH绑定

将本地与GitHub进行SSH绑定。

1. 生成随机密钥。

```bash
ssh-keygen -t rsa -C “xxx@gamil.com”
```

> 必须对应github账号注册邮箱。



2. 将`id_rsa.pub`文件内容添加到[SSH and GPG keys](https://github.com/settings/keys)。

3. 配置全局用户信息。

```bash
git config --global user.name “lancit”
git config --global user.email “xxx@gamil.com”
```

> name随意，但推荐与GitHub一致；
>
> email必须与GitHub注册邮箱相同。



4. 测试：

```bash
ssh -T git@github.com
```

---

## 2. git常用命令



![command](./img/command.png)

### 2.1 创建版本库

#### 2.1.1 git clone

`clone`远程仓库到本地指定路径：

```bash
git clone <repository-url> <local-directory>
```

`clone`仓库**特定分支副本**：

```bash
git clone --branch branch1 <repository-url>
```

`clone`仓库**特定标签副本**：

```bash
git clone --branch v1.0.0 <repository-url>
```

`clone`仓库**特定提交副本**，选项为**哈希值**：

```bash
git clone --branch 4c4ba1d <repository-url>
```

`clone`仓库**子目录副本**，要使用`--depth`和`--filter`选项：

```bash
git clone --depth 1 --filter=blob:none <repository-url> subdirectory
```

> 克隆`repository-url`仓库下 一级子目录`subdirectory`副本。

---

#### 2.1.2 git init

初始化一个新的Git仓库：

```bash
git init <Options> <local-directory>
```

- `--bare`：初始化一个裸 Git 仓库，即没有工作目录的仓库。裸仓库通常用作协同目的的集中仓库。
- `--template=<template_directory>`：指定一个目录来初始化 Git 仓库。可以使用此选项来为仓库提供自定义模板。
- `--separate-git-dir=<git_directory>`：在指定的目录中初始化仓库，但将 Git 元数据存储在单独的目录中。当工作目录在单独的文件系统上或者与他人共享仓库时，这很有用。

---

### 2.2 修改与提交

#### 2.2.1 git status

查看**工作区**和**暂存区**状态：

```bash
git status
```

---

#### 2.2.2 git diff

查看**工作区**和**暂存区**的差异：

```bash
git diff
```

查看**分支**差异：

```bash
git diff branch1 branch2
```

查看单个文件的差异：

```bash
git diff <file>
```

查看**两次提交的差异**（哈希值）;

```bash
git diff a1b2c3d4 e5f6g7h8
```

**生成差异补丁文件**：

```bash
git diff > patch.diff
```

**忽略特定文件**：

```bash
git diff -- . ":(exclude)*.log"
```

**查看合并冲突前的版本差异**：

```bash
git diff --base
```

**查看合并中”我们“的版本差异**：

```bash
git diff --ours
```

**查看合并中”他们“的版本差异**：

```bash
git diff --theirs
```

**常用选项：**

| 选项                    | 作用                               |
| :---------------------- | :--------------------------------- |
| `--cached` / `--staged` | 比较暂存区和上次提交之间的差异     |
| `--name-only`           | 仅显示修改的文件名                 |
| `--name-status`         | 显示修改文件的状态和文件名         |
| `--stat`                | 显示统计信息（增删行数、文件数量） |
| `-w`                    | 忽略空白字符的差异                 |
| `-U<n>`                 | 设置上下文行数                     |

---

#### 2.2.3 git add

添加单个文件：

```bash
git add <file>
```

添加指定目录下的变更文件：

```bash
git add <directory>
```

 使用**交互模式**添加：

```bash
git add -i
```

- 进入交互模式，允许选择性地暂存文件的更改。
- 在交互模式下，可以执行以下操作：
  - `status`：查看当前更改。
  - `update`：将**工作区**变化更新到**暂存区**。
  - `revert`：**撤销对暂存区的更改，并回退到上次提交版本**。
  - `patch`：逐行选择暂存内容。

**分块添加文件更改**：

```bash
git add -p
```

- 以交互模式逐块（hunk）选择文件的更改。
- 提供选项：
  - `y`：暂存当前块。
  - `n`：跳过当前块。
  - `q`：退出分块添加模式。
  - `s`：分割当前块。
  - `e`：手动编辑当前块。

 **常用选项：**

| 选项                   | 功能                                                   |
| ---------------------- | ------------------------------------------------------ |
| `.`                    | 添加当前目录下的所有更改，包括新增、修改和删除的文件。 |
| `-A` / `--all`         | 添加当前工作区的所有更改，包括新增、修改和删除的文件。 |
| `-u` / `--update`      | 仅添加已跟踪文件的更改或删除的文件，不包括新增文件。   |
| `-p` / `--patch`       | 分块模式逐块选择更改进行暂存。                         |
| `-i` / `--interactive` | 进入交互模式，选择性暂存文件和更改。                   |
| `--ignore-errors`      | 忽略无法添加的文件错误，继续处理其他文件。             |
| `--dry-run`            | 显示将被暂存的文件，但不实际添加到暂存区。             |
| `--force` / `-f`       | 强制添加被 `.gitignore` 忽略的文件。                   |

---

#### 2.2.4 git mv

重命名文件：

```bash
git mv old_filename new_filename
```

移动文件到指定目录下：

```bash
git mv <file> <directory>
```

移动文件到指定目录下并重命名：

```bash
git mv <source> <destination>
```

---

#### 2.2.5 git rm

删除**工作区文件**：

```bash
git rm <file>
```

删除指定目录及其文件：

```bash
git rm -r <directory>
```

**只停止追踪，但保留文件**：

```bash
git rm --cached config.json
```

 **常用选项**：

| 选项                 | 功能                                       |
| -------------------- | ------------------------------------------ |
| `--cached`           | 停止跟踪文件，但保留文件在工作区中。       |
| `-r` / `--recursive` | 递归删除目录及其内容。                     |
| `-f` / `--force`     | 强制删除文件，即使工作区中有未保存的修改。 |
| `-n` / `--dry-run`   | 显示将被删除的文件，但不实际执行删除操作。 |

---

#### 2.2.6 git commit

提交**暂存区**的更改：

```bash
git commit -m "Fixed bug"
```

直接提交所有更改到**暂存区**，并提交：

```bash
git commit -a -m "INFO"
```

**交互式提交**：

```bash
git commit --interactive
```

**逐块提交**：

```bash
git commit --patch
```

 **常用选项**：

| 选项                        | 功能                                                     |
| --------------------------- | -------------------------------------------------------- |
| `-m <message>`              | 添加提交信息。                                           |
| `-a`                        | 跳过 `git add`，自动将所有已跟踪文件的更改添加到暂存区。 |
| `--amend`                   | 修改最近一次提交（包括提交信息或内容）。                 |
| `--no-edit`                 | 使用最近一次提交的消息，不编辑提交信息。                 |
| `--allow-empty`             | 提交一个空的提交（没有文件更改）。                       |
| `--author="<name> <email>"` | 为提交指定作者。                                         |
| `--date=<date>`             | 指定提交的日期。                                         |
| `--dry-run`                 | 显示将被提交的内容，但不实际执行提交。                   |

---

### 2.3 撤消

`HEAD`：**指向当前分支的最新提交**。

**特点**：

1. **动态变化**：
   - 每次提交新的更改（`git commit`），`HEAD` 会自动移动到新的提交位置。
   - 如果切换分支（`git checkout branch_name`），`HEAD` 会指向该分支的最新提交。
2. **当前分支指针**：
   - 在正常情况下，`HEAD` 是指向当前分支（如 `main` 或 `feature` 分支）的指针。
3. **分离状态**：
   - 如果 `HEAD` 直接指向某个特定的提交而不是分支，这种状态称为“分离 HEAD”（Detached HEAD）

---

#### 2.3.1 git reset

基本语法：

```bash
git reset [options] [<commit>]
```

- `<commit>`：指定需要重置到的目标提交。如果不提供，默认为 `HEAD`。

- `[options]`：控制重置范围（`--soft`、`--mixed`、`--hard`）。

| 模式      | 描述                                                         | 暂存区 | 工作区 |
| :-------- | ------------------------------------------------------------ | ------ | ------ |
| `--soft`  | 重置到指定提交，只改变 HEAD 指向，不影响暂存区和工作区的内容。 | 保留   | 保留   |
| `--mixed` | 重置到指定提交，改变 HEAD 和暂存区，取消暂存的更改，但保留工作区的内容。 | 重置   | 保留   |
| `--hard`  | 重置到指定提交，改变 HEAD、暂存区和工作区，所有更改（已提交或未提交）都会被丢弃。 | 重置   | 重置   |

**撤销文件的暂存**：

```bash
git reset <file>
```

撤销本地所有未提交的更改：

```bash
git reset --hard
```

撤销本地所有文件的修改，**但不删除新文件**：

```bash
git restore <file>
```

**重置暂存区和工作区到特定提交**：

```bash
git reset --hard <commit_hash>
```

撤销最近一次的提交，将提交更改放回暂存区，不影响工作区文件：

```bash
git reset --soft HEAD~1
```

- `HEAD~1`：表示上一个提交。

撤销最近一次的提交，**不保留提交更改到暂存区**，不影响工作区文件：

```bash
git reset --mixed HEAD~1
```

撤销最近一次提交，**同时清空暂存区和工作区的更改**，恢复到上一次提交的状态:

```bash
git reset --hard HEAD~1
```

**查看重置状态**;

- 查看HEAD指向：`git log`

- 查看**工作区**和**暂存区**状态：`git status`

---

#### 2.3.2 git revert

与 `git reset` 不同，**它不会直接删除提交历史，而是通过创建一个新的提交来逆转指定提交的改动**。

基本语法：

```bash
git revert [options] <commit>
```

- `<commit>`：指定需要撤销的提交（通常是提交哈希 `commit_hash`）。

- `[options]`：指定行为的附加选项（如静默模式等）。



撤销某次提交：

```bash
git revert <commit>
```

撤销最近一次提交：

```bash
git revert HEAD
```

撤销**多个提交**，按顺序撤销：

```bash
git revert <commit1> <commit2>
```

撤销一系列提交，从父提交`start_commit`到`end_commit`：

```bash
git revert <start_commit>^..<end_commit>
```

**常用选项**：

| 选项                 | 功能                                                 |
| -------------------- | ---------------------------------------------------- |
| `-n` / `--no-commit` | 逆转提交，但不立即创建新的提交，改动保留在暂存区中。 |
| `--continue`         | 在冲突解决后继续完成 `revert` 操作。                 |
| `--abort`            | 取消当前的 `revert` 操作。                           |
| `--no-edit`          | 不进入编辑模式，直接使用默认的提交信息。             |

---

#### 2.3.3 git checkout

恢复文件到最近提交状态：

```bash
git checkout HEAD index.html
```

恢复文件到某次提交状态：

```bash
git checkout <commit_hash> <file>
```

查看历史提交内容，会**将HEAD指向某次提交，进入分离状态**：

```bash
git checkout <commit_hash>
```

---

### 2.4 分支与标签

#### 2.4.1 git branch

查看**本地分支**和**远程分支**：

```bash
git branch
git branch -r
git branch -a
```

---

### 2.5 合并与衍合

- 

---

### 2.6 远程操作

- 

---

## 3. git进阶用法

- 

---

## 4. 日常debug总结

- 

