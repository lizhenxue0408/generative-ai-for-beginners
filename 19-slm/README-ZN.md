# 面向初学者的生成式 AI：小型语言模型（SLM）入门

生成式 AI 是人工智能中一个引人入胜的领域，专注于创建能够生成新内容的系统。这些内容可以是文本、图像，也可以是音乐，甚至是完整的虚拟环境。生成式 AI 最令人激动的应用之一，出现在语言模型领域。

## 什么是小型语言模型？

小型语言模型（Small Language Model，SLM）代表着大语言模型（LLM）的缩小版变体，它借鉴了 LLM 的许多架构原则与技术，同时显著降低了计算占用。

SLM 是语言模型的一个子集，旨在生成类人文本。与 GPT-4 等更大的同类模型不同，SLM 更加紧凑且高效，非常适合计算资源受限的应用。尽管规模更小，它们仍能执行多种任务。通常，SLM 通过对 LLM 进行压缩或蒸馏（distilling）来构建，旨在保留原始模型的相当一部分功能与语言能力。这种模型规模的减小降低了整体复杂性，使 SLM 在内存使用和计算需求方面都更高效。尽管有这些优化，SLM 仍能执行广泛的自然语言处理（NLP）任务：

- 文本生成：创建连贯且上下文相关的句子或段落。
- 文本补全：根据给定提示预测并补全句子。
- 翻译：将文本从一种语言转换为另一种语言。
- 摘要：将长文本浓缩为更短、更易消化的摘要。

尽管在性能或理解深度上相比更大的同类模型会有一些权衡。

## 小型语言模型是如何工作的？

SLM 在大量文本数据上进行训练。在训练期间，它们学习语言的模式与结构，使它们能够生成既语法正确又上下文恰当的文本。训练过程包括：

- 数据收集：从各种来源收集大型文本数据集。
- 预处理：清理并组织数据，使其适合训练。
- 训练：使用机器学习算法教导模型如何理解并生成文本。
- 微调：调整模型以提升其在特定任务上的表现。

SLM 的发展与对能够在资源受限环境中部署的模型日益增长的需求相一致，例如移动设备或边缘计算平台，在这些场景中，全尺寸的 LLM 由于其沉重的资源需求可能并不实用。通过专注于效率，SLM 在性能与可访问性之间取得平衡，使其能够在各个领域得到更广泛的应用。

![slm](./img/slm.png?WT.mc_id=academic-105485-koreyst)

## 学习目标

在本课中，我们希望介绍 SLM 的知识，并将其与微软 Phi-3 结合，学习文本、视觉和 MoE（混合专家）中的不同场景。

到本课结束时，你应该能够回答以下问题：

- 什么是 SLM？
- SLM 与 LLM 有什么区别？
- 什么是微软 Phi-3/3.5 系列？
- 如何运行微软 Phi-3/3.5 系列的推理？

准备好了吗？我们开始吧。

## 大语言模型（LLMs）与小型语言模型（SLMs）的区别

LLM 和 SLM 都建立在概率机器学习的基础原理之上，在架构设计、训练方法、数据生成过程和模型评估技术方面遵循相似的方法。然而，有几个关键因素使这两类模型有所区别。

## 小型语言模型的应用

SLM 有广泛的应用，包括：

- 聊天机器人：提供客户支持并以对话方式与用户互动。
- 内容创作：通过生成创意甚至起草整篇文章来协助写作者。
- 教育：帮助学生完成写作作业或学习新语言。
- 可访问性：为残障人士创建工具，例如文本转语音系统。

**规模（Size）**

LLM 与 SLM 的一个主要区别在于模型的规模。LLM，例如 ChatGPT（GPT-4），估计包含约 1.76 万亿参数，而像 Mistral 7B 这样的开源 SLM 设计的参数数量显著更少 —— 约 70 亿。这种差异主要源于模型架构与训练过程的不同。例如，ChatGPT 在编码器-解码器（encoder-decoder）框架内采用自注意力机制，而 Mistral 7B 使用滑动窗口注意力（sliding window attention），这使得在仅解码器（decoder-only）模型中能够更高效地进行训练。这种架构上的差异对模型的复杂性和性能有着深远影响。

**理解力（Comprehension）**

SLM 通常针对特定领域内的性能进行了优化，使其高度专门化，但在跨多个知识领域提供广泛上下文理解方面的能力可能有限。相比之下，LLM 旨在更全面地模拟类人智能。LLM 在庞大、多样的数据集上训练，被设计为在多个领域表现良好，提供更大的通用性与适应性。因此，LLM 更适合范围更广的下游任务，例如自然语言处理和编程。

