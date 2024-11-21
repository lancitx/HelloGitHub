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

-

---

### 2.3 撤消

-

---

### 2.4 分支与标签

-

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

