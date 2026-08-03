# 本地配置 🖥️

**如果你更愿意在自己的笔记本电脑上运行所有内容，请使用本指南。**  
你有两条路径可选：**（A）原生 Python + 虚拟环境**，或 **（B）带 Docker 的 VS Code 开发容器**。  
选择更轻松的那一种即可——两条路径最终都能进入相同的课程。

## 1. 先决条件

| 工具                | 版本 / 说明                                                                         |
|---------------------|-------------------------------------------------------------------------------------|
| **Python**          | 3.10 +（从 <https://python.org> 获取）                                              |
| **Git**             | 最新版（随 Xcode / Git for Windows / Linux 包管理器 一同提供）                      |
| **VS Code**         | 可选但推荐使用 <https://code.visualstudio.com>                                       |
| **Docker Desktop**  | *仅* 方案 B 需要。免费安装：<https://docs.docker.com/desktop/>                       |

> 💡 **小贴士** – 在终端中验证工具是否就绪：  
> `python --version`、`git --version`、`docker --version`、`code --version`

## 2. 方案 A – 原生 Python（最快捷）

### 步骤 1 克隆本仓库

```bash
git clone https://github.com/<your-github>/generative-ai-for-beginners
cd generative-ai-for-beginners
```

### 步骤 2 创建并激活虚拟环境

```bash
python -m venv .venv          # 创建一个
source .venv/bin/activate     # macOS / Linux
.\.venv\Scripts\activate      # Windows PowerShell
```

✅ 提示符现在应以 (.venv) 开头——这意味着你已进入该环境。

### 步骤 3 安装依赖

```bash
pip install -r requirements.txt
```