**计算（Computing）**

LLM 的训练与部署都是资源密集的过程，通常需要大量计算基础设施，包括大规模 GPU 集群。例如，从头训练像 ChatGPT 这样的模型可能需要数千个 GPU，历时很长一段时间。相比之下，参数数量更少的 SLM 在计算资源方面更容易获取。像 Mistral 7B 这样的模型可以在配备中等 GPU 能力的本地机器上训练和运行，尽管训练仍然需要在多个 GPU 上耗时数小时。

**偏见（Bias）**

偏见是 LLM 中的一个已知问题，主要源于训练数据的性质。这些模型通常依赖互联网上原始、公开可用的数据，这些数据可能低估或错误呈现某些群体、引入错误标注，或反映受方言、地理差异和语法规则影响而产生的语言偏见。此外，LLM 架构的复杂性可能无意中加剧偏见，若没有仔细的微调，这些问题可能不会被注意到。另一方面，SLM 由于在更受限、领域特定的数据集上训练，本质上较少受到此类偏见的影响，尽管它们也并非完全免疫。

**推理（Inference）**

SLM 较小的规模使其在推理速度方面具有显著优势，能够在本地硬件上高效生成输出，而无需大量并行处理。相比之下，LLM 由于其规模与复杂性，通常需要大量并行计算资源才能达到可接受的推理时间。多个并发用户的存在会进一步拖慢 LLM 的响应时间，尤其是在大规模部署时。

总之，尽管 LLM 和 SLM 都共享机器学习的根本基础，但他们在模型规模、资源需求、上下文理解、偏见敏感性和推理速度方面存在显著差异。这些区别反映了它们各自对不同用例的适用性：LLM 更加通用但资源密集，而 SLM 以更少的计算需求提供更多的领域特定效率。

***注意：在本课中，我们将以微软 Phi-3 / 3.5 为例来介绍 SLM。***

## 介绍 Phi-3 / Phi-3.5 系列

Phi-3 / 3.5 系列主要面向文本、视觉和智能体（MoE）应用场景：

### Phi-3 / 3.5 Instruct（指令模型）

主要用于文本生成、聊天补全和内容信息提取等。

**Phi-3-mini**

这个 38 亿参数的语言模型可在 Microsoft Foundry、Hugging Face 和 Ollama 上获取。Phi-3 模型在关键基准测试上显著优于同等及更大规模的模型（基准数字见下方，数字越大越好）。Phi-3-mini 优于两倍于其规模的模型，而 Phi-3-small 和 Phi-3-medium 则优于更大的模型，包括 GPT-3.5。

**Phi-3-small 与 medium**

凭借仅 70 亿参数，Phi-3-small 在多种语言、推理、编码和数学基准测试上击败了 GPT-3.5T。

拥有 140 亿参数的 Phi-3-medium 延续了这一趋势，并优于 Gemini 1.0 Pro。

**Phi-3.5-mini**

我们可以把它看作 Phi-3-mini 的升级版。虽然参数保持不变，但它增强了对多语言的支持（支持 20+ 种语言：阿拉伯语、中文、捷克语、丹麦语、荷兰语、英语、芬兰语、法语、德语、希伯来语、匈牙利语、意大利语、日语、韩语、挪威语、波兰语、葡萄牙语、俄语、西班牙语、瑞典语、泰语、土耳其语、乌克兰语），并增加了对长上下文的更强支持。

拥有 38 亿参数的 Phi-3.5-mini 优于同等规模的模型，并与两倍于其规模的模型表现相当。

### Phi-3 / 3.5 Vision（视觉模型）

我们可以把 Phi-3/3.5 的指令模型理解为 Phi 的“理解”能力，而视觉则是赋予 Phi “眼睛”、以理解世界的能力。

**Phi-3-Vision**

Phi-3-vision 仅拥有 42 亿参数，延续了这一趋势，并在通用视觉推理任务、OCR，以及表格和图表理解任务上优于更大的模型，如 Claude-3 Haiku 和 Gemini 1.0 Pro V。

**Phi-3.5-Vision**

Phi-3.5-Vision 也是 Phi-3-Vision 的升级版，增加了对多张图片的支持。你可以把它看作视觉能力的提升，不仅能看图，还能看视频。

