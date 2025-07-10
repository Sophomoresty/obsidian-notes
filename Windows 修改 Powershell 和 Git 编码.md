
### **1. 查看 PowerShell 编码**

在 PowerShell 窗口中运行以下命令：

```powershell
chcp
[Console]::OutputEncoding
[Console]::InputEncoding
```

* `chcp`：显示当前控制台的代码页（`65001` 表示 UTF-8，`936` 表示 GBK/GB2312）。
* `[Console]::OutputEncoding`：显示 PowerShell 输出的编码。
* `[Console]::InputEncoding`：显示 PowerShell 输入的编码。

---

### **2. 修改 PowerShell 编码为 UTF-8 (推荐永久生效)**

通过修改 PowerShell 配置文件 `$PROFILE` 来实现，这样每次打开 PowerShell 都会自动应用设置。

1.  **打开 PowerShell 配置文件：**
    ```powershell
    notepad $PROFILE
    ```
    这会打开一个文本编辑器（通常是记事本）。如果文件不存在，会提示你创建。

2.  **添加编码设置：**
    在打开的记事本中，将以下三行内容粘贴到文件末尾并保存：
    ```powershell
    chcp 65001
    [Console]::OutputEncoding = [System.Text.Encoding]::UTF8
    [Console]::InputEncoding = [System.Text.Encoding]::UTF8
    ```

3.  **重启 PowerShell：**
    **务必关闭所有当前 PowerShell 窗口**，然后重新打开一个新的 PowerShell 窗口，让配置生效。

4.  **验证：**
    再次运行第一步的查看命令，确认 `chcp` 显示 `65001`，并且 `OutputEncoding` 和 `InputEncoding` 都显示 `System.Text.UTF8Encoding`。

---

### **3. 查看 Git 全局编码设置**

在 PowerShell 窗口中运行以下命令：

```powershell
git config --global core.quotepath
git config --global i18n.commitencoding
git config --global i18n.logoutputencoding
```

* `git config --global core.quotepath`：查看 Git 是否会对非 ASCII 路径进行八进制转义（`false` 表示不转义，有助于中文显示）。
* `git config --global i18n.commitencoding`：查看 Git commit message 的编码。
* `git config --global i18n.logoutputencoding`：查看 Git log 输出的编码。

---

### **4. 修改 Git 全局编码为 UTF-8**

在 PowerShell 窗口中运行以下命令：

```powershell
git config --global core.quotepath false
git config --global i18n.commitencoding utf-8
git config --global i18n.logoutputencoding utf-8
```

这些命令会将 Git 的相关编码设置更改为 UTF-8，并确保 Git 在处理包含非 ASCII 字符的路径时不会出现问题。
