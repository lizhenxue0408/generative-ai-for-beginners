# 创建高级提示

[![创建高级提示](./images/05-lesson-banner.png?WT.mc_id=academic-105485-koreyst)](https://youtu.be/BAjzkaCdRok?si=NmUIyRf7-cDgbjtt)

让我们回顾一下上一章学到的一些内容：

> 提示 _工程_ 是我们通过提供更有用的指令或上下文，来 **引导模型产生更相关回复** 的过程。

编写提示也包含两步：通过提供相关上下文来 _构建_ 提示，以及 _优化_ ——如何逐步改进提示。

到目前为止，我们对如何编写提示有了一些基本理解，但还需要更深入。在本章中，你将从一个个尝试各种提示，走向理解为什么某个提示比另一个更好。你将学习如何运用一些基本技巧来构建提示，这些技巧可应用于任何 LLM。

## 简介

本章我们将涵盖以下主题：

- 运用不同的技巧扩展你在提示工程方面的知识。
- 配置你的提示以改变输出。

## 学习目标

完成本课后，你将能够：

- 应用能够改善提示效果的提示工程技巧。
- 执行多变或确定性的提示。

## 提示工程

提示工程是指创建能够产生期望结果的提示的过程。提示工程不仅仅是写一个文本提示这么简单。它并非一门工程学科，而更是一组你可以用来获得期望结果的技术。

### 一个提示示例

让我们以这样一个基础提示为例：

> 生成 10 道关于地理的题目。

在这个提示中，你实际上运用了一系列不同的提示技巧。

让我们拆解一下：

- **上下文**，你指定它应该与“地理”相关。
- **限制输出**，你希望题目不超过 10 道。

### 简单提示的局限性

你可能会也可能不会得到期望的结果。题目会生成，但地理是一个很大的主题，由于以下原因你可能得不到想要的：

- **主题太大**，你不知道它是否会涉及国家、首都、河流等等。
- **格式**，如果你希望题目以某种特定方式格式化呢？

如你所见，创建提示时有太多需要考虑的因素。

到目前为止，我们看到的是一个简单的提示示例，但生成式 AI 的能力远不止于此，能够帮助不同角色和行业的人。接下来让我们探索一些基本技巧。

### 提示技巧

首先，我们需要理解，提示是 LLM 的一种 _涌现（emergent）_ 特性，这意味着它不是模型内置的功能，而是我们在使用模型过程中发现的东西。

我们可以使用一些基本技巧来提示 LLM。让我们来探索它们。

- **零样本提示（Zero-shot prompting）**：这是最基础的提示形式。它是一个单一的提示，仅基于其训练数据向 LLM 请求回复。
- **少样本提示（Few-shot prompting）**：这种提示通过提供 1 个或多个示例来引导 LLM，使其可以据此生成回复。
- **思维链（Chain-of-thought）**：这种提示告诉 LLM 如何将问题分解为步骤。
- **生成知识（Generated knowledge）**：为了改善提示的回复，你可以在提示之外额外提供生成的事实或知识。
- **由易到难（Least to most）**：类似于思维链，这种技巧是将问题分解为一系列步骤，然后要求按顺序执行这些步骤。
- **自我完善（Self-refine）**：这种技巧是对 LLM 的输出进行批评，然后要求它改进。
- **产婆式提示（Maieutic prompting）**：这里你想要确保 LLM 的答案是正确的，并让它解释答案的各个部分。这是自我完善的一种形式。

### 零样本提示

这种提示风格非常简单，它由单一的提示组成。当你刚开始学习 LLM 时，你可能用的就是这种技巧。下面是一个示例：

- 提示：“什么是代数？”
- 答案：“代数是数学的一个分支，研究数学符号以及操作这些符号的规则。”

### 少样本提示

这种提示风格通过提供一些示例以及请求，来帮助模型。它由一个包含额外任务特定数据的单一提示组成。下面是一个示例：

- 提示：“以莎士比亚的风格写一首诗。这里有几首莎士比亚十四行诗的例子：
  十四行诗 18：‘我能否将你比作夏日？你更可爱也更温婉……’
  十四行诗 116：‘别让我承认坚贞的意志会有任何障碍；爱不是爱，若它遇有变节便转移……’
  十四行诗 132：‘我爱你的双眼，而它们，怜悯着我，深知你的心以轻蔑折磨我……’
  现在，写一首关于月亮之美的十四行诗。”
- 答案：“天空中，月儿轻轻闪耀，银辉洒下温柔的光辉……”

示例为 LLM 提供了期望输出的上下文、格式或风格。它们帮助模型理解具体任务并生成更准确、更相关的回复。

### 思维链

思维链是一项非常有趣的技巧，因为它引导 LLM 经历一系列步骤。其思路是通过指令让 LLM 理解如何完成某事。考虑下面的示例，有和没有思维链的情况：

    - 提示：“Alice 有 5 个苹果，扔掉 3 个，给 Bob 2 个，Bob 又还回 1 个，Alice 还有几个苹果？”
    - 答案：5

LLM 回答为 5，这是不正确的。正确答案是 1 个苹果，计算如下（5 - 3 - 2 + 1 = 1）。

那么我们如何教会 LLM 正确地做这道题呢？

让我们试试思维链。运用思维链意味着：

1. 给 LLM 一个类似的示例。
1. 展示计算过程，以及如何正确计算。
1. 提供原始提示。

方法如下：

- 提示：“Lisa 有 7 个苹果，扔掉 1 个，给 Bart 4 个，Bart 又还回 1 个：
  7 - 1 = 6
  6 - 4 = 2
  2 + 1 = 3
  Alice 有 5 个苹果，扔掉 3 个，给 Bob 2 个，Bob 又还回 1 个，Alice 还有几个苹果？”
  答案：1

注意我们写了更长的提示，包含另一个示例、计算过程，然后是原始提示，最终得到了正确的答案 1。

如你所见，思维链是一项非常强大的技巧。

### 生成知识

很多时候当你想构建一个提示时，你希望使用自己公司的数据。你希望提示的一部分来自公司，另一部分才是你真正关心的提示内容。

举个例子，如果你从事保险业务，你的提示可能看起来像这样：

```text
{{company}}: {{company_name}}
{{products}}:
{{products_list}}
请根据以下预算和要求推荐一份保险：
预算：{{budget}}
要求：{{requirements}}
```

如上所示，提示是使用模板构建的。模板中有若干用 `{{变量}}` 表示的变量，这些变量会被公司 API 中的实际值替换。

以下是变量被公司内容替换后，提示可能呈现的样子：

```text
保险公司：ACME 保险
保险产品（每月费用）：
- 汽车，便宜，500 美元
- 汽车，昂贵，1100 美元
- 房屋，便宜，600 美元
- 房屋，昂贵，1200 美元
- 人寿，便宜，100 美元

请根据以下预算和要求推荐一份保险：
预算：1000 美元
要求：汽车、房屋和人寿保险
```

运行这个提示经过 LLM 后，会产生类似如下的回复：

```output
根据预算和要求，我们推荐来自 ACME 保险的以下保险组合：
- 汽车，便宜，500 美元
- 房屋，便宜，600 美元
- 人寿，便宜，100 美元
总费用：1200 美元
```

如你所见，它竟然还推荐了人寿保险，这并不应该。这个结果说明我们需要通过修改提示，使其更清楚地表明允许哪些选项，来优化提示。经过一些 _试错_，我们得到了如下提示：

```text
保险公司：ACME 保险
保险产品（每月费用）：
- 类型：汽车，便宜，费用：500 美元
- 类型：汽车，昂贵，费用：1100 美元
- 类型：房屋，便宜，费用：600 美元
- 类型：房屋，昂贵，费用：1200 美元
- 类型：人寿，便宜，费用：100 美元

请根据以下预算和要求推荐一份保险：
预算：1000 美元，将选择限制在类型：汽车、房屋
```

注意添加 _类型_ 和 _费用_，以及使用关键词 _限制（restrict）_，如何帮助 LLM 理解我们的意图。

现在我们得到了如下回复：

```output
根据预算和要求，我们推荐每月 500 美元的汽车便宜型保险产品。
```

这个例子的重点在于说明，即使我们使用的是像 _生成知识_ 这样的基础技巧，在大多数情况下我们仍然需要优化提示以获得期望的结果。

### 由易到难

由易到难提示的思路是将一个更大的问题分解为子问题。这样，你帮助引导 LLM 如何“攻克”更大的问题。一个很好的例子是数据科学，你可以让 LLM 像这样分解问题：

> 提示：如何用 5 个步骤执行数据科学？

你的 AI 助手回答如下：

1. 收集数据
1. 清洗数据
1. 分析数据
1. 绘制数据
1. 展示数据

### 自我完善，批评结果

对于生成式 AI 和 LLM，你不能轻信输出。你需要进行验证。毕竟，LLM 只是向你展示下一个最有可能说的话，而不是正确的话。因此，一个好主意是让 LLM 自我批评，这就引出了自我完善技巧。

它的工作方式如下，遵循这些步骤：

1. 初始提示，要求 LLM 解决问题
1. LLM 回答
1. 你批评答案，并要求 AI 改进
1. LLM 再次回答，这次会考虑批评，并给出它想出的解决方案

你可以根据需要重复这个过程任意多次。

下面是使用这个技巧的示例：

> 提示：“创建一个包含 products 和 customers 路由的 Python Web API”

AI 回答：

```python
import flask
app = Flask(__name__)

@app.route('/products')
def products():
    return 'Products'

@app.route('/customers')
def customers():
    return 'Customers'
```

> 提示：为上面的代码提出 3 点改进建议

AI 回答：

```text
当然，以下是改进代码的三点建议：

导入正确的模块：第一行应该导入 Flask 而不是 flask。正确的导入语句应该是 from flask import Flask。

添加主函数：在文件末尾添加 if __name__ == '__main__': 代码块，以便在文件被执行时运行应用程序。

使用 f-string：在路由函数中使用 f-string 来格式化返回语句。例如，return 'Products' 可以改为 return f'This is the {request.path} page'。
```

```python
from flask import Flask, request

app = Flask(__name__)

@app.route('/products')
def products():
    return f'This is the {request.path} page'

@app.route('/customers')
def customers():
    return f'This is the {request.path} page'

if __name__ == '__main__':
    app.run()
```

如你所见，由于对第一个答案进行了批评，上面的 AI 回复改进了第一个建议的代码。

### 产婆式提示

产婆式提示是一种类似于自我完善但更侧重让 LLM 解释自身的技巧。其目标是减少 LLM 输出中的不一致性，从而确保它得出正确答案。要遵循的工作流是：

1. 让 LLM 回答问题
1. 对于答案的每一部分，让 LLM 更深入地解释它
1. 如果存在不一致，舍弃那些不一致的部分

重复第 2 和第 3 步，直到你遍历了所有部分，并且对答案感到满意。

下面是一个示例提示：

> 提示：如何用 5 个步骤创建一个危机计划来缓解疫情？
> LLM 回答：

```text
1. 识别风险
1. 识别利益相关者
1. 识别资源
1. 识别行动
1. 识别沟通渠道
```

它列出了 5 个步骤，但我们能确定这是正确的吗？让我们让 LLM 解释每一步：

> 提示：更详细地解释第一步，疫情中具体有哪些风险？

```text
风险如下：

- 生命损失
- 业务损失
- 声誉损失
- 客户损失
- 员工损失
```

此时，我们想确保 LLM 是正确的，所以我们让它更详细地解释第一个风险，并希望它与上面的答案一致：

> 提示：在疫情中，最大的风险是什么，为什么？

```text
最大的风险是生命损失，因为它是最需要保护的东西。
```

> 疫情中最大的两个风险是什么？

```text
最大的两个风险是生命损失和业务损失。
```

此时，LLM 是一致的，并提到“生命”和“业务”作为最大的两个风险。我们现在可以继续下一步，并相当有信心。不过，我们不应盲信 LLM，我们应始终验证输出。

## 让你的输出多样化

LLM 本质上是不确定的（nondeterministic），这意味着每次运行相同的提示，你都会得到不同的结果。尝试如下提示：

> “为 Python Web API 生成代码”

```python
# 导入必要的模块
import flask
from flask import request, jsonify

# 创建 Flask 应用
app = flask.Flask(__name__)

# 为 API 创建路由
@app.route('/api', methods=['GET'])
def api():
    # 从请求中获取数据
    data = request.args

    # 处理数据
    result = process_data(data)

    # 将结果作为 JSON 返回
    return jsonify(result)

# 处理数据的函数
def process_data(data):
    # 对数据做一些处理
    result = data + 1

    # 返回结果
    return result

# 运行应用
if __name__ == '__main__':
    app.run()
```

再次运行相同的提示会生成略微不同的回复：

```python
#import necessary packages
import flask
from flask import request, jsonify

#create the Flask application
app = flask.Flask(__name__)
app.config["DEBUG"] = True

#create a list of books
books = [
    {'id': 0,
     'title': 'A Fire Upon the Deep',
     'author': 'Vernor Vinge',
     'first_sentence': 'The coldsleep itself was dreamless.',
     'year_published': '1992'},
    {'id': 1,
     'title': 'The Ones Who Walk Away From Omelas',
     'author': 'Ursula K. Le Guin',
     'first_sentence': 'With a clamor of bells that set the swallows soaring, the Festival of Summer came to the city Omelas, bright-towered by the sea.',
     'published': '1973'},
    {'id': 2,
     'title': 'Dhalgren',
     'author': 'Samuel R. Delany',
     'first_sentence': 'to wound the autumnal city.',
     'published': '1975'}
]

#create an endpoint for the API
@app.route('/', methods=['GET'])
def home():
    return '''<h1>Books API</h1>
<p>A prototype API for retrieving books.</p>'''

#create an endpoint to return all books
@app.route('/api/v1/resources/books/all', methods=['GET'])
def api_all():
    return jsonify(books)

#create an endpoint to return a single book
@app.route('/api/v1/resources/books', methods=['GET'])
def api_id():
    #check if an ID was provided as part of the URL
    #if ID is provided, assign it to a variable
    #if no ID is provided, display an error
    if 'id' in request.args:
        id = int(request.args['id'])
    else:
        return "Error: No id field provided. Please specify an id."

    #create an empty list for our results
    results = []

    #loop through the data and match results that fit the requested ID
    #IDs are unique, but other fields might return many results
    for book in books:
        if book['id'] == id:
            results.append(book)

    #use the jsonify function from Flask to convert our list of
    #Python dictionaries to the JSON format
    return jsonify(results)

app.run()
```

> 那么，多样的输出是个问题吗？

取决于你想做什么。如果你想要一个特定的回复，那就是个问题。如果你能接受多样化的输出，比如“生成关于地理的任意 3 个问题”，那就不是问题。

### 使用温度来让输出多样化

好，所以我们决定限制输出，使其更具可预测性，也就是更具确定性。我们该怎么做呢？

温度（temperature）是一个介于 0 和 1 之间的值，其中 0 最具确定性，1 最具多样性。默认值为 0.7。让我们看看将温度设为 0.1 时，相同提示两次运行会发生什么：

> “为 Python Web API 生成代码”

```python
#导入必要的库
import flask
from flask import request, jsonify

#创建 Flask 类的实例
app = flask.Flask(__name__)

#为 API 创建端点
@app.route('/api/v1/endpoint', methods=['GET'])
def api_endpoint():
    #获取请求数据
    data = request.get_json()

    #处理数据
    result = process_data(data)

    #返回结果
    return jsonify(result)

#处理数据的函数
def process_data(data):
    #处理数据
    result = {'result': 'success'}

    #返回结果
    return result

#运行 Flask 应用
if __name__ == '__main__':
    app.run()
```

再次运行该提示会给我们这样的结果：

```python
#导入必要的库
import flask
from flask import request, jsonify

#创建 Flask 应用
app = flask.Flask(__name__)

#为 API 创建路由
@app.route('/api', methods=['GET'])
def api_call():
    #获取请求数据
    data = request.get_json()

    #处理数据
    result = process_data(data)

    #返回结果
    return jsonify(result)

#定义 process_data 函数
def process_data(data):
    #对数据做一些处理
    result = data + 1

    #返回结果
    return result

#运行应用
if __name__ == '__main__':
    app.run()
```

这两个输出之间只有极微小的差异。这次让我们反过来，将温度设为 0.9：

```python
# 导入必要的库
import flask
from flask import request, jsonify

# 创建 Flask 应用
app = flask.Flask(__name__)

# 为 API 创建路由
@app.route('/api', methods=['GET'])
def api_call():
    # 获取请求数据
    data = request.args

    # 处理数据
    result = process_data(data)

    # 返回结果
    return jsonify(result)

# 处理数据的函数
def process_data(data):
    # 对数据做一些处理
    result = data + 1

    # 返回结果
    return result

# 运行应用
if __name__ == '__main__':
    app.run()
```

而第二次尝试，温度值为 0.9：

```python
import flask
from flask import request, jsonify

# 创建 Flask 应用
app = flask.Flask(__name__)
app.config['DEBUG'] = True

# 创建一些测试数据
books = [
    {'id': 0, 'title': 'A Fire Upon The Deep', 'author': 'Vernor Vinge', 'first_sentence': 'The coldsleep itself was dreamless.', 'year_published': '1992'},
    {'id': 1, 'title': 'The Ones Who Walk Away From Omelas', 'author': 'Ursula K. Le Guin', 'first_sentence': 'With a clamor of bells that set the swallows soaring, the Festival of Summer came to the city Omelas, bright-towered by the sea.', 'published': '1973'},
    {'id': 2, 'title': 'Dhalgren', 'author': 'Samuel R. Delany', 'first_sentence': 'to wound the autumnal city.', 'published': '1975'}
]

# 创建一个端点
@app.route('/', methods=['GET'])
def home():
    return '''<h1>Welcome to our book API!</h1>'''

@app.route('/api/v1/resources/books
```

如你所见，结果的差异再大不过了。

> 注意，还有更多参数可以改变输出来增加多样性，比如 top-k、top-p、重复惩罚（repetition penalty）、长度惩罚（length penalty）和多样性惩罚（diversity penalty），但这些超出了本课程的范围。

## 良好实践

你可以应用许多实践来尝试获得你想要的结果。随着你越来越频繁地使用提示，你会找到自己的风格。

除了我们已涵盖的技巧之外，在提示 LLM 时还有一些值得考虑的良好实践。

以下是一些值得考虑的良好实践：

- **指定上下文**。上下文很重要，你能指定的越多越好，比如领域、主题等。
- **限制输出**。如果你想要特定数量的项目或特定的长度，请指定它。
- **同时指定“什么”和“如何”**。记得同时说明你想要什么以及你想要怎么做，例如“创建一个包含 products 和 customers 路由的 Python Web API，并将其拆分为 3 个文件”。
- **使用模板**。通常，你会希望用公司的数据丰富你的提示。使用模板来做到这一点。模板可以有你用实际数据替换的变量。
- **拼写正确**。LLM 可能会给你一个正确的回复，但如果你拼写正确，你会得到更好的回复。

## 作业

下面是使用 Flask 构建简单 API 的 Python 代码：

```python
from flask import Flask, request

app = Flask(__name__)

@app.route('/')
def hello():
    name = request.args.get('name', 'World')
    return f'Hello, {name}!'

if __name__ == '__main__':
    app.run()
```

使用像 GitHub Copilot 或 ChatGPT 这样的 AI 助手，并应用“自我完善”技巧来改进代码。

## 解决方案

请尝试通过为代码添加合适的提示来解决作业。

> [!TIP]
> 表述一个提示来要求它改进，限制改进的数量是个好主意。你也可以要求以某种方式改进，例如架构、性能、安全性等。

[解决方案](./python/aoai-solution.py?WT.mc_id=academic-105485-koreyst)

## 知识检测

为什么我会使用思维链提示？给我 1 个正确的回答和 2 个不正确的回答。

1. 为了教 LLM 如何解决问题。
1. B，为了教 LLM 在代码中查找错误。
1. C，为了指示 LLM 想出不同的解决方案。

答：1，因为思维链是通过向 LLM 提供一系列步骤以及类似的问题和它们是如何被解决的，来展示 LLM 如何解决一个问题。

## 🚀 挑战

你刚刚在作业中使用了自我完善技巧。拿你构建的任何程序，思考一下你想对它应用哪些改进。现在使用自我完善技巧来应用这些提议的更改。你觉得结果如何，更好还是更差？

## 干得漂亮！继续你的学习

完成本课后，请查看我们的 [生成式 AI 学习合集](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)，继续提升你的生成式 AI 知识！

前往第 6 课，我们将通过 [构建文本生成应用](../06-text-generation-apps/README.md?WT.mc_id=academic-105485-koreyst) 来应用我们的提示工程知识！