Phi-3.5-vision 在 OCR、表格和图表理解任务上优于 Claude-3.5 Sonnet 和 Gemini 1.5 Flash 等更大的模型，并在通用视觉知识推理任务上表现相当。支持多帧输入，即可以对多张输入图像进行推理。

### Phi-3.5-MoE

***混合专家（Mixture of Experts，MoE）*** 使模型能够以远少得多的计算量进行预训练，这意味着你可以用与稠密（dense）模型相同的计算预算，大幅增加模型或数据集的规模。具体而言，MoE 模型在预训练期间应能比其稠密对应模型更快达到相同的质量。

Phi-3.5-MoE 由 16x3.8B 的专家模块组成。Phi-3.5-MoE 仅使用 66 亿活跃参数，就达到了与更大模型相当的推理、语言理解和数学水平。

我们可以基于不同场景使用 Phi-3/3.5 系列模型。与 LLM 不同，你可以将 Phi-3/3.5-mini 或 Phi-3/3.5-Vision 部署在边缘设备上。

## 如何使用 Phi-3/3.5 系列模型

我们希望在不同场景中使用 Phi-3/3.5。接下来，我们将基于不同场景来使用 Phi-3/3.5。

![phi3](./img/phi3.png?WT.mc_id=academic-105485-koreyst)

### 通过云端 API 进行推理

**Microsoft Foundry Models**

