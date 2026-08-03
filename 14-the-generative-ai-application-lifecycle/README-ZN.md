[![与函数调用集成](./images/14-lesson-banner.png?WT.mc_id=academic-105485-koreyst)](https://youtu.be/ewtQY_RJrzs?si=dyJ2bjiljH7UUHCh)

# 生成式 AI 应用生命周期

对于所有 AI 应用而言，一个重要的问题在于 AI 功能的时效性——由于 AI 是一个快速演进的领域，要确保应用始终保持相关、可靠且稳健，你需要持续地监控、评估并改进它。这正是生成式 AI 生命周期（lifecycle）的用武之地。

生成式 AI 生命周期是一个框架，指引你经历开发、部署和维护一个生成式 AI 应用的各个阶段。它帮助你明确目标、衡量表现、识别挑战并落实解决方案。它还有助于让你的应用符合所在领域及利益相关方的伦理与法律标准。遵循生成式 AI 生命周期，你能够确保应用持续创造价值并令用户满意。

## 简介

在本章中，你将：

- 理解从 MLOps 到 LLMOps 的范式转变
- LLM 生命周期
- 生命周期工具
- 生命周期指标化与评估

## 理解从 MLOps 到 LLMOps 的范式转变

LLM 是人工智能工具箱中的新工具，在分析任务和生成任务上都极为强大，但这种能力也对我们如何简化 AI 与经典机器学习任务产生了一些影响。

为此，我们需要一种全新的范式，以动态的方式、配合正确的激励机制来适配这一工具。我们可以将较早期的 AI 应用归类为“ML 应用”，将较新的 AI 应用归类为“GenAI 应用”或直接称为“AI 应用”，以此反映当时主流的技术与方法。这从多个维度改变了我们的叙事，请看下面的对比。

![LLMOps 与 MLOps 对比](./images/01-llmops-shift.png?WT.mc_id=academic-105485-koreyst)

注意，在 LLMOps 中，我们更加聚焦于应用开发者，以集成为关键点，采用“模型即服务”（Models-as-a-Service）模式，并在指标上思考以下几点：

- 质量（Quality）：响应质量
- 危害（Harm）：负责任 AI
- 诚实（Honesty）：响应的忠实度（groundedness，说得通吗？是否正确？）
- 成本（Cost）：解决方案预算
- 延迟（Latency）：token 响应的平均耗时

## LLM 生命周期

首先，为了理解生命周期及其变化，我们先来看下面这张信息图。

![LLMOps 信息图](./images/02-llmops.png?WT.mc_id=academic-105485-koreyst)

如你所见，这与 MLOps 通常的生命周期不同。LLM 有许多新要求，例如提示词工程、提升质量的不同技术（微调、RAG、元提示）、结合负责任 AI 的评估与责任，以及新的评估指标（质量、危害、诚实、成本与延迟）。

例如，看看我们如何构思。使用提示词工程来试验各种 LLM，探索可能性，以检验自己的假设是否正确。

注意，这不是线性的，而是集成的循环、迭代式的，并带有一个总括性的大周期。

我们该如何探索这些步骤？让我们详细了解如何构建一个生命周期。

![LLMOps 工作流](./images/03-llm-stage-flows.png?WT.mc_id=academic-105485-koreyst)

这看起来可能有点复杂，我们先聚焦三大步骤。

1. 构思/探索（Ideating/Exploring）：探索阶段，我们可以依据业务需求进行探索。制作原型、创建 [PromptFlow](https://microsoft.github.io/promptflow/index.html?WT.mc_id=academic-105485-koreyst)，并测试其对我们假设的验证是否足够高效。
1. 构建/增强（Building/Augmenting）：实现阶段，现在我们开始对更大的数据集进行评估，实施诸如微调和 RAG 等技术，以检验解决方案的鲁棒性。如果不奏效，重新实现、在流程中加入新步骤或重组数据，可能会有帮助。在测试完我们的流程与规模之后，如果它有效并验证了我们的指标，就准备好进入下一步。
1. 运营化（Operationalizing）：集成阶段，现在为我们的系统加入监控与告警系统、部署，并将应用集成到我们的应用程序中。

接着，我们有一个总括性的管理周期，聚焦于安全、合规与治理。

恭喜，现在你的 AI 应用已经准备就绪并投入运营。如需动手体验，请查看 [Contoso Chat 演示](https://nitya.github.io/contoso-chat/?WT.mc_id=academic-105485-koreyst)。

那么，我们可以使用哪些工具呢？

## 生命周期工具

在工具方面，微软提供了 [Azure AI 平台](https://azure.microsoft.com/solutions/ai/?WT.mc_id=academic-105485-koreyst) 与 [PromptFlow](https://microsoft.github.io/promptflow/index.html?WT.mc_id=academic-105485-koreyst)，帮助你轻松落地并实现你的周期。

[Azure AI 平台](https://azure.microsoft.com/solutions/ai/?WT.mc_id=academic-105485-koreyst) 让你可以使用 [Microsoft Foundry](https://ai.azure.com/?WT.mc_id=academic-105485-koreyst)。Microsoft Foundry（原 Azure AI Studio）是一个 Web 门户，让你可以探索模型、示例与工具，管理你的资源，并使用 UI 开发流程以及 SDK/CLI 选项进行代码优先（Code-First）开发。

![Azure AI 的可能性](./images/04-azure-ai-platform.png?WT.mc_id=academic-105485-koreyst)

Azure AI 让你可以使用多种资源，来管理你的运营、服务、项目、向量搜索和数据库需求。

![使用 Azure AI 的 LLMOps](./images/05-llm-azure-ai-prompt.png?WT.mc_id=academic-105485-koreyst)

借助 PromptFlow，从概念验证（POC）一直构建到大规模应用：

- 在 VS Code 中设计和构建应用，使用可视化与功能性工具
- 轻松测试和微调你的应用，以获得高质量的 AI
- 使用 Microsoft Foundry 与云端集成和迭代，推送并部署以实现快速集成

![使用 PromptFlow 的 LLMOps](./images/06-llm-promptflow.png?WT.mc_id=academic-105485-koreyst)

## 太棒了！继续你的学习！

很棒，现在进一步了解我们如何结构化一个应用，以运用这些概念，请查看 [Contoso Chat App](https://nitya.github.io/contoso-chat/?WT.mc_id=academic-105485-koreyst)，看看云技术布道团队是如何在演示中加入这些概念的。如需更多内容，请查看我们的 [Ignite 分会场演讲！](https://www.youtube.com/watch?v=DdOylyrTOWg)

现在，请查看第 15 课，了解 [检索增强生成（RAG）与向量数据库](../15-rag-and-vector-databases/README.md?WT.mc_id=academic-105485-koreyst) 如何影响生成式 AI，以及如何打造更具吸引力的应用！
