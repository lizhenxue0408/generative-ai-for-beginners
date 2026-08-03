[![开源模型](./img/18-lesson-banner.png?WT.mc_id=academic-105485-koreyst)](https://youtu.be/6UAwhL9Q-TQ?si=5jJd8yeQsCfJ97em)

# 微调你的 LLM

使用大语言模型来构建生成式 AI 应用带来了新的挑战。一个关键问题是在模型针对给定用户请求所生成的内容中，确保响应质量（准确性与相关性）。在之前的课程中，我们讨论了提示词工程（prompt engineering）和检索增强生成（retrieval-augmented generation）等技术，它们试图通过 _修改现有模型的提示输入_ 来解决这一问题。

在今天的课程中，我们讨论第三种技术 —— **微调（fine-tuning）**，它试图通过 _用额外数据重新训练模型本身_ 来应对这一挑战。让我们深入细节。

## 学习目标

本课介绍了预训练语言模型的微调概念，探讨了这种方法的优势与挑战，并提供了何时以及如何使用微调来提升生成式 AI 模型性能的指导。

到本课结束时，你应该能够回答以下问题：

- 什么是语言模型的微调？
- 何时，以及为什么，微调是有用的？
- 我如何微调一个预训练模型？
- 微调的局限性是什么？

准备好了吗？我们开始吧。

## 图解指南

在深入之前，想先了解我们将要涵盖内容的整体概貌？查看这份图解指南，它描述了本课的学习旅程 —— 从学习微调的核心概念与动机，到了解执行微调任务的流程与最佳实践。这是一个引人入胜的探索主题，别忘了查看 [资源（Resources）](./RESOURCES.md?WT.mc_id=academic-105485-koreyst) 页面，获取更多支持你自主学习旅程的链接！

![微调语言模型图解指南](./img/18-fine-tuning-sketchnote.png?WT.mc_id=academic-105485-koreyst)

## 什么是语言模型的微调？

根据定义，大语言模型是在来自不同来源（包括互联网）的大量文本上 _预训练（pre-trained）_ 的。正如我们在之前的课程中所学到的，我们需要像 _提示词工程_ 和 _检索增强生成_ 这样的技术，来提升模型对用户问题（“提示词”）响应的质量。

一种流行的提示词工程技术是通过提供 _指令_（显式引导）或 _给出几个示例_（隐式引导）来为模型提供更多关于响应中应包含内容的指引。这被称为 _少样本学习（few-shot learning）_，但它有两个局限：

- 模型 token 限制会约束你能给出的示例数量，并限制其有效性。
- 模型 token 成本可能使得为每个提示词添加示例变得昂贵，并限制灵活性。

微调是机器学习系统中的一种常见做法，即我们取一个预训练模型并用新数据重新训练它，以提升其在特定任务上的表现。在语言模型的语境下，我们可以用 _针对某个给定任务或应用领域精心整理的一组示例_ 来微调预训练模型，从而创建一个 **自定义模型（custom model）**，该模型对于该特定任务或领域可能更准确、更相关。微调的一个附带好处是，它还可以减少少样本学习所需的示例数量 —— 从而降低 token 使用量及相关成本。

## 我们何时以及为何应该微调模型？

在 _这个_ 语境下，当我们谈论微调时，指的是 **有监督的（supervised）** 微调，即通过 **添加原本不属于原始训练数据集的新数据** 来重新训练。这与无监督微调方法不同，后者是在原始数据上重新训练模型，但使用不同的超参数。

需要记住的关键一点是，微调是一项高级技术，需要一定程度的专业知识才能获得期望的结果。如果操作不当，它可能无法带来预期的改进，甚至可能降低模型在目标领域上的表现。

因此，在你学习“如何”微调语言模型之前，你需要先知道“为什么”要走这条路，以及“何时”开始微调过程。先问自己这些问题：

- **用例（Use Case）**：你微调的 _用例_ 是什么？你希望改进当前预训练模型的哪些方面？
- **替代方案（Alternatives）**：你是否尝试过 _其他技术_ 来达到期望的结果？把它们作为对比的基线。
  - 提示词工程：尝试如少样本提示（few-shot prompting）并提供相关提示响应的示例等技术。评估响应的质量。
  - 检索增强生成：尝试用通过搜索你的数据所检索到的查询结果来增强提示词。评估响应的质量。
- **成本（Costs）**：你是否已确定微调的成本？
  - 可微调性（Tunability）—— 预训练模型是否可用于微调？
  - 工作量（Effort）—— 用于准备训练数据、评估与改进模型。
  - 计算（Compute）—— 用于运行微调任务，以及部署微调后的模型。
  - 数据（Data）—— 能否获取足够高质量的示例以产生微调效果。
- **收益（Benefits）**：你是否已确认微调带来的收益？
  - 质量（Quality）—— 微调后的模型是否优于基线？
  - 成本（Cost）—— 它是否通过简化提示词减少了 token 使用？
  - 可扩展性（Extensibility）—— 你能否将基础模型重新用于新的领域？

通过回答这些问题，你应该能够判断微调是否是适合你用例的方法。理想情况下，只有当收益大于成本时，该方法才成立。一旦决定继续，就到了思考 _如何_ 微调预训练模型的时候了。

想深入了解这个决策过程？观看 [微调还是不微调（To fine-tune or not to fine-tune）](https://www.youtube.com/watch?v=0Jo-z-MFxJs)

## 我们如何微调一个预训练模型？

要微调一个预训练模型，你需要具备：

- 一个用于微调的预训练模型
- 用于微调的数据集
- 运行微调任务的训练环境
- 部署微调后模型的托管环境

## 在 Microsoft Foundry 上进行微调

[Microsoft Foundry](https://ai.azure.com?WT.mc_id=academic-105485-koreyst) 是今天你在 Azure 上微调、部署和管理自定义模型的地方（它整合了原来的 Azure OpenAI Studio 和 Azure AI Studio）。在你开始一个任务之前，理解 Foundry 提供给你的选择 —— 以及平台推荐的最佳实践 —— 会很有帮助。在底层，Foundry 使用 **LoRA（低秩适配，low-rank adaptation）** 来高效地微调模型，这比重新训练每个权重更快、更经济。

### 步骤 1：选择一种训练技术

Foundry 支持三种微调技术。**从 SFT 开始** —— 它覆盖了最广泛的场景。

| 技术 | 作用 | 何时使用 |
| --- | --- | --- |
| **有监督微调（SFT）** | 在输入/输出示例对上进行训练，使模型学会生成你想要的响应。 | 大多数任务的默认选择：领域专门化、任务表现、风格与语气、指令遵循，以及语言适配。 |
| **直接偏好优化（DPO）** | 从 _偏好 vs. 非偏好_ 响应对中学习，使输出与人类偏好对齐。 | 当你拥有对比性反馈，想要改进响应质量、安全性与对齐时。 |
| **强化微调（RFT）** | 使用来自 _评分器（graders）_ 的奖励信号，通过强化学习优化复杂行为。 | 具有明确对错答案的客观、重推理领域（数学、化学、物理）。需要更多 ML 专业知识。 |

### 步骤 2：选择训练层级

Foundry 让你选择训练运行的方式与位置：

- **标准（Standard）** —— 在你的资源所在区域训练，并保障数据驻留（data residency）。当数据必须保留在特定区域时使用。
- **全局（Global）** —— 通过使用超出你所在区域的容量来排队，更便宜、更快，但数据和权重会被复制到训练区域。当数据驻留不是硬性要求时，这是一个不错的默认选择。
- **开发者（Developer）** —— 成本最低，使用空闲容量，不保障延迟/SLA（任务可能被抢占并恢复）。非常适合实验。

### 步骤 3：选择基础模型

可微调的模型包括 OpenAI 的 `gpt-4o-mini`、`gpt-4o`、`gpt-4.1`、`gpt-4.1-mini` 和 `gpt-4.1-nano`（SFT；4o/4.1 系列也支持 DPO），推理模型 `o4-mini` 和 `gpt-5`（RFT），以及开源模型如 `Ministral-3B`、`Qwen-32B`、`Llama-3.3-70B-Instruct` 和 `gpt-oss-20b`（在 Foundry 资源上进行 SFT）。请始终查阅当前的 [微调模型列表](https://learn.microsoft.com/azure/ai-foundry/foundry-models/concepts/models-sold-directly-by-azure?WT.mc_id=academic-105485-koreyst#fine-tuning-models)，了解支持的方法、区域与可用性。

> Foundry 提供两种模式：**无服务器（serverless）**（按用量计费，无需管理 GPU 配额，适用于 OpenAI 及选定模型）和 **托管计算（managed compute）**（通过 Azure Machine Learning 自带虚拟机，覆盖最广的模型范围）。大多数人应从无服务器模式开始。

### Foundry 最佳实践

- **先建立基线（Baseline first）。** 在微调之前，先用提示词工程和 RAG 测量基础模型，以便证明收益。
- **从小处着手，再扩展（Start small, then scale）。** 从 50-100 个高质量示例开始以验证方法，然后增长到 500+ 用于生产。质量胜于数量 —— 剔除低质量示例。
- **正确格式化数据。** 训练和验证文件必须是 JSONL、UTF-8 **带 BOM**、小于 512 MB，并使用 chat-completions 消息格式。务必包含一个验证文件，以便观察过拟合。
- **在推理时保留训练用的系统提示词。** 调用模型时使用与训练期间相同的系统消息。
- **评估检查点 —— 不要盲目部署最后一个。** Foundry 会将最后三个 epoch 保留为可部署的检查点；通过观察 `train_loss` / `valid_loss` 和 token 准确率，挑选泛化最好的那个。
- 将微调模型与基线比较时，**同时衡量 token 成本与质量**。
- **通过持续微调进行迭代。** 你可以在新数据上微调一个已经被微调过的模型（OpenAI 模型支持此功能）。
- **留意托管成本。** 已部署的自定义模型按小时计费，且空闲部署会在 15 天后被移除 —— 清理你不需要的部分。

按照 [使用微调定制模型](https://learn.microsoft.com/azure/ai-foundry/openai/how-to/fine-tuning?WT.mc_id=academic-105485-koreyst) 中的端到端演练进行操作，当你准备好使用其他技术时，请参阅 [DPO](https://learn.microsoft.com/azure/ai-foundry/openai/how-to/fine-tuning-direct-preference-optimization?WT.mc_id=academic-105485-koreyst) 和 [RFT](https://learn.microsoft.com/azure/ai-foundry/openai/how-to/reinforcement-fine-tuning?WT.mc_id=academic-105485-koreyst) 指南。

## 微调实战

以下资源提供了分步教程，带你通过一个真实示例，在当前支持的模型上、使用精心整理的数据集进行演练。要完成这些教程，你需要特定提供商的账户，以及对应模型和数据集的访问权限。

| 提供商 | 教程 | 描述 |
| --- | --- | --- |
| OpenAI | [如何微调聊天模型](https://github.com/openai/openai-cookbook/blob/main/examples/How_to_finetune_chat_models.ipynb?WT.mc_id=academic-105485-koreyst) | 学习针对特定领域（“食谱助手”）微调一个较新的 OpenAI 聊天模型：准备训练数据、运行微调任务，并使用微调后的模型进行推理。 |
| Microsoft Foundry | [使用微调定制模型](https://learn.microsoft.com/azure/ai-foundry/openai/tutorials/fine-tune?WT.mc_id=academic-105485-koreyst) | 学习在 Azure 上使用 Microsoft Foundry 微调一个当前支持的模型，例如 `gpt-4.1-mini`：准备并上传训练和验证数据、运行微调任务，然后部署并使用新模型。 |
| Hugging Face | [使用 Hugging Face 微调 LLM](https://www.philschmid.de/fine-tune-llms-in-2024-with-trl?WT.mc_id=academic-105485-koreyst) | 这篇博客文章带你使用 [transformers](https://huggingface.co/docs/transformers/index?WT.mc_id=academic-105485-koreyst) 库与 [Transformer 强化学习（TRL）](https://huggingface.co/docs/trl/index?WT.mc_id=academic-105485-koreyst)，在 Hugging Face 上利用开放的 [数据集](https://huggingface.co/docs/datasets/index?WT.mc_id=academic-105485-koreyst) 微调一个 _开源 LLM_（例如 `CodeLlama 7B`）。 |
| 🤗 AutoTrain | [使用 AutoTrain 微调 LLM](https://github.com/huggingface/autotrain-advanced/?WT.mc_id=academic-105485-koreyst) | AutoTrain（或 AutoTrain Advanced）是 Hugging Face 开发的一个 Python 库，支持许多不同任务的微调，包括 LLM 微调。AutoTrain 是一种无代码（no-code）解决方案，可以在你自己的云、Hugging Face Spaces 或本地进行微调。它支持基于 Web 的图形界面、CLI 以及通过 yaml 配置文件进行训练。 |
| 🦥 Unsloth | [使用 Unsloth 微调 LLM](https://github.com/unslothai/unsloth?WT.mc_id=academic-105485-koreyst) | Unsloth 是一个支持 LLM 微调和强化学习（RL）的开源框架。Unsloth 通过开箱即用的 [notebooks](https://github.com/unslothai/notebooks?WT.mc_id=academic-105485-koreyst) 简化了本地训练、评估与部署。它还支持文本转语音（TTS）、BERT 和多模态模型。要开始使用，请阅读它们分步的 [微调 LLM 指南](https://docs.unsloth.ai/get-started/fine-tuning-llms-guide)。 |

## 作业

选择上面其中一个教程并完成它。_我们可能会在本仓库中以 Jupyter Notebooks 形式复刻这些教程的版本，仅供参。请直接使用原始来源以获取最新版本_。

## 干得漂亮！继续你的学习。

完成本课后，请查看我们的 [生成式 AI 学习合集](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)，继续提升你的生成式 AI 知识！

恭喜你！！你已完成本课程 v2 系列的最终一课！不要停止学习与构建。\*\*请查看 [资源（RESOURCES）](RESOURCES.md?WT.mc_id=academic-105485-koreyst) 页面，获取仅针对本主题的更多建议列表。

我们的 v1 系列课程也已更新，增加了更多作业与概念。所以花点时间刷新你的知识 —— 并请 [分享你的问题与反馈](https://github.com/microsoft/generative-ai-for-beginners/issues?WT.mc_id=academic-105485-koreyst)，帮助我们为社区改进这些课程。
