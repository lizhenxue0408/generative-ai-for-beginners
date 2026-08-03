# 检索增强生成（RAG）与向量数据库

[![检索增强生成（RAG）与向量数据库](./images/15-lesson-banner.png?WT.mc_id=academic-105485-koreyst)](https://youtu.be/4l8zhHUBeyI?si=BmvDmL1fnHtgQYkL)

在搜索应用课程中，我们简要了解了如何将你自己的数据集成到大型语言模型（LLMs）中。在本课中，我们将更深入地探讨将你的数据“接地”（grounding）到 LLM 应用中的概念、该过程的机制，以及存储数据的方法，包括嵌入（embeddings）和文本。

> **视频即将上线**

## 简介

在本课中，我们将涵盖以下内容：

- RAG 入门：它是什么，以及为什么在 AI（人工智能）中使用它。
- 理解什么是向量数据库，并为我们的应用创建一个。
- 如何将 RAG 集成到应用中的一个实践示例。

## 学习目标

完成本课后，你将能够：

- 解释 RAG 在 data retrieval（数据检索）与处理中的重要性。
- 搭建 RAG 应用并将你的数据接地到 LLM。
- 在 LLM 应用中有效集成 RAG 与向量数据库。

## 我们的场景：用我们自己的数据增强 LLM

在本课中，我们希望将我们自己的笔记添加到教育初创公司中，让聊天机器人能够获取关于不同学科的更多信息。利用这些笔记，学习者将能够更好地学习并理解不同主题，从而更容易为考试复习。为了构建我们的场景，我们将使用：

- `Azure OpenAI：` 我们将用于创建聊天机器人的 LLM
- `面向初学者的神经网络课程`：这将是我们的 LLM 所接地（grounding）的数据
- `Azure AI Search` 与 `Azure Cosmos DB：` 用于存储我们的数据并创建搜索索引的向量数据库

用户将能够基于他们的笔记创建练习测验、复习抽认卡（flashcards），并将其总结为简洁的概览。首先，让我们看看什么是 RAG 以及它是如何工作的：

## 检索增强生成（RAG）

一个由 LLM 驱动的聊天机器人会处理用户提示词以生成响应。它被设计为具有交互性，并与用户就广泛的主题进行交流。然而，它的响应受限于所提供的上下文及其基础训练数据。例如，GPT-4 的知识截止日期是 2021 年 9 月，这意味着它缺乏在此之后发生的事件的知识。此外，用于训练 LLM 的数据排除了机密信息，例如个人笔记或公司的产品手册。

### RAG（检索增强生成）是如何工作的

![展示 RAG 工作方式的示意图](images/how-rag-works.png?WT.mc_id=academic-105485-koreyst)

假设你想部署一个能基于你的笔记创建测验的聊天机器人，你将需要连接到知识库。这正是 RAG 派上用场的地方。RAG 的工作方式如下：

- **知识库（Knowledge base）**：在检索之前，这些文档需要先被摄取和预处理，通常是将大型文档拆分成更小的块（chunks），转换为文本嵌入（text embedding），并存储到数据库中。

- **用户查询（User Query）**：用户提出一个问题。

- **检索（Retrieval）**：当用户提出问题时，嵌入模型会从我们的知识库中检索相关信息，以提供将被纳入提示词的更多上下文。

- **增强生成（Augmented Generation）**：LLM 基于检索到的数据增强其响应。它使得生成的响应不仅基于预训练数据，还基于所添加上下文中的相关信息。检索到的数据被用来增强 LLM 的响应。然后 LLM 返回对用户问题的答案。

![展示 RAG 架构的示意图](images/encoder-decode.png?WT.mc_id=academic-105485-koreyst)

RAG 的架构使用由两部分组成的 transformers 来实现：一个编码器（encoder）和一个解码器（decoder）。例如，当用户提出一个问题时，输入文本被“编码”为捕捉词义的向量，这些向量被“解码”到我们的文档索引中，并基于用户查询生成新文本。LLM 使用一个编码器-解码器模型来生成输出。

根据提出该方法的论文：[Retrieval-Augmented Generation for Knowledge intensive NLP（自然语言处理）Tasks](https://arxiv.org/pdf/2005.11401.pdf?WT.mc_id=academic-105485-koreyst)，实现 RAG 的两种方法是：

- **_RAG-Sequence_（序列式 RAG）**：使用检索到的文档来预测用户查询的最佳可能答案。
- **RAG-Token**：使用文档来生成下一个 token，然后检索它们以回答用户的查询。

### 为什么要使用 RAG？

- **信息丰富性（Information richness）**：确保文本响应是最新的、当前的。因此，它通过访问内部知识库，提升了在领域特定任务上的表现。
- 通过利用知识库中的**可验证数据**来为用户查询提供上下文，从而减少编造（fabrication）。
- 它**具有成本效益**，因为与微调一个 LLM 相比，它们更经济。

## 创建知识库

我们的应用基于我们自己的数据，即“面向初学者的 AI”课程中的神经网络章节。

### 向量数据库

向量数据库不同于传统数据库，它是一种专门设计用于存储、管理和搜索嵌入向量的数据库。它存储文档的数值表示。将数据分解为数值嵌入，使我们的 AI 系统能够更轻松地理解和处理数据。

我们将嵌入存储在向量数据库中，因为 LLM 对它们接受的输入 token 数量有限制。由于无法将整个嵌入一次性传给 LLM，我们需要将其拆分为块（chunks），当用户提出问题时，与问题最相似的嵌入会连同提示词一起返回。分块（chunking）也降低了通过 LLM 传输的 token 数量成本。

一些流行的向量数据库包括 Azure Cosmos DB、Clarifyai、Pinecone、Chromadb、ScaNN、Qdrant 和 DeepLake。你可以使用以下命令通过 Azure CLI 创建 Azure Cosmos DB 模型：

```bash
az login
az group create -n <resource-group-name> -l <location>
az cosmosdb create -n <cosmos-db-name> -r <resource-group-name>
az cosmosdb list-keys -n <cosmos-db-name> -g <resource-group-name>
```

### 从文本到嵌入

在存储数据之前，我们需要先将其转换为向量嵌入，然后才能存入数据库。如果你正在处理大型文档或长文本，可以根据你预期的查询进行分块。分块可以在句子级别或段落级别进行。由于分块会从周围单词中派生含义，你可以为某个块添加一些额外的上下文，例如添加文档标题，或包含该块之前或之后的部分文本。你可以按如下方式对数据进行分块：

```python
def split_text(text, max_length, min_length):
    words = text.split()
    chunks = []
    current_chunk = []

    for word in words:
        current_chunk.append(word)
        if len(' '.join(current_chunk)) < max_length and len(' '.join(current_chunk)) > min_length:
            chunks.append(' '.join(current_chunk))
            current_chunk = []

    # If the last chunk didn't reach the minimum length, add it anyway
    if current_chunk:
        chunks.append(' '.join(current_chunk))

    return chunks
```

一旦分块完成，我们就可以使用不同的嵌入模型来嵌入文本。你可以使用的一些模型包括：word2vec、OpenAI 的 ada-002、Azure Computer Vision 等等。选择使用哪个模型，取决于你使用的语言、被编码内容的类型（文本/图像/音频）、它能编码的输入大小以及嵌入输出的长度。

使用 OpenAI 的 `text-embedding-ada-002` 模型进行文本嵌入的一个例子如下：
![单词 cat 的嵌入](images/cat.png?WT.mc_id=academic-105485-koreyst)

## 检索与向量搜索

当用户提出问题时，检索器（retriever）会使用查询编码器将其转换为一个向量，然后在文档搜索索引中搜索与输入相关的文档向量。完成后，它将输入向量和文档向量都转换为文本，并传递给 LLM。

### 检索

检索发生在系统试图从索引中快速找到满足搜索条件的文档时。检索器的目标是获取将用于提供上下文、并将 LLM 接地（grounding）到你的数据上的文档。

在我们的数据库中有几种执行搜索的方式，例如：

- **关键词搜索（Keyword search）** - 用于文本搜索。
- **向量搜索（Vector search）** - 使用嵌入模型将文档从文本转换为向量表示，从而允许基于词义的**语义搜索（semantic search）**。检索将通过查询那些向量表示与用户问题最接近（closest）的文档来完成。
- **混合搜索（Hybrid）** - 关键词搜索与向量搜索的结合。

检索面临的一个挑战是，当数据库中没有与查询相似的响应时，系统会返回它能得到的最佳信息，不过你可以使用一些策略，例如设置相关性的最大距离，或使用结合了关键词和向量搜索的混合搜索。在本课中，我们将使用混合搜索，即向量搜索与关键词搜索的结合。我们将数据存入一个 dataframe，其列包含分块（chunks）以及嵌入（embeddings）。

### 向量相似度（Vector Similarity）

检索器会在知识数据库中搜索彼此接近（最接近的邻居，因为它们是相似的文本）的嵌入。在场景中，当用户提出查询时，它首先被嵌入，然后与相似的嵌入进行匹配。用于衡量不同向量相似度常用的度量是余弦相似度（cosine similarity），它基于两个向量之间的夹角。

我们还可以使用其他替代方式来衡量相似度，例如欧几里得距离（Euclidean distance，即向量端点之间的直线距离），以及点积（dot product，衡量两个向量对应元素乘积之和）。

### 搜索索引

在进行检索时，我们需要在搜索之前为知识库构建一个搜索索引。索引将存储我们的嵌入，并能快速检索最相似的块，即使在大型数据库中也是如此。我们可以使用以下方式在本地创建索引：

```python
from sklearn.neighbors import NearestNeighbors

embeddings = flattened_df['embeddings'].to_list()

# Create the search index
nbrs = NearestNeighbors(n_neighbors=5, algorithm='ball_tree').fit(embeddings)

# To query the index, you can use the kneighbors method
distances, indices = nbrs.kneighbors(embeddings)
```

### 重排序（Re-ranking）

查询数据库后，你可能需要将结果按最相关的顺序排序。重排序 LLM 利用机器学习，通过从最相关到最不相关对搜索结果排序，来提升其相关性。使用 Azure AI Search 时，重排序会由语义重排序器（semantic reranker）自动为你完成。以下是使用最近邻进行重排序的一个示例：

```python
# Find the most similar documents
distances, indices = nbrs.kneighbors([query_vector])

index = []
# Print the most similar documents
for i in range(3):
    index = indices[0][i]
    for index in indices[0]:
        print(flattened_df['chunks'].iloc[index])
        print(flattened_df['path'].iloc[index])
        print(flattened_df['distances'].iloc[index])
    else:
        print(f"Index {index} not found in DataFrame")
```

## 将所有内容整合在一起

最后一步是将我们的 LLM 加入进来，以便能够获得基于我们的数据接地（grounded）的响应。我们可以按如下方式实现它：

```python
user_input = "what is a perceptron?"

def chatbot(user_input):
    # Convert the question to a query vector
    query_vector = create_embeddings(user_input)

    # Find the most similar documents
    distances, indices = nbrs.kneighbors([query_vector])

    # add documents to query  to provide context
    history = []
    for index in indices[0]:
        history.append(flattened_df['chunks'].iloc[index])

    # combine the history and the user input
    history.append(user_input)

    # create a message object
    messages=[
        {"role": "system", "content": "You are an AI assistant that helps with AI questions."},
        {"role": "user", "content": "\n\n".join(history) }
    ]

    # use the Responses API to generate a response
    response = client.responses.create(
        model="gpt-5-mini",
        max_output_tokens=800,
        input=messages,
        store=False,
    )

    return response.output_text

chatbot(user_input)
```

## 评估我们的应用

### 评估指标

- 所提供响应的质量：确保听起来自然、流畅且拟人化。
- 数据的忠实度（Groundedness）：评估响应是否来自所提供的文档。
- 相关性（Relevance）：评估响应是否与所提问题匹配且相关。
- 流畅度（Fluency）：响应在语法上是否合理。

## RAG（检索增强生成）与向量数据库的使用场景

函数调用可以在许多场景中改进你的应用，例如：

- 问答（Question and Answering）：将你的公司数据接地到一个可供员工提问的聊天系统中。
- 推荐系统（Recommendation Systems）：你可以创建一个匹配最相似值的系统，例如电影、餐厅等等。
- 聊天机器人服务（Chatbot services）：你可以存储聊天历史，并基于用户数据进行个性化对话。
- 基于向量嵌入的图像搜索，在图像识别和异常检测中很有用。

## 总结

我们已经涵盖了 RAG 的基础领域，从将数据添加到应用、用户查询到输出。为了简化 RAG 的创建，你可以使用诸如 Semantic Kernel、Langchain 或 Autogen 之类的框架。

## 作业

为了继续学习检索增强生成（RAG），你可以构建：

- 使用你选择的框架为应用构建一个前端。
- 利用一个框架（LangChain 或 Semantic Kernel）重建你的应用。

恭喜你完成了本课 👏。

## 学习不止于此，继续你的旅程

完成本课后，请查看我们的 [生成式 AI 学习合集](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)，继续提升你的生成式 AI 知识！
