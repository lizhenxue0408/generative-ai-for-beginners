# 更新日志（CHANGELOG，中文版）

本文件记录了 Generative AI for Beginners 课程的所有重要变更。

格式基于 [Keep a Changelog](https://keepachangelog.com/en/1.1.0/)。由于这是一个学习课程而非带版本号的软件包，因此条目按日期分组。

## [2026-07-16] — 内容校验 + 第 09 课图片资源

### 变更（Changed）

- **第 10 课（低代码 AI 应用）**：将两个已停用的 `docs.microsoft.com/powerapps/...` Dataverse 链接更新为当前的 `learn.microsoft.com/power-apps/maker/data-platform/data-platform-intro`（已验证可用）。
- **第 17 课（AI agents）**：将一个过时的模型示例（`GPT-3.5, GPT-4, Llama-2` → `GPT-5, GPT-4o, and Llama 3.3`）以及 Agent Framework 示例中的占位部署名（`my-gpt-4o-deployment` → `my-gpt-5-mini-deployment`）现代化。
- **根目录 `README.md`**：为 *Microsoft for Startups* 链接补上了缺失的 `?WT.mc_id=academic-105485-koreyst` 跟踪 ID。
- **第 09 课图片资源** 使用 `gpt-image` 模型重新生成：`images/generated-image.png`、`images/sunlit_lounge.png`、`images/mask.png`、`images/sunlit_lounge_result.png` 以及 `images/startup.png`（编辑示例的前后对比图对是通过真实的 `client.images.edit` 调用并配合生成的遮罩（mask）生成的）。

### 校验（Validated）

- 审查了第 01、03、05、12、14、16 课的 README——均为最新内容（正确的 Microsoft Foundry 命名与链接）；无需改动。
- 对所有 41 个仓库内 Markdown 文件（翻译除外）运行了完整的 Markdown 校验，检查项包括：已弃用的文档路径、`/en-us/` 形式的 Microsoft 区域设置、过时的产品/模型名称、缺失的跟踪 ID，以及失效的相对链接/图片。唯一可处理的问题是 *Microsoft for Startups* 缺少跟踪 ID；所有其他标记均被确认为误报（自动生成的翻译链接、已注释掉的占位符，以及第三方的 `/en/` 结构性 URL）。

## [2026-07-15] — 第 09 课（图像应用）针对 GPT 图像模型重写

### 变更（Changed）

- **重写第 09 课 "Building Image Generation Applications"（构建图像生成应用）**，围绕当前的 **`gpt-image`** 模型系列（默认 **`gpt-image-2`**；`gpt-image-1.5` / `gpt-image-1-mini` 也已正式可用），替换了旧的 DALL·E 2/3 内容。关键修正：
  - `gpt-image` 模型以 **base64（`b64_json`）** 形式返回图像，而非 URL。将所有示例更新为使用 `base64.b64decode(...)`，而非用 `requests` 下载 `url`。
  - 将图像 API 版本提升至 `2025-04-01-preview`。
  - 将编造的 "temperature" 小节（图像模型不接受 `temperature`）以及仅适用于 DALL·E-2 的图像**变体（variations）**内容替换为**图像编辑**（遮罩/修复（mask/inpainting））小节。
  - 更新了 `README.md`、`python/aoai-app.py`、`python/oai-app.py`、`python/aoai-solution.py`、两个作业 notebook（`aoai-assignment.ipynb`、`oai-assignment.ipynb`）、`typescript/image-generation-app`（`main.ts`、`.env-sample`）以及 .NET 的 `.dib` notebook。

### 移除（Removed）

- 删除了过时的 `python/aoai-app-variation.py` 和 `python/oai-app-variation.py` 示例（`images.create_variation` 仅适用于 DALL·E-2，不被 `gpt-image` 支持）。
- 删除了 4 个与被移除的 temperature 对比小节相关联的孤立图片资源（`v1-generated-image.png`、`v2-generated-image.png`、`v1-temp-generated-image.png`、`v2-temp-generated-image.png`）。
- 从本课 Python 示例和 requirements 中移除了不必要的 `requests` 依赖。

### 校验（Validated）

- 端到端运行 `aoai-app.py`，针对已部署的 `gpt-image-1.5` 模型，确认 base64 解码/保存流程能生成 PNG。Notebook 已确认为有效的 JSON。

## [2026-07-14] — 默认模型更新 + 推理模型指南

### 变更（Changed）

- **默认聊天模型 `gpt-4o-mini` → `gpt-5-mini`**，应用于课程中所有可运行的示例、文档和配置。这一改动由模型生命周期状态驱动：在 Microsoft Foundry 上，`gpt-4o-mini`（2026-10-01 停用）以及整个 `gpt-4.1` 系列（`gpt-4.1`、`gpt-4.1-mini`、`gpt-4.1-nano`，2026-10-14 停用）正处于**逐步弃用**状态，而 **GPT-5 系列（`gpt-5-mini`、`gpt-5`、`gpt-5-nano`）已正式可用（GA）**（2027-02-06 停用）。已更新：
  - `.env.copy`、`00-course-setup/03-providers.md`（推荐的部署和 `az cognitiveservices` 部署命令），以及第 04、06、07、15 课的 README。
  - 第 06 课的 Python 示例（`oai-app.py`、`oai-app-recipe.py`、`oai-history-bot.py`、`oai-study-buddy.py`、`githubmodels-app.py`）以及第 08 课的脚本。
  - 第 06、07、11 课的 TypeScript / JavaScript 示例，以及第 06、07 课的 `.dib` .NET notebook。
  - 第 04、06、07、11 课的作业 notebook（代码单元格），以及 `shared/python/api_utils.py` 的文档字符串示例。
- **推理模型参数指南（新增）**。`gpt-5-mini` 是一款*推理*模型：它**不支持** `temperature`/`top_p`，并使用 `max_completion_tokens`（聊天补全）/ `max_output_tokens`（Responses API）而非 `max_tokens`。据此：
  - 从现已默认使用 `gpt-5-mini` 的示例中移除了 `temperature`/`top_p`/`max_tokens`（`githubmodels-app.py`、`aoai-app-recipe.py`、`oai-app-recipe.py`、第 15 课 RAG README）。
  - 在第 06 课新增了一条 **"推理模型不使用 `temperature`"** 说明，解释推理模型是通过**提示词工程 + 推理控制**来引导，而非采样参数（sampling knobs）；而 `temperature`/`top_p` 在非推理模型（GPT-4.x、Mistral、Llama、Phi、开源模型）上仍然有效。
- **微调教程（第 18 课）未使用 `gpt-5-mini`**。GPT-5 仅支持强化微调（RFT）；第 18 课的监督微调（SFT）讲解保留 `gpt-4.1-mini`，它支持 SFT/DPO。
- **temperature 演示使用 Llama 模型**。为了在继续讲授 `temperature`（推理模型会拒绝）的同时保留该内容，通过 Foundry Models 端点使用了 `Llama-3.3-70B-Instruct` 模型。向 `.env.copy` 新增了 `AZURE_INFERENCE_CHAT_MODEL` 变量；第 04/06 课的 `githubmodels` notebook 以及 `06` 课的 `js-githubmodels` 示例读取该变量（回退到 `Llama-3.3-70B-Instruct`）并保留其 `temperature`/`top_p`/`max_tokens` 演示。
- **JS / .NET 示例针对 GPT-5 更新**。从 GPT-5 示例中移除了 `temperature`/`top_p`/`max_tokens`（`06` 课的 `recipe-app` TypeScript、`06` 课的 `.dib` .NET——后者还提升了 `MaxOutputTokenCount`，以免推理输出被截断）。`06` 课的 `js-githubmodels` 示例现改用 Llama 以保留其 temperature 演示。`.dib` 中注明，在 .NET 中演示 `Temperature` 的方式是使用 `Azure.AI.Inference` + Llama 模型。
- 在 `gpt-4o-mini` / `gpt-5-mini` 仍然准确之处保留了它们：tiktoken token 编码引用、模型目录可用性列表，以及第 02 课的语音模型（`gpt-4o-transcribe`）。
- 第 20 课（Mistral）和第 21 课（Meta）的示例保留了 `temperature`/`max_tokens`，因为它们面向的是支持这些参数的 Mistral/Llama 模型。

## [2026-07-06] — 内容现代化刷新

一次大范围的刷新，以使课程在 2026 年保持准确：现代化的 API、当前的产品名称和模型名称、更新的提供商指南，以及新的开发者体验工具链。

### 新增（Added）

- 第 `17-ai-agents` 课中的 **Microsoft Agent Framework** 小节，涵盖单聊 agent、工具/函数调用、Azure OpenAI（Microsoft Foundry）配置，以及多 agent 工作流编排（`SequentialBuilder` / `ConcurrentBuilder`）。
- 在 `00-course-setup/03-providers.md` 和第 `19-slm` 课中，将 **Foundry Local** 作为一种离线 / 设备端提供商（与 Ollama 并列）进行了文档说明。
- **持续集成工作流**：
  - `.github/workflows/code-quality.yml` —— Ruff + Black（在受维护的 `shared/` 模块上强制，在课程其余部分作为建议）、一项建议性的 ESLint 检查，以及一个 pytest 任务。
  - `.github/workflows/security.yml` —— CodeQL 分析（Python + JavaScript/TypeScript）以及对 pull request 的依赖项审查。
- `tests/` 下的**测试套件** —— 41 个 pytest 测试，覆盖共享工具模块。
- `.github/skills/azure-openai-to-responses/` 下的 **Azure OpenAI → Responses API 迁移技能**，用于指导 API 迁移。

### 变更（Changed）

- 所有 Python 和 TypeScript 聊天示例中的 **Chat Completions API → Responses API**（`client.responses.create(...)` → `response.output_text`），包括第 04、06、07、11、15、18 课及其 README。
- 全篇正文、链接和示例中的 **GitHub Models → Microsoft Foundry Models**。GitHub Models 将于 2026 年 7 月底停用；示例现已指向 Microsoft Foundry 模型目录，并使用 `AZURE_INFERENCE_ENDPOINT` / `AZURE_INFERENCE_CREDENTIAL` 环境变量。
- **`.env.copy`、`AGENTS.md` 和提供商文档**已更新，以反映 Azure OpenAI 现已并入 Microsoft Foundry，且默认 API 版本提升至 `2024-10-21`。
- **TypeScript 示例**（第 06、07、08、11 课）从已弃用的 `@azure/openai` beta SDK 迁移到了 `openai` 包（聊天应用使用 Responses API；搜索应用使用 embeddings 客户端）。
- **.NET notebook**（`dotnet/*.dib`）统一使用 `Azure.AI.OpenAI` **2.1.0**：第 06、07 课使用 `ChatClient` API，第 08 课使用 `EmbeddingClient`（`GenerateEmbedding` / `ToFloats`），第 09 课使用 `ImageClient`（`GenerateImage`），配合 `gpt-image-1`，替换了 `1.0.0-beta.9` 中旧的 `OpenAIClient` / `GetEmbeddingsAsync` / `GetImageGenerationsAsync`。
- **产品名称现代化**：在指代当前产品之处，将 "Azure AI Studio" / "Azure AI Foundry" → **Microsoft Foundry**（第 14、16、17 课），将 "Bing" → **Microsoft Copilot**（第 12 课）。
- **DevContainer**（`.devcontainer/`）现已附带 Pylance、Black、Ruff、ESLint、Prettier 和 Copilot 扩展，启用了保存时格式化，并安装了 `ruff`、`black`、`mypy` 和 `pytest`，以便能在本地复现 CI 检查。
- **图像生成**（第 09 课）在 Azure 上推荐使用 `gpt-image-1`（Azure 目录已下架 `dall-e-3`）。
- **`docs/ENHANCED_FEATURES_ROADMAP.md`** 已更新，以反映已完成的工作（API 迁移、CI、DevContainer、测试）和当前事实（翻译由 Azure Co-op Translator 自动生成；Assistants API 已被 Responses API 取代）。

### 修复（Fixed）

- **`shared/python/input_validation.py`** —— `validate_text_input(allow_empty=True)` 现在对仅包含空白的输入返回空字符串，而非抛出 "too short" 错误（与 `None` 情况保持一致）。由新增的测试套件发现并覆盖。
- **第 09 课图像示例** —— 修正了真实存在的 bug：`InvalidRequestError` → `BadRequestError`、`images.create` → `images.generate`、`Image.create_variation` → `client.images.create_variation`，以及一个遮蔽了 `openai` 模块的变量。
- **第 15 课 RAG notebook** —— 修复了客户端设置，将已移除的 `DataFrame.append` 替换为 `pd.concat`，并现代化了旧的 SDK 用法。
- 在活跃示例中，将已弃用/停用的模型名称（`gpt-3.5-turbo`、`gpt-35-turbo`）替换为 `gpt-4o-mini`；第 18 课中历史性的微调输出被保留并加注说明，而非重写。

### 弃用 / 说明（Deprecated / Notes）

- 使用 `azure-ai-inference` / `@azure-rest/ai-inference` SDK（`client.complete()`）的 **Microsoft Foundry Models 示例**——即 `githubmodels-*` 和 `js-githubmodels` 示例，以及第 19、20、21 课——仍保留在 Model Inference API 上，该 API **不支持** Responses API。这些示例是有意保留在该 SDK 上的。
- 在仍然合适之处有意保留 `AzureOpenAI()`（嵌入和图像生成），因为这些工作流不属于 Responses API 迁移范围。
- 在预计算的嵌入索引依赖它们之处，保留了 `text-embedding-ada-002` 的引用。
