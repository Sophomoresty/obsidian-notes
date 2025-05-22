

我们通过配置 Git 钩子（Git Hooks）和 Git 别名（Git Alias），实现了以下功能：当你执行 `git autocommit` 命令时，它会自动暂存所有更改，并以 `更新时间:YYYY-MM-DD-HH-MM` 的格式生成提交信息。

---

### 步骤 1: 配置 `prepare-commit-msg` Git 钩子

这个钩子负责在提交时自动生成并填充提交信息。

1.  **进入 `.git/hooks/` 目录：**
打开你的终端，导航到你的 Git 仓库的 `.git/hooks/` 目录。
```bash
cd .git/hooks
```

2.  **禁用旧的 `pre-commit` 钩子 (如果存在)：**
如果你之前有名为 `pre-commit` 的文件（没有 `.sample` 后缀），它可能会干扰我们的新设置。为了避免冲突，请将其重命名。


```bash
mv pre-commit pre-commit.bak
```
    
    *(如果你执行 `ls` 后没有看到 `pre-commit` 文件，可以跳过此步骤。)*

3.  **激活 `prepare-commit-msg` 钩子：**
你的仓库中应该有一个 `prepare-commit-msg.sample` 文件。我们需要将其复制一份并移除 `.sample` 后缀，这样 Git 才能识别它。

```bash
cp prepare-commit-msg.sample prepare-commit-msg
```

4.  **编辑 `prepare-commit-msg` 文件内容：**
使用你喜欢的文本编辑器打开 `prepare-commit-msg` 文件，并将里面的所有内容替换为以下脚本：

```bash
#!/bin/bash

# 获取当前时间并格式化为 YYYY-MM-DD-HH-MM
TIME_FORMAT=$(date "+%Y-%m-%d-%H-%M")
COMMIT_MSG_FILE=$1 # Git 会把提交信息文件的路径作为第一个参数传递给这个脚本

# 直接将模板信息写入提交信息文件
echo "更新时间:${TIME_FORMAT}" > "$COMMIT_MSG_FILE"
echo "" >> "$COMMIT_MSG_FILE" # 添加一个空行

exit 0
```
    **保存并关闭文件。**

5.  **赋予 `prepare-commit-msg` 执行权限：**
确保这个钩子脚本是可执行的。

```bash
chmod +x prepare-commit-msg
```

---

### 步骤 2: 配置 Git 别名 `autocommit`

这个别名将简化你的提交命令，让你能一键完成暂存和提交。

1.  **设置全局 `autocommit` 别名：**
在你的终端中，**精确地**复制并粘贴以下这行命令来设置别名。这个别名是**全局**的，意味着它在你的所有 Git 仓库中都有效。

```bash
git config --global alias.autocommit "!git add . && git commit --allow-empty-message -m ''"
```
**请特别注意：**
* 整个命令字符串被 **单引号 `'`** 包裹。
* 开头的 **`!`** 告诉 Git 这是一个 Shell 命令。
* **`&&`** 连接符确保 `git add .` 成功后才执行 `git commit`。
* `m ''` 传递一个空字符串，让 `prepare-commit-msg` 钩子有机会填充它。

2.  **验证别名是否设置成功：**
    你可以通过查看你的全局 Git 配置文件来确认别名是否正确设置。
```bash
cat ~/.gitconfig
```
你应该在 `[alias]` 部分看到类似这样的内容：
```ini
[alias]
	autocommit = "!git add . && git commit --allow-empty-message -m \"\""
```
（`\"\"` 是正常的显示，表示内部的空字符串。）

---

### 步骤 3: 使用 `autocommit` 别名

配置完成后，当你进行代码修改并希望提交时，只需：

1.  在 Git 仓库的根目录或其子目录中，对文件进行修改或创建新文件。
2.  运行你的自定义命令：
```bash
git autocommit
```

这将自动暂存所有更改，并创建一个提交，其信息将是 `更新时间:YYYY-MM-DD-HH-MM` 的格式。