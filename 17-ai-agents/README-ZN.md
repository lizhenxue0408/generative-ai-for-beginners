[![开源模型](./images/17-lesson-banner.png?WT.mc_id=academic-105485-koreyst)](https://youtu.be/yAXVW-lUINc?si=bOtW9nL6jc3XJgOM)

## 简介

AI 智能体（AI Agents）是生成式 AI 中一个令人激动的发展方向，它使大语言模型（LLMs）能够从“助手”演进为能够采取行动的“智能体”。AI 智能体框架让开发者能够创建赋予 LLM 访问工具与状态管理能力（state management）的应用。这些框架还增强了可观测性，让用户和开发者能够监控 LLM 计划采取的动作，从而改善体验管理。

本课将涵盖以下领域：

- 理解什么是 AI 智能体 —— 它究竟是什么？
- 探索五种不同的 AI 智能体框架 —— 它们各自独特之处在哪里？
- 将这些 AI 智能体应用到不同的用例中 —— 我们何时应该使用 AI 智能体？

## 学习目标

完成本课后，你将能够：

- 解释什么是 AI 智能体，以及它们如何被使用。
- 理解一些流行的 AI 智能体框架之间的差异，以及它们的不同之处。
- 理解 AI 智能体如何运作，从而用它们构建应用。

## 什么是 AI 智能体？

AI 智能体是生成式 AI 世界中一个非常令人激动的领域。伴随着这种兴奋，有时也会出现术语及其应用的混淆。为了让事情保持简单，并涵盖大多数提及 AI 智能体的工具，我们将采用如下定义：

AI 智能体通过赋予大语言模型（LLMs）访问 **状态（state）** 和 **工具（tools）** 的能力，使其执行任务。

![智能体模型](images/what-agent.png?WT.mc_id=academic-105485-koreyst)

我们来定义这些术语：

**大语言模型（Large Language Models）** —— 这些是本课程中始终提到的模型，例如 GPT-5、GPT-4o 和 Llama 3.3 等。

**状态（State）** —— 指 LLM 所处的上下文。LLM 利用其过去动作和当前上下文来指导其后续动作的决策。AI 智能体框架让开发者更容易维护这一上下文。

**工具（Tools）** —— 为了完成用户所请求、LLM 所规划的任务，LLM 需要访问工具。工具的一些例子可以是数据库、API、外部应用，甚至是另一个 LLM！

这些定义希望能为你打下一个良好的基础，接下来我们看看它们是如何实现的。我们来探索几种不同的 AI 智能体框架：

## LangChain 智能体

[LangChain Agents](https://python.langchain.com/docs/how_to/#agents?WT.mc_id=academic-105485-koreyst) 是对我们上述定义的一种实现。

为了管理 **状态（state）**，它使用一个名为 `AgentExecutor` 的内置函数。该函数接受已定义的 `agent` 以及可供其使用的 `tools`。

`Agent Executor` 还会存储聊天历史，以提供聊天的上下文。

![Langchain 智能体](images/langchain-agents.png?WT.mc_id=academic-105485-koreyst)

LangChain 提供了一个 [工具目录](https://integrations.langchain.com/tools?WT.mc_id=academic-105485-koreyst)，可以导入你的应用中，让 LLM 能够访问这些工具。它们由社区和 LangChain 团队共同打造。

然后你可以定义这些工具，并将它们传递给 `Agent Executor`。

可观测性（visibility）是谈论 AI 智能体时的另一个重要方面。应用开发者理解 LLM 正在使用哪个工具、以及为何使用，这一点很重要。为此，LangChain 团队开发了 LangSmith。

## AutoGen

我们要讨论的下一个 AI 智能体框架是 [AutoGen](https://microsoft.github.io/autogen/?WT.mc_id=academic-105485-koreyst)。AutoGen 的主要焦点在于对话（conversations）。智能体既是 **可对话的（conversable）**，又是 **可定制的（customizable）**。

**可对话的** —— LLM 可以启动并持续与另一个 LLM 对话，以完成任务。这是通过创建 `AssistantAgents` 并赋予它们特定的系统消息来实现的。

```python

autogen.AssistantAgent( name="Coder", llm_config=llm_config, ) pm = autogen.AssistantAgent( name="Product_manager", system_message="Creative in software product ideas.", llm_config=llm_config, )

```

**可定制的** —— 智能体不仅可以定义为 LLM，还可以是用户或工具。作为开发者，你可以定义一个 `UserProxyAgent`，它负责与用户交互以获取反馈来完成任务。该反馈可以继续任务的执行，也可以停止执行。

```python
user_proxy = UserProxyAgent(name="user_proxy")
```

### 状态与工具

为了改变和管理状态，一个助手智能体会生成 Python 代码来完成任务。

下面是该过程的示例：

![AutoGen](images/autogen.png?WT.mc_id=academic-105485-koreyst)

#### 用系统消息定义 LLM

```python
system_message="For weather related tasks, only use the functions you have been provided with. Reply TERMINATE when the task is done."
```

这个系统消息指示这个特定的 LLM 哪些函数与其任务相关。请记住，在 AutoGen 中，你可以拥有多个具有不同系统消息的已定义 AssistantAgents。

#### 由用户发起聊天

```python
user_proxy.initiate_chat( chatbot, message="I am planning a trip to NYC next week, can you help me pick out what to wear? ", )

```

来自 user_proxy（人类）的这条消息，将启动智能体去探索它应该执行的可用函数的流程。

#### 函数被执行

```bash
chatbot (to user_proxy):

***** Suggested tool Call: get_weather ***** Arguments: {"location":"New York City, NY","time_periond:"7","temperature_unit":"Celsius"} ******************************************************** --------------------------------------------------------------------------------

>>>>>>>> EXECUTING FUNCTION get_weather... user_proxy (to chatbot): ***** Response from calling function "get_weather" ***** 112.22727272727272 EUR ****************************************************************

```

一旦初始聊天被处理，智能体就会发送建议调用的函数。在此例中，它是一个名为 `get_weather` 的函数。根据你的配置，这个函数可以自动执行并被智能体读取，也可以基于用户输入来执行。

你可以在 [AutoGen 代码示例](https://microsoft.github.io/autogen/docs/Examples/?WT.mc_id=academic-105485-koreyst) 中找到一份列表，进一步探索如何开始构建。

## Microsoft Agent Framework

[Microsoft Agent Framework](https://learn.microsoft.com/agent-framework/?WT.mc_id=academic-105485-koreyst) 是微软的开源 SDK，用于在 **Python** 和 **.NET** 中构建 AI 智能体及多智能体系统。它将两个早期微软项目的优势 —— **Semantic Kernel** 的企业级特性与 **AutoGen** 的多智能体编排能力 —— 整合到一个受支持的框架中。如果你今天要启动一个新的智能体项目，这是我们推荐作为 AutoGen 继任者的选择。

该框架的规模从一个单一的 **聊天智能体（chat agent）** 一直到复杂的 **多智能体工作流（multi-agent workflows）**，并且它与 Microsoft Foundry、Azure OpenAI 和 OpenAI 直接集成。它还通过 OpenTelemetry 提供内置的可观测性，让你能够准确追踪智能体正在做什么。

### 状态与工具

**状态（State）** —— 框架通过 **threads（线程）** 为你管理对话上下文。一个智能体会跟踪消息历史（用户请求、工具调用及其结果），因此每一轮对话都建立在前一轮的基础上。线程还可以被持久化，从而允许对话被暂停并在之后恢复。

**工具（Tools）** —— 你通过传递纯 Python 函数来赋予智能体工具。带有类型注解的参数会自动转换为 schema，因此模型知道如何以及何时调用它们（函数调用）。该框架还支持模型上下文协议（Model Context Protocol，MCP）服务器以及托管工具，例如代码解释器。

下面是一个带有自定义工具的单一智能体示例：

```python
import asyncio
from typing import Annotated

from pydantic import Field
from agent_framework import Agent
from agent_framework.openai import OpenAIChatClient


def get_weather(
    location: Annotated[str, Field(description="The location to get the weather for.")],
) -> str:
    """Get the weather for a given location."""
    return f"The weather in {location} is sunny with a high of 22°C."


async def main():
    agent = Agent(
        client=OpenAIChatClient(),
        instructions="You are a helpful assistant that can answer weather questions.",
        tools=[get_weather],
    )

    response = await agent.run("What's the weather in Amsterdam?")
    print(response)


asyncio.run(main())
```

要改为连接 Microsoft Foundry 中的 Azure OpenAI，只需将你的终结点与凭据传递给该客户端：

```python
from azure.identity.aio import AzureCliCredential
from agent_framework.openai import OpenAIChatClient

client = OpenAIChatClient(
    model="my-gpt-5-mini-deployment",
    azure_endpoint="https://my-resource.openai.azure.com",
    credential=AzureCliCredential(),
)
```

### 多智能体工作流

该框架真正出彩的地方在于编排多个智能体协同工作。例如，你可以让多个智能体依次运行（每个将上下文传递给下一个），或者以并行方式扇出（fan out）到多个智能体并汇总它们的结果：

```python
from agent_framework.orchestrations import SequentialBuilder, ConcurrentBuilder

# 依次运行智能体，将对话上下文沿链传递
sequential = SequentialBuilder(participants=[researcher, writer, editor]).build()

# 并行扇出到多个智能体，然后汇总它们的响应
concurrent = ConcurrentBuilder(participants=[analyst_a, analyst_b, analyst_c]).build()
```

要安装该框架并开始使用：

```bash
pip install agent-framework-core
# 可选集成
pip install agent-framework-openai       # OpenAI 与 Azure OpenAI
pip install agent-framework-foundry      # Microsoft Foundry
```

你可以在 [Microsoft Agent Framework 代码仓库](https://github.com/microsoft/agent-framework?WT.mc_id=academic-105485-koreyst) 以及 [官方文档](https://learn.microsoft.com/agent-framework/?WT.mc_id=academic-105485-koreyst) 中探索更多内容。

## Taskweaver

我们要探索的下一个智能体框架是 [Taskweaver](https://microsoft.github.io/TaskWeaver/?WT.mc_id=academic-105485-koreyst)。它被称为“代码优先（code-first）”智能体，因为它不是严格地处理 `strings`（字符串），而是可以处理 Python 中的 DataFrame。这对于数据分析和生成任务极为有用，例如创建图表，或生成随机数。

### 状态与工具

为了管理对话的状态，TaskWeaver 使用 `Planner`（规划器）的概念。`Planner` 是一个 LLM，它接收用户的请求，并规划出要完成该请求所需执行的任务。

为了完成这些任务，`Planner` 会接触到称为 `Plugins`（插件）的工具集合。这些可以是 Python 类，或是通用的代码解释器。这些插件以嵌入（embeddings）的形式存储，以便 LLM 更好地检索到正确的插件。

![Taskweaver](images/taskweaver.png?WT.mc_id=academic-105485-koreyst)

下面是一个用于处理异常检测的插件示例：

```python
class AnomalyDetectionPlugin(Plugin): def __call__(self, df: pd.DataFrame, time_col_name: str, value_col_name: str):
```

代码在执行前会被验证。Taskweaver 中用于管理上下文的另一个特性是 `experience`（经验）。经验允许将对话的上下文长期存储在一个 YAML 文件中。这可以配置为：鉴于 LLM 接触到了先前的对话，它能够在某些任务上随时间不断改进。

## JARVIS

我们要探索的最后一个智能体框架是 [JARVIS](https://github.com/microsoft/JARVIS?tab=readme-ov-file&WT.mc_id=academic-105485-koreyst)。JARVIS 的独特之处在于它使用一个 LLM 来管理对话的 `state`（状态），而 `tools`（工具）则是其他的 AI 模型。每个 AI 模型都是专门化的模型，执行特定任务，例如目标检测、语音转录或图像描述（image captioning）。

![JARVIS](images/jarvis.png?WT.mc_id=academic-105485-koreyst)

LLM 作为通用模型，接收来自用户的请求，并识别出具体的任务以及完成该任务所需的任何参数/数据。

```python
[{"task": "object-detection", "id": 0, "dep": [-1], "args": {"image": "e1.jpg" }}]
```

然后 LLM 将请求格式化为专门化 AI 模型能够解释的形式，例如 JSON。一旦 AI 模型基于任务返回了其预测，LLM 就会接收该响应。

如果需要多个模型来完成任务，它还会在将这些模型的响应汇总起来生成对用户的回复之前，先对它们进行解读。

下面的示例展示了当用户请求对图片中的物体进行描述与计数时，这将如何工作：

## 作业

为了继续你的 AI 智能体学习，你可以用 Microsoft Agent Framework 构建：

- 一个模拟教育初创公司不同部门举行业务会议的 application。
- 创建系统消息来引导 LLM 理解不同的角色画像与优先事项，并让用户能够推介一个新的产品创意。
- LLM 随后应当从每个部门生成后续追问，以完善和改进该推介以及产品创意。

## 学习不止于此，继续你的旅程

完成本课后，请查看我们的 [生成式 AI 学习合集](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)，继续提升你的生成式 AI 知识！
