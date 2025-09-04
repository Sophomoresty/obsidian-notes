## 批量推送

```zsh
#!/bin/bash

# --- 仓库列表配置 ---
# 在这里把你所有需要同步的仓库路径都加进去，每个路径一行。
# 注意：Windows路径要转换成 /mnt/盘符/ 的格式哦。
REPOS=(
    "/mnt/c/Users/Sophomores/Desktop/Axmath-notes"
    "/mnt/d/obsidian-notes"
    "/home/sophomores/算法练习"
)

# --- 脚本主逻辑 ---

# 自动生成当前时间作为 commit message
COMMIT_MSG=$(date +"%Y-%m-%d %H:%M")

echo "准备开始同步 ${#REPOS[@]} 个仓库..."
echo "使用的 Commit Message 是: \"$COMMIT_MSG\""
echo "----------------------------------------"

# 循环处理每一个仓库
for repo_path in "${REPOS[@]}"; do
    # 检查路径是否存在
    if [ -d "$repo_path" ]; then
        echo ">>> 正在处理仓库: $repo_path"
        
        # 进入仓库目录
        cd "$repo_path" || exit
        
        # 执行 git add 和 git commit
        # "git status --porcelain" 会检查是否有文件变动，如果没有变动，输出为空
        if [ -n "$(git status --porcelain)" ]; then
            git add .
            git commit -m "$COMMIT_MSG"
            
            # 检查 commit 是否成功
            if [ $? -eq 0 ]; then
                echo "  ✅ Git Add & Commit 成功。"
                
                # 执行 git push
                echo "  ... 正在执行 Git Push ..."
                git push
                
                # 检查 push 是否成功
                if [ $? -eq 0 ]; then
                    echo "  ✅ Git Push 成功！"
                else
                    echo "  ❌ 错误: Git Push 失败了。请检查网络或远程仓库权限。"
                fi
            else
                echo "  ❌ 错误: Git Commit 失败了。请检查 Git 状态。"
            fi
        else
            echo "  👍 仓库没有文件变动，无需同步。"
        fi
        
        echo "----------------------------------------"
    else
        echo ">>> ⚠️ 警告: 路径不存在，跳过: $repo_path"
        echo "----------------------------------------"
    fi
done

echo "🎉 所有仓库同步任务执行完毕！"
```

## 批量拉取
```bash
#!/bin/bash

# --- 仓库列表配置 ---
# 在这里把你所有需要更新的本地仓库路径都加进去。
# 我已经帮你把 Windows 路径转换好了，也修正了 home 目录的写法哦。
REPOS=(
    "/mnt/c/Users/Sophomores/Desktop/Axmath-notes"
    "/mnt/e/Obsidian Vault"
    "/home/sophomores/Coding-Journal"
)

# --- 脚本主逻辑 ---

echo "🚀 开始批量更新 ${#REPOS[@]} 个 Git 仓库..."
echo "----------------------------------------"

# 循环处理每一个仓库
for repo_path in "${REPOS[@]}"; do
    # 检查路径是否存在
    if [ -d "$repo_path" ]; then
        # 检查路径是否为一个Git仓库（通过判断.git文件夹是否存在）
        if [ -d "$repo_path/.git" ]; then
            echo ">>> 正在更新仓库: $repo_path"
            
            # 进入仓库目录
            cd "$repo_path" || continue # 如果cd失败则跳过当前循环
            
            # 执行 git pull
            git pull
            
            # 检查 pull 的结果
            if [ $? -eq 0 ]; then
                echo "  ✅ 更新成功！"
            else
                echo "  ❌ 更新失败！可能存在冲突或网络问题，请手动处理。"
            fi
            
            echo "----------------------------------------"
        else
            echo "🤔 跳过路径: $repo_path"
            echo "   原因：该目录不是一个 Git 仓库。"
            echo "----------------------------------------"
        fi
    else
        echo "⚠️ 警告: 路径不存在，跳过: $repo_path"
        echo "----------------------------------------"
    fi
done

echo "🎉 所有仓库更新任务执行完毕！"
```