跳转到第 3 节 [API 密钥](#3-添加你的-api-密钥)

## 2. 方案 B – VS Code 开发容器（Docker）

我们为这个仓库和课程配置了[开发容器](https://containers.dev?WT.mc_id=academic-105485-koreyst)，它内置通用运行时，可支持 Python3、.NET、Node.js 和 Java 开发。相关配置定义在仓库根目录 `.devcontainer/` 文件夹下的 `devcontainer.json` 文件中。

>**为什么选择这个？**
>与 Codespaces 完全一致的环境；不会出现依赖漂移。

### 步骤 0 安装附加组件

Docker Desktop – 确认 ```docker --version``` 可用。
VS Code Remote – Containers 扩展（ID：ms-vscode-remote.remote-containers）。

### 步骤 1 在 VS Code 中打开仓库

文件 ▸ 打开文件夹…  → generative-ai-for-beginners

VS Code 检测到 .devcontainer/ 后会弹出提示。

### 步骤 2 在容器中重新打开

点击“在容器中重新打开”（Reopen in Container）。Docker 开始构建镜像（首次约 3 分钟）。
当终端提示符出现时，说明你已进入容器内部。

## 2. 方案 C – Miniconda

[Miniconda](https://conda.io/en/latest/miniconda.html?WT.mc_id=academic-105485-koreyst) 是一个轻量级的安装程序，用于安装 [Conda](https://docs.conda.io/en/latest?WT.mc_id=academic-105485-koreyst)、Python 以及少量包。
Conda 本身是一个包管理器，可以方便地搭建并在不同的 Python [**虚拟环境**](https://docs.python.org/3/tutorial/venv.html?WT.mc_id=academic-105485-koreyst) 与包之间切换。它对于安装 `pip` 无法获取的包也很有帮助。

### 步骤 0 安装 Miniconda

按照 [MiniConda 安装指南](https://docs.anaconda.com/free/miniconda/#quick-command-line-install?WT.mc_id=academic-105485-koreyst) 进行配置。

```bash
conda --version
```

### 步骤 1 创建虚拟环境

新建一个环境文件（*environment.yml*）。如果你使用 Codespaces 跟随操作，请在 `.devcontainer` 目录中创建，即 `.devcontainer/environment.yml`。

### 步骤 2 填充你的环境文件

将以下片段添加到你的 `environment.yml` 中：

```yml
name: <environment-name>
channels:
 - defaults
 - microsoft
dependencies:
- python=<python-version>
- openai
- python-dotenv
- pip
- pip:
    - azure-ai-ml

```

### 步骤 3 创建你的 Conda 环境

在命令行/终端中运行以下命令：

```bash 
conda env create --name ai4beg --file .devcontainer/environment.yml # .devcontainer 子路径仅适用于 Codespace 环境
conda activate ai4beg
```

如遇任何问题，请参阅 [Conda 环境指南](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html?WT.mc_id=academic-105485-koreyst)。

## 2 方案 D – 经典 Jupyter / Jupyter Lab（浏览器内）

> **适用人群？**  
> 喜欢经典 Jupyter 界面，或希望不依赖 VS Code 运行 notebook 的任何人。

### 步骤 1 确保已安装 Jupyter

要在本地启动 Jupyter，请前往终端/命令行，进入课程目录，然后执行：

```bash
jupyter notebook
```

或者：

```bash
jupyterhub
```

这将启动一个 Jupyter 实例，访问地址（URL）会显示在命令行窗口中。

访问该 URL 后，你应该能看到课程大纲，并能够导航到任意 `*.ipynb` 文件。例如 `08-building-search-applications/python/oai-solution.ipynb`。

## 3. 添加你的 API 密钥

在构建任何类型的应用时，妥善保护你的 API 密钥安全都十分重要。我们建议不要将任何 API 密钥直接存储在代码中。将这些信息提交到公开仓库可能会导致安全问题，甚至在被恶意使用者利用时产生不必要的费用。
以下是一份逐步指南，介绍如何为 Python 创建 `.env` 文件并添加你的 Microsoft Foundry Models 凭据：

> **注意：** GitHub Models（及其 `GITHUB_TOKEN` 变量）将于 2026 年 7 月底停用。本指南改用 [Microsoft Foundry Models](https://ai.azure.com/catalog/models?WT.mc_id=academic-105485-koreyst)。希望完全离线工作？请参阅 [Foundry Local](https://foundrylocal.ai?WT.mc_id=academic-105485-koreyst)。

1. **导航到你的项目目录**：打开终端或命令提示符，进入你希望创建 `.env` 文件的项目根目录。

   ```bash
   cd path/to/your/project
   ```

2. **创建 `.env` 文件**：使用你偏好的文本编辑器创建一个名为 `.env` 的新文件。如果你使用命令行，可以使用 `touch`（Unix 系统）或 `echo`（Windows）：

   Unix 系统：

   ```bash
   touch .env
   ```

   Windows：

   ```cmd
   echo . > .env
   ```

3. **编辑 `.env` 文件**：在文本编辑器（例如 VS Code、Notepad++ 或任意编辑器）中打开 `.env` 文件。在文件中添加以下内容，将占位符替换为你实际的 Microsoft Foundry 项目终结点和 API 密钥：

   ```env
   AZURE_INFERENCE_ENDPOINT=your_foundry_endpoint_here
   AZURE_INFERENCE_CREDENTIAL=your_foundry_api_key_here
   ```

4. **保存文件**：保存修改并关闭文本编辑器。

5. **安装 `python-dotenv`**：如果尚未安装，你需要安装 `python-dotenv` 包，以便从 `.env` 文件将环境变量加载到 Python 应用中。可以使用 `pip` 进行安装：

   ```bash
   pip install python-dotenv
   ```

6. **在 Python 脚本中加载环境变量**：在你的 Python 脚本中，使用 `python-dotenv` 包从 `.env` 文件加载环境变量：

   ```python
   from dotenv import load_dotenv
   import os

   # 从 .env 文件加载环境变量
   load_dotenv()

   # 获取 Microsoft Foundry Models 变量
   endpoint = os.getenv("AZURE_INFERENCE_ENDPOINT")
   token = os.getenv("AZURE_INFERENCE_CREDENTIAL")

   print(endpoint)
   ```

就这样！你已成功创建了 `.env` 文件，添加了 Microsoft Foundry Models 凭据，并将其加载到了 Python 应用中。

🔐 切勿提交 .env——它已在 .gitignore 中。
完整的服务商配置说明见 [`providers.md`](03-providers.md)。

## 4. 接下来做什么？

| 我想…               | 前往…                                                                 |
|---------------------|-----------------------------------------------------------------------|
| 开始第 1 课         | [`01-introduction-to-genai`](../01-introduction-to-genai/README.md)  |
| 配置 LLM 服务商      | [`providers.md`](03-providers.md)                                     |
| 结识其他学习者      | [加入我们的 Discord](https://aka.ms/genai-discord?WT.mc_id=academic-105485-koreyst) |

## 5. 故障排查

| 现象                                          | 解决办法                                                            |
|-----------------------------------------------|---------------------------------------------------------------------|
| `python not found`                            | 将 Python 添加到 PATH，或在安装后重新打开终端                       |
| `pip` 无法构建 wheel（Windows）               | 先执行 `pip install --upgrade pip setuptools wheel` 再重试          |
| `ModuleNotFoundError: dotenv`                 | 运行 `pip install -r requirements.txt`（环境尚未安装）              |
| Docker 构建失败 *No space left*               | Docker Desktop ▸ *Settings* ▸ *Resources* → 增大磁盘大小            |
| VS Code 不断提示重新打开                      | 你可能同时启用了多个方案；请选择其一（venv **或** 容器）            |
| OpenAI 401 / 429 错误                         | 检查 `OPENAI_API_KEY` 的值 / 请求速率限制                           |
| 使用 Conda 出错                               | 使用 `conda install -c microsoft azure-ai-ml` 安装 Microsoft AI 库  |
