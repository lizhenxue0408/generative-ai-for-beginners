# AGENTS.md（中文版）

## 项目概述

本仓库包含一个全面的 21 课课程，讲解生成式 AI 基础知识和应用开发。课程面向初学者设计，涵盖从基础概念到构建可用于生产环境的应用程序的所有内容。

**关键技术：**
- Python 3.9+，相关库：`openai`、`python-dotenv`、`tiktoken`、`azure-ai-inference`、`pandas`、`numpy`、`matplotlib`
- TypeScript/JavaScript（配合 Node.js），相关库：`openai`（通过 v1 端点和 Responses API 访问 Azure OpenAI）、`@azure-rest/ai-inference`（Microsoft Foundry Models）
- Azure OpenAI 服务、OpenAI API，以及 Microsoft Foundry Models（GitHub Models 将于 2026 年 7 月底停用）
- 用于交互式学习的 Jupyter Notebook
- 用于一致性开发环境的 Dev Container（开发容器）

**仓库结构：**
- 21 个带编号的课程目录（00-21），包含 README、代码示例和作业
- 多种实现方式：Python、TypeScript，有时还有 .NET 示例
- 包含 40 多种语言版本的翻译目录
- 通过 `.env` 文件进行集中配置（以 `.env.copy` 作为模板）

## 安装命令

### 仓库初始化

```bash
# 克隆仓库
git clone https://github.com/microsoft/generative-ai-for-beginners.git
cd generative-ai-for-beginners

# 复制环境模板
cp .env.copy .env
# 用你的 API 密钥和端点编辑 .env
```

### Python 环境配置

```bash
# 创建虚拟环境
python3 -m venv venv

# 激活虚拟环境
# 在 macOS/Linux 上：
source venv/bin/activate
# 在 Windows 上：
venv\Scripts\activate

# 安装依赖
pip install -r requirements.txt
```

### Node.js/TypeScript 配置

```bash
# 安装根级依赖（用于文档工具链）
npm install

# 对于单独课程的 TypeScript 示例，请进入对应的课程目录：
cd 06-text-generation-apps/typescript/recipe-app
npm install
```

### Dev Container 配置（推荐）

仓库包含用于 GitHub Codespaces 或 VS Code Dev Containers 的 `.devcontainer` 配置：

1. 在 GitHub Codespaces 或带有 Dev Containers 扩展的 VS Code 中打开仓库
2. Dev Container 将自动执行：
   - 从 `requirements.txt` 安装 Python 依赖
   - 运行创建后脚本（`.devcontainer/post-create.sh`）
   - 配置 Jupyter 内核

## 开发工作流

### 环境变量

所有需要 API 访问的课程都使用 `.env` 中定义的环境变量：

- `OPENAI_API_KEY` - 用于 OpenAI API
- `AZURE_OPENAI_API_KEY` - 用于 Microsoft Foundry 中的 Azure OpenAI（Azure OpenAI 服务现已并入 Microsoft Foundry：https://ai.azure.com）
- `AZURE_OPENAI_ENDPOINT` - Azure OpenAI 端点 URL（Foundry 资源端点）
- `AZURE_OPENAI_DEPLOYMENT` - 聊天补全模型部署名称（课程默认：`gpt-5-mini`）
- `AZURE_OPENAI_EMBEDDINGS_DEPLOYMENT` - 嵌入模型部署名称（课程默认：`text-embedding-3-small`）
- `AZURE_OPENAI_API_VERSION` - API 版本（默认：`2024-10-21`）
- `HUGGING_FACE_API_KEY` - 用于 Hugging Face 模型
- `AZURE_INFERENCE_ENDPOINT` - Microsoft Foundry Models 端点（多供应商模型目录）
- `AZURE_INFERENCE_CREDENTIAL` - Microsoft Foundry Models API 密钥（取代即将停用的 `GITHUB_TOKEN`）
- `AZURE_INFERENCE_CHAT_MODEL` - 一个非推理模型（例如 `Llama-3.3-70B-Instruct`），用于 `temperature` 示例，因为推理模型不支持采样控制

### 模型约定（重要）

