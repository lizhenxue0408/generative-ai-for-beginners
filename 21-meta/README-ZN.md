# 使用 Meta 系列模型进行构建

## 简介

本课将涵盖：

- 探索 Meta 系列的两个主要模型 —— Llama 3.1 和 Llama 3.2
- 理解每个模型的用例与场景
- 展示每个模型独特特性的代码示例

## Meta 系列模型

在本课中，我们将探索来自 Meta 家族（或称“Llama Herd”羊群）的两个模型 —— Llama 3.1 和 Llama 3.2。

这些模型有不同的变体，可在 [Microsoft Foundry Models 目录](https://ai.azure.com/catalog/models?WT.mc_id=academic-105485-koreyst) 中获取。

> **注意：** GitHub Models 将于 2026 年 7 月底停用。更多关于使用 [Microsoft Foundry Models](https://learn.microsoft.com/azure/ai-foundry/model-inference/overview?WT.mc_id=academic-105485-koreyst) 进行 AI 模型原型开发的信息，请见此处。

模型变体：

- Llama 3.1 - 70B Instruct
- Llama 3.1 - 405B Instruct
- Llama 3.2 - 11B Vision Instruct
- Llama 3.2 - 90B Vision Instruct

*注意：Llama 3 在 Microsoft Foundry Models 中也可用，但本课不会涵盖。*

## Llama 3.1

拥有 4050 亿参数的 Llama 3.1 属于开源 LLM 类别。

该模型通过对早期发布的 Llama 3 进行升级，提供了：

- 更大的上下文窗口 —— 128k token vs 8k token
- 更大的最大输出 token 数 —— 4096 vs 2048
- 更好的多语言支持 —— 得益于训练 token 的增加

这些使 Llama 3.1 在构建生成式 AI 应用时，能够处理更复杂的用例，包括：

- 原生函数调用（Native Function Calling）—— 能够调用 LLM 工作流之外的外部工具与函数
- 更好的 RAG 表现 —— 得益于更高的上下文窗口
- 合成数据生成（Synthetic Data Generation）—— 能够为诸如微调等任务创建有效的数据

### 原生函数调用

Llama 3.1 经过微调，在发起函数或工具调用方面更加高效。它还有两个内置工具，模型可以根据用户的提示词识别出需要使用它们。这些工具是：

- **Brave Search（勇敢搜索）** —— 可用于通过执行网络搜索获取最新信息，如天气
- **Wolfram Alpha** —— 可用于更复杂的数学计算，因此你无需自己编写函数。

你也可以创建自己的自定义工具供 LLM 调用。

在下面的代码示例中：

- 我们在系统提示词中定义可用的工具（brave_search、wolfram_alpha）。
- 发送一条询问某个城市天气的用户提示词。
- LLM 将响应一个对 Brave Search 工具的函数调用，看起来像这样 `<|python_tag|>brave_search.call(query="Stockholm weather")`

*注意：此示例仅发起工具调用，如果你想获取结果，需要在 Brave API 页面创建一个免费账户并自行定义该函数。*

```python 
import os
from azure.ai.inference import ChatCompletionsClient
from azure.ai.inference.models import AssistantMessage, SystemMessage, UserMessage
from azure.core.credentials import AzureKeyCredential

# 这些信息从你的 Microsoft Foundry 项目的“概览”页面获取
token = os.environ["AZURE_INFERENCE_CREDENTIAL"]
endpoint = os.environ["AZURE_INFERENCE_ENDPOINT"]
model_name = "Meta-Llama-3.1-405B-Instruct"

client = ChatCompletionsClient(
    endpoint=endpoint,
    credential=AzureKeyCredential(token),
)


tool_prompt=f"""
<|begin_of_text|><|start_header_id|>system<|end_header_id|>

Environment: ipython
Tools: brave_search, wolfram_alpha
Cutting Knowledge Date: December 2023
Today Date: 23 July 2024

You are a helpful assistant<|eot_id|>
"""

messages = [
    SystemMessage(content=tool_prompt),
    UserMessage(content="What is the weather in Stockholm?"),

]

response = client.complete(messages=messages, model=model_name)

print(response.choices[0].message.content)
```

## Llama 3.2

尽管是一个 LLM，Llama 3.1 的一个局限在于其缺乏多模态能力。也就是说，它无法使用不同类型的输入（如图像）作为提示词并提供响应。这种能力正是 Llama 3.2 的主要特性之一。这些特性还包括：

- 多模态（Multimodality）—— 具备评估文本和图像提示词的能力
- 小型到中型变体（11B 和 90B）—— 这提供了灵活的部署选项
- 纯文本变体（1B 和 3B）—— 这允许模型部署在边缘 / 移动设备上，并提供低延迟

多模态支持代表了开源模型世界的重大进步。下面的代码示例同时接收一张图像和文本提示词，以从 Llama 3.2 90B 获取对图像的分析。

### 使用 Llama 3.2 的多模态支持

```python 
import os
from azure.ai.inference import ChatCompletionsClient
from azure.ai.inference.models import (
    SystemMessage,
    UserMessage,
    TextContentItem,
    ImageContentItem,
    ImageUrl,
    ImageDetailLevel,
)
from azure.core.credentials import AzureKeyCredential

# 这些信息从你的 Microsoft Foundry 项目的“概览”页面获取
token = os.environ["AZURE_INFERENCE_CREDENTIAL"]
endpoint = os.environ["AZURE_INFERENCE_ENDPOINT"]
model_name = "Llama-3.2-90B-Vision-Instruct"

client = ChatCompletionsClient(
    endpoint=endpoint,
    credential=AzureKeyCredential(token),
)

response = client.complete(
    messages=[
        SystemMessage(
            content="You are a helpful assistant that describes images in details."
        ),
        UserMessage(
            content=[
                TextContentItem(text="What's in this image?"),
                ImageContentItem(
                    image_url=ImageUrl.load(
                        image_file="sample.jpg",
                        image_format="jpg",
                        detail=ImageDetailLevel.LOW)
                ),
            ],
        ),
    ],
    model=model_name,
)

print(response.choices[0].message.content)
```

## 学习不止于此，继续你的旅程

完成本课后，请查看我们的 [生成式 AI 学习合集](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)，继续提升你的生成式 AI 知识！
