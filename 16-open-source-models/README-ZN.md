[![开源模型](./images/16-lesson-banner.png?WT.mc_id=academic-105485-koreyst)](https://youtu.be/CuICgfuHFSg?si=x8SpFRUsIxM9dohN)

## 简介

开源大语言模型（LLMs）的世界令人兴奋，且不断演进。本课旨在深入介绍开源模型。如果你在寻找专有模型与开源模型对比的相关信息，请前往 [“探索与对比不同的 LLM”课程](../02-exploring-and-comparing-different-llms/README.md?WT.mc_id=academic-105485-koreyst)。本课也会涉及微调的主题，但更详细的讲解可以在 [“微调 LLM”课程](../18-fine-tuning/README.md?WT.mc_id=academic-105485-koreyst) 中找到。

## 学习目标

- 理解开源模型
- 了解使用开源模型的优势
- 探索 Hugging Face 与 Microsoft Foundry 模型目录上可用的开源模型

## 什么是开源模型？

开源软件在多个领域的技术发展中发挥了至关重要的作用。开放源代码促进会（OSI）定义了软件被归类为开源的 [10 条标准](https://web.archive.org/web/20241126001143/https://opensource.org/osd?WT.mc_id=academic-105485-koreyst)。源代码必须在 OSI 批准的许可证下公开共享。

尽管 LLM 的开发与软件开发有相似之处，但过程并不完全相同。这在社区中引发了很多关于 LLM 语境下“开源”定义的讨论。对于一个模型要符合传统意义上的开源定义，以下信息应当公开可用：

- 用于训练模型的数据集。
- 训练过程中的完整模型权重。
- 评估代码。
- 微调代码。
- 完整的模型权重与训练指标。

目前只有极少数模型符合这些标准。[艾伦人工智能研究所（AllenAI）创建的 OLMo 模型](https://huggingface.co/allenai/OLMo-7B?WT.mc_id=academic-105485-koreyst) 就是属于这一类别的一个例子。

在本课中，我们此后将这些模型称为“开源模型”，因为它们在本文撰写时可能不完全符合上述标准。

## 开源模型的优势

**高度可定制** —— 由于开源模型发布时附带详细的训练信息，研究人员和开发者可以修改模型内部。这使得创建针对特定任务或研究领域高度专门化的模型成为可能。例如代码生成、数学运算和生物学领域的一些应用。

**成本** —— 使用与部署这些模型的每 token 成本低于专有模型。在构建生成式 AI 应用时，应当结合你的实际用例，权衡这些模型的性能与价格。

![模型成本](./images/model-price.png?WT.mc_id=academic-105485-koreyst)
来源：Artificial Analysis

**灵活性** —— 使用开源模型使你在采用不同模型或组合模型方面具备灵活性。一个例子是 [HuggingChat Assistants](https://huggingface.co/chat?WT.mc_id=academic-105485-koreyst)，用户可以在用户界面中直接选择所使用的模型：

![选择模型](./images/choose-model.png?WT.mc_id=academic-105485-koreyst)

## 探索不同的开源模型

### Llama 2

[LLama2](https://huggingface.co/meta-llama?WT.mc_id=academic-105485-koreyst) 由 Meta 开发，是一个为聊天类应用优化的开源模型。这得益于其微调方法，该方法包含了大量对话和人类反馈。通过这种方式，模型产生的更多结果符合人类预期，从而提供更好的用户体验。

Llama 的一些微调版本包括专注于日语的 [Japanese Llama](https://huggingface.co/elyza/ELYZA-japanese-Llama-2-7b?WT.mc_id=academic-105485-koreyst)，以及基础模型的增强版 [Llama Pro](https://huggingface.co/TencentARC/LLaMA-Pro-8B?WT.mc_id=academic-105485-koreyst)。

### Mistral

[Mistral](https://huggingface.co/mistralai?WT.mc_id=academic-105485-koreyst) 是一个高度专注于高性能与高效率的开源模型。它采用了混合专家（Mixture-of-Experts）方法，将一组专门化的专家模型组合成一个系统，根据输入的不同，选择特定的模型来使用。这使得计算更高效，因为模型只处理它们专门擅长的输入。

Mistral 的微调版本包括专注于医疗领域的 [BioMistral](https://huggingface.co/BioMistral/BioMistral-7B?text=Mon+nom+est+Thomas+et+mon+principal?WT.mc_id=academic-105485-koreyst)，以及用于数学计算的 [OpenMath Mistral](https://huggingface.co/nvidia/OpenMath-Mistral-7B-v0.1-hf?WT.mc_id=academic-105485-koreyst)。

### Falcon

[Falcon](https://huggingface.co/tiiuae?WT.mc_id=academic-105485-koreyst) 是由技术创新研究院（**TII**）创建的 LLM。Falcon-40B 在 400 亿参数上训练，其表现已被证明在更少计算预算的情况下优于 GPT-3。这得益于它使用了 FlashAttention 算法和多查询注意力（multiquery attention），能够在推理时降低内存需求。凭借更短的推理时间，Falcon-40B 适用于聊天应用。

Falcon 的微调版本包括基于开源模型构建的助手 [OpenAssistant](https://huggingface.co/OpenAssistant/falcon-40b-sft-top1-560?WT.mc_id=academic-105485-koreyst)，以及性能优于基础模型的 [GPT4ALL](https://huggingface.co/nomic-ai/gpt4all-falcon?WT.mc_id=academic-105485-koreyst)。

## 如何选择

选择开源模型没有唯一的答案。一个不错的起点是使用 Microsoft Foundry 模型目录的“按任务筛选”功能。这将帮助你了解模型训练所针对的任务类型。Hugging Face 还维护着一个 LLM 排行榜，根据某些指标展示表现最好的模型。

当想要跨不同类型对比 LLM 时，[Artificial Analysis](https://artificialanalysis.ai/?WT.mc_id=academic-105485-koreyst) 是另一个很好的资源：

![模型质量](./images/model-quality.png?WT.mc_id=academic-105485-koreyst)
来源：Artificial Analysis

如果针对某个具体用例，搜索专注于同一领域的微调版本会很有效。尝试多个开源模型，看看它们根据你和用户的预期表现如何，也是一项不错的做法。

## 下一步

开源模型最棒的一点在于，你可以相当快速地上手使用它们。查看 [Microsoft Foundry 模型目录](https://ai.azure.com?WT.mc_id=academic-105485-koreyst)，其中收录了我们在本文中讨论的这些模型所对应的 Hugging Face 合集。

## 学习不止于此，继续你的旅程

完成本课后，请查看我们的 [生成式 AI 学习合集](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)，继续提升你的生成式 AI 知识！
