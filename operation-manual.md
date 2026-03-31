[toc]

# Git & GitHub Operation Manual

## Introduction

![img](image/162fcc0987bf1c0atplv-t2oaga2asx-jj-mark3024000q75-1774948027328-9.webp)

- **Workspace (工作区)**: 本地电脑存放项目文件的地方，实际编写代码和存放资料的目录。
- **Index / Staging Area (暂存区)**: 暂时存放准备提交的修改内容的地方，相当于一个“打包区”（数据存在 `.git` 文件夹中）。
- **Local Repository (本地仓库)**: 使用 `commit` 命令将暂存区中的文件正式记录并生成历史版本的本地空间。
- **Remote (远程仓库)**: 托管在云端服务器（如 GitHub）上的项目代码。可以使用 `clone` 拷贝到本地，开发后使用 `push` 推送到云端进行备份或共享。

---

## Part 1: Git Operation Instructions

### 1. 全局配置与身份验证 (Setup)
在使用 Git 前，需要先配置个人信息和连接密钥，一生只需在电脑上配置一次。

```bash
# 设置你的名字和邮箱（提交记录中会显示）
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"

# 生成 SSH 密钥（用于免密安全连接 GitHub）
# 运行后一路回车，然后将 ~/.ssh/id_ed25519.pub 中的内容添加到 GitHub Settings
ssh-keygen -t ed25519 -C "your_email@example.com"
```

### 2. 初始化与获取项目 (Init & Clone)
开启一个 Git 项目的两种常见方式。

```bash
# 方式一：在当前文件夹新建本地仓库（会生成隐藏的 .git 文件夹）
git init

# 方式二：从远程服务器克隆一个完整的项目到本地
git clone <url>
```

### 3. 本地文件操作 (Add & Commit)
日常写代码最常用的循环逻辑：修改 -> 暂存 -> 提交。

```bash
# 将指定文件或文件夹添加到暂存区 (Index)
git add <file_name>
git add src/

# 将工作区所有修改过的文件添加到暂存区（最常用）
git add .

# 将暂存区的文件提交到本地仓库，并附带修改说明
git commit -m "feat: 新增了首页UI"

# 撤销上次提交（或将新的修改合并到上一次提交中，不增加新的历史记录）
git commit --amend
```

### 4. 状态查询与历史记录 (Query)

```bash
# 查询工作区所有文件的状态（标红为未暂存，标绿为已暂存，必用命令！）
git status

# 查看过去的提交历史记录（精简模式）
git log --oneline
```

### 5. 远程仓库同步 (Remote & Sync)
管理本地与云端代码的交互。

```bash
# 为本地仓库绑定远程服务器地址（origin是习惯命名）
git remote add origin git@github.com:User/Repo.git

# 查看已绑定的远程地址
git remote -v

# 首次将本地 main 分支推送到远程 origin，并建立追踪关系
git push -u origin main

# 日常推送本地更新到远程仓库
git push

# 将远程仓库的最新代码拉取到本地并合并
git pull origin main
```

### 6. 忽略文件配置 (.gitignore)
**非常重要**：在执行 `git add .` 之前，必须配置 `.gitignore` 文件，防止大型文件（如 PDF、数据集、模型权重）或系统临时文件被提交到仓库，导致仓库体积臃肿。

在项目根目录新建名为 `.gitignore` 的文件，添加以下内容示例：

```text
# 1. 大型文档与压缩包
*.pdf
*.zip
*.tar.gz

# 2. 深度学习与 Python 常见忽略项
__pycache__/
*.py[cod]
.ipynb_checkpoints/
*.pt
*.pth
*.h5

# 3. 存放大量数据的特定文件夹（需根据实际目录修改）
data/
datasets/
logs/

# 4. 操作系统临时文件
.DS_Store
Thumbs.db
```

### 7. 常见报错与急救 (Troubleshooting)

```bash
# 报错：找不到 main 分支 (本地分支名为 master 时)
# 解决：强制将当前本地分支重命名为 main
git branch -M main

# 报错：Updates were rejected... (远程仓库有本地没有的文件)
# 解决：强制拉取并合并互不相关的历史记录，然后再 push
git pull origin main --allow-unrelated-histories

# 报错：Connection was reset (网络连接问题)
# 解决：配置代理，或将仓库的 HTTPS 链接切换为 SSH 链接
git remote set-url origin git@github.com:User/Repo.git
```

---

## Part 2: Git Bash 常用电脑操作指令

Git Bash 模拟了 Linux 终端环境，掌握以下命令可以大幅提升日常文件管理的效率，告别鼠标点击。

### 1. 导航与查看（我在哪？这里有什么？）

| 命令            | 作用与说明                                                   | 实用示例 |
| :-------------- | :----------------------------------------------------------- | :------- |
| **`pwd`**       | **查看当前位置** (Print Working Directory)。显示当前的绝对路径。 | `pwd`    |
| **`ls`**        | **列出当前文件夹的内容** (List)。                            | `ls`     |
| **`ls -a`**     | **列出所有内容（包括隐藏文件）**。查看 `.git` 和 `.gitignore` 必备。 | `ls -a`  |
| **`cd <路径>`** | **进入某个文件夹** (Change Directory)。支持按 `Tab` 键自动补全。 | `cd src` |
| **`cd ..`**     | **返回上一级目录**（注意 `cd` 和 `..` 之间有空格）。         | `cd ..`  |
| **`cd ~`**      | **瞬间回老家**。直接跳回电脑的用户根目录。                   | `cd ~`   |

### 2. 文件与文件夹的“增删改查”

| 命令                 | 作用与说明                                                   | 实用示例                          |
| :------------------- | :----------------------------------------------------------- | :-------------------------------- |
| **`mkdir <名字>`**   | **新建文件夹** (Make Directory)。                            | `mkdir datasets`                  |
| **`touch <名字>`**   | **新建一个空文件**。支持带后缀名。                           | `touch README.md`                 |
| **`rm <名字>`**      | **删除单个文件** (Remove)。注意：不会进回收站，直接永久删除！ | `rm old_code.py`                  |
| **`rm -rf <名字>`**  | **强制删除文件夹及其内部所有内容**。⚠️ **极其危险**，请谨慎确认路径！ | `rm -rf temp_data`                |
| **`cp <源> <目标>`** | **复制文件** (Copy)。                                        | `cp config.txt config_backup.txt` |
| **`mv <源> <目标>`** | **移动或重命名文件** (Move)。                                | `mv test.py src/`                 |

### 3. 提升幸福感的“神仙命令/快捷键”

- **`explorer .`**：在 Windows 中立刻弹出图形化窗口，打开当前终端所在的文件夹（注意 `.` 前面有空格）。
- **`clear`**（或快捷键 `Ctrl + L`）：清屏。清除终端中过多的报错和历史信息。
- **`Ctrl + C`**：强行中断当前正在运行的程序或卡住的命令。
- **`方向键 ⬆️ ⬇️`**：快速翻阅、调用之前敲过的历史命令。
- **`Shift + Insert`**：在 Git Bash 中执行粘贴操作（复制为 `Ctrl + Insert`）。
