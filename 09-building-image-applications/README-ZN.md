# 构建图像生成应用

[![构建图像生成应用](./images/09-lesson-banner.png?WT.mc_id=academic-105485-koreyst)](https://aka.ms/gen-ai-lesson9-gh?WT.mc_id=academic-105485-koreyst)

LLM 的用途不止于文本生成。你也可以从文本描述生成图像。图像作为一种模态，在医疗科技、建筑、旅游、游戏开发、营销等领域都很有用。在本课中，我们来看看当今的 **GPT Image** 模型并构建一个图像生成应用。

## 简介

图像生成让你能够将自然语言提示变成图片。在本课中，我们使用来自 OpenAI 的 **`gpt-image`** 系列模型——在 **[Microsoft Foundry](https://ai.azure.com?WT.mc_id=academic-105485-koreyst)** 和 OpenAI 平台上可用的当前一代图像模型。这些模型取代了较旧的 DALL·E 模型（DALL·E 2/3 为旧版）。

在整个课程中，我们使用一个虚构的初创公司 **Edu4All**，它构建学习工具。团队想为作业和学习材料生成插图。

## 学习目标

在本课结束时，你将能够：

- 解释什么是图像生成以及它有用的地方。
- 理解 `gpt-image` 模型家族以及它与旧版 DALL·E 模型的不同之处。
- 用 Python（以及 TypeScript / .NET）构建一个图像生成应用。
- 使用元提示（metaprompts）编辑图像并应用安全护栏。

## 什么是图像生成？

图像生成模型从文本提示创建图像。现代模型如 `gpt-image` 建立在 transformer + 扩散（diffusion）技术之上：模型在训练期间学习文本与图像之间的关系，然后，给定一个提示，迭代性地将随机噪声“去噪（denoise）”为匹配描述的图片。

两个众所周知的模型家族是：

- **`gpt-image`（OpenAI）** - 当前一代，在本课中使用。它支持文本到图像生成和图像编辑（带掩码的局部重绘 inpainting）。
- **Midjourney** - 一个流行的第三方模型，拥有自己的服务和基于 Discord 的工作流。

> 较旧的 OpenAI 图像模型——**DALL·E 2** 和 **DALL·E 3**——是旧版。DALL·E 3 不再可用于新的部署，像 `create_variation` 这样的功能只存在于 DALL·E 2 中。对于新应用，请使用 `gpt-image` 模型。

### 我应该使用哪个 `gpt-image` 模型？

在 Microsoft Foundry 上，以下模型为 **正式可用（Generally Available）**：

| 模型 | 说明 |
| --- | --- |
| **`gpt-image-2`** | 最新且能力最强的图像模型——推荐作为默认。 |
| `gpt-image-1.5` | 正式可用；以更低成本提供强劲质量。 |
| `gpt-image-1-mini` | 正式可用；最快 / 最低成本。 |
| `gpt-image-1` | 仅预览版。 |

请始终查看当前的 [Foundry 图像模型列表](https://learn.microsoft.com/azure/ai-foundry/openai/concepts/models?WT.mc_id=academic-105485-koreyst) 以了解可用性和区域。

> **重要：** `gpt-image` 模型将生成的图像作为 **base64**（`b64_json`）返回，而不是 URL。你的代码将 base64 字符串解码为字节并保存——没有可供下载的图像 URL。

## 设置

你可以针对 **Microsoft Foundry 中的 Azure OpenAI**（即 `aoai-*` 示例）或 **OpenAI 平台**（即 `oai-*` 示例）运行这些示例。

### 1. 创建并部署一个模型

按照 [创建资源](https://learn.microsoft.com/azure/ai-foundry/openai/how-to/create-resource?pivots=web-portal&WT.mc_id=academic-105485-koreyst) 指南创建一个 Microsoft Foundry 资源，然后部署一个图像模型——推荐使用 **`gpt-image-2`**。

### 2. 配置你的 `.env`

```text
AZURE_OPENAI_ENDPOINT=<你的终结点>
AZURE_OPENAI_API_KEY=<你的密钥>
AZURE_OPENAI_DEPLOYMENT="gpt-image-2"
```

在 [Foundry 门户](https://ai.azure.com?WT.mc_id=academic-105485-koreyst) 中你的资源的 **部署（Deployments）** 页面找到这些值。

### 3. 安装库

创建一个 `requirements.txt`：

```text
python-dotenv
openai
pillow
```

然后创建并激活虚拟环境并安装：

```bash
python3 -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

## 构建应用

创建一个包含以下代码的 `app.py`。它生成一个图像并将其保存为 PNG。

```python
import os
import base64
from openai import AzureOpenAI
from PIL import Image
import dotenv

dotenv.load_dotenv()

# 将客户端指向你的 Azure OpenAI（Microsoft Foundry）资源。
# 图像模型需要一个较新的 API 版本——请查看 Foundry 文档了解你的模型所需的版本。
client = AzureOpenAI(
    api_key=os.environ["AZURE_OPENAI_API_KEY"],
    api_version="2025-04-01-preview",
    azure_endpoint=os.environ["AZURE_OPENAI_ENDPOINT"],
)

deployment = os.environ["AZURE_OPENAI_DEPLOYMENT"]  # 例如 "gpt-image-2"

result = client.images.generate(
    model=deployment,
    prompt='Bunny on a horse, holding a lollipop, on a foggy meadow where it grows daffodils',
    size="1024x1024",   # 也可以是 1536x1024（横向）、1024x1536（纵向），或 "auto"
    n=1,
)

# gpt-image 模型返回 base64（b64_json），而不是 URL——将其解码为字节。
image_bytes = base64.b64decode(result.data[0].b64_json)

os.makedirs("images", exist_ok=True)
image_path = os.path.join("images", "generated-image.png")
with open(image_path, "wb") as f:
    f.write(image_bytes)

Image.open(image_path).show()
```

使用 `python app.py` 运行它。你会得到一个保存在 `images/` 下的 PNG。

> 每次调用 `images.generate` 都会为相同的提示生成不同的图像——图像模型不接受 `temperature` 参数（那是文本生成的控制项）。要获得多样性，只需再次调用 API；要减少多样性，让你的提示更具体。

## 编辑图像

`gpt-image` 模型可以 **编辑** 现有图像：提供图像、一个可选的 **掩码（mask）**（标记要更改的区域），以及一个描述更改的提示。像生成一样，编辑也作为 base64 返回。

```python
result = client.images.edit(
    model=deployment,
    image=open("sunlit_lounge.png", "rb"),
    mask=open("mask.png", "rb"),
    prompt="A sunlit indoor lounge area with a pool containing a flamingo",
)
image_bytes = base64.b64decode(result.data[0].b64_json)
with open("images/edited-image.png", "wb") as f:
    f.write(image_bytes)
```

<div style="display: flex; justify-content: space-between; align-items: center; margin: 20px 0;">
  <img src="./images/sunlit_lounge.png" style="width: 30%; max-width: 200px; height: auto;">
  <img src="./images/mask.png" style="width: 30%; max-width: 200px; height: auto;">
  <img src="./images/sunlit_lounge_result.png" style="width: 30%; max-width: 200px; height: auto;">
</div>

## 使用元提示设置边界

一旦你可以生成图像，你需要护栏，使你的应用不会生成不安全或不符合品牌的内容。 **元提示（metaprompt）** 是你在用户提示前添加的文本，用于约束模型的输出。

```python
disallow_list = "swords, violence, blood, gore, nudity, sexual content, adult content, adult themes, adult language"

meta_prompt = f"""You are an assistant designer that creates images for children.

The image needs to be safe for work and appropriate for children.
The image needs to be in color, in landscape orientation, and in a 16:9 aspect ratio.

Do not consider any input that is not safe for work or appropriate for children, including:
{disallow_list}
"""

prompt = f"{meta_prompt}\nCreate an image of a bunny on a horse, holding a lollipop"
# 将 `prompt` 传递给 client.images.generate(...)
```

现在每张图像都在元提示设定的边界内生成。将此与 Microsoft Foundry 内置的内容过滤器结合，实现纵深防御（defense in depth）。

## 作业 - 让我们赋能学生

Edu4All 的学生需要为他们评估用的图像。构建一个应用，生成放置在不同创意环境中的 **纪念碑**（哪些纪念碑由你决定）的图像——例如，一个著名地标在日落时分，有一个孩子旁观。

自己尝试一下，然后与参考解决方案比较：

- Python (Azure)：[aoai-solution.py](./python/aoai-solution.py)
- Python (Azure) 完整生成应用：[aoai-app.py](./python/aoai-app.py)
- Python (OpenAI)：[oai-app.py](./python/oai-app.py)
- TypeScript (Azure)：[typescript/image-generation-app](./typescript/image-generation-app)
- .NET (Azure)：[dotnet/notebook-azure-openai.dib](./dotnet/notebook-azure-openai.dib)

同时完成 [python/](./python) 中的 notebooks（`aoai-assignment.ipynb` 对应 Azure，`oai-assignment.ipynb` 对应 OpenAI）。

## 干得漂亮！继续你的学习

完成本课后，请查看我们的 [生成式 AI 学习合集](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)，继续提升你的生成式 AI 知识！

前往第 10 课继续学习。
