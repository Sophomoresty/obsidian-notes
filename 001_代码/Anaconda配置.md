---
tags:
  - anaconda
  - jupyter
  - 配置指南
---
# Anaconda 配置
## Anaconda 换源

- 添加清华大学的conda镜像源
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
```


- 如果需要使用conda-forge，可以添加清华的conda-forge镜像
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
```

- 设置搜索优先级, 确保确保使用刚刚设置的镜像源
```
conda config --set show_channel_urls yes
```

综上, 使用conda install命令时，conda将会优先从清华的镜像源搜索和下载软件包
## Anaconda 环境问题解决

### 默认状态
- `anaconda` 虚拟环境会安装到 c 盘
- `Pycharm` 的 `终端` 运行 ` anaconda ` 命令会报错
### 解决办法
- 找到 conda 的根目录, 设置该目录的属性
  
![[Anaconda配置.md_Attachments/Anaconda配置-20250225124242479.png|1000]]

- `安全` -> `编辑`
  
![[Anaconda配置.md_Attachments/Anaconda配置-20250225124526598.png|1000]]
- 选中 `User`, 打勾 `完全控制`, 点击 `应用` 
- 等待 `设置安全信息`

设置完成后, 以后安装虚拟环境的位置就会自动更改到
```
E:\anaconda3\envs\虚拟环境名称
```
## Anaconda 环境管理

## 查看环境

```
conda env list
```
## 创建环境

```
conda create --name ENV_NAME
conda create -n ENV_NAME
```

> [!note]
>  `--name` 可以简写为 `-n`
>  
>  总结: 命令全称前用 `--`, 简写用 `-` 



如果想在环境中指定Python版本, 可以使用
```
conda create -n myenv python=3.10
```

`3.10` 是指 Python 的版本.

## 激活环境
```
conda activate ENV_NAME
```

## 退出环境

```
conda deactivate
```

## 删除环境
- 停用当前环境
```
conda deactivate
```
- 删除环境
```
conda remove --name ENV_NAME --all
```

`ENV_NAME` 表示要移除/删除的环境名称。
使用 `--all` 标志会删除安装在该环境中的所有软件包。

## 重命名环境

conda 没有提供直接重命名环境的功能. 
但是, 可以通过克隆原环境, 再删除原环境达到目的

- 克隆 原环境 -> 新环境
```
conda create --n ENV_NAME_NEW --clone ENV_NAME_OLD 
```
- 删除原环境
```
conda remove -n ENV_NAME_OLD  --all
```


## 程序包的安装

### pip 安装
```
pip install package-name
```
### conda 安装
```
conda install package-name
```


> [!note] 
> pip 和 conda 均可以安装软件包, 为了书写方便, 后续统一用 conda
> 
### 查询安装的包
```
conda list
pip list
```

# Jupyter Notebook 配置

## 切换环境
```
conda activate ENV_NAME
```

## 安装Jupyter Notebook
```
conda install jupyter notebook
```

## 更新 Jupyter 和相关扩展:

确保你的 Jupyter Notebook 和相关扩展都是最新版本。可以使用以下命令更新：

不更新使用会报错
```
pip install --upgrade jupyter jupyterlab jupyter-server jupyter-lsp notebook-shim
```

## 安装中文插件包
> [!error]
> 用 conda 安装此插件会报错, 以后遇到类似 conda 安装报错可以尝试 pip

```
pip install jupyterlab-language-pack-zh-CN
```

## 启动Jupyter Notebook

```
jupyter notebook
```
## 设置
### 设置中文
![[Anaconda配置.md_Attachments/Anaconda配置-20250225123500617.png|1000]]
### 设置自动补全
- 打开 `设置编辑器`, 快捷键 `Ctrl+,`

![[Anaconda配置.md_Attachments/Anaconda配置-20250225123930548.png|1000]]
- 代码补全-启用自动补全

![[Anaconda配置.md_Attachments/Anaconda配置-20250225124008156.png|1000]]


# Anaconda 常用命令

```
# 添加镜像
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
# 删除镜像
conda config --remove channels  https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
# 展示目前已有的镜像channel。
conda config --show channels
# 更新conda版本
conda update -n base conda
# 查看环境不同版本的历史
conda list --revisions
# 回溯到对应的版本
conda install --rev 0
conda clean -p      //删除没有用的包（推荐）
conda clean -t      //tar打包
conda clean -y -all //删除全部的安装包及cache
```

# Anaconda 常见问题

## Anaconda-Navigator 打开或切换环境等操作时 VSCode 会弹出 cli.js
以管理员身份启动 anaconda

