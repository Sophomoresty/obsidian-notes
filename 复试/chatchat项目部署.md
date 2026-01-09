## 安装chatchat
```python
# 1. 创建一个名为 chatchat 的新环境，指定 Python 3.11 (文档支持的最高版本，通常较快)
mamba create -n chatchat python=3.11

# 2. 激活环境
mamba activate chatchat

# 3. 安装项目 (根据文档推荐的安装方式)
pip install langchain-chatchat -U
```

## Ollama

### 下载

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

### 配置守护进程
#### 1)创建服务配置文件
```bash
sudo tee /etc/systemd/system/ollama.service > /dev/null <<EOF
[Unit]
# 第一部分：自我介绍与启动顺序
Description=Ollama Service
After=network-online.target
# 解释: 设置依赖关系, 当前服务需要等待 网络服务 完全连通后
# 为什么：Ollama 需要联网下载模型，如果不加这句，开机瞬间没网它可能就报错

[Service]
# 第二部分：工作职责
ExecStart=/usr/local/bin/ollama serve
# 解释：ollama 服务的启动命令
# 注意：这里必须写绝对路径 (/usr/local/bin/...)，不能只写 ollama。

User=sophomores
Group=sophomores
# 解释：【权限关键】指定以谁的身份运行。
# 为什么要填你的用户名？
# 因为模型文件都存在你的家目录 (/home/sophomores/.ollama) 下。
# 如果默认用 root 运行，它可能找不到你的模型，或者把权限搞乱。

Restart=always
# 解释：【复活机制】如果不倒翁倒了怎么办？
# always = 只要进程挂了（比如被杀掉、崩溃），不管什么原因，立刻重启。
RestartSec=3
# 解释：冷静期。挂了之后，喘息 3 秒钟再重启，防止疯狂重启把 CPU 跑满。
Environment="PATH=$PATH"
# 解释：环境变量。确保它能找到系统里的其他工具。

[Install]
WantedBy=multi-user.target
# 解释：【开机自启的钩子】
# multi-user.target 相当于 Windows 的“正常进入桌面模式”。
# 这句话的意思是：当系统进入多用户模式时，请务必把我带上！
EOF
```
#### 2)激活服务
```bash
# 1. 刷新系统配置，让它知道新加了个服务
sudo systemctl daemon-reload

# 2. 设置开机自启 (Enable)
sudo systemctl enable ollama

# 3. 立刻启动服务 (Start)
sudo systemctl start ollama
```

#### 3)检查状态
```bash
systemctl status ollama
```

### 下载模型

```bash
# 1.下载大模型 (Qwen):
ollama run qwen2.5:7b
# 2.下载向量模型 (Embedding):
ollama pull nomic-embed-text
```

## 配置 chatchat

设置chatchat配置路径
```bash
export CHATCHAT_ROOT=$HOME/chatchat_data
```
初始化chatchat
```bash
chatchat init
```

### 配置模型(model_settings.yaml)
1.打开文件
```bash
code ~/chatchat_data/model_settings.yaml
```
2.修改模型配置
- 修改字段`DEFAULT_LLM_MODEL`
	- 值: `qwen2.5:7b`
- 修改字段`DEFAULT_EMBEDDING_MODEL`
	- 值: `nomic-embed-text`
- 修改`platform_name: ollama`下
	- `llm_models`, `embed_models`字段添加对应的模型`qwen2.5:7b`, `nomic-embed-text`
![[chatchat项目部署.md_Attachments/chatchat项目部署-20260107205757414.png|300]]
### 配置知识库


