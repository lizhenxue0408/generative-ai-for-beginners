# 构建搜索应用

[![生成式 AI 与大语言模型简介](./images/08-lesson-banner.png?WT.mc_id=academic-105485-koreyst)](https://youtu.be/W0-nzXjOjr0?si=GcsqiTTvd7RKbo7V)

> > _点击上方图片观看本课视频_

LLM 的用途不止于聊天机器人和文本生成。还可以使用嵌入（Embeddings）来构建搜索应用。嵌入是数据的数值表示，也称为向量（vectors），可用于数据的语义搜索。

在本课中，你将为我们这家教育初创公司构建一个搜索应用。我们的初创公司是一家非营利组织，为发展中国家的学生提供免费教育。我们的初创公司拥有大量 YouTube 视频，学生可以用它们来学习 AI。我们的初创公司想要构建一个搜索应用，允许学生通过输入一个问题来搜索 YouTube 视频。

例如，一个学生可能会输入“什么是 Jupyter Notebooks？”或“什么是 Azure ML”，搜索应用将返回与问题相关的 YouTube 视频列表，更好的是，搜索应用将返回一个指向视频中答案所在位置的链接。

## 简介

在本课中，我们将涵盖：

- 语义搜索与关键词搜索。
- 什么是文本嵌入（Text Embeddings）。
- 创建文本嵌入索引。
- 搜索文本嵌入索引。

## 学习目标

完成本课后，你将能够：

- 区分语义搜索和关键词搜索。
- 解释什么是文本嵌入。
- 使用嵌入构建一个搜索数据的应用。

## 为什么要构建搜索应用？

构建搜索应用将帮助你理解如何使用嵌入来搜索数据。你还将学习如何构建一个搜索应用，供学生用来快速查找信息。

本课包含一个 Microsoft [AI Show](https://www.youtube.com/playlist?list=PLlrxD0HtieHi0mwteKBOfEeOYf0LJU4O1) YouTube 频道 YouTube 成绩单的嵌入索引。AI Show 是一个教你关于 AI 和机器学习的 YouTube 频道。该嵌入索引包含截至 2023 年 10 月每个 YouTube 成绩单的嵌入。你将使用该嵌入索引为我们初创公司构建一个搜索应用。该搜索应用返回一个指向视频中答案所在位置的链接。这是学生快速找到所需信息的好方法。

以下是问题“你能将 rstudio 与 azure ml 一起使用吗？”的语义查询示例。查看 YouTube url，你会看到该 url 包含一个时间戳，可将你带到视频中答案所在的位置。

![问题“你能将 rstudio 与 Azure ML 一起使用吗？”的语义查询](./images/query-results.png?WT.mc_id=academic-105485-koreyst)

## 什么是语义搜索？

现在你可能想知道，什么是语义搜索？语义搜索是一种使用查询中词语的语义或含义来返回相关结果的搜索技术。

以下是一个语义搜索的示例。假设你正在寻找买一辆车，你可能会搜索“我的梦想之车”，语义搜索理解你不是在 `做梦` 关于一辆车，而是你在寻找购买你的 `理想` 之车。语义搜索理解你的意图并返回相关结果。另一种替代是 `关键词搜索`，它会按字面搜索关于汽车的梦，并经常返回不相关的结果。

## 什么是文本嵌入？

[文本嵌入](https://en.wikipedia.org/wiki/Word_embedding?WT.mc_id=academic-105485-koreyst) 是一种用于 [自然语言处理](https://en.wikipedia.org/wiki/Natural_language_processing?WT.mc_id=academic-105485-koreyst) 的文本表示技术。文本嵌入是文本的语义数值表示。嵌入用于表示数据，以机器易于理解的方式。有许多构建文本嵌入的模型，在本课中，我们将专注于使用 OpenAI 嵌入模型生成嵌入。

这是一个示例，想象以下文本来自 AI Show YouTube 频道某集的成绩单：

```text
Today we are going to learn about Azure Machine Learning.
```

我们会将文本传递给 OpenAI 嵌入 API，它将返回由 1536 个数字组成的以下嵌入，即一个向量。向量中的每个数字代表文本的不同方面。为简洁起见，这里给出向量中的前 10 个数字。

```python
[-0.006655829958617687, 0.0026128944009542465, 0.008792596869170666, -0.02446001023054123, -0.008540431968867779, 0.022071078419685364, -0.010703742504119873, 0.003311325330287218, -0.011632772162556648, -0.02187200076878071, ...]
```

## 嵌入索引是如何创建的？

本课的嵌入索引是通过一系列 Python 脚本创建的。你会在本课的 `scripts` 文件夹中的 [README](./scripts/README.md?WT.mc_id=academic-105485-koreyst) 里找到脚本以及说明。你不需要运行这些脚本就能完成本课，因为嵌入索引已经提供给你了。

这些脚本执行以下操作：

1. 下载 [AI Show](https://www.youtube.com/playlist?list=PLlrxD0HtieHi0mwteKBOfEeOYf0LJU4O1) 播放列表中每个 YouTube 视频的成绩单。
2. 使用 [OpenAI Functions](https://learn.microsoft.com/azure/ai-foundry/openai/how-to/function-calling?WT.mc_id=academic-105485-koreyst)，尝试从 YouTube 成绩单的前 3 分钟提取演讲者姓名。每个视频的演讲者姓名存储在名为 `embedding_index_3m.json` 的嵌入索引中。
3. 然后将成绩单文本分块为 **3 分钟文本片段**。该片段包含来自下一个片段约 20 个重叠的单词，以确保片段的嵌入不被切断，并提供更好的搜索上下文。
4. 然后每个文本片段被传递给 OpenAI Chat API 以将文本总结为 60 个词。该摘要也存储在嵌入索引 `embedding_index_3m.json` 中。
5. 最后，片段文本被传递给 OpenAI 嵌入 API。嵌入 API 返回一个代表片段语义含义的 1536 个数字的向量。该片段连同 OpenAI 嵌入向量一起存储在名为 `embedding_index_3m.json` 的嵌入索引中。

### 向量数据库

为了课程的简单性，嵌入索引存储在一个名为 `embedding_index_3m.json` 的 JSON 文件中，并加载到 Pandas DataFrame 中。然而，在生产环境中，嵌入索引会存储在向量数据库中，例如 [Azure Cognitive Search](https://learn.microsoft.com/training/modules/improve-search-results-vector-search?WT.mc_id=academic-105485-koreyst)、[Redis](https://cookbook.openai.com/examples/vector_databases/redis/readme?WT.mc_id=academic-105485-koreyst)、[Pinecone](https://cookbook.openai.com/examples/vector_databases/pinecone/readme?WT.mc_id=academic-105485-koreyst)、[Weaviate](https://cookbook.openai.com/examples/vector_databases/weaviate/readme?WT.mc_id=academic-105485-koreyst) 等，仅举几例。

## 理解余弦相似度

我们了解了文本嵌入，下一步是学习如何使用文本嵌入来搜索数据，特别是使用余弦相似度找到与给定查询最相似的嵌入。

### 什么是余弦相似度？

余弦相似度是两个向量之间相似度的度量，你也可能听到这被称为 `最近邻搜索（nearest neighbor search）`。要执行余弦相似度搜索，你需要使用 OpenAI 嵌入 API 将 _查询_ 文本 _向量化（vectorize）_。然后计算查询向量与嵌入索引中每个向量之间的 _余弦相似度_。记住，嵌入索引为每个 YouTube 成绩单文本片段都有一个向量。最后，按余弦相似度对结果排序，余弦相似度最高的文本片段与查询最相似。

从数学角度来看，余弦相似度衡量在多维空间中投影的两个向量之间夹角的余弦。这个度量是很有益的，因为如果两个文档由于大小而在欧几里得距离上相距甚远，它们仍可能在它们之间具有更小的夹角，因此有更高的余弦相似度。关于余弦相似度方程的更多信息，请参阅 [余弦相似度](https://en.wikipedia.org/wiki/Cosine_similarity?WT.mc_id=academic-105485-koreyst)。

## 构建你的第一个搜索应用

接下来，我们将学习如何使用嵌入构建搜索应用。该搜索应用将允许学生通过输入一个问题来搜索视频。该搜索应用将返回与问题相关的视频列表。该搜索应用还将返回一个指向视频中答案所在位置的链接。

该解决方案在 Windows 11、macOS 和 Ubuntu 22.04 上使用 Python 3.10 或更高版本构建并测试。你可以从 [python.org](https://www.python.org/downloads/?WT.mc_id=academic-105485-koreyst) 下载 Python。

## 作业 - 构建一个搜索应用，赋能学生

我们在本课开始时介绍了我们的初创公司。现在是时候赋能学生构建一个搜索应用来完成他们的评估了。

在这个作业中，你将创建将用于构建搜索应用的 Azure OpenAI 服务。你将创建以下 Azure OpenAI 服务。你需要一个 Azure 订阅来完成这个作业。

### 启动 Azure Cloud Shell

1. 登录到 [Azure 门户](https://portal.azure.com/?WT.mc_id=academic-105485-koreyst)。
2. 选择 Azure 门户右上角的 Cloud Shell 图标。
3. 选择 **Bash** 作为环境类型。

#### 创建一个资源组

> 对于这些说明，我们使用位于美国东部的名为 "semantic-video-search" 的资源组。
> 你可以更改资源组的名称，但在更改资源的位置时，
> 请检查 [模型可用性表](https://aka.ms/oai/models?WT.mc_id=academic-105485-koreyst)。

```shell
az group create --name semantic-video-search --location eastus
```

#### 创建一个 Azure OpenAI 服务资源

从 Azure Cloud Shell 运行以下命令，创建一个 Azure OpenAI 服务资源。

```shell
az cognitiveservices account create --name semantic-video-openai --resource-group semantic-video-search \
    --location eastus --kind OpenAI --sku s0
```

#### 获取用于本应用的终结点和密钥

从 Azure Cloud Shell 运行以下命令，获取 Azure OpenAI 服务资源的终结点和密钥。

```shell
az cognitiveservices account show --name semantic-video-openai \
   --resource-group  semantic-video-search | jq -r .properties.endpoint
az cognitiveservices account keys list --name semantic-video-openai \
   --resource-group semantic-video-search | jq -r .key1
```

#### 部署 OpenAI 嵌入模型

从 Azure Cloud Shell 运行以下命令，部署 OpenAI 嵌入模型。

```shell
az cognitiveservices account deployment create \
    --name semantic-video-openai \
    --resource-group  semantic-video-search \
    --deployment-name text-embedding-ada-002 \
    --model-name text-embedding-ada-002 \
    --model-version "2"  \
    --model-format OpenAI \
    --sku-capacity 100 --sku-name "Standard"
```

## 解决方案

在 GitHub Codespaces 中打开 [解决方案 notebook](./python/aoai-solution.ipynb?WT.mc_id=academic-105485-koreyst)，并按照 Jupyter Notebook 中的说明进行操作。

当你运行 notebook 时，会提示你输入一个查询。输入框将看起来像这样：

![供用户输入查询的输入框](./images/notebook-search.png?WT.mc_id=academic-105485-koreyst)

## 干得漂亮！继续你的学习

完成本课后，请查看我们的 [生成式 AI 学习合集](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)，继续提升你的生成式 AI 知识！

前往第 9 课，我们将了解如何 [构建图像生成应用](../09-building-image-applications/README.md?WT.mc_id=academic-105485-koreyst)！
