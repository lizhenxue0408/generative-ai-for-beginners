# 构建文本生成应用

[![构建文本生成应用](./images/06-lesson-banner.png?WT.mc_id=academic-105485-koreyst)](https://youtu.be/0Y5Luf5sRQA?si=t_xVg0clnAI4oUFZ)

> _（点击上方图片观看本课视频）_

通过本课程体系，你已经看到了像提示（prompts）这样的核心概念，甚至有一整个被称为“提示工程”的学科。你所能交互的许多工具，如 ChatGPT、Office 365、Microsoft Power Platform 等，都支持你使用提示来完成某事。

要将这样一种体验添加到应用中，你需要理解像提示、补全（completion）这样的概念，并选择一个要使用的库。这正是你将在本章学到的内容。

## 简介

在本章中，你将：

- 了解 openai 库及其核心概念。
- 使用 openai 构建一个文本生成应用。
- 理解如何使用提示、温度（temperature）、token 等概念来构建文本生成应用。

## 学习目标

在本课结束时，你将能够：

- 解释什么是文本生成应用。
- 使用 openai 构建一个文本生成应用。
- 配置你的应用以使用更多或更少的 token，并改变温度以获得多样化的输出。

## 什么是文本生成应用？

通常当你构建一个应用时，它会有一些类似如下的界面：

- 基于命令的。控制台应用是典型的、你键入命令并执行任务的程序。例如，`git` 就是一个基于命令的应用。
- 用户界面（UI）。一些应用具有图形用户界面（GUI），你可以点击按钮、输入文本、选择选项等。

### 控制台和 UI 应用的局限

将其与键入命令的基于命令的应用做比较：

- **它受限**。你不能只是键入任何命令，只能是应用支持的那一些。
- **特定于语言**。一些应用支持多种语言，但默认情况下应用是为特定语言构建的，即使你可以添加更多语言支持。

### 文本生成应用的好处

那么文本生成应用有何不同呢？

在文本生成应用中，你有更大的灵活性，不受一组命令或特定输入语言的限制。相反，你可以使用自然语言与应用程序交互。另一个好处是，你已经在与一个在海量信息语料上训练过的数据源交互，而传统应用可能受限于数据库中的内容。

### 我可以用文本生成应用构建什么？

你可以构建很多东西。例如：

- **聊天机器人**。一个回答关于主题（如你的公司和其产品）的问题的聊天机器人可能是个不错的选择。
- **助手**。LLM 擅长诸如总结文本、从文本中获取洞见、生成简历等文本。
- **代码助手**。取决于你使用的语言模型，你可以构建一个帮助你编写代码的代码助手。例如，你可以使用像 GitHub Copilot 以及 ChatGPT 这样的产品来帮助你编写代码。

## 我如何开始？

好吧，你需要找到一种与 LLM 集成的方式，这通常包含以下两种方法：

- 使用 API。在这里你构造带有提示的 Web 请求，并取回生成的文本。
- 使用库。库有助于封装 API 调用，使其更易于使用。

## 库/SDK

有几个众所周知的用于与 LLM 协作的库，例如：

- **openai**，这个库使连接到你的模型并发送提示变得容易。

然后还有工作在更高层次的库，例如：

- **Langchain**。Langchain 很有名，并且支持 Python。
- **Semantic Kernel**。Semantic Kernel 是微软的一个库，支持 C#、Python 和 Java 语言。

## 使用 openai 的第一个应用

让我们看看如何构建我们的第一个应用、需要哪些库、需要多少工作量等等。

### 安装 openai

有许多库可用于与 OpenAI 或 Azure OpenAI 交互。也可以使用众多编程语言，如 C#、Python、JavaScript、Java 等。我们选择了使用 `openai` Python 库，所以我们将使用 `pip` 来安装它。

```bash
pip install openai
```

### 创建资源

你需要执行以下步骤：

- 在 Azure 上创建一个账户 [https://azure.microsoft.com/free/](https://azure.microsoft.com/free/?WT.mc_id=academic-105485-koreyst)。
- 获取 Azure OpenAI 的访问权限。前往 [https://learn.microsoft.com/azure/ai-foundry/openai/overview#how-do-i-get-access-to-azure-openai](https://learn.microsoft.com/azure/ai-foundry/openai/overview#how-do-i-get-access-to-azure-openai?WT.mc_id=academic-105485-koreyst) 并请求访问。

  > [!NOTE]
  > 在撰写本文时，你需要申请 Azure OpenAI 的访问权限。

- 安装 Python <https://www.python.org/>
- 已创建 Azure OpenAI 服务资源。有关如何 [创建资源](https://learn.microsoft.com/azure/ai-foundry/openai/how-to/create-resource?pivots=web-portal?WT.mc_id=academic-105485-koreyst) 的指南，请参阅此指南。

### 查找 API 密钥和终结点

此时，你需要告诉你的 `openai` 库要使用哪个 API 密钥。要找到你的 API 密钥，请转到 Azure OpenAI 资源的“密钥和终结点（Keys and Endpoint）”部分，并复制“Key 1”的值。

![Azure 门户中的密钥和终结点资源边栏](https://learn.microsoft.com/azure/ai-foundry/openai/media/quickstarts/endpoint.png?WT.mc_id=academic-105485-koreyst)

既然你已经复制了这些信息，让我们指示库来使用它。

> [!NOTE]
> 值得将你的 API 密钥与代码分离。你可以通过使用环境变量来实现这一点。
>
> - 将环境变量 `OPENAI_API_KEY` 设置为你的 API 密钥。
>   `export OPENAI_API_KEY='sk-...'`

### 配置 Azure

如果你使用的是 Azure OpenAI（现为 Microsoft Foundry 的一部分），以下是配置方法。我们使用标准的 `OpenAI` 客户端，指向 Azure OpenAI 的 `/openai/v1/` 终结点，它与 Responses API 配合工作，且不需要 `api_version`：

```python
import os
from openai import OpenAI

client = OpenAI(
    api_key=os.environ["AZURE_OPENAI_API_KEY"],
    base_url=f"{os.environ['AZURE_OPENAI_ENDPOINT'].rstrip('/')}/openai/v1/",
)
```

上面我们设置了以下内容：

- `api_key`，这是你在 Azure 门户或 Microsoft Foundry 门户中找到的 API 密钥。
- `base_url`，这是你的 Foundry 资源终结点，并附加了 `/openai/v1/`。稳定的 v1 终结点在 OpenAI 和 Azure OpenAI 上均可工作，无需管理 `api_version`。

> [!NOTE] > `os.environ` 读取环境变量。你可以用它来读取像 `AZURE_OPENAI_API_KEY` 和 `AZURE_OPENAI_ENDPOINT` 这样的环境变量。在你的终端中或使用像 `dotenv` 这样的库来设置这些环境变量。

## 生成文本

生成文本的方式是通过 `responses.create` 方法使用 Responses API。这里有个示例：

```python
prompt = "Complete the following: Once upon a time there was a"

response = client.responses.create(
    model="gpt-5-mini",  # 这是你的模型部署名称
    input=prompt,
    store=False,
)
print(response.output_text)
```

在上面的代码中，我们创建了一个响应，并传入我们想要使用的模型和提示。然后我们通过 `response.output_text` 打印生成的文本。

### 多轮对话

Responses API 非常适合单轮文本生成和多轮聊天机器人——你在 `input` 中提供一组消息来构建对话：

```python
from openai import OpenAI

client = OpenAI(api_key="sk-...")

response = client.responses.create(model="gpt-5-mini", input="Hello world", store=False)
print(response.output_text)
```

关于此功能的更多内容在后续章节中。

## 练习 - 你的第一个文本生成应用

既然我们学会了如何设置和配置 openai，是时候构建你的第一个文本生成应用了。要构建你的应用，请遵循以下步骤：

1. 创建虚拟环境并安装 openai：

   ```bash
   python -m venv venv
   source venv/bin/activate
   pip install openai
   ```

   > [!NOTE]
   > 如果你使用的是 Windows，请输入 `venv\Scripts\activate` 而不是 `source venv/bin/activate`。

   > [!NOTE]
   > 通过前往 [https://portal.azure.com/](https://portal.azure.com/?WT.mc_id=academic-105485-koreyst) 并搜索 `Open AI`，选择 `Open AI 资源`，然后选择 `密钥和终结点` 并复制 `Key 1` 的值，来查找你的 Azure OpenAI 密钥。

1. 创建一个 _app.py_ 文件，并赋予它以下代码：

   ```python
   import os
   from openai import OpenAI

   client = OpenAI(
       api_key="<用你的 Azure OpenAI 密钥替换此值>",
       base_url="<在 Azure 门户中找到的终结点>/openai/v1/",
   )
   deployment_name = "<部署名称>"

   # 添加你的补全代码
   prompt = "Complete the following: Once upon a time there was a"

   # 使用 Responses API 发出请求
   response = client.responses.create(model=deployment_name, input=prompt, store=False)

   # 打印响应
   print(response.output_text)
   ```

   > [!NOTE]
   > 如果你使用的是纯 OpenAI（非 Azure），使用 `client = OpenAI(api_key="<用你的 OpenAI 密钥替换此值>")`（无 `base_url`），并传入像 `gpt-5-mini` 这样的模型名称，而不是部署名称。

   你应该会看到类似如下的输出：

   ```output
    very unhappy _____.

   Once upon a time there was a very unhappy mermaid.
   ```

## 用于不同事情的不同类型的提示

现在你已经看到了如何使用提示生成文本。你甚至有一个正在运行的程序，可以修改和更改它来生成不同类型的文本。

提示可用于各种各样的任务。例如：

- **生成某种类型的文本**。例如，你可以生成一首诗、测验的题目等。
- **查找信息**。你可以使用提示来查找信息，如下面的示例“CORS 在 Web 开发中是什么意思？”。
- **生成代码**。你可以使用提示来生成代码，例如开发一个用于验证电子邮件的正则表达式，或者为什么不生成一个完整的程序，比如一个 Web 应用？

## 一个更实际的用例：菜谱生成器

想象你家里有食材，想做饭。为此，你需要一个菜谱。找到菜谱的一种方法是使用搜索引擎，或者你可以使用 LLM 来做到这一点。

你可以这样写一个提示：

> “用以下食材给我展示 5 道菜的食谱：鸡肉、土豆和胡萝卜。每道菜列出所有用到的食材”

给定上面的提示，你可能会得到类似如下的回复：

```output
1. 烤鸡肉和蔬菜：
食材：
- 4 块鸡腿
- 2 个土豆，切丁
- 2 根胡萝卜，切丁
- 2 汤匙橄榄油
- 2 瓣大蒜，切碎
- 1 茶匙干百里香
- 1 茶匙干牛至
- 盐、黑胡椒，适量

2. 鸡肉土豆炖菜：
食材：
- 2 汤匙橄榄油
- 1 个洋葱，切丁
- 2 瓣大蒜，切碎
- 2 块鸡胸肉，切丁
- 2 个土豆，切丁
- 2 根胡萝卜，切丁
- 1 茶匙干牛至
- 1 茶匙干百里香
- 1 杯鸡汤
- 盐、黑胡椒，适量

3. 鸡肉土豆烘盘：
食材：
- 2 汤匙橄榄油
- 2 块鸡胸肉，切丁
- 2 个土豆，切丁
- 2 根胡萝卜，切丁
- 1 个洋葱，切丁
- 2 瓣大蒜，切碎
- 1 茶匙干牛至
- 1 茶匙干百里香
- 1 杯鸡汤
- 盐、黑胡椒，适量

4. 鸡肉土豆汤：
食材：
- 2 汤匙橄榄油
- 1 个洋葱，切丁
- 2 瓣大蒜，切碎
- 2 块鸡胸肉，切丁
- 2 个土豆，切丁
- 2 根胡萝卜，切丁
- 1 茶匙干牛至
- 1 茶匙干百里香
- 4 杯鸡汤
- 盐、黑胡椒，适量

5. 鸡肉土豆煎饼：
食材：
- 2 汤匙橄榄油
- 2 块鸡胸肉，切丁
- 2 个土豆，切丁
- 2 根胡萝卜，切丁
- 1 个洋葱，切丁
- 2 瓣大蒜，切碎
- 1 茶匙干牛至
```

这个结果是很棒的，我知道该做什么菜了。此时，可能有用的改进是：

- 过滤掉我不喜欢或过敏的食材。
- 生成一个购物清单，以防我家里没有所有食材。

对于上述情况，让我们添加一个额外的提示：

> “请去掉含大蒜的食谱，因为我过敏，并用其他东西替换它。另外，请为食谱生成一个购物清单，考虑到我家里已经有鸡肉、土豆和胡萝卜。”

现在你有了新的结果，即：

```output
1. 烤鸡肉和蔬菜：
食材：
- 4 块鸡腿
- 2 个土豆，切丁
- 2 根胡萝卜，切丁
- 2 汤匙橄榄油
- 1 茶匙干百里香
- 1 茶匙干牛至
- 盐、黑胡椒，适量

2. 鸡肉土豆炖菜：
食材：
- 2 汤匙橄榄油
- 1 个洋葱，切丁
- 2 块鸡胸肉，切丁
- 2 个土豆，切丁
- 2 根胡萝卜，切丁
- 1 茶匙干牛至
- 1 茶匙干百里香
- 1 杯鸡汤
- 盐、黑胡椒，适量

3. 鸡肉土豆烘盘：
食材：
- 2 汤匙橄榄油
- 2 块鸡胸肉，切丁
- 2 个土豆，切丁
- 2 根胡萝卜，切丁
- 1 个洋葱，切丁
- 1 茶匙干牛至
- 1 茶匙干百里香
- 1 杯鸡汤
- 盐、黑胡椒，适量

4. 鸡肉土豆汤：
食材：
- 2 汤匙橄榄油
- 1 个洋葱，切丁
- 2 块鸡胸肉，切丁
- 2 个土豆，切丁
- 2 根胡萝卜，切丁
- 1 茶匙干牛至
- 1 茶匙干百里香
- 4 杯鸡汤
- 盐、黑胡椒，适量

5. 鸡肉土豆煎饼：
食材：
- 2 汤匙橄榄油
- 2 块鸡胸肉，切丁
- 2 个土豆，切丁
- 2 根胡萝卜，切丁
- 1 个洋葱，切丁
- 1 茶匙干牛至

购物清单：
- 橄榄油
- 洋葱
- 百里香
- 牛至
- 盐
- 黑胡椒
```

这是你的五道食谱，没有提到大蒜，并且你还得到了一个购物清单，考虑了你家里已有的东西。

## 练习 - 构建一个菜谱生成器

既然我们已经演练了一个场景，让我们编写代码来匹配所演示的场景。要这样做，请遵循以下步骤：

1. 使用现有的 _app.py_ 文件作为起点
1. 找到 `prompt` 变量，并将其代码更改为如下：

   ```python
   prompt = "Show me 5 recipes for a dish with the following ingredients: chicken, potatoes, and carrots. Per recipe, list all the ingredients used"
   ```

   如果你现在运行代码，你应该会看到类似如下的输出：

   ```output
   -Chicken Stew with Potatoes and Carrots: 3 tablespoons oil, 1 onion, chopped, 2 cloves garlic, minced, 1 carrot, peeled and chopped, 1 potato, peeled and chopped, 1 bay leaf, 1 thyme sprig, 1/2 teaspoon salt, 1/4 teaspoon black pepper, 1 1/2 cups chicken broth, 1/2 cup dry white wine, 2 tablespoons chopped fresh parsley, 2 tablespoons unsalted butter, 1 1/2 pounds boneless, skinless chicken thighs, cut into 1-inch pieces
   -Oven-Roasted Chicken with Potatoes and Carrots: 3 tablespoons extra-virgin olive oil, 1 tablespoon Dijon mustard, 1 tablespoon chopped fresh rosemary, 1 tablespoon chopped fresh thyme, 4 cloves garlic, minced, 1 1/2 pounds small red potatoes, quartered, 1 1/2 pounds carrots, quartered lengthwise, 1/2 teaspoon salt, 1/4 teaspoon black pepper, 1 (4-pound) whole chicken
   -Chicken, Potato, and Carrot Casserole: cooking spray, 1 large onion, chopped, 2 cloves garlic, minced, 1 carrot, peeled and shredded, 1 potato, peeled and shredded, 1/2 teaspoon dried thyme leaves, 1/4 teaspoon salt, 1/4 teaspoon black pepper, 2 cups fat-free, low-sodium chicken broth, 1 cup frozen peas, 1/4 cup all-purpose flour, 1 cup 2% reduced-fat milk, 1/4 cup grated Parmesan cheese

   -One Pot Chicken and Potato Dinner: 2 tablespoons olive oil, 1 pound boneless, skinless chicken thighs, cut into 1-inch pieces, 1 large onion, chopped, 3 cloves garlic, minced, 1 carrot, peeled and chopped, 1 potato, peeled and chopped, 1 bay leaf, 1 thyme sprig, 1/2 teaspoon salt, 1/4 teaspoon black pepper, 2 cups chicken broth, 1/2 cup dry white wine

   -Chicken, Potato, and Carrot Curry: 1 tablespoon vegetable oil, 1 large onion, chopped, 2 cloves garlic, minced, 1 carrot, peeled and chopped, 1 potato, peeled and chopped, 1 teaspoon ground coriander, 1 teaspoon ground cumin, 1/2 teaspoon ground turmeric, 1/2 teaspoon ground ginger, 1/4 teaspoon cayenne pepper, 2 cups chicken broth, 1/2 cup dry white wine, 1 (15-ounce) can chickpeas, drained and rinsed, 1/2 cup raisins, 1/2 cup chopped fresh cilantro
   ```

   > 注意，你的 LLM 是不确定的，所以每次运行程序你可能得到不同的结果。

   很好，让我们看看如何改进。为了改进，我们希望确保代码是灵活的，所以可以改进和更改食材数量以及食谱数量。

1. 让我们以下面的方式更改代码：

   ```python
   no_recipes = input("食谱数量（例如，5）： ")

   ingredients = input("食材列表（例如，鸡肉、土豆和胡萝卜）： ")

   # 将食谱数量和食材插值到提示中
   prompt = f"Show me {no_recipes} recipes for a dish with the following ingredients: {ingredients}. Per recipe, list all the ingredients used"
   ```

   试运行代码，可能看起来像这样：

   ```output
   食谱数量（例如，5）： 3
   食材列表（例如，鸡肉、土豆和胡萝卜）： milk,strawberries

   -Strawberry milk shake: milk, strawberries, sugar, vanilla extract, ice cubes
   -Strawberry shortcake: milk, flour, baking powder, sugar, salt, unsalted butter, strawberries, whipped cream
   -Strawberry milk: milk, strawberries, sugar, vanilla extract
   ```

### 通过添加过滤器和购物清单来改进

我们现在有一个可以生成食谱的工作应用，并且它是灵活的，因为它依赖用户的输入，无论是食谱数量还是所用食材。

为了进一步改进它，我们要添加以下内容：

- **过滤掉食材**。我们希望能够过滤掉我们不喜欢或过敏的食材。为了实现这个改变，我们可以编辑我们现有的提示，并在其末尾添加一个过滤条件，如下所示：

  ```python
  filter = input("过滤器（例如，素食、纯素或无麸质）： ")

  prompt = f"Show me {no_recipes} recipes for a dish with the following ingredients: {ingredients}. Per recipe, list all the ingredients used, no {filter}"
  ```

  上面，我们在提示的末尾添加了 `{filter}`，并且我们从用户那里捕获了过滤值。

  运行该程序的一个示例输入现在可能看起来像这样：

  ```output
  食谱数量（例如，5）： 3
  食材列表（例如，鸡肉、土豆和胡萝卜）： onion,milk
  过滤器（例如，素食、纯素或无麸质）： no milk

  1. 法式洋葱汤

  食材：

  -1 个大洋葱，切片
  -3 杯牛肉高汤
  -1 杯牛奶
  -6 片法式面包
  -1/4 杯帕玛森芝士碎
  -1 汤匙黄油
  -1 茶匙干百里香
  -1/4 茶匙盐
  -1/4 茶匙黑胡椒

  说明：

  1. 在一个大锅中，用黄油将洋葱炒至金黄。
  2. 加入牛肉高汤、牛奶、百里香、盐和胡椒。煮沸。
  3. 减小火候，慢炖 10 分钟。
  4. 将法式面包片放在汤碗上。
  5. 将汤舀在面包上。
  6. 撒上帕玛森芝士。

  2. 洋葱土豆汤

  食材：

  -1 个大洋葱，切碎
  -2 杯土豆，切丁
  -3 杯蔬菜高汤
  -1 杯牛奶
  -1/4 茶匙黑胡椒

  说明：

  1. 在一个大锅中，用黄油将洋葱炒至金黄。
  2. 加入土豆、蔬菜高汤、牛奶和胡椒。煮沸。
  3. 减小火候，慢炖 10 分钟。
  4. 趁热食用。

  3. 奶油洋葱汤

  食材：

  -1 个大洋葱，切碎
  -3 杯蔬菜高汤
  -1 杯牛奶
  -1/4 茶匙黑胡椒
  -1/4 杯中筋面粉
  -1/2 杯帕玛森芝士碎

  说明：

  1. 在一个大锅中，用黄油将洋葱炒至金黄。
  2. 加入蔬菜高汤、牛奶和胡椒。煮沸。
  3. 减小火候，慢炖 10 分钟。
  4. 在一个小碗中，将面粉和帕玛森芝士搅打至顺滑。
  5. 加入汤中，再慢炖 5 分钟，或直到汤变浓稠。
  ```

  如你所见，任何含牛奶的食谱都已被过滤掉。但是，如果你乳糖不耐受，你可能还想过滤掉含奶酪的食谱，所以需要表述清楚。

- **生成购物清单**。我们希望生成一个购物清单，考虑到我们家里已有的东西。

  要实现这个功能，我们可以试着在一个提示中解决所有问题，或者将其拆分为两个提示。让我们试试后一种方法。这里我们建议添加一个额外的提示，但为了让它起作用，我们需要将前一个提示的结果作为上下文添加到后一个提示中。

  找到代码中打印第一个提示结果的部分，并在其下方添加以下代码：

  ```python
  old_prompt_result = response.output_text
  prompt = "为生成的食谱生成一个购物清单，请不要包含我已经有的食材。"

  new_prompt = f"{old_prompt_result} {prompt}"
  response = client.responses.create(model=deployment_name, input=new_prompt, max_output_tokens=1200, store=False)

  # 打印响应
  print("购物清单：")
  print(response.output_text)
  ```

  注意以下事项：

  1. 我们通过将第一个提示的结果添加到新提示中来构造一个新提示：

     ```python
     new_prompt = f"{old_prompt_result} {prompt}"
     ```

  1. 我们发出了一个新请求，但也考虑到了我们在第一个提示中要求的 token 数量，所以这次我们说 `max_output_tokens` 是 1200。

     ```python
     response = client.responses.create(model=deployment_name, input=new_prompt, max_output_tokens=1200, store=False)
     ```

     运行这段代码，我们现在得到以下输出：

     ```output
     食谱数量（例如，5）： 2
     食材列表（例如，鸡肉、土豆和胡萝卜）： apple,flour
     过滤器（例如，素食、纯素或无麸质）： sugar


     -Apple and flour pancakes: 1 cup flour, 1/2 tsp baking powder, 1/2 tsp baking soda, 1/4 tsp salt, 1 tbsp sugar, 1 egg, 1 cup buttermilk or sour milk, 1/4 cup melted butter, 1 Granny Smith apple, peeled and grated
     -Apple fritters: 1-1/2 cups flour, 1 tsp baking powder, 1/4 tsp salt, 1/4 tsp baking soda, 1/4 tsp nutmeg, 1/4 tsp cinnamon, 1/4 tsp allspice, 1/4 cup sugar, 1/4 cup vegetable shortening, 1/4 cup milk, 1 egg, 2 cups shredded, peeled apples
     Shopping list:
     -Flour, baking powder, baking soda, salt, sugar, egg, buttermilk, butter, apple, nutmeg, cinnamon, allspice
     ```

## 改进你的设置

我们目前拥有的代码是能工作的，但有些调整我们应该做以进一步改进。我们应该做的一些事情是：

- **将密钥与代码分离**，比如 API 密钥。密钥不应出现在代码中，而应存储在安全的位置。要将密钥与代码分离，我们可以使用环境变量以及像 `python-dotenv` 这样的库从文件中加载它们。在代码中看起来像这样：

  1. 创建一个包含以下内容的 `.env` 文件：

     ```bash
     OPENAI_API_KEY=sk-...
     ```

     > 注意，对于 Microsoft Foundry 中的 Azure OpenAI，你需要改为设置以下环境变量：

     ```bash
     AZURE_OPENAI_API_KEY=<替换>
     AZURE_OPENAI_ENDPOINT=<替换>
     AZURE_OPENAI_API_VERSION=2024-10-21
     ```

     在代码中，你会像这样加载环境变量：

     ```python
     import os
     from dotenv import load_dotenv
     from openai import OpenAI

     load_dotenv()

     client = OpenAI(api_key=os.environ["OPENAI_API_KEY"])
     ```

- **关于 token 长度的一句话**。我们应该考虑生成我们想要的文本需要多少 token。token 是要花钱的，所以尽可能我们应该力求在使用的 token 数量上节俭。例如，我们能否重新组织提示，以便使用更少的 token？

  要更改使用的 token，你可以使用 `max_output_tokens` 参数。例如，如果你想使用 100 个 token，你可以这样做：

  ```python
  response = client.responses.create(model=deployment, input=prompt, max_output_tokens=100, store=False)
  ```

- **试验温度**。温度是我们到目前为止还没有提到过、但对我们程序的性能很重要的一个上下文。温度值越高，输出越随机。相反，温度值越低，输出越可预测。考虑你是否希望输出有变化。

  要改变温度，你可以使用 `temperature` 参数。例如，如果你想使用 0.5 的温度，你可以这样做：

  ```python
  response = client.responses.create(model=deployment, input=prompt, temperature=0.5, store=False)
  ```

  > 注意，越接近 1.0，输出越多样化。

- **推理模型不使用 `temperature`**。这是 2026 年的一个重要转变。Microsoft Foundry 上当前的、未弃用的模型是 **推理模型**（GPT-5 系列、o 系列）——它们 **不支持 `temperature` 或 `top_p`**（也不支持 `max_tokens`；请使用 `max_output_tokens`）。如果你向 `gpt-5-mini` 发送 `temperature`，你会得到一个“参数不支持”的错误。所以要想尝试上面的温度示例，请将它指向一个仍然支持采样控制的模型——例如一个开放的 **Llama** 模型，如来自 [Microsoft Foundry 模型目录](https://ai.azure.com/catalog/models?WT.mc_id=academic-105485-koreyst) 的 `Llama-3.3-70B-Instruct`，通过 Foundry Models / Azure AI Inference 终结点（与 `githubmodels-*` 示例相同的方式）调用。对于像 GPT-5 这样的推理模型，你以不同的方式引导输出：
  - **提示工程** - 清晰的指令、示例和结构化输出（见第 [04 - 提示工程](../04-prompt-engineering-fundamentals/README.md?WT.mc_id=academic-105485-koreyst) 课）来完成采样旋钮过去所做的事。
  - **推理控制** - 像推理强度（reasoning effort）/详细程度（verbosity）这样的参数，用来权衡推理深度与延迟和成本。

  简而言之：`temperature`/`top_p` 在许多模型（Llama、Mistral、Phi，以及 GPT-4.x 系列——尽管 GPT-4.x 正在弃用）上仍然有效，但趋势方向是在像 GPT-5 这样的推理模型上使用提示工程 + 推理控制。

## 作业

对于此作业，你可以选择构建什么。

这里有一些建议：

- 调整菜谱生成器应用以进一步改进它。尝试不同的温度值和提示，看看你能想出什么。
- 构建一个“学习伙伴”。这个应用应该能够回答关于某个主题（例如 Python）的问题，你可以有像“Python 中某个主题是什么？”这样的提示，或者你可以有一个提示说，展示某个主题的代码等等。
- 历史机器人，让历史活起来，指示机器人扮演某个历史人物，并询问关于其生活和时代的问题。

## 解决方案

### 学习伙伴

下面是一个起始提示，看看你如何使用它并根据你的喜好调整它。

```text
- "你是 Python 语言的专家

    建议一个 Python 初学者课程，格式如下：

    格式：
    - 概念：
    - 课程简述：
    - 带解决方案的代码练习"
```

### 历史机器人

这里有一些你可以使用的提示：

```text
- "你是亚伯拉罕·林肯，用 3 句话介绍你自己，并使用林肯会使用过的语法和词汇"
- "你是亚伯拉罕·林肯，使用林肯会使用过的语法和词汇：

   用 300 字告诉我你最大的成就"
```

## 知识检测

温度这个概念是做什么的？

1. 它控制输出的随机程度。
1. 它控制响应的大小。
1. 它控制使用了多少 token。

## 🚀 挑战

在做作业时，尝试改变温度，试着将它设置为 0、0.5 和 1。记住 0 是最少变化的，1 是最多变化的。什么值对你的应用最有效？

## 干得漂亮！继续你的学习

完成本课后，请查看我们的 [生成式 AI 学习合集](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)，继续提升你的生成式 AI 知识！

前往第 7 课，我们将了解如何 [构建聊天应用](../07-building-chat-applications/README.md?WT.mc_id=academic-105485-koreyst)！
