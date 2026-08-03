# 课程入门指南

我们非常高兴你能开始这门课程，并期待看到你使用生成式 AI 能构建出什么有趣的作品！

为了帮助你顺利完成课程，本页将说明环境准备步骤、技术要求，以及遇到困难时在哪里可以获取帮助。

## 准备步骤

要开始学习本课程，你需要完成以下步骤。

### 1. Fork 本仓库

将[整个仓库 Fork](https://github.com/microsoft/generative-ai-for-beginners/fork?WT.mc_id=academic-105485-koreyst) 到你自己的 GitHub 账号下，以便修改代码并完成各项挑战。你也可以[给本仓库点星（🌟）](https://docs.github.com/en/get-started/exploring-projects-on-github/saving-repositories-with-stars?WT.mc_id=academic-105485-koreyst)，方便日后查找它及相关的仓库。

### 2. 创建 codespace

为避免在运行代码时出现依赖问题，我们建议在本课程的 [GitHub Codespaces](https://github.com/features/codespaces?WT.mc_id=academic-105485-koreyst) 中运行。

在你的 Fork 仓库中：**Code -> Codespaces -> New on main**

![显示创建 codespace 按钮的对话框](./images/who-will-pay.webp?WT.mc_id=academic-105485-koreyst)

#### 2.1 添加密钥

1. 点击 ⚙️ 齿轮图标 -> 命令面板（Command Pallete）-> Codespaces：管理用户密钥 -> 添加新密钥。
2. 名称填写 OPENAI_API_KEY，粘贴你的密钥，保存。

### 3. 接下来做什么？

| 我想…               | 前往…                                                                 |
|---------------------|-----------------------------------------------------------------------|
| 开始第 1 课         | [`01-introduction-to-genai`](../01-introduction-to-genai/README.md)  |
| 离线学习            | [`setup-local.md`](02-setup-local.md)                                 |
| 配置 LLM 服务商     | [`providers.md`](03-providers.md)                                      |
| 结识其他学习者      | [加入我们的 Discord](https://aka.ms/genai-discord?WT.mc_id=academic-105485-koreyst) |

## 故障排查

| 现象                                          | 解决办法                                                            |
|-----------------------------------------------|---------------------------------------------------------------------|
| 容器构建卡住超过 10 分钟                      | **Codespaces ➜ “重新构建容器”（Rebuild Container）**               |
| 提示 `python: command not found`              | 终端未正确挂载；点击 **+** ➜ *bash*                                |
| 来自 OpenAI 的 `401 Unauthorized`             | `OPENAI_API_KEY` 错误 / 已过期                                     |
| VS Code 显示 “Dev container mounting…”        | 刷新浏览器标签页——Codespaces 有时会出现连接丢失                    |
| Notebook 内核缺失                             | Notebook 菜单 ➜ **Kernel ▸ Select Kernel ▸ Python 3**              |

   Unix 系统：

   ```bash
   touch .env
   ```

   Windows：

   ```cmd
   echo . > .env
   ```

3. **编辑 `.env` 文件**：在文本编辑器（例如 VS Code、Notepad++ 或任意编辑器）中打开 `.env` 文件。在文件中添加以下内容，将占位符替换为你实际的 Microsoft Foundry Models 终结点和密钥（如何获取请参见 [`providers.md`](03-providers.md)）：

   > **注意：** GitHub Models（及其 `GITHUB_TOKEN` 变量）将于 2026 年 7 月底停用。请改用 [Microsoft Foundry Models](https://ai.azure.com/catalog/models?WT.mc_id=academic-105485-koreyst)。

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

## 如何在本地计算机上运行

要在本地计算机上运行代码，你需要安装某个版本的 [Python](https://www.python.org/downloads/?WT.mc_id=academic-105485-koreyst)。

随后要使用本仓库，你需要克隆它：

```shell
git clone https://github.com/microsoft/generative-ai-for-beginners
cd generative-ai-for-beginners
```

完成上述操作后，你便可以开始学习了！

## 可选步骤

### 安装 Miniconda

[Miniconda](https://conda.io/en/latest/miniconda.html?WT.mc_id=academic-105485-koreyst) 是一个轻量级的安装程序，用于安装 [Conda](https://docs.conda.io/en/latest?WT.mc_id=academic-105485-koreyst)、Python 以及少量包。
Conda 本身是一个包管理器，可以方便地搭建并在不同的 Python [**虚拟环境**](https://docs.python.org/3/tutorial/venv.html?WT.mc_id=academic-105485-koreyst) 与包之间切换。它对于安装 `pip` 无法获取的包也很有帮助。

你可以按照 [MiniConda 安装指南](https://docs.anaconda.com/free/miniconda/#quick-command-line-install?WT.mc_id=academic-105485-koreyst) 进行配置。

安装好 Miniconda 后，你需要克隆[仓库](https://github.com/microsoft/generative-ai-for-beginners/fork?WT.mc_id=academic-105485-koreyst)（如果尚未克隆）。

接下来，你需要创建一个虚拟环境。使用 Conda 时，请新建一个环境文件（_environment.yml_）。如果你使用 Codespaces 跟随操作，请在 `.devcontainer` 目录中创建，即 `.devcontainer/environment.yml`。

请在你的环境文件中填入以下片段：

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

如果你在使用 conda 时遇到错误，可以在终端中使用以下命令手动安装 Microsoft AI 库：

```
conda install -c microsoft azure-ai-ml
```

环境文件指定了我们需要的依赖项。`<environment-name>` 指你希望为 Conda 环境使用的名称，`<python-version>` 是你希望使用的 Python 版本，例如 `3` 表示最新的 Python 主版本。

完成后，你可以在命令行/终端中运行以下命令来创建 Conda 环境：

```bash
conda env create --name ai4beg --file .devcontainer/environment.yml # .devcontainer 子路径仅适用于 Codespace 环境
conda activate ai4beg
```

如遇任何问题，请参阅 [Conda 环境指南](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html?WT.mc_id=academic-105485-koreyst)。

### 配合 Visual Studio Code 及 Python 支持插件使用

我们推荐使用 [Visual Studio Code（VS Code）](https://code.visualstudio.com/?WT.mc_id=academic-105485-koreyst) 编辑器，并安装 [Python 支持插件](https://marketplace.visualstudio.com/items?itemName=ms-python.python&WT.mc_id=academic-105485-koreyst) 来学习本课程。不过，这更多是一个建议而非硬性要求。

> **注意**：在 VS Code 中打开课程仓库时，你可以选择将项目搭建在容器内。这是因为课程仓库中包含[特殊的 `.devcontainer`](https://code.visualstudio.com/docs/devcontainers/containers?itemName=ms-python.python&WT.mc_id=academic-105485-koreyst) 目录。更多信息将在后文说明。

> **注意**：一旦你克隆并在 VS Code 中打开目录，它会自动建议你安装 Python 支持插件。

> **注意**：如果 VS Code 建议你重新在容器中打开仓库，请拒绝该请求，以便使用本地安装的 Python 版本。

### 在浏览器中使用 Jupyter

你也可以通过浏览器中的 [Jupyter 环境](https://jupyter.org?WT.mc_id=academic-105485-koreyst) 来完成项目。经典的 Jupyter 和 [Jupyter Hub](https://jupyter.org/hub?WT.mc_id=academic-105485-koreyst) 都提供了相当友好的开发环境，具备自动补全、代码高亮等特性。

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

### 在容器中运行

除了在本地计算机或 Codespace 中完成所有配置外，另一种选择是使用[容器](<https://en.wikipedia.org/wiki/Containerization_(computing)?WT.mc_id=academic-105485-koreyst>)。课程仓库中特殊的 `.devcontainer` 文件夹使得 VS Code 能够在容器内搭建项目。在 Codespaces 之外使用此方式需要安装 Docker，而且坦白说，这涉及一定的工作量，因此我们仅建议有容器使用经验的用户采用。

在使用 GitHub Codespaces 时，保护 API 密钥安全的最佳方式之一是使用 Codespace 密钥。请参阅 [Codespaces 密钥管理](https://docs.github.com/en/codespaces/managing-your-codespaces/managing-secrets-for-your-codespaces?WT.mc_id=academic-105485-koreyst) 指南了解更多。

## 课程内容与技术要求

本课程包含讲解生成式 AI 概念的“学习”课，以及提供动手代码实例的“构建”课，在可能的情况下使用 **Python** 和 **TypeScript** 两种语言。

对于编程课，我们使用 Microsoft Foundry 中的 Azure OpenAI。你需要一个 Azure 订阅和 API 密钥。访问是开放的——无需申请——你可以[创建 Microsoft Foundry 资源并部署模型](https://learn.microsoft.com/azure/ai-foundry/openai/how-to/create-resource?pivots=web-portal&WT.mc_id=academic-105485-koreyst) 来获取你的终结点和密钥。

每节编程课还包含一个 `README.md` 文件，你无需运行任何代码即可查看代码和输出。

## 首次使用 Azure OpenAI 服务

如果你是第一次使用 Azure OpenAI 服务，请按照此指南了解如何[创建并部署 Azure OpenAI 服务资源。](https://learn.microsoft.com/azure/ai-foundry/openai/how-to/create-resource?pivots=web-portal&WT.mc_id=academic-105485-koreyst)

## 首次使用 OpenAI API

如果你是第一次使用 OpenAI API，请按照指南了解如何[创建并使用该接口。](https://platform.openai.com/docs/quickstart?context=pythont&WT.mc_id=academic-105485-koreyst)

## 结识其他学习者

我们在官方 [AI Community Discord 服务器](https://aka.ms/genai-discord?WT.mc_id=academic-105485-koreyst) 中创建了频道，用于结识其他学习者。这是一个与其他志同道合的企业家、构建者、学生以及任何希望在生成式 AI 领域提升自我的人建立联系的绝佳方式。

[![加入 discord 频道](https://dcbadge.limes.pink/api/server/ByRwuEEgH4)](https://aka.ms/genai-discord?WT.mc_id=academic-105485-koreyst)

项目团队也会在这个 Discord 服务器上帮助各位学习者。

## 贡献

本课程是一个开源项目。如果你发现可以改进的地方或存在问题，请创建 [Pull Request](https://github.com/microsoft/generative-ai-for-beginners/pulls?WT.mc_id=academic-105485-koreyst) 或提交 [GitHub issue](https://github.com/microsoft/generative-ai-for-beginners/issues?WT.mc_id=academic-105485-koreyst)。

项目团队会跟踪所有贡献。参与开源是在生成式 AI 领域积累职业经验的一种绝佳方式。

大多数贡献都要求你同意一份贡献者许可协议（CLA），声明你有权并且确实授予我们使用你贡献内容的权利。详情请访问 [CLA，贡献者许可协议网站](https://cla.microsoft.com?WT.mc_id=academic-105485-koreyst)。

重要提示：在翻译本仓库中的文本时，请勿使用机器翻译。我们将通过社区对译文进行核对，因此请仅在你能熟练掌握的语言范围内自愿参与翻译。

当你提交 Pull Request 时，CLA 机器人会自动判断你是否需要提供 CLA，并对 PR 进行相应标记（例如标签、评论）。只需按照机器人给出的说明操作即可。在所有使用我们 CLA 的仓库中，你只需完成一次这样的操作。

本项目采用了 [Microsoft 开源行为准则](https://opensource.microsoft.com/codeofconduct/?WT.mc_id=academic-105485-koreyst)。如需更多信息，请阅读行为准则 FAQ，或通过 [Email opencode](opencode@microsoft.com) 联系我们提出其他问题或意见。

## 让我们开始吧

现在你已经完成了本课程所需的准备步骤，让我们通过 [生成式 AI 与 LLM 简介](../01-introduction-to-genai/README.md?WT.mc_id=academic-105485-koreyst) 来正式开始吧。