- **默认聊天模型为 `gpt-5-mini`**——这是一款当前未弃用、非过时的**推理**模型。截至 2026 年，较早支持 temperature 的 "mini" 模型（`gpt-4o-mini`、`gpt-4.1-mini`）正在*逐步弃用*，因此课程统一采用 GPT-5 系列。
- **推理模型会拒绝 `temperature` 和 `top_p`**，并使用 `max_output_tokens`（Responses API）/ `max_completion_tokens`（聊天补全）而非 `max_tokens`。**不要**在调用 `gpt-5-mini` 的示例中添加 `temperature`/`top_p`/`max_tokens`。
- **为了演示 `temperature`**，示例通过 Microsoft Foundry Models 端点（`AZURE_INFERENCE_CHAT_MODEL`）使用 **Llama** 模型（`Llama-3.3-70B-Instruct`）。对于推理模型，应使用提示词工程（prompt engineering）+ 推理控制来引导，而不是使用采样参数（sampling knobs）。
- **微调（第 18 课）**保留 `gpt-4.1-mini`：GPT-5 仅支持强化微调（RFT），而非该课所展示的监督微调（SFT）。
- 第 20 课（Mistral）和第 21 课（Meta）保留 `temperature`/`max_tokens`，因为它们面向的是支持这些参数的 Mistral/Llama 模型。

### 运行 Python 示例

```bash
# 进入课程目录
cd 06-text-generation-apps/python

# 运行 Python 脚本
python aoai-app.py
```

### 运行 TypeScript 示例

```bash
# 进入 TypeScript 应用目录
cd 06-text-generation-apps/typescript/recipe-app

# 构建 TypeScript 代码
npm run build

# 运行应用
npm start
```

### 运行 Jupyter Notebook

```bash
# 在仓库根目录启动 Jupyter
jupyter notebook

# 或使用带有 Jupyter 扩展的 VS Code
```

### 处理不同类型的课程

- **"Learn"（学习）类课程**：侧重于 README.md 文档和概念
- **"Build"（构建）类课程**：包含可运行的 Python 和 TypeScript 代码示例
- 每节课都有一个 README.md，包含理论、代码讲解以及视频内容链接

## 代码风格指南

### Python

- 使用 `python-dotenv` 管理环境变量
- 导入 `openai` 库进行 API 交互
- 使用 `pylint` 进行代码检查（部分示例包含 `# pylint: disable=all` 以简化）
- 遵循 PEP 8 命名规范
- 将 API 凭据存储在 `.env` 文件中，切勿写在代码里

### TypeScript

- 使用 `dotenv` 包管理环境变量
- 每个应用都有 `tsconfig.json` 中的 TypeScript 配置
- 使用 `openai` 包访问 Azure OpenAI（将客户端指向 `/openai/v1/` 端点并调用 `client.responses.create`）；使用 `@azure-rest/ai-inference` 访问 Microsoft Foundry Models
- 使用 `nodemon` 进行带自动重载的开发
- 运行前先构建：`npm run build`，然后 `npm start`

### 通用约定

- 保持代码示例简单且具有教学性
- 包含解释关键概念的注释
- 每节课的代码应当自包含且可运行
- 使用一致的命名：Azure OpenAI 使用 `aoai-` 前缀，OpenAI API 使用 `oai-` 前缀，Microsoft Foundry Models 使用 `githubmodels-` 前缀（保留 GitHub Models 时代的旧前缀）

## 文档指南

### Markdown 风格

- 所有 URL 必须使用 `[text](url)` 格式包裹，且不加多余空格
- 相对链接必须以 `./` 或 `../` 开头
- 所有指向 Microsoft 域名的链接都必须包含跟踪 ID：`?WT.mc_id=academic-105485-koreyst`
- URL 中不要包含特定国家/地区的区域设置（避免使用 `/en-us/`）
- 图片存储在 `./images` 文件夹中，使用描述性名称
- 文件名使用英文字母、数字和连字符

### 翻译支持

- 仓库通过自动化的 GitHub Actions 支持 40 多种语言
- 翻译存储在 `translations/` 目录中
- 不要提交不完整的翻译
- 不接受机器翻译
- 翻译后的图片存储在 `translated_images/` 目录中

## 测试和验证

### 提交前检查

本仓库使用 GitHub Actions 进行验证。提交 PR 之前：

1. **检查 Markdown 链接**：
   ```bash
   # validate-markdown.yml 工作流会检查：
   # - 失效的相对路径
   # - 路径上缺失的跟踪 ID
   # - URL 上缺失的跟踪 ID
   # - 包含国家区域设置的 URL
   # - 失效的外部 URL
   ```

2. **手动测试**：
   - 测试 Python 示例：激活 venv 并运行脚本
   - 测试 TypeScript 示例：`npm install`、`npm run build`、`npm start`
   - 确认环境变量配置正确
   - 检查 API 密钥是否能与代码示例配合使用

3. **代码示例**：
   - 确保所有代码无错误运行
   - 在适用时同时用 Azure OpenAI 和 OpenAI API 进行测试
   - 在支持的地方用 Microsoft Foundry Models 验证示例是否可用

