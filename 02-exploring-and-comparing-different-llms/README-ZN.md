# 探索并比较不同的 LLM

[![探索并比较不同的 LLM](./images/02-lesson-banner.png?WT.mc_id=academic-105485-koreyst)](https://youtu.be/KIRUeDKscfI?si=8BHX1zvwzQBn-PlK)

> _点击上方图片观看本课视频_

在上一课中，我们看到了生成式 AI 如何改变技术格局，大语言模型（LLMs）如何运作，以及一个企业（如我们的初创公司）如何将它们应用到自身的用例中并实现增长！在本章中，我们将比较和对比不同类型的大语言模型（LLMs），以了解它们的优缺点。

我们初创公司旅程的下一步是探索当前的 LLM 格局，并了解哪些适合我们的用例。

## 简介

本课将涵盖：

- 当前格局中不同类型的 LLM。
- 在 Azure 中针对你的用例测试、迭代和比较不同的模型。
- 如何部署一个 LLM。

## 学习目标

完成本课后，你将能够：

- 为你的用例选择合适的模型。
- 了解如何测试、迭代并提升模型的性能。
- 了解企业如何部署模型。

## 了解不同类型的 LLM

LLM 可以根据其架构、训练数据和用例进行多种分类。理解这些差异将帮助我们的初创公司选择合适的模型，并理解如何测试、迭代和提升性能。

有许多不同类型的 LLM 模型，模型的选择取决于你打算用它来做什么、你的数据、你准备支付多少费用等等。

根据你打算将模型用于文本、音频、视频、图像生成等，你可能会选择不同类型的模型。

- **音频与语音识别**。Whisper 风格的模型仍然是有用的通用语音识别模型，但如今生产环境的选择也包括更新的语音转文本模型，如 `gpt-4o-transcribe`、`gpt-4o-mini-transcribe` 以及说话人分离（diarization）变体。请针对你的场景评估语言覆盖范围、说话人分离、实时支持、延迟和成本。在 [OpenAI 语音转文本文档](https://platform.openai.com/docs/guides/speech-to-text?WT.mc_id=academic-105485-koreyst) 中了解更多。

- **图像生成**。DALL-E 和 Midjourney 是知名的图像生成选项，但当前的 OpenAI 图像 API 以 GPT Image 模型（如 `gpt-image-2`）为核心，同时 Stable Diffusion、Imagen、Flux 和其他模型系列也是常见选择。请比较提示遵循度、编辑支持、风格控制、安全要求和许可协议。在 [OpenAI 图像生成指南](https://platform.openai.com/docs/guides/images?WT.mc_id=academic-105485-koreyst) 以及本课程体系第 9 章中了解更多。

- **文本生成**。文本模型现在涵盖前沿模型、推理模型、低延迟小模型和开放权重模型。当前示例包括 OpenAI GPT-5.x 模型、Anthropic Claude 4.x 模型、Google Gemini 3.x 模型、Meta Llama 4 模型和 Mistral 模型。不要仅根据发布日期或价格来选择；请比较任务质量、延迟、上下文窗口、工具调用、安全行为、区域可用性和总成本。[Microsoft Foundry 模型目录](https://ai.azure.com/catalog?WT.mc_id=academic-105485-koreyst) 是比较 Azure 上可用模型的好去处。

- **多模态**。许多当前模型能够处理文本以外的更多内容。有些接受图像、音频或视频输入；有些可以调用工具；专门的模型可以生成图像、音频或视频。例如，当前的 OpenAI 模型支持文本和图像输入，Gemini 模型根据变体不同可支持文本、代码、图像、音频和视频输入，而 Llama 4 Scout 和 Maverick 是原生多模态的开放权重模型。在围绕某个模型构建工作流之前，务必查看每个模型卡片所支持的输入和输出模态。

选择模型意味着你获得了一些基本能力，但这可能还不够。通常你拥有公司特定的数据，需要以某种方式告知 LLM。关于如何实现这一点有几种不同的选择，更多内容将在后续章节中介绍。

### 基础模型 versus LLM

术语“基础模型（Foundation Model）”由[斯坦福研究人员提出](https://arxiv.org/abs/2108.07258?WT.mc_id=academic-105485-koreyst)，定义为遵循某些标准的 AI 模型，例如：

- **它们使用无监督学习或自监督学习进行训练**，这意味着它们在无标签的多模态数据上训练，且训练过程不需要人工标注或数据标记。
- **它们是非常大的模型**，基于在数十亿参数上训练的极深神经网络。
- **它们通常旨在作为其他模型的“基础”**，意味着它们可以用作其他模型在其之上构建的起点，这可以通过微调来实现。

![基础模型 versus LLM](./images/FoundationModel.png?WT.mc_id=academic-105485-koreyst)

图片来源：[Essential Guide to Foundation Models and Large Language Models | by Babar M Bhatti | Medium
](https://thebabar.medium.com/essential-guide-to-foundation-models-and-large-language-models-27dab58f7404)

为了进一步澄清这一区别，让我们以 ChatGPT 作为一个历史示例。ChatGPT 的早期版本使用 GPT-3.5 作为基础模型。OpenAI 随后使用了聊天专用数据和对齐（alignment）技术，创建了一个在对话场景（如聊天机器人）中表现更好的微调版本。现代 AI 服务通常会在多个模型变体之间进行路由，因此服务名称与底层模型名称并不总是一回事。

![基础模型](./images/Multimodal.png?WT.mc_id=academic-105485-koreyst)

图片来源：[2108.07258.pdf (arxiv.org)](https://arxiv.org/pdf/2108.07258.pdf?WT.mc_id=academic-105485-koreyst)

### 开放权重/开源 versus 专有模型

另一种对 LLM 进行分类的方式是看它们是开放权重、开源还是专有的。

开源和开放权重模型提供可供检查、下载或定制的模型产物，但它们的许可证各不相同。有些是完全开源的，而另一些则是带有使用限制的开放权重模型。当企业需要对部署、数据本地化、成本或定制有更多控制时，它们会很有用。但是，团队在将其用于生产环境之前，仍然需要审查许可条款、服务成本、维护、安全更新和评估质量。示例包括 [Meta Llama 4](https://ai.meta.com/blog/llama-4-multimodal-intelligence/?WT.mc_id=academic-105485-koreyst)、一些 [Mistral 模型](https://docs.mistral.ai/models/overview?WT.mc_id=academic-105485-koreyst)，以及托管在 [Hugging Face](https://huggingface.co/models?WT.mc_id=academic-105485-koreyst) 上的许多模型。

专有模型由提供商拥有和托管。这些模型通常为托管生产用途而优化，并能提供强有力的支持、安全系统、工具集成和规模能力。但是，客户通常无法检查或修改模型权重，并且必须审查提供商的隐私、保留、合规和可接受使用条款。示例包括 [OpenAI 模型](https://platform.openai.com/docs/models?WT.mc_id=academic-105485-koreyst)、[Google Gemini](https://deepmind.google/models/gemini/pro/?WT.mc_id=academic-105485-koreyst) 和 [Anthropic Claude](https://platform.claude.com/docs/en/about-claude/models/overview?WT.mc_id=academic-105485-koreyst)。

### 嵌入 versus 图像生成 versus 文本与代码生成

LLM 也可以根据它们生成的输出来分类。

嵌入（Embeddings）是一类可以将文本转换为数值形式（称为嵌入）的模型，它是输入文本的数值表示。嵌入使机器更容易理解单词或句子之间的关系，并且可以被其他模型（如分类模型或在数值数据上表现更好的聚类模型）作为输入使用。嵌入模型经常用于迁移学习，即先为一个拥有大量数据的代理任务构建模型，然后复用模型权重（嵌入）用于其他下游任务。这一类的一个示例是 [OpenAI embeddings](https://platform.openai.com/docs/models/embeddings?WT.mc_id=academic-105485-koreyst)。

![嵌入](./images/Embedding.png?WT.mc_id=academic-105485-koreyst)

图像生成模型是生成图像的模型。这些模型常用于图像编辑、图像合成和图像翻译。图像生成模型通常在大型图像数据集（如 [LAION-5B](https://laion.ai/blog/laion-5b/?WT.mc_id=academic-105485-koreyst)）上训练，可用于生成新图像，或使用图像修复（inpainting）、超分辨率和着色技术来编辑现有图像。示例包括 [GPT Image 模型](https://platform.openai.com/docs/guides/images?WT.mc_id=academic-105485-koreyst)、[Stable Diffusion 模型](https://github.com/Stability-AI/StableDiffusion?WT.mc_id=academic-105485-koreyst) 和 Imagen 模型。

![图像生成](./images/Image.png?WT.mc_id=academic-105485-koreyst)

文本和代码生成模型是生成文本或代码的模型。这些模型常用于文本摘要、翻译和问答。文本生成模型通常在大型文本数据集（如 [BookCorpus](https://www.cv-foundation.org/openaccess/content_iccv_2015/html/Zhu_Aligning_Books_and_ICCV_2015_paper.html?WT.mc_id=academic-105485-koreyst)）上训练，可用于生成新文本或回答问题。代码生成模型（如 [CodeParrot](https://huggingface.co/codeparrot?WT.mc_id=academic-105485-koreyst)）通常在大型代码数据集（如 GitHub）上训练，可用于生成新代码或修复现有代码中的错误。

![文本和代码生成](./images/Text.png?WT.mc_id=academic-105485-koreyst)

### 编码器-解码器 versus 仅解码器

为了谈论 LLM 的不同架构类型，让我们用一个类比。

想象你的经理给你布置了一个任务，要为学生出一份测验。你有两个同事；一个负责创建内容，另一个负责审阅它们。

内容创作者就像一个仅解码器（decoder-only）模型：他们可以查看主题，看到你已经写了什么，然后基于该上下文继续生成内容。他们非常擅长写出引人入胜且信息丰富的内容，但当任务仅仅是分类、检索或编码信息时，它们并不总是最佳选择。仅解码器模型系列的示例包括 GPT 和 Llama 模型。

审阅者就像一个仅编码器（Encoder only）模型，他们查看写好的课程和答案，注意到它们之间的关系并理解上下文，但他们不擅长生成内容。仅编码器模型的一个示例是 BERT。

想象一下，我们也可以有一个既能创建又能审阅测验的人，这就是编码器-解码器（Encoder-Decoder）模型。一些示例是 BART 和 T5。

### 服务 versus 模型

现在，让我们谈谈服务（service）和模型（model）之间的区别。服务是云服务提供商提供的产品，通常是模型、数据和其他组件的组合。模型是服务的核心组件，通常是一个基础模型，例如 LLM。

服务通常针对生产用途进行了优化，并且通常比模型更易于通过图形用户界面使用。但是，服务并非总是免费可用，可能需要订阅或付费才能使用，以换取利用服务所有者的设备和资源，从而优化支出并轻松扩展。服务的一个示例是 [Azure OpenAI Service](https://learn.microsoft.com/azure/ai-foundry/openai/overview?WT.mc_id=academic-105485-koreyst)，它提供按量付费（pay-as-you-go）的费率计划，意味着用户根据其使用量按比例收费。Azure OpenAI Service 还在模型能力之上提供了企业级安全和负责任 AI 框架。

模型是神经网络产物：参数、权重、架构、分词器和配套配置。在本地或私有环境中运行模型需要合适的硬件、服务基础设施、监控，以及兼容的开源/开放权重许可证或商业许可证。像 Llama 4 或 Mistral 这样的开放权重模型可以自托管，但它们仍然需要计算能力和运维专业知识。

## 如何在 Azure 上测试并迭代不同模型以了解性能

一旦我们的团队探索了当前的 LLM 格局，并为他们的场景确定了几个不错的候选模型，下一步就是在他们的数据和负载上测试它们。这是一个通过实验和度量来完成的迭代过程。
我们在前面段落中提到的大多数模型（OpenAI 模型、像 Llama 4 和 Mistral 这样的开放权重模型，以及 Hugging Face 模型）都可在 [Microsoft Foundry Models](https://learn.microsoft.com/azure/foundry/concepts/foundry-models-overview?WT.mc_id=academic-105485-koreyst) 中获取。

[Microsoft Foundry](https://learn.microsoft.com/azure/foundry/what-is-foundry?WT.mc_id=academic-105485-koreyst)（原 Azure AI Studio / Azure AI Foundry）是一个用于构建 AI 应用和智能体的统一 Azure 平台。它帮助开发者管理从实验和评估到部署、监控和治理的生命周期。Microsoft Foundry 中的模型目录使用户能够：

- 在目录中查找感兴趣的基础模型，包括 Azure 销售的模型以及来自合作伙伴和社区提供商的模型。用户可按任务、提供商、许可证、部署选项或名称进行筛选。

![模型目录](./images/AzureAIStudioModelCatalog.png?WT.mc_id=academic-105485-koreyst)

- 查看模型卡片，包括预期用途和训练数据的详细描述、代码示例以及内部评估库上的评估结果。

![模型卡片](./images/ModelCard.png?WT.mc_id=academic-105485-koreyst)

- 通过[模型基准测试](https://learn.microsoft.com/azure/ai-foundry/concepts/model-benchmarks?WT.mc_id=academic-105485-koreyst) 面板，跨行业和数据集比较模型基准，以评估哪个满足业务场景。

![模型基准测试](./images/ModelBenchmarks.png?WT.mc_id=academic-105485-koreyst)

- 在自定义训练数据上微调受支持的模型，以在特定负载中提升模型性能，并充分利用 Microsoft Foundry 的实验和跟踪能力。

![模型微调](./images/FineTuning.png?WT.mc_id=academic-105485-koreyst)

- 将原始预训练模型或微调版本部署到远程实时推理终结点，使用托管计算或无服务器部署选项，使应用程序能够消费它。

![模型部署](./images/ModelDeploy.png?WT.mc_id=academic-105485-koreyst)

> [!NOTE]
> 目录中并非所有模型当前都可用于微调和/或按量付费部署。请查看模型卡片以了解模型能力和限制的详细信息。

## 改进 LLM 结果

我们已经与初创团队一起探索了不同类型的 LLM，以及一个云平台的（Microsoft Foundry），它使我们能够比较不同模型、在测试数据上评估它们、提升性能，并将其部署到推理终结点。

但是，他们应该在什么时候考虑微调模型，而不是使用预训练模型？还有没有其他方法来在特定负载中提升模型性能？

企业有几种方法可以从 LLM 获得所需的结果。在将 LLM 部署到生产环境时，你可以选择具有不同训练程度的、不同类型的模型，其复杂度、成本和质量各不相同。以下是一些不同的方法：

- **带上下文的提示工程（Prompt engineering with context）**。其理念是在提示时提供足够的上下文，以确保获得所需的回复。

- **检索增强生成（Retrieval Augmented Generation，RAG）**。你的数据可能存在于数据库或 Web 终结点中，例如，为了确保这些数据或其子集在提示时被包含，你可以获取相关数据，并将其作为用户提示的一部分。

- **微调模型（Fine-tuned model）**。在这里，你在自己的数据上进一步训练模型，使模型更精确并响应你的需求，但可能成本较高。

![LLM 部署](./images/Deploy.png?WT.mc_id=academic-105485-koreyst)

图片来源：[Four Ways that Enterprises Deploy LLMs | Fiddler AI Blog](https://www.fiddler.ai/blog/four-ways-that-enterprises-deploy-llms?WT.mc_id=academic-105485-koreyst)

### 带上下文的提示工程

预训练 LLM 在通用自然语言任务上表现非常好，即使只用一个简短的提示（如要补全的句子或问题）来调用——即所谓的“零样本（zero-shot）”学习。

然而，用户越能通过一个详细的请求和示例（即上下文）来框定他们的查询，答案就会越准确、越接近用户的期望。在这种情况下，如果提示只包含单个示例，我们称之为“单样本（one-shot）”学习；如果包含多个示例，则称之为“少样本（few shot）”学习。带上下文的提示工程是起步最具成本效益的方法。

### 检索增强生成（RAG）

LLM 有一个局限，即它们只能使用训练期间所用的数据来生成答案。这意味着它们对训练过程之后发生的事件一无所知，也无法访问非公开信息（如公司数据）。
这可以通过 RAG 来克服，RAG 是一种通过文档片段形式的外部数据来增强提示的技术，同时考虑提示长度限制。这由向量数据库工具（如 [Azure Vector Search](https://learn.microsoft.com/azure/search/vector-search-overview?WT.mc_id=academic-105485-koreyst)）提供支持，可从各种预定义数据源中检索有用的片段并将其添加到提示上下文中。

当企业没有足够的数据、时间或资源来微调 LLM，但仍希望在特定负载中提升性能并降低产生幻觉、过时或不被支持的答案的风险时，这种技术非常有帮助。

### 微调模型

微调是利用迁移学习来“适配”模型以适应下游任务或解决特定问题的过程。与少样本学习和 RAG 不同，它会导致生成一个具有更新权重和偏置的新模型。它需要一个由单个输入（提示）及其关联输出（补全）组成的训练示例集。
在以下情况下，这将是首选方法：

- **使用较小的任务专用模型**。企业希望针对某个狭窄任务微调一个较小的模型，而不是反复提示一个更大的前沿模型，从而获得更具成本效益且更快的解决方案。

- **考虑延迟**。延迟对某个特定用例很重要，因此无法使用非常长的提示，或者应从模型学习的示例数量超出了提示长度限制。

- **适配稳定行为**。企业拥有许多高质量示例，并希望模型一致地遵循某种任务模式、输出格式、语气或领域特定风格。如果主要问题是经常变化的新事实或私有知识，请使用 RAG 而不是仅依赖微调。

### 训练模型

从头训练 LLM 无疑是最困难、最复杂的方法，需要海量数据、熟练的资源和适当的计算能力。只有在企业拥有领域特定用例和大量领域中心数据的情况下，才应考虑此选项。

## 知识检测

什么可能是改进 LLM 补全结果的好方法？

1. 带上下文的提示工程
1. RAG
1. 微调模型

答：三者都能提供帮助。从提示工程和上下文入手以快速改进，当模型需要当前事实或私有业务数据时，使用 RAG。当你拥有足够的高质量示例，并且需要模型一致地遵循任务、格式、语气或领域模式时，选择微调。

## 🚀 挑战

进一步了解如何为你的业务[使用 RAG](https://learn.microsoft.com/azure/search/retrieval-augmented-generation-overview?WT.mc_id=academic-105485-koreyst)。

## 干得漂亮，继续你的学习

完成本课后，请查看我们的[生成式 AI 学习合集](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)，继续提升你的生成式 AI 知识！

前往第 3 课，我们将了解如何[负责任地使用生成式 AI 进行构建](../03-using-generative-ai-responsibly/README.md?WT.mc_id=academic-105485-koreyst)！
