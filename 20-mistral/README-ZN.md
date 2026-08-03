# 使用 Mistral 模型进行构建

## 简介

本课将涵盖：

- 探索不同的 Mistral 模型
- 理解每个模型的用例与场景
- 探索展示每个模型独特特性的代码示例

## Mistral 模型

在本课中，我们将探索 3 种不同的 Mistral 模型：**Mistral Large**、**Mistral Small** 和 **Mistral Nemo**。

每个模型都可以在 [Microsoft Foundry Models](https://ai.azure.com/catalog/models?WT.mc_id=academic-105485-koreyst) 上免费获取。本 notebook 中的代码将使用这些模型来运行代码。

> **注意：** GitHub Models 将于 2026 年 7 月底停用。更多关于使用 [Microsoft Foundry Models](https://learn.microsoft.com/azure/ai-foundry/model-inference/overview?WT.mc_id=academic-105485-koreyst) 进行 AI 模型原型开发的信息，请见此处。

## Mistral Large 2（2407）

Mistral Large 2 是目前 Mistral 的旗舰模型，专为企业级使用而设计。

该模型通过对原始 Mistral Large 的升级，提供了：

- 更大的上下文窗口 —— 128k vs 32k
- 在数学和编码任务上更好的表现 —— 平均准确率 76.9% vs 60.4%
- 增强的多语言表现 —— 支持的语言包括：英语、法语、德语、西班牙语、意大利语、葡萄牙语、荷兰语、俄语、中文、日语、韩语、阿拉伯语和印地语。

凭借这些特性，Mistral Large 在以下方面表现出色：

- *检索增强生成（RAG）* —— 得益于更大的上下文窗口
- *函数调用（Function Calling）* —— 该模型具备原生的函数调用能力，允许与外部工具和 API 集成。这些调用既可以并行发起，也可以按顺序一个接一个地发起。
- *代码生成* —— 该模型在 Python、Java、TypeScript 和 C++ 的生成上表现出色。

### 使用 Mistral Large 2 的 RAG 示例

在此示例中，我们使用 Mistral Large 2 在一段文本文档上运行 RAG 模式。问题用韩语书写，询问作者在上大学之前的activities。

它使用 Cohere Embeddings 模型来创建文本文档以及问题的嵌入。对于此示例，它使用 faiss Python 包作为向量存储。

发送给 Mistral 模型的提示词同时包含问题以及检索到的、与问题相似的文本块。然后模型提供一个自然语言响应。

```python 
pip install faiss-cpu
```

```python 
import requests
import numpy as np
import faiss
import os

from azure.ai.inference import ChatCompletionsClient
from azure.ai.inference.models import SystemMessage, UserMessage
from azure.core.credentials import AzureKeyCredential
from azure.ai.inference import EmbeddingsClient

# 这些信息从你的 Microsoft Foundry 项目的“概览”页面获取
endpoint = os.environ["AZURE_INFERENCE_ENDPOINT"]
model_name = "Mistral-large"
token = os.environ["AZURE_INFERENCE_CREDENTIAL"]

client = ChatCompletionsClient(
    endpoint=endpoint,
    credential=AzureKeyCredential(token),
)

response = requests.get('https://raw.githubusercontent.com/run-llama/llama_index/main/docs/docs/examples/data/paul_graham/paul_graham_essay.txt')
text = response.text

chunk_size = 2048
chunks = [text[i:i + chunk_size] for i in range(0, len(text), chunk_size)]
len(chunks)

embed_model_name = "cohere-embed-v3-multilingual" 

embed_client = EmbeddingsClient(
        endpoint=endpoint,
        credential=AzureKeyCredential(token)
)

embed_response = embed_client.embed(
    input=chunks,
    model=embed_model_name
)



text_embeddings = []
for item in embed_response.data:
    length = len(item.embedding)
    text_embeddings.append(item.embedding)
text_embeddings = np.array(text_embeddings)


d = text_embeddings.shape[1]
index = faiss.IndexFlatL2(d)
index.add(text_embeddings)

question = "저자가 대학에 오기 전에 주로 했던 두 가지 일은 무엇이었나요?"

question_embedding = embed_client.embed(
    input=[question],
    model=embed_model_name
)

question_embeddings = np.array(question_embedding.data[0].embedding)


D, I = index.search(question_embeddings.reshape(1, -1), k=2) # distance, index
retrieved_chunks = [chunks[i] for i in I.tolist()[0]]

prompt = f"""
Context information is below.
---------------------
{retrieved_chunks}
---------------------
Given the context information and not prior knowledge, answer the query.
Query: {question}
Answer:
"""


chat_response = client.complete(
    messages=[
        SystemMessage(content="You are a helpful assistant."),
        UserMessage(content=prompt),
    ],
    temperature=1.0,
    top_p=1.0,
    max_tokens=1000,
    model=model_name
)

print(chat_response.choices[0].message.content)
```

## Mistral Small

Mistral Small 是 Mistral 模型家族中属于premier/企业类别的另一个模型。顾名思义，该模型是一个小型语言模型（SLM）。使用 Mistral Small 的优势在于：

- 相比于 Mistral 的 LLM（如 Mistral Large 和 NeMo），成本节省 —— 价格下降 80%
- 低延迟 —— 相比 Mistral 的 LLM 响应更快
- 灵活 —— 可部署在不同的环境中，对所需资源的限制更少。

Mistral Small 非常适合：

- 基于文本的任务，如摘要、情感分析和翻译。
- 由于其成本效益，适用于频繁发起请求的应用
- 低延迟的代码任务，如代码审查与代码建议

## 对比 Mistral Small 与 Mistral Large

为了展示 Mistral Small 与 Large 之间延迟的差异，运行下面的单元。

你应该会看到 3-5 秒的响应时间差异。同时请注意在相同提示下响应的长度与风格。

```python 

import os 
endpoint = os.environ["AZURE_INFERENCE_ENDPOINT"]
model_name = "Mistral-small"
token = os.environ["AZURE_INFERENCE_CREDENTIAL"]

client = ChatCompletionsClient(
    endpoint=endpoint,
    credential=AzureKeyCredential(token),
)

response = client.complete(
    messages=[
        SystemMessage(content="You are a helpful coding assistant."),
        UserMessage(content="Can you write a Python function to the fizz buzz test?"),
    ],
    temperature=1.0,
    top_p=1.0,
    max_tokens=1000,
    model=model_name
)

print(response.choices[0].message.content)

```

```python 

import os
from azure.ai.inference import ChatCompletionsClient
from azure.ai.inference.models import SystemMessage, UserMessage
from azure.core.credentials import AzureKeyCredential

endpoint = os.environ["AZURE_INFERENCE_ENDPOINT"]
model_name = "Mistral-large"
token = os.environ["AZURE_INFERENCE_CREDENTIAL"]

client = ChatCompletionsClient(
    endpoint=endpoint,
    credential=AzureKeyCredential(token),
)

response = client.complete(
    messages=[
        SystemMessage(content="You are a helpful coding assistant."),
        UserMessage(content="Can you write a Python function to the fizz buzz test?"),
    ],
    temperature=1.0,
    top_p=1.0,
    max_tokens=1000,
    model=model_name
)

print(response.choices[0].message.content)

```

## Mistral NeMo

与本节讨论的另外两个模型相比，Mistral NeMo 是唯一的、采用 Apache2 许可证的免费模型。

它被视为 Mistral 早期开源 LLM（Mistral 7B）的升级版。

NeMo 模型的其他一些特性包括：

- *更高效的分词（tokenization）：* 该模型使用 Tekken 分词器，而非更常用的 tiktoken。这使其在更多语言和代码上表现更好。

- *微调（Finetuning）：* 基础模型可用于微调。这为可能需要微调的用例提供了更大的灵活性。

- *原生函数调用（Native Function Calling）* —— 与 Mistral Large 类似，该模型在函数调用上进行了训练。这使其独具特色，成为首批如此做的开源模型之一。

### 对比分词器

在此示例中，我们将观察 Mistral NeMo 与 Mistral Large 在分词处理上的差异。

两个示例采用相同的提示词，但你应该会看到 NeMo 返回的 token 数量少于 Mistral Large。

```bash
pip install mistral-common
```

```python 
# 导入所需包：
from mistral_common.protocol.instruct.messages import (
    UserMessage,
)
from mistral_common.protocol.instruct.request import ChatCompletionRequest
from mistral_common.protocol.instruct.tool_calls import (
    Function,
    Tool,
)
from mistral_common.tokens.tokenizers.mistral import MistralTokenizer

# 加载 Mistral 分词器

model_name = "open-mistral-nemo"

tokenizer = MistralTokenizer.from_model(model_name)

# 对一组消息进行分词
tokenized = tokenizer.encode_chat_completion(
    ChatCompletionRequest(
        tools=[
            Tool(
                function=Function(
                    name="get_current_weather",
                    description="Get the current weather",
                    parameters={
                        "type": "object",
                        "properties": {
                            "location": {
                                "type": "string",
                                "description": "The city and state, e.g. San Francisco, CA",
                            },
                            "format": {
                                "type": "string",
                                "enum": ["celsius", "fahrenheit"],
                                "description": "The temperature unit to use. Infer this from the user's location.",
                            },
                        },
                        "required": ["location", "format"],
                    },
                )
            )
        ],
        messages=[
            UserMessage(content="What's the weather like today in Paris"),
        ],
        model=model_name,
    )
)
tokens, text = tokenized.tokens, tokenized.text

# 统计 token 数量
print(len(tokens))
```

```python
# 导入所需包：
from mistral_common.protocol.instruct.messages import (
    UserMessage,
)
from mistral_common.protocol.instruct.request import ChatCompletionRequest
from mistral_common.protocol.instruct.tool_calls import (
    Function,
    Tool,
)
from mistral_common.tokens.tokenizers.mistral import MistralTokenizer

# 加载 Mistral 分词器

model_name = "mistral-large-latest"

tokenizer = MistralTokenizer.from_model(model_name)

# 对一组消息进行分词
tokenized = tokenizer.encode_chat_completion(
    ChatCompletionRequest(
        tools=[
            Tool(
                function=Function(
                    name="get_current_weather",
                    description="Get the current weather",
                    parameters={
                        "type": "object",
                        "properties": {
                            "location": {
                                "type": "string",
                                "description": "The city and state, e.g. San Francisco, CA",
                            },
                            "format": {
                                "type": "string",
                                "enum": ["celsius", "fahrenheit"],
                                "description": "The temperature unit to use. Infer this from the user's location.",
                            },
                        },
                        "required": ["location", "format"],
                    },
                )
            )
        ],
        messages=[
            UserMessage(content="What's the weather like today in Paris"),
        ],
        model=model_name,
    )
)
tokens, text = tokenized.tokens, tokenized.text

# 统计 token 数量
print(len(tokens))
```

## 学习不止于此，继续你的旅程

完成本课后，请查看我们的 [生成式 AI 学习合集](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)，继续提升你的生成式 AI 知识！