### 无自动化测试

这是一个以教程和示例为主的教育类仓库，没有需要运行的单元测试或集成测试。验证主要包括：
- 手动测试代码示例
- 用于 Markdown 验证的 GitHub Actions
- 社区对教学内容进行的审查

## Pull Request 指南

### 提交之前

1. 在适用时，用 Python 和 TypeScript 测试代码改动
2. 运行 Markdown 验证（PR 触发时自动运行）
3. 确保所有 Microsoft URL 都带有跟踪 ID
4. 检查相对链接是否有效
5. 确认图片引用正确

### PR 标题格式

- 使用描述性标题：`[Lesson 06] Fix Python example typo`（修复第 6 课 Python 示例笔误）或 `Update README for lesson 08`（更新第 8 课 README）
- 在适用时引用 issue 编号：`Fixes #123`（修复 #123）

### PR 描述

- 说明改动内容及其原因
- 链接到相关 issue
- 对于代码改动，说明测试了哪些示例
- 对于翻译 PR，需包含完整翻译的所有文件

### 贡献要求

- 签署 Microsoft CLA（首次提交 PR 时自动完成）
- 修改前先将仓库 fork 到你自己的账户
- 每个逻辑改动一个 PR（不要合并不相关的修复）
- 尽可能让 PR 聚焦且小巧

## 常见工作流

### 添加新的代码示例

1. 进入对应的课程目录
2. 在 `python/` 或 `typescript/` 子目录中创建示例
3. 遵循命名约定：`{provider}-{example-name}.{py|ts|js}`
4. 使用真实的 API 凭据进行测试
5. 在课程 README 中记录任何新的环境变量

### 更新文档

1. 编辑课程目录中的 README.md
2. 遵循 Markdown 指南（跟踪 ID、相对链接）
3. 翻译更新由 GitHub Actions 处理（不要手动编辑）
4. 测试所有链接是否有效

### 使用 Dev Container

1. 仓库包含 `.devcontainer/devcontainer.json`
2. 创建后脚本会自动安装 Python 依赖
3. 已预配置 Python 和 Jupyter 的扩展
4. 环境基于 `mcr.microsoft.com/devcontainers/universal:2.11.2`

## 部署与发布

这是一个学习类仓库——没有部署流程。课程的使用途径包括：

1. **GitHub 仓库**：直接访问代码和文档
2. **GitHub Codespaces**：带有预配置设置的即时开发环境
3. **Microsoft Learn**：内容可能会被联合发布到官方学习平台
4. **docsify**：基于 Markdown 构建的文档站点（参见 `docsifytopdf.js` 和 `package.json`）

### 构建文档站点

```bash
# 从文档生成 PDF（如需）
npm run convert
```

## 故障排查

### 常见问题

**Python 导入错误**：
- 确保已激活虚拟环境
- 运行 `pip install -r requirements.txt`
- 检查 Python 版本是否为 3.9+

**TypeScript 构建错误**：
- 在具体的应用目录中运行 `npm install`
- 检查 Node.js 版本是否兼容
- 必要时清理 `node_modules` 并重新安装

**API 身份验证错误**：
- 确认 `.env` 文件存在且值正确
- 检查 API 密钥是否有效且未过期
- 确保端点 URL 与你的区域匹配

**缺少环境变量**：
- 将 `.env.copy` 复制为 `.env`
- 填写你正在处理的课程所需的所有值
- 更新 `.env` 后重启应用

## 附加资源

- [课程安装指南](./00-course-setup/README.md?WT.mc_id=academic-105485-koreyst)
- [贡献指南](./CONTRIBUTING.md)
- [行为准则](./CODE_OF_CONDUCT.md)
- [安全策略](./SECURITY.md)
- [Azure AI Discord](https://aka.ms/genai-discord?WT.mc_id=academic-105485-koreyst)
- [高级代码示例合集](https://aka.ms/genai-beg-code?WT.mc_id=academic-105485-koreyst)

## 项目相关说明

- 这是一个**教育类仓库**，侧重于学习而非生产代码
- 示例刻意保持简单，聚焦于教学概念
- 代码质量与教学清晰度之间取得平衡
- 每节课都是自包含的，可以独立完成
- 仓库支持多种 API 提供商：Azure OpenAI、OpenAI、Microsoft Foundry Models，以及 Foundry Local 和 Ollama 等离线提供商
- 内容支持多语言，并配有自动化翻译工作流
- Discord 上有一个活跃的社区，用于提问和获取支持
