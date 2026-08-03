# 与函数调用集成

[![与函数调用集成](./images/11-lesson-banner.png?WT.mc_id=academic-105485-koreyst)](https://youtu.be/DgUdCLX8qYQ?si=f1ouQU5HQx6F8Gl2)

在前面的课程中你已经学到了不少内容。不过，我们还能做得更好。有一些问题可以解决，例如：如何让模型的响应格式更加一致，以便下游更轻松地处理返回结果；以及，是否可以从其他来源引入数据来进一步丰富我们的应用。

本章要解决的正是上述问题。

## 简介

本课将涵盖：

- 解释什么是函数调用及其使用场景。
- 使用 Azure OpenAI 创建函数调用。
- 如何将函数调用集成到应用程序中。

## 学习目标

完成本课后，你将能够：

- 解释使用函数调用的目的。
- 使用 Azure OpenAI 服务配置函数调用。
- 针对你的应用场景设计有效的函数调用。

## 场景：用函数改进我们的聊天机器人

在本课中，我们要为虚构的教育初创公司构建一个功能，让用户可以通过聊天机器人查找技术课程。我们将根据用户的技术水平、当前角色和感兴趣的技术，来推荐适合他们的课程。

要完成这个场景，我们将结合使用：

- `Azure OpenAI`：为用户创建聊天体验。
- `Microsoft Learn 目录 API`：根据用户请求帮助查找课程。
- `Function Calling`（函数调用）：将用户的查询交给函数，从而发起 API 请求。

我们先来看看，为什么首先要使用函数调用：

## 为什么要使用函数调用

在引入函数调用之前，大语言模型（LLM）的响应是非结构化且不一致的。开发者必须编写复杂的校验代码，才能处理响应可能出现的各种变体。用户也无法得到诸如“斯德哥尔摩现在的天气如何？”这类问题的答案。这是因为模型的能力受限于其训练数据所覆盖的时间范围。

函数调用是 Azure OpenAI 服务提供的一项功能，用于克服以下局限：

- **一致的响应格式**。如果我们能更好地控制响应格式，就能更轻松地将响应集成到下游的其他系统中。
- **外部数据**。能够在聊天上下文中使用来自应用其他来源的数据。

## 通过一个场景说明问题

> 如果你想运行下面的场景，我们建议你使用[附带的 notebook](./python/aoai-assignment.ipynb?WT.mc_id=academic-105485-koreyst)。当然，你也可以直接阅读，因为我们正试图说明一个函数可以帮助解决的问题。

让我们看一个体现响应格式问题的示例：

假设我们要创建一个学生数据库，以便为他们推荐合适的课程。下面是两个学生描述，它们包含的数据非常相似。

1. 建立与 Azure OpenAI 资源的连接：

   ```python
   import os
   import json
   from openai import OpenAI
   from dotenv import load_dotenv
   load_dotenv()

   # The Responses API is served from the Azure OpenAI (Microsoft Foundry) v1
   # endpoint, so we point the OpenAI client at <your-endpoint>/openai/v1/.
   endpoint = os.environ['AZURE_OPENAI_ENDPOINT']
   client = OpenAI(
   api_key=os.environ['AZURE_OPENAI_API_KEY'],
   base_url=f"{endpoint.rstrip('/')}/openai/v1/",
   )

   deployment=os.environ['AZURE_OPENAI_DEPLOYMENT']
   ```

   下面是一些用于配置 Azure OpenAI 连接的 Python 代码。由于我们使用的是 v1 终结点，只需设置 `api_key` 和 `base_url`（无需 `api_version`）。

1. 使用变量 `student_1_description` 和 `student_2_description` 创建两个学生描述。

   ```python
   student_1_description="Emily Johnson is a sophomore majoring in computer science at Duke University. She has a 3.7 GPA. Emily is an active member of the university's Chess Club and Debate Team. She hopes to pursue a career in software engineering after graduating."

   student_2_description = "Michael Lee is a sophomore majoring in computer science at Stanford University. He has a 3.8 GPA. Michael is known for his programming skills and is an active member of the university's Robotics Club. He hopes to pursue a career in artificial intelligence after finishing his studies."
   ```

   我们希望将上述学生描述发送给 LLM 进行解析。这些数据之后可用于我们的应用程序，发送给 API 或存储到数据库中。

1. 创建两条相同的提示词，在其中告知 LLM 我们感兴趣的信息：

   ```python
   prompt1 = f'''
   Please extract the following information from the given text and return it as a JSON object:

   name
   major
   school
   grades
   club

   This is the body of text to extract the information from:
   {student_1_description}
   '''

   prompt2 = f'''
   Please extract the following information from the given text and return it as a JSON object:

   name
   major
   school
   grades
   club

   This is the body of text to extract the information from:
   {student_2_description}
   '''
   ```

   上述提示词指示 LLM 提取信息，并以 JSON 格式返回响应。

1. 配置好提示词并连接到 Azure OpenAI 之后，我们现在通过 `client.responses.create` 将提示词发送给 LLM。我们将提示词存放在 `input` 变量中，并将角色赋值为 `user`。这是为了模拟用户向聊天机器人发送消息。

   ```python
   # response from prompt one
   openai_response1 = client.responses.create(
   model=deployment,
   input = [{'role': 'user', 'content': prompt1}],
   store=False,
   )
   openai_response1.output_text

   # response from prompt two
   openai_response2 = client.responses.create(
   model=deployment,
   input = [{'role': 'user', 'content': prompt2}],
   store=False,
   )
   openai_response2.output_text
   ```

现在我们可以将这两个请求都发送给 LLM，并通过类似 `openai_response1.output_text` 的方式查看收到的响应。

1. 最后，我们可以通过调用 `json.loads` 将响应转换为 JSON 格式：

   ```python
   # Loading the response as a JSON object
   json_response1 = json.loads(openai_response1.output_text)
   json_response1
   ```

   响应 1：

   ```json
   {
     "name": "Emily Johnson",
     "major": "computer science",
     "school": "Duke University",
     "grades": "3.7",
     "club": "Chess Club"
   }
   ```

   响应 2：

   ```json
   {
     "name": "Michael Lee",
     "major": "computer science",
     "school": "Stanford University",
     "grades": "3.8 GPA",
     "club": "Robotics Club"
   }
   ```

   尽管提示词相同、描述也相似，但我们看到 `Grades`（成绩）属性的值格式却不一致，比如有时会得到 `3.7`，有时则是 `3.7 GPA`。

   出现这个结果，是因为 LLM 接受的是以文字提示词形式存在的非结构化数据，返回的同样是非结构化数据。我们需要一种结构化的格式，这样在存储或使用这些数据时才能明确预期。

那么，如何解决这个问题呢？通过使用函数调用，我们可以确保收到结构化的数据。使用函数调用时，LLM 实际上并不会去调用或运行任何函数。相反，我们为 LLM 创建一种它应遵循的响应结构。然后，我们利用这些结构化响应来判断应用中应当运行哪个函数。

![函数调用流程](./images/Function-Flow.png?WT.mc_id=academic-105485-koreyst)

接着，我们可以把函数返回的结果再发回给 LLM。LLM 随后会用自然语言来回答用户的查询。

## 函数调用的使用场景

函数调用可以在许多场景中改进你的应用，例如：

- **调用外部工具**。聊天机器人在回答用户问题方面表现出色。借助函数调用，聊天机器人可以利用用户的消息来完成某些任务。例如，学生可以让聊天机器人“给我的导师发一封邮件，说我需要这门课更多的帮助”。这就会触发对 `send_email(to: string, body: string)` 的函数调用。

- **创建 API 或数据库查询**。用户可以用自然语言查找信息，这些信息会被转换为格式化的查询或 API 请求。例如，教师问“完成上次作业的学生有哪些？”，这会调用一个名为 `get_completed(student_name: string, assignment: int, current_status: string)` 的函数。

- **创建结构化数据**。用户可以提供一段文本或 CSV，利用 LLM 从中提取重要信息。例如，学生可以将维基百科上关于和平协议的文章转换为 AI 记忆 flashcards（抽认卡）。这可以通过一个名为 `get_important_facts(agreement_name: string, date_signed: string, parties_involved: list)` 的函数来实现。

## 创建你的第一个函数调用

创建函数调用的过程包含 3 个主要步骤：

1. **调用** Responses API，传入你的函数（工具）列表以及一条用户消息。
2. **读取** 模型的响应以执行某个动作，即执行某个函数或 API 调用。
3. **再次发起** 对 Responses API 的调用，将函数的响应一并传入，从而利用这些信息为用户生成最终响应。

![LLM 流程](./images/LLM-Flow.png?WT.mc_id=academic-105485-koreyst)

### 步骤 1 —— 创建消息

第一步是创建一条用户消息。它可以通过读取文本输入框的值来动态赋值，也可以在这里直接赋一个值。如果你之前没有用过 Responses API，我们需要定义消息的 `role`（角色）和 `content`（内容）。

`role` 可以是 `system`（制定规则）、`assistant`（模型）或 `user`（最终用户）。对于函数调用，我们将其赋值为 `user`，并给出一个示例问题。

```python
messages= [ {"role": "user", "content": "Find me a good course for a beginner student to learn Azure."} ]
```

通过赋予不同的角色，LLM 就能清楚地知道这是系统在说话还是用户在说话，从而有助于构建一段对话历史，供 LLM 在此基础上继续生成。

### 步骤 2 —— 创建函数

接下来，我们将定义一个函数及其参数。这里我们只用一个名为 `search_courses` 的函数，但你也可以创建多个函数。

> **重要**：函数包含在发送给 LLM 的系统消息中，并会占用你可用的 token 额度。

下面我们将函数创建为一个数组。数组中的每一项都是扁平 Responses API 格式下的一个工具，包含 `type`、`name`、`description` 和 `parameters` 等属性：

```python
functions = [
   {
      "type":"function",
      "name":"search_courses",
      "description":"Retrieves courses from the search index based on the parameters provided",
      "parameters":{
         "type":"object",
         "properties":{
            "role":{
               "type":"string",
               "description":"The role of the learner (i.e. developer, data scientist, student, etc.)"
            },
            "product":{
               "type":"string",
               "description":"The product that the lesson is covering (i.e. Azure, Power BI, etc.)"
            },
            "level":{
               "type":"string",
               "description":"The level of experience the learner has prior to taking the course (i.e. beginner, intermediate, advanced)"
            }
         },
         "required":[
            "role"
         ]
      }
   }
]
```

下面我们更详细地描述每个函数项：

- `name` - 我们希望被调用的函数名称。
- `description` - 对函数工作方式的说明。这里务必做到具体而清晰。
- `parameters` - 你希望模型在其响应中生成的值及其格式列表。parameters 数组由若干项组成，每项包含以下属性：
  1.  `type` - 属性值将被存储的数据类型。
  1.  `properties` - 模型将用于其响应的具体值列表
      1. `name` - 键是模型在格式化响应中使用的属性名称，例如 `product`。
      1. `type` - 该属性的数据类型，例如 `string`。
      1. `description` - 对具体属性的描述。

还有一个可选属性 `required` - 完成该函数调用所必需的属性。

### 步骤 3 —— 发起函数调用

定义好函数后，我们现在需要将它加入到对 Responses API 的调用中。为此，我们在请求中添加 `tools`。在本例中为 `tools=functions`。

还可以将 `tool_choice` 设置为 `auto`。这意味着我们将让 LLM 根据用户的消息自行决定调用哪个函数，而不是由我们手动指定。

下面的代码中我们调用了 `client.responses.create`，注意我们设置了 `tools=functions` 和 `tool_choice="auto"`，从而把是否调用我们提供的函数这一选择权交给 LLM：

```python
response = client.responses.create(model=deployment,
                                        input=messages,
                                        tools=functions,
                                        tool_choice="auto",
                                        store=False)

print(response.output)
```

现在返回的响应中，会在 `response.output` 里包含一个 `function_call` 项，如下所示：

```json
{
  "type": "function_call",
  "name": "search_courses",
  "call_id": "call_abc123",
  "arguments": "{\n  \"role\": \"student\",\n  \"product\": \"Azure\",\n  \"level\": \"beginner\"\n}"
}
```

这里我们可以看到函数 `search_courses` 被调用了，以及它所带的具体参数（列在 JSON 响应的 `arguments` 属性中）。

LLM 之所以能为函数的参数找到合适的数据，是因为它从 Responses API 调用中 `input` 参数所提供的值里提取了这些信息。下面回顾一下 `messages` 的值：

```python
messages= [ {"role": "user", "content": "Find me a good course for a beginner student to learn Azure."} ]
```

可以看到，`student`、`Azure` 和 `beginner` 是从 `messages` 中提取出来的，并作为函数的输入。以这种方式使用函数，既能从提示词中抽取信息，又能为 LLM 提供结构，并具备可复用的功能。

接下来，我们看看如何在应用中使用它。

## 将函数调用集成到应用程序中

在测试过 LLM 的格式化响应之后，我们现在可以将其集成到应用程序中。

### 管理流程

为了将其集成到我们的应用中，我们采取以下步骤：

1. 首先，调用 OpenAI 服务，并从响应 `output` 中提取函数调用项。

   ```python
   response_items = response.output
   tool_calls = [item for item in response_items if item.type == "function_call"]
   ```

1. 现在定义一个函数，用于调用 Microsoft Learn API 获取课程列表：

   ```python
   import requests

   def search_courses(role, product, level):
     url = "https://learn.microsoft.com/api/catalog/"
     params = {
        "role": role,
        "product": product,
        "level": level
     }
     response = requests.get(url, params=params)
     modules = response.json()["modules"]
     results = []
     for module in modules[:5]:
        title = module["title"]
        url = module["url"]
        results.append({"title": title, "url": url})
     return str(results)
   ```

   注意，我们现在创建了一个真实的 Python 函数，它映射到 `functions` 变量中引入的函数名。我们还发起了真实的外部 API 调用来获取所需数据。在本例中，我们调用 Microsoft Learn API 来搜索培训模块。

好了，我们已经创建了 `functions` 变量和对应的 Python 函数，那么如何让 LLM 把这两者关联起来，从而调用我们的 Python 函数呢？

1. 要判断是否需要调用 Python 函数，我们需要检查 LLM 的响应，看其中是否包含 `function_call` 项，并调用所指出的函数。以下就是做该检查的方式：

   ```python
   # Check if the model wants to call a function
   if tool_calls:
    for tool_call in tool_calls:
     print("Recommended Function call:")
     print(tool_call.name)
     print()

     # Call the function.
     function_name = tool_call.name

     available_functions = {
             "search_courses": search_courses,
     }
     function_to_call = available_functions[function_name]

     function_args = json.loads(tool_call.arguments)
     function_response = function_to_call(**function_args)

     print("Output of function call:")
     print(function_response)
     print(type(function_response))

     # Add the function call and its result back to the conversation.
     # The model's function_call item must be appended before its output.
     messages.append(tool_call)  # the assistant's function_call item
     messages.append( # the function result
         {
             "type": "function_call_output",
             "call_id": tool_call.call_id,
             "output": function_response,
         }
     )
   ```

   下面这三行代码，确保我们提取了函数名、参数并发起了调用：

   ```python
   function_to_call = available_functions[function_name]

   function_args = json.loads(tool_call.arguments)
   function_response = function_to_call(**function_args)
   ```

   以下是我们运行代码后的输出：

   **输出**

   ```Recommended Function call:
   {
     "name": "search_courses",
     "arguments": "{\n  \"role\": \"student\",\n  \"product\": \"Azure\",\n  \"level\": \"beginner\"\n}"
   }

   Output of function call:
   [{'title': 'Describe concepts of cryptography', 'url': 'https://learn.microsoft.com/training/modules/describe-concepts-of-cryptography/?
   WT.mc_id=api_CatalogApi'}, {'title': 'Introduction to audio classification with TensorFlow', 'url': 'https://learn.microsoft.com/en-
   us/training/modules/intro-audio-classification-tensorflow/?WT.mc_id=api_CatalogApi'}, {'title': 'Design a Performant Data Model in Azure SQL
   Database with Azure Data Studio', 'url': 'https://learn.microsoft.com/training/modules/design-a-data-model-with-ads/?
   WT.mc_id=api_CatalogApi'}, {'title': 'Getting started with the Microsoft Cloud Adoption Framework for Azure', 'url':
   'https://learn.microsoft.com/training/modules/cloud-adoption-framework-getting-started/?WT.mc_id=api_CatalogApi'}, {'title': 'Set up the
   Rust development environment', 'url': 'https://learn.microsoft.com/training/modules/rust-set-up-environment/?WT.mc_id=api_CatalogApi'}]
   <class 'str'>
   ```

1. 现在，我们将更新后的消息 `messages` 发送回 LLM，从而得到一个自然语言响应，而不是 API 的 JSON 格式响应。

   ```python
   print("Messages in next request:")
   print(messages)
   print()

   second_response = client.responses.create(
      input=messages,
      model=deployment,
      tool_choice="auto",
      tools=functions,
      temperature=0,
      store=False,
         )  # get a new response from the model where it can see the function response


   print(second_response.output_text)
   ```

   **输出**

   ```text
   I found some good courses for beginner students to learn Azure:

   1. [Describe concepts of cryptography](https://learn.microsoft.com/training/modules/describe-concepts-of-cryptography/?WT.mc_id=api_CatalogApi)
   2. [Introduction to audio classification with TensorFlow](https://learn.microsoft.com/training/modules/intro-audio-classification-tensorflow/?WT.mc_id=api_CatalogApi)
   3. [Design a Performant Data Model in Azure SQL Database with Azure Data Studio](https://learn.microsoft.com/training/modules/design-a-data-model-with-ads/?WT.mc_id=api_CatalogApi)
   4. [Getting started with the Microsoft Cloud Adoption Framework for Azure](https://learn.microsoft.com/training/modules/cloud-adoption-framework-getting-started/?WT.mc_id=api_CatalogApi)
   5. [Set up the Rust development environment](https://learn.microsoft.com/training/modules/rust-set-up-environment/?WT.mc_id=api_CatalogApi)

   You can click on the links to access the courses.
   ```

## 作业

为了继续学习 Azure OpenAI 函数调用，你可以构建：

- 为函数增加更多参数，帮助学习者找到更多课程。
- 创建另一个函数调用，从学习者那里获取更多信息，例如他们的母语。
- 当函数调用和/或 API 调用没有返回任何合适的课程时，创建错误处理机制。

提示：访问 [Learn API 参考文档](https://learn.microsoft.com/training/support/catalog-api-developer-reference?WT.mc_id=academic-105485-koreyst) 页面，了解这些数据在何处以及如何获取。

## 干得漂亮！继续你的旅程

完成本课后，请查看我们的 [生成式 AI 学习合集](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)，继续提升你的生成式 AI 知识！

前往第 12 课，我们将了解如何 [为 AI 应用设计用户体验](../12-designing-ux-for-ai-applications/README.md?WT.mc_id=academic-105485-koreyst)！
