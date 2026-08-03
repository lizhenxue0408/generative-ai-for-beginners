# 选择并配置 LLM 服务商 🔑

作业**也可以**配置为通过受支持的服务商（如 OpenAI、Azure 或 Hugging Face）对接一个或多个大语言模型（LLM）部署。这些服务商提供可由程序访问的*托管终结点*（API），访问时需要使用正确的凭据（API 密钥或令牌）。在本课程中，我们讨论以下服务商：

 - [OpenAI](https://platform.openai.com/docs/models?WT.mc_id=academic-105485-koreyst)，包含多样化的模型，包括核心的 GPT 系列。
 - [Azure OpenAI](https://learn.microsoft.com/azure/ai-foundry/openai/?WT.mc_id=academic-105485-koreyst)，面向企业级应用的 OpenAI 模型。
 - [Microsoft Foundry Models](https://ai.azure.com/catalog/models?WT.mc_id=academic-105485-koreyst)，通过单一终结点和 API 密钥即可访问来自 OpenAI、Meta、Mistral、Cohere、Microsoft 等的数百种模型（取代 GitHub Models，后者将于 2026 年 7 月底停用）。
 - [Hugging Face](https://huggingface.co/docs/hub/index?WT.mc_id=academic-105485-koreyst)，提供开源模型及推理服务器。
 - [Foundry Local](https://foundrylocal.ai?WT.mc_id=academic-105485-koreyst) 或 [Ollama](https://ollama.com/?WT.mc_id=academic-105485-koreyst)，如果你希望完全离线、在自有设备上运行模型，且无需云订阅。

**你需要使用自己的账号来完成这些练习**。作业是可选的，因此你可以根据自己的兴趣，选择配置其中一家、全部，或一家都不配置。一些注册指引：

| 注册 | 费用 | API 密钥 | 在线试用 | 说明 |
|:---|:---|:---|:---|:---|
| [OpenAI](https://platform.openai.com/signup?WT.mc_id=academic-105485-koreyst)| [定价](https://openai.com/pricing#language-models?WT.mc_id=academic-105485-koreyst)| [基于项目](https://platform.openai.com/api-keys?WT.mc_id=academic-105485-koreyst) | [无代码，网页版](https://platform.openai.com/playground?WT.mc_id=academic-105485-koreyst) | 提供多种模型 |
| [Azure](https://aka.ms/azure/free?WT.mc_id=academic-105485-koreyst)| [定价](https://azure.microsoft.com/pricing/details/cognitive-services/openai-service/?WT.mc_id=academic-105485-koreyst)| [SDK 快速入门](https://learn.microsoft.com/azure/ai-foundry/openai/quickstart?WT.mc_id=academic-105485-koreyst)| [Studio 快速入门](https://learn.microsoft.com/azure/ai-foundry/openai/quickstart?WT.mc_id=academic-105485-koreyst) |  [需提前申请访问权限](https://learn.microsoft.com/azure/ai-foundry/openai/?WT.mc_id=academic-105485-koreyst)|
| [Microsoft Foundry](https://ai.azure.com?WT.mc_id=academic-105485-koreyst) | [定价](https://azure.microsoft.com/pricing/details/ai-foundry/?WT.mc_id=academic-105485-koreyst) | [项目概览页](https://learn.microsoft.com/azure/ai-foundry/model-inference/overview?WT.mc_id=academic-105485-koreyst) | [Foundry 在线试用](https://ai.azure.com/catalog/models?WT.mc_id=academic-105485-koreyst) | 提供免费层；一个终结点 + 密钥可对接多个模型服务商 |
| [Hugging Face](https://huggingface.co/join?WT.mc_id=academic-105485-koreyst) | [定价](https://huggingface.co/pricing) | [访问令牌](https://huggingface.co/docs/hub/security-tokens?WT.mc_id=academic-105485-koreyst) | [Hugging Chat](https://huggingface.co/chat/?WT.mc_id=academic-105485-koreyst)| [Hugging Chat 支持的模型有限](https://huggingface.co/chat/models?WT.mc_id=academic-105485-koreyst) |
| [Foundry Local](https://foundrylocal.ai?WT.mc_id=academic-105485-koreyst) | 免费（在你的设备上运行） | 无需 | [本地 CLI/SDK](https://learn.microsoft.com/azure/ai-foundry/foundry-local/get-started?WT.mc_id=academic-105485-koreyst) | 完全离线，兼容 OpenAI 的终结点 |
| | | | | |

按照以下说明将本仓库*配置* 为对接不同的服务商使用。需要特定服务商的作业，会在文件名中包含以下标签之一：

- `aoai` - 需要 Azure OpenAI 终结点、密钥
- `oai` - 需要 OpenAI 终结点、密钥
- `hf` - 需要 Hugging Face 令牌
- `githubmodels` - 需要 Microsoft Foundry Models 终结点、密钥（GitHub Models 将于 2026 年 7 月底停用）

你可以配置其中一家、一家都不配置，或全部配置。相关的作业在缺少凭据时会直接报错。

## 创建 `.env` 文件

我们假设你已经阅读了上述指引，并已向相关服务商注册，且获取了所需的身份验证凭据（API_KEY 或令牌）。对于 Azure OpenAI，我们假设你已拥有一个有效的 Azure OpenAI 服务（终结点）部署，并至少部署了一个用于聊天补全的 GPT 模型。

下一步是配置你的**本地环境变量**，如下所示：

1. 在根文件夹中查找 `.env.copy` 文件，其内容应如下所示：

   ```bash
   # OpenAI 服务商
   OPENAI_API_KEY='<在此添加你的 OpenAI API 密钥>'

   ## Microsoft Foundry 中的 Azure OpenAI
   ## （Azure OpenAI 服务现已成为 Microsoft Foundry 的一部分：https://ai.azure.com）
   AZURE_OPENAI_API_VERSION='2024-10-21' # 已设置默认值！（当前稳定 GA API 版本）
   AZURE_OPENAI_API_KEY='<在此添加你的 Foundry 资源密钥>'
   AZURE_OPENAI_ENDPOINT='<在此添加你的 Foundry 资源终结点，例如 https://<resource-name>.openai.azure.com>'
   AZURE_OPENAI_DEPLOYMENT='<在此添加你的聊天补全模型部署名称，例如 gpt-5-mini>'
   AZURE_OPENAI_EMBEDDINGS_DEPLOYMENT='<在此添加你的嵌入模型部署名称，例如 text-embedding-3-small>'

   ## Microsoft Foundry Models（多服务商模型目录，取代 GitHub Models，后者于 2026 年 7 月底停用）
   AZURE_INFERENCE_ENDPOINT='<在此添加你的 Microsoft Foundry 项目终结点>'
   AZURE_INFERENCE_CREDENTIAL='<在此添加你的 Microsoft Foundry Models API 密钥>'

   ## Hugging Face
   HUGGING_FACE_API_KEY='<在此添加你的 HuggingFace API 或令牌>'
   ```

2. 使用以下命令将该文件复制为 `.env`。此文件已被 gitignore，可保障密钥安全。

   ```bash
   cp .env.copy .env
   ```

3. 填写各值（替换 `=` 右侧的占位符），详见下一节。

4. （可选）如果你使用 GitHub Codespaces，可以选择将环境变量保存为与该仓库关联的 *Codespaces 密钥*。在这种情况下，你无需配置本地 `.env` 文件。**但请注意，此选项仅在你使用 GitHub Codespaces 时有效。** 如果你改用 Docker Desktop，仍然需要配置 `.env` 文件。

## 填充 `.env` 文件

让我们快速了解一下这些变量名称及其含义：

| 变量  | 说明  |
| :--- | :--- |
| HUGGING_FACE_API_KEY | 这是你在个人资料中设置的用户访问令牌 |
| OPENAI_API_KEY | 这是用于非 Azure OpenAI 终结点调用该服务的授权密钥 |
| AZURE_OPENAI_API_KEY | 这是用于调用该服务的授权密钥 |
| AZURE_OPENAI_ENDPOINT | 这是 Azure OpenAI 资源的已部署终结点 |
| AZURE_OPENAI_DEPLOYMENT | 这是*文本生成*模型的部署终结点 |
| AZURE_OPENAI_EMBEDDINGS_DEPLOYMENT | 这是*文本嵌入*模型的部署终结点 |
| AZURE_INFERENCE_ENDPOINT | 这是你的 Microsoft Foundry 项目的终结点，用于 Microsoft Foundry Models |
| AZURE_INFERENCE_CREDENTIAL | 这是你的 Microsoft Foundry 项目的 API 密钥 |
| | |

注意：最后两个 Azure OpenAI 变量分别对应聊天补全（文本生成）和向量搜索（嵌入）的默认模型。它们的设置说明会在相关作业中给出。

## 配置 Azure OpenAI：通过门户

> **注意：** Azure OpenAI 服务现已成为 [Microsoft Foundry](https://ai.azure.com?WT.mc_id=academic-105485-koreyst) 的一部分。资源和部署仍会显示在 Azure 门户中，但日常模型管理（部署、在线试用、监控）现已转移到 Foundry 门户，而非原先独立的“Azure OpenAI Studio”。

Azure OpenAI 的终结点和密钥值可在 [Azure 门户](https://portal.azure.com?WT.mc_id=academic-105485-koreyst) 中找到，因此我们从那里开始。

1. 前往 [Azure 门户](https://portal.azure.com?WT.mc_id=academic-105485-koreyst)
1. 点击侧边栏（左侧菜单）中的 **密钥和终结点（Keys and Endpoint）** 选项。
1. 点击 **显示密钥（Show Keys）** - 你应该会看到：KEY 1、KEY 2 和终结点。
1. 将 KEY 1 的值用于 AZURE_OPENAI_API_KEY
1. 将终结点的值用于 AZURE_OPENAI_ENDPOINT

接下来，我们需要已部署具体模型的终结点。

1. 点击 Azure OpenAI 资源侧边栏（左侧菜单）中的 **模型部署（Model deployments）** 选项。
1. 在目标页面中，点击 **前往 Microsoft Foundry 门户**（或 **管理部署 Manage Deployments**，取决于你的资源类型）

这将带你进入 Microsoft Foundry 门户，我们将在其中找到如下文所述的其他值。

## 配置 Azure OpenAI：通过 Microsoft Foundry 门户

1. 按上述说明**从你的资源**导航到 [Microsoft Foundry 门户](https://ai.azure.com?WT.mc_id=academic-105485-koreyst)。
1. 点击侧边栏（左侧）的 **部署（Deployments）** 标签，查看当前已部署的模型。
1. 如果你想要的模型尚未部署，请使用 **部署模型（Deploy model）** 从[模型目录](https://ai.azure.com/catalog/models?WT.mc_id=academic-105485-koreyst) 中部署它。
1. 你需要一个*文本生成*模型 - 我们推荐：**gpt-5-mini**
1. 你需要一个*文本嵌入*模型 - 我们推荐 **text-embedding-3-small**

现在更新环境变量，以反映所使用的*部署名称*。该名称通常与模型名称一致，除非你显式进行了修改。例如，可能会有：

```bash
AZURE_OPENAI_DEPLOYMENT='gpt-5-mini'
AZURE_OPENAI_EMBEDDINGS_DEPLOYMENT='text-embedding-3-small'
```

**完成后别忘了保存 `.env` 文件**。你现在可以退出文件，返回运行 notebook 的说明。

## 配置 OpenAI：通过个人资料

你的 OpenAI API 密钥可在你的 [OpenAI 账户](https://platform.openai.com/api-keys?WT.mc_id=academic-105485-koreyst) 中找到。如果你还没有账户，可以注册一个并创建 API 密钥。获得密钥后，你可以用它来填充 `.env` 文件中的 `OPENAI_API_KEY` 变量。

## 配置 Hugging Face：通过个人资料

你的 Hugging Face 令牌可在个人资料下的 [访问令牌](https://huggingface.co/settings/tokens?WT.mc_id=academic-105485-koreyst) 中找到。请勿公开发布或分享。相反，为本项目使用创建一个新令牌，并将其复制到 `.env` 文件的 `HUGGING_FACE_API_KEY` 变量下。*注意：* 这在技术上并非 API 密钥，而是用于身份验证，为保持一致性我们沿用该命名。

## 配置 Microsoft Foundry Models：通过门户

> **注意：** GitHub Models 将于 2026 年 7 月底停用。Microsoft Foundry Models 是直接替代方案，提供同样的免费试用模型目录以及 Azure AI Inference SDK / OpenAI SDK 体验。

1. 前往 [Microsoft Foundry](https://ai.azure.com?WT.mc_id=academic-105485-koreyst) 并创建（或打开）一个 Foundry 项目。
1. 浏览[模型目录](https://ai.azure.com/catalog/models?WT.mc_id=academic-105485-koreyst) 并部署一个模型，例如 `gpt-5-mini`。
1. 在项目**概览（Overview）**页面，复制**终结点**和 **API 密钥**。
1. 在你的 `.env` 文件中，将终结点值用于 `AZURE_INFERENCE_ENDPOINT`，将密钥值用于 `AZURE_INFERENCE_CREDENTIAL`。

## 离线 / 本地服务商

如果你完全不想使用云订阅，可以直接在自己的设备上运行兼容的开源模型：

- **[Foundry Local](https://foundrylocal.ai?WT.mc_id=academic-105485-koreyst)** - 微软的设备端运行时。它会自动选择最佳执行提供方（NPU、GPU 或 CPU），并暴露一个兼容 OpenAI 的终结点，因此你只需做极少改动即可复用本课程中的大部分示例代码。请参阅 [Foundry Local 文档](https://learn.microsoft.com/azure/ai-foundry/foundry-local/get-started?WT.mc_id=academic-105485-koreyst) 入门，或使用 `winget install Microsoft.FoundryLocal`（Windows）/ `brew install microsoft/foundrylocal/foundrylocal`（macOS）安装。
- **[Ollama](https://ollama.com/?WT.mc_id=academic-105485-koreyst)** - 一个流行的替代方案，可在本地运行 Llama、Phi、Mistral、Gemma 等开源模型。

有关两种方案的动手示例，请参阅 [第 19 课：使用 SLM 构建](../19-slm/README.md?WT.mc_id=academic-105485-koreyst)。
