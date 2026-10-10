# Git 使用教程（从零到首次托管）

> 本人实战踩坑记录，适合完全没接触过 Git 的新手一步一步跟着做。

## 一、Git 是什么

Git 是**版本控制工具**，相当于"代码的时光机 + 备份工具"。它记录每次改动，随时能回退历史版本，也能多人协作。**GitHub / Gitee 是代码托管平台**，把本地仓库传到网上，简历、分享都用它。

## 二、必备概念

| 概念 | 大白话解释 |
|------|-----------|
| 仓库 Repository | 被 Git 管理的文件夹，里面有隐藏的 `.git` 目录 |
| 提交 Commit | 一次"保存快照" |
| 暂存区 Staging | 提交前的待确认区 |
| 分支 Branch | 平行开发线，主分支叫 main |
| 推送 Push | 本地提交上传到远程 |
| 拉取 Pull | 从远程下载最新代码 |
| 克隆 Clone | 把远程整个仓库下载到本地 |

## 三、首次托管全流程（手把手）

### 第1步 检查 Git 是否安装

```powershell
git --version
```

看到 `git version xxx` 就装好了；提示"不是内部命令"则需先安装 Git。

### 第2步 配置身份（只需一次）

```powershell
git config --global user.name "你的用户名"
git config --global user.email "你的邮箱"
```

> ⚠️ 我踩过的坑：`-- global` 中间**不能有空格**，必须是 `--global`，否则报 `fatal: not in a git directory`。

验证：`git config --list`（能看到 user.name / user.email）

### 第3步 初始化仓库

```powershell
git init
git branch -M main    # 把默认分支名改成 main
```

### 第4步 配置文件忽略清单 .gitignore

新建 `.gitignore` 文件，写上不希望上传的内容：
- Java 编译产物 `target/`
- IDE 配置 `.idea/`、`*.iml`
- 含密钥的真实配置文件 `application.yml`（只留模板）

### 第5步 加入暂存区

```powershell
git add .
```

看哪些文件会被提交：

```powershell
git status
```

> 检查重点：**密钥文件、编译产物绝不能出现在列表里**。

### 第6步 提交

```powershell
git commit -m "初始化项目"
```

### 第7步 连接远程仓库

先在 GitHub 建一个**空的** Public 仓库（不要勾选自动生成 README/.gitignore），然后：

```powershell
git remote add origin https://github.com/用户名/仓库名.git
```

改地址用 `git remote set-url origin 新地址`

> 注意：`用户名.github.io` 是个人主页仓库，别拿它当项目代码仓库。

### 第8步 推送（关键！token 认证）

```powershell
git push origin main
```

> ⚠️ 2021 年后的 GitHub **不接受账号密码**。弹出登录框时：
> - Username：填用户名
> - Password：**填 Personal Access Token（ghp_ 开头），不是登录密码！**

**Token 怎么生成**：
1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Generate new token (classic)
3. Note 随便写，Expiration 选有效期，权限勾 **repo**
4. Generate，复制 `ghp_...`（只显示一次，马上存好）

推成功的标志：`* [new branch] main -> main`

## 四、日常使用（写代码后三步）

```powershell
git add .
git commit -m "修改了xxx功能"
git push
```

## 五、多台电脑同步

**第一台电脑**：clone 一键把仓库搬下来

```powershell
git clone https://github.com/用户名/仓库名.git
```

**日常同步**：
- 从远程拉到本地：`git pull`
- 本地推到远程：`git push`
- **习惯**：写代码前先 `git pull`，push 被拒时也先 `git pull` 拉最新再 push

**push 被拒绝（non-fast-forward）**：说明远程有你本地没有的提交，先 `git pull origin main` 合并再推送。

## 六、常用命令速查

| 命令 | 作用 |
|------|------|
| `git status` | 查看哪些文件改过 |
| `git add .` | 全部改动进暂存区 |
| `git commit -m "说明"` | 提交 |
| `git push` | 推送到远程 |
| `git pull` | 拉取远程最新 |
| `git log` | 查看提交历史 |
| `git branch` | 查看分支 |
| `git remote -v` | 查看远程地址 |

## 七、几个记忆点

- **commit 只是本地保存，别人看不到；必须 push 才上传。**
- `clone` 只需一次，`pull/push` 是日常双向同步。
- 先 `pull` 再 `push`，避免覆盖别人/另一台电脑的代码。