> **注意：** GitHub Models 将于 2026 年 7 月底停用。[Microsoft Foundry Models](https://ai.azure.com/catalog/models?WT.mc_id=academic-105485-koreyst) 是直接替代品。

Microsoft Foundry Models 是最直接的方式。你可以通过 Foundry 模型目录快速访问 Phi-3/3.5-Instruct 模型。结合 Azure AI Inference SDK / OpenAI SDK，你可以通过代码访问 API，从而完成对 Phi-3/3.5-Instruct 的调用。你也可以通过 Playground 测试不同的效果。

- 演示：Phi-3-mini 与 Phi-3.5-mini 在中文场景下的效果对比

![phi3](./img/gh1.png?WT.mc_id=academic-105485-koreyst)

![phi35](./img/gh2.png?WT.mc_id=academic-105485-koreyst)

**Microsoft Foundry**

或者，如果我们想使用视觉和 MoE 模型，可以使用 Microsoft Foundry 来完成调用。如果你感兴趣，可以阅读 Phi-3 Cookbook，了解如何通过 Microsoft Foundry 调用 Phi-3/3.5 Instruct、Vision、MoE [点击此链接](https://github.com/microsoft/Phi-3CookBook/blob/main/md/02.QuickStart/AzureAIStudio_QuickStart.md?WT.mc_id=academic-105485-koreyst)

**NVIDIA NIM**

除了基于云的 Microsoft Foundry Models 目录，你还可以使用 [NVIDIA NIM](https://developer.nvidia.com/nim?WT.mc_id=academic-105485-koreyst) 来完成相关调用。你可以访问 NVIDIA NIM 以完成对 Phi-3/3.5 系列的 API 调用。NVIDIA NIM（NVIDIA Inference Microservices，NVIDIA 推理微服务）是一组加速推理微服务，旨在帮助开发者跨各种环境（包括云、数据中心和工作站）高效部署 AI 模型。

以下是 NVIDIA NIM 的一些关键特性：

- **部署简便**：NIM 允许通过一条命令部署 AI 模型，使其能够轻松集成到现有工作流中。
- **性能优化**：它利用 NVIDIA 预优化的推理引擎，如 TensorRT 和 TensorRT-LLM，确保低延迟和高吞吐。
- **可扩展性**：NIM 支持在 Kubernetes 上自动扩缩容，使其能够有效处理不同的工作负载。
- **安全性与控制**：组织可以通过在自己的受管基础设施上自托管 NIM 微服务，保持对其数据和应用程序的控制。
- **标准 API**：NIM 提供行业标准的 API，使构建和集成聊天机器人、AI 助手等 AI 应用变得容易。

NIM 是 NVIDIA AI Enterprise 的一部分，旨在简化 AI 模型的部署与运营，确保它们在 NVIDIA GPU 上高效运行。

- 演示：使用 NVIDIA NIM 调用 Phi-3.5-Vision-API [[点击此链接](./python/Phi-3-Vision-Nividia-NIM.ipynb?WT.mc_id=academic-105485-koreyst)]

### 在本地运行 Phi-3/3.5

关于 Phi-3 或任何像 GPT-3 这样的语言模型的推理，指的是基于其接收到的输入生成响应或预测的过程。当你向 Phi-3 提供提示或问题时，它利用训练好的神经网络，通过分析训练数据中存在的模式与关系，推断最可能且相关的响应。

**Hugging Face Transformer**

Hugging Face Transformers 是一个为自然语言处理（NLP）和其他机器学习任务设计的强大库。以下是关于它的一些关键点：

1. **预训练模型**：它提供数千个预训练模型，可用于各种任务，如文本分类、命名实体识别、问答、摘要、翻译和文本生成。

2. **框架互操作性**：该库支持多个深度学习框架，包括 PyTorch、TensorFlow 和 JAX。这允许你在一个框架中训练模型，并在另一个框架中使用它。

3. **多模态能力**：除了 NLP，Hugging Face Transformers 还支持计算机视觉（如图像分类、目标检测）和音频处理（如语音识别、音频分类）任务。

4. **易用性**：该库提供 API 和工具，可轻松下载和微调模型，使其对初学者和专家都易于上手。

5. **社区与资源**：Hugging Face 拥有活跃的社区和丰富的文档、教程与指南，帮助用户快速上手并充分利用该库。

[官方文档](https://huggingface.co/docs/transformers/index?WT.mc_id=academic-105485-koreyst) 或他们的 [GitHub 代码仓库](https://github.com/huggingface/transformers?WT.mc_id=academic-105485-koreyst)。

这是最常用的方法，但它也需要 GPU 加速。毕竟，像视觉和 MoE 这样的场景需要大量计算，如果没有量化，在 CPU 上会非常慢。

- 演示：使用 Transformer 调用 Phi-3.5-Instruct [点击此链接](./python/phi35-instruct-demo.ipynb?WT.mc_id=academic-105485-koreyst)

- 演示：使用 Transformer 调用 Phi-3.5-Vision [点击此链接](./python/phi35-vision-demo.ipynb?WT.mc_id=academic-105485-koreyst)

- 演示：使用 Transformer 调用 Phi-3.5-MoE [点击此链接](./python/phi35_moe_demo.ipynb?WT.mc_id=academic-105485-koreyst)

**Ollama**

[Ollama](https://ollama.com/?WT.mc_id=academic-105485-koreyst) 是一个旨在让你更轻松地在本地机器上运行大语言模型（LLMs）的平台。它支持各种模型，如 Llama 3.1、Phi 3、Mistral 和 Gemma 2 等。该平台通过将模型权重、配置和数据打包成一个单独的包，简化了这一过程，使用户更容易自定义和创建自己的模型。Ollama 可用于 macOS、Linux 和 Windows。如果你想在不依赖云服务的情况下试验或部署 LLM，它是一个很棒的工具。Ollama 是最直接的方式，你只需执行以下命令。

```bash

ollama run phi3.5

```

**Foundry Local**

[Foundry Local](https://foundrylocal.ai?WT.mc_id=academic-105485-koreyst) 是微软的离线、设备端运行时，用于在你自己的硬件上完全运行像 Phi 这样的模型 —— 无需 Azure 订阅、API 密钥或网络连接。它会自动选择可用的最佳执行提供程序（NPU、GPU 或 CPU），并暴露一个兼容 OpenAI 的终结点，因此现有的 `openai` / Azure AI Inference SDK 代码几乎无需改动即可指向它。请参阅 [Foundry Local 文档](https://learn.microsoft.com/azure/ai-foundry/foundry-local/get-started?WT.mc_id=academic-105485-koreyst) 以开始使用。

```bash

winget install Microsoft.FoundryLocal
foundry model run phi-3.5-mini

```

或者直接在 Python 中使用 SDK：

```bash

pip install foundry-local-sdk

```

```python

from foundry_local import FoundryLocalManager

manager = FoundryLocalManager("phi-3.5-mini")
print(manager.endpoint, manager.api_key)

```

**ONNX Runtime for GenAI**

[ONNX Runtime](https://github.com/microsoft/onnxruntime-genai?WT.mc_id=academic-105485-koreyst) 是一个跨平台的推理与训练机器学习加速器。用于生成式 AI 的 ONNX Runtime（GENAI）是一个强大的工具，可帮助你在各种平台上高效运行生成式 AI 模型。

## 什么是 ONNX Runtime？

ONNX Runtime 是一个开源项目，支持机器学习模型的高性能推理。它支持 ONNX（开放神经网络交换，Open Neural Network Exchange）格式的模型，这是表示机器学习模型的一种标准。ONNX Runtime 推理能够带来更快的客户体验并降低成本，支持来自 PyTorch 和 TensorFlow/Keras 等深度学习框架，以及 scikit-learn、LightGBM、XGBoost 等经典机器学习库中的模型。ONNX Runtime 兼容不同的硬件、驱动程序和操作系统，并通过利用适用的硬件加速器以及图优化和转换来提供最佳性能。

## 什么是生成式 AI？

生成式 AI 指的是能够根据其所训练的数据生成新内容（如文本、图像或音乐）的 AI 系统。示例包括像 GPT-3 这样的语言模型，以及像 Stable Diffusion 这样的图像生成模型。ONNX Runtime for GenAI 库为 ONNX 模型提供生成式 AI 循环，包括使用 ONNX Runtime 推理、logits 处理、搜索与采样，以及 KV 缓存管理。

## ONNX Runtime for GENAI

ONNX Runtime for GENAI 扩展了 ONNX Runtime 的能力，以支持生成式 AI 模型。以下是一些关键特性：

- **广泛的平台支持**：它可在各种平台上运行，包括 Windows、Linux、macOS、Android 和 iOS。
- **模型支持**：它支持许多流行的生成式 AI 模型，如 LLaMA、GPT-Neo、BLOOM 等。
- **性能优化**：它包括针对不同硬件加速器（如 NVIDIA GPU、AMD GPU 等）的优化。
- **易用性**：它提供 API 以便轻松集成到应用中，让你用最少的代码生成文本、图像和其他内容。
- 用户可以调用高级的 generate() 方法，也可以在循环中运行模型的每次迭代，一次生成一个 token，并可选择在循环内更新生成参数。
- ONNX runtime 还支持贪心/束搜索（greedy/beam search）以及 TopP、TopK 采样来生成 token 序列，并内置了如重复惩罚（repetition penalties）等 logits 处理。你也可以轻松添加自定义评分。

## 入门

要开始使用 ONNX Runtime for GENAI，你可以按照以下步骤操作：

### 安装 ONNX Runtime：

```Python
pip install onnxruntime
```

### 安装生成式 AI 扩展：

```Python
pip install onnxruntime-genai
```

### 运行一个模型：以下是一个简单的 Python 示例：

```Python
import onnxruntime_genai as og

model = og.Model('path_to_your_model.onnx')

tokenizer = og.Tokenizer(model)

input_text = "Hello, how are you?"

input_tokens = tokenizer.encode(input_text)

output_tokens = model.generate(input_tokens)

output_text = tokenizer.decode(output_tokens)

print(output_text) 
```

### 演示：使用 ONNX Runtime GenAI 调用 Phi-3.5-Vision

```python

import onnxruntime_genai as og

model_path = './Your Phi-3.5-vision-instruct ONNX Path'

img_path = './Your Image Path'

model = og.Model(model_path)

processor = model.create_multimodal_processor()

tokenizer_stream = processor.create_stream()

text = "Your Prompt"

prompt = "<|user|>\n"

prompt += "<|image_1|>\n"

prompt += f"{text}<|end|>\n"

prompt += "<|assistant|>\n"

image = og.Images.open(img_path)

inputs = processor(prompt, images=image)

params = og.GeneratorParams(model)

params.set_inputs(inputs)

params.set_search_options(max_length=3072)

generator = og.Generator(model, params)

while not generator.is_done():

    generator.compute_logits()
    
    generator.generate_next_token()

    new_token = generator.get_next_tokens()[0]
    
    output = tokenizer_stream.decode(new_token)
    
    print(tokenizer_stream.decode(new_token), end='', flush=True)

```

**其他（Others）**

除了 ONNX Runtime、Ollama 和 Foundry Local 参考方法，我们还可以基于不同厂商提供的模型参考方法，完成量化模型的引用。例如 Apple MLX 框架配合 Apple Metal、Qualcomm QNN 配合 NPU、Intel OpenVINO 配合 CPU/GPU 等。你也可以从 [Phi-3 Cookbook](https://github.com/microsoft/phi-3cookbook?WT.mc_id=academic-105485-koreyst) 获取更多内容。

## 了解更多

我们已经学习了 Phi-3/3.5 系列的基础知识，但要更深入地了解 SLM，我们需要更多知识。你可以在 Phi-3 Cookbook 中找到答案。如果你想了解更多，请访问 [Phi-3 Cookbook](https://github.com/microsoft/phi-3cookbook?WT.mc_id=academic-105485-koreyst)。
