# 构建低代码 AI 应用

[![构建低代码 AI 应用](./images/10-lesson-banner.png?WT.mc_id=academic-105485-koreyst)](https://youtu.be/1vzq3Nd8GBA?si=h6LHWJXdmqf6mhDg)

> _（点击上方图片观看本课视频）_

## 简介

既然我们已经学习了如何构建图像生成应用，让我们来谈谈低代码。生成式 AI 可用于各种不同的领域，包括低代码，但什么是低代码，我们又如何为其添加 AI 呢？

通过低代码开发平台（Low Code Development Platforms），传统开发者和非开发者构建应用和解决方案都变得更容易了。低代码开发平台通过提供可视化开发环境，让你拖放组件来构建应用和解决方案，从而让你以更少或几乎无代码的方式构建应用和解决方案。这使得你能够更快、以更少的资源构建应用和解决方案。在本课中，我们将深入探讨如何使用低代码，以及如何使用 Power Platform 通过 AI 增强低代码开发。

Power Platform 为组织提供了机会，通过直观的低代码或无代码环境赋能其团队构建自己的解决方案。这种环境有助于简化构建解决方案的过程。借助 Power Platform，解决方案可以在数天或数周内构建完成，而不是数月或数年。Power Platform 由五个关键产品组成：Power Apps、Power Automate、Power BI、Power Pages 和 Copilot Studio。

本课涵盖：

- Power Platform 中的生成式 AI 简介
- Copilot 简介以及如何使用它
- 使用生成式 AI 在 Power Platform 中构建应用和流
- 使用 AI Builder 理解 Power Platform 中的 AI 模型
- 使用 Microsoft Copilot Studio 构建智能体（agents）

## 学习目标

在本课结束时，你将能够：

- 理解 Copilot 在 Power Platform 中的工作方式。
- 为我们的教育初创公司构建一个学生作业跟踪器应用。
- 构建一个使用 AI 从发票中提取信息的发票处理流。
- 在使用“使用 GPT 创建文本（Create Text with GPT）”AI 模型时应用最佳实践。
- 理解什么是 Microsoft Copilot Studio，以及如何使用它构建智能体。

你将在本课中使用的工具和技术是：

- **Power Apps**，用于学生作业跟踪器应用，它提供了一个低代码开发环境，用于构建应用以跟踪、管理和与数据交互。
- **Dataverse**，用于存储学生作业跟踪器应用的数据，其中 Dataverse 将提供一个低代码数据平台来存储应用的数据。
- **Power Automate**，用于发票处理流，你将拥有一个低代码开发环境来构建工作流以自动化发票处理过程。
- **AI Builder**，用于发票处理 AI 模型，你将使用预构建的 AI 模型来处理我们初创公司的发票。

## Power Platform 中的生成式 AI

增强低代码开发和利用生成式 AI 的应用是 Power Platform 的一个关键重点领域。目标是让每个人都能构建由 AI 驱动的应用、站点、仪表板并使流程自动化，_无需任何数据科学专业知识_。这一目标是通过将生成式 AI 以 Copilot 和 AI Builder 的形式集成到 Power Platform 的低代码开发体验中来实现的。

### 这是如何工作的？

Copilot 是一个 AI 助手，使你能够通过使用自然语言的一系列对话步骤描述你的需求来构建 Power Platform 解决方案。例如，你可以指示你的 AI 助手说明你的应用将使用哪些字段，它将创建应用和底层数据模型，或者你可以指定如何在 Power Automate 中设置流。

你可以将 Copilot 驱动的功能作为应用屏幕中的一个特性，使用户能够通过对话交互发现洞见。

AI Builder 是 Power Platform 中可用的低代码 AI 能力，使你能够使用 AI 模型来帮助自动化流程并预测结果。借助 AI Builder，你可以将 AI 引入到你的应用和流中，连接到你在 Dataverse 或各种云数据源（如 SharePoint、OneDrive 或 Azure）中的数据。

Copilot 在所有的 Power Platform 产品中均可用：Power Apps、Power Automate、Power BI、Power Pages 和 Copilot Studio（前身为 Power Virtual Agents）。AI Builder 在 Power Apps 和 Power Automate 中可用。在本课中，我们将专注于如何在 Power Apps 和 Power Automate 中使用 Copilot 和 AI Builder，为我们这家教育初创公司构建一个解决方案。

### Power Apps 中的 Copilot

作为 Power Platform 的一部分，Power Apps 提供了一个低代码开发环境，用于构建应用以跟踪、管理和与数据交互。它是一套应用开发服务，带有一个可扩展的数据平台，并能够连接到云服务以及本地数据。Power Apps 允许你构建在浏览器、平板电脑和手机上运行的应用，并可以与同事共享。Power Apps 通过简单的界面让用户体验应用开发，使每个业务用户或专业开发者都能构建自定义应用。应用开发体验也通过 Copilot 借助生成式 AI 得到了增强。

Power Apps 中的 Copilot AI 助手功能使你能够描述你需要什么类型的应用以及你希望应用跟踪、收集或显示什么信息。Copilot 然后根据你的描述生成一个响应式的画布（Canvas）应用。你可以随后自定义应用以满足你的需求。AI Copilot 还会生成一个建议的 Dataverse 表，其中包含你需要存储想要跟踪的数据的字段以及一些示例数据。我们将在本课后面了解什么是 Dataverse 以及如何在 Power Apps 中使用它。然后你可以通过对话步骤使用 AI Copilot 助手功能自定义表以满足你的需求。此功能可从 Power Apps 主屏幕轻松获得。

### Power Automate 中的 Copilot

作为 Power Platform 的一部分，Power Automate 让用户在应用和服务之间创建自动化工作流。它有助于自动化重复性的业务流程，如沟通、数据收集和决策审批。其简单的界面允许任何技术能力的人（从初学者到资深开发者）自动化工作任务。工作流开发体验也通过 Copilot 借助生成式 AI 得到了增强。

Power Automate 中的 Copilot AI 助手功能使你能够描述你需要什么类型的流以及你希望流执行什么操作。Copilot 然后根据你的描述生成一个流。你可以随后自定义流以满足你的需求。AI Copilot 还会生成并建议你执行想要自动化的任务所需的操作。我们将在本课后面了解什么是流以及如何在 Power Automate 中使用它们。然后你可以通过对话步骤使用 AI Copilot 助手功能自定义操作以满足你的需求。此功能可从 Power Automate 主屏幕轻松获得。

## 使用 Microsoft Copilot Studio 构建智能体

[Microsoft Copilot Studio](https://learn.microsoft.com/microsoft-copilot-studio/fundamentals-what-is-copilot-studio?WT.mc_id=academic-105485-koreyst)（前身为 Power Virtual Agents）是 Power Platform 中用于构建 **AI 智能体（agents）** 的低代码成员——即可回答疑问、执行操作并代表用户自动化任务的对话式 copilot。就像 Power Platform 的其余部分一样，你在一个可视化的、自然语言优先的体验中构建这些智能体：你描述你希望智能体做什么，Copilot Studio 帮助你搭建其指令、知识和操作。

对于我们这家教育初创公司，你可以构建一个智能体，回答学生关于课程的问题、检查作业截止日期，甚至发送电子邮件给讲师——所有这些都无需编写代码。

以下是使 Copilot Studio 强大的一些最新能力：

- **来自你的知识的生成式答案**。你无需手工编写每个对话，可以连接 **知识源**——公共网站、SharePoint、OneDrive、Dataverse、上传的文件，或通过连接器连接的企业数据——智能体会从中生成有依据（grounded）的答案。
- **生成式编排（Generative orchestration）**。智能体不再依赖死板的触发短语，而是使用 AI 来理解请求，并动态决定组合哪些知识、主题和操作来完成它，包括将多个步骤链接在一起。
- **操作与连接器**。智能体可以 *做* 事情，而不仅仅是聊天。你可以为智能体提供由 1500 多个预构建的 Power Platform 连接器、Power Automate 流、自定义 REST API、提示或 **模型上下文协议（Model Context Protocol，MCP）** 服务器支持的操作。
- **自主智能体（Autonomous agents）**。智能体不限于在聊天窗口中响应。你可以构建由事件触发的 **自主智能体**——例如一封新电子邮件、Dataverse 中的新记录或文件上传——然后在后台行动以完成任务。
- **多智能体编排（Multi-agent orchestration）**。智能体可以调用其他智能体。一个 Copilot Studio 智能体可以移交给其他智能体，或被其他智能体扩展，包括发布到 Microsoft 365 Copilot 的智能体和在 Microsoft Foundry 中构建的智能体。
- **模型选择**。除了内置模型，你还可以从 Microsoft Foundry 模型目录引入模型，以定制你的智能体如何推理和响应。
- **随处发布（Publish anywhere）**。构建完成后，智能体可以发布到多个渠道——Microsoft Teams、Microsoft 365 Copilot、网站或自定义应用等——安全性、身份验证和分析通过 Power Platform 管理体验进行管理。

你可以在 [copilotstudio.microsoft.com](https://copilotstudio.microsoft.com?WT.mc_id=academic-105485-koreyst) 开始构建你的第一个智能体，并在 [Microsoft Copilot Studio 文档](https://learn.microsoft.com/microsoft-copilot-studio/?WT.mc_id=academic-105485-koreyst) 中了解更多信息。

## 作业：使用 Copilot 管理我们初创公司的学生作业和发票

我们的初创公司为学生的在线课程提供服务。该初创公司发展迅速，现在难以跟上对其课程的需求。该初创公司聘请你作为 Power Platform 开发者，帮助他们构建一个低代码解决方案，以帮助他们管理学生的作业和发票。他们的解决方案应该能够通过一个应用帮助他们跟踪和管理学生作业，并通过一个工作流自动化发票处理过程。你被要求使用生成式 AI 来开发该解决方案。

当你开始使用 Copilot 时，你可以使用 [Power Platform Copilot 提示库](https://github.com/pnp/powerplatform-prompts?WT.mc_id=academic-109639-somelezediko) 来开始使用提示。这个库包含了一份你可以用来通过 Copilot 构建应用和流的提示列表。你也可以使用库中的提示来了解如何向 Copilot 描述你的需求。

### 为我们的初创公司构建一个学生作业跟踪器应用

我们初创公司的教育工作者一直在努力跟踪学生的作业。他们一直在使用电子表格来跟踪作业，但随着学生数量的增加，这变得难以管理。他们请你构建一个应用来帮助他们跟踪和管理学生作业。该应用应该使他们能够添加新作业、查看作业、更新作业和删除作业。该应用还应该使教育工作者和学生能够查看已评分和未评分的作业。

你将使用 Power Apps 中的 Copilot 按照以下步骤构建该应用：

1. 导航到 [Power Apps](https://make.powerapps.com?WT.mc_id=academic-105485-koreyst) 主屏幕。
1. 使用主屏幕上的文本区域来描述你想要构建的应用。例如，**_我想构建一个应用来跟踪和管理学生作业_**。点击 **发送（Send）** 按钮将提示发送给 AI Copilot。

![描述你想要构建的应用](./images/copilot-chat-prompt-powerapps.png?WT.mc_id=academic-105485-koreyst)

1. AI Copilot 将建议一个带有你需要存储想要跟踪的数据的字段以及一些示例数据的 Dataverse 表。然后你可以通过对话步骤使用 AI Copilot 助手功能自定义表以满足你的需求。

   > **重要**：Dataverse 是 Power Platform 的底层数据平台。它是一个用于存储应用数据的低代码数据平台。它是一个完全托管的服务，将数据安全存储在 Microsoft Cloud 中，并在你的 Power Platform 环境中预配。它带有内置的数据治理能力，如数据分类、数据血缘（data lineage）、细粒度访问控制等。你可以在 [此处](https://learn.microsoft.com/power-apps/maker/data-platform/data-platform-intro?WT.mc_id=academic-109639-somelezediko) 了解更多关于 Dataverse 的信息。

   ![新表中的建议字段](./images/copilot-dataverse-table-powerapps.png?WT.mc_id=academic-105485-koreyst)

1. 教育工作者想要发送电子邮件给学生，这些学生已经提交了他们的作业，以让他们了解作业的进度。你可以使用 Copilot 向表中添加一个新字段来存储学生电子邮件。例如，你可以使用以下提示向表中添加新字段：**_我想添加一个用于存储学生电子邮件的列_**。点击 **发送（Send）** 按钮将提示发送给 AI Copilot。

![添加一个新字段](./images/copilot-new-column.png?WT.mc_id=academic-105485-koreyst)

1. AI Copilot 将生成一个新的字段，然后你可以自定义该字段以满足你的需求。
1. 完成表后，点击 **创建应用（Create app）** 按钮来创建应用。
1. AI Copilot 将根据你的描述生成一个响应式的画布应用。然后你可以自定义应用以满足你的需求。
1. 为了让教育工作者向学生发送电子邮件，你可以使用 Copilot 向应用添加一个新屏幕。例如，你可以使用以下提示向应用添加一个新屏幕：**_我想添加一个向学生发送电子邮件的屏幕_**。点击 **发送（Send）** 按钮将提示发送给 AI Copilot。

![通过提示指令添加新屏幕](./images/copilot-new-screen.png?WT.mc_id=academic-105485-koreyst)

1. AI Copilot 将生成一个新的屏幕，然后你可以自定义该屏幕以满足你的需求。
1. 完成应用后，点击 **保存（Save）** 按钮保存应用。
1. 要与教育工作者共享该应用，点击 **共享（Share）** 按钮，然后再次点击 **共享（Share）** 按钮。然后你可以通过输入他们的电子邮件地址与教育工作者共享该应用。

> **你的作业**：你刚刚构建的应用是一个良好的开端，但还可以改进。有了电子邮件功能，教育工作者只能通过手动输入学生电子邮件来发送电子邮件。你能使用 Copilot 构建一种自动化，使教育工作者在学生提交作业时能自动向他们发送电子邮件吗？你的提示是，使用正确的提示，你可以通过 Power Automate 中的 Copilot 构建这个功能。

### 为我们的初创公司构建一个发票信息表

我们初创公司的财务团队一直在努力跟踪发票。他们一直在使用电子表格来跟踪发票，但随着发票数量的增加，这变得难以管理。他们请你构建一个表，帮助他们存储、跟踪和管理他们收到的发票信息。该表应该用于构建一个自动化，提取所有发票信息并将其存储在表中。该表还应该使财务团队能够查看已支付和未支付的发票。

Power Platform 有一个称为 Dataverse 的底层数据平台，使你能够存储应用和解决方案的数据。Dataverse 提供了一个用于存储应用数据的低代码数据平台。它是一个完全托管的服务，将数据安全存储在 Microsoft Cloud 中，并在你的 Power Platform 环境中预配。它带有内置的数据治理能力，如数据分类、数据血缘、细粒度访问控制等。你可以在 [此处了解更多关于 Dataverse 的信息](https://learn.microsoft.com/power-apps/maker/data-platform/data-platform-intro?WT.mc_id=academic-109639-somelezediko)。

为什么我们应该为我们的初创公司使用 Dataverse？Dataverse 中的标准表和自定义表为你的数据提供了一个安全的、基于云的存储选项。表让你可以存储不同类型的数据，类似于你在单个 Excel 工作簿中使用多个工作表的方式。你可以使用表来存储特定于你的组织或业务需求的数据。我们的初创公司从使用 Dataverse 中获得的一些好处包括但不限于：

- **易于管理**：元数据和数据都存储在云中，所以你不必担心它们如何存储或管理的细节。你可以专注于构建你的应用和解决方案。
- **安全**：Dataverse 为你的数据提供了一个安全的、基于云的存储选项。你可以通过基于角色的安全性控制谁可以访问你表中的数据以及如何访问。
- **丰富的元数据**：数据类型和关系直接在 Power Apps 中使用
- **逻辑和验证**：你可以使用业务规则、计算字段和验证规则来执行业务逻辑并维护数据准确性。

既然你知道了什么是 Dataverse 以及为什么要使用它，让我们看看如何使用 Copilot 在 Dataverse 中创建一个表，以满足我们财务团队的需求。

> **注意**：你将在下一节中使用此表来构建一个自动化，提取所有发票信息并将其存储在表中。

要使用 Copilot 在 Dataverse 中创建表，请遵循以下步骤：

1. 导航到 [Power Apps](https://make.powerapps.com?WT.mc_id=academic-105485-koreyst) 主屏幕。
1. 在左侧导航栏，选择 **表（Tables）**，然后点击 **描述新表（Describe the new Table）**。
1. 在 **描述新表（Describe the new Table）** 屏幕上，使用文本区域描述你想要创建的表。点击 **发送（Send）** 按钮将提示发送给 AI Copilot。

![选择新表](./images/describe-new-table.png?WT.mc_id=academic-105485-koreyst)

![描述表](./images/copilot-chat-prompt-dataverse.png?WT.mc_id=academic-105485-koreyst)

1. AI Copilot 将建议一个带有你需要存储想要跟踪的数据的字段以及一些示例数据的 Dataverse 表。然后你可以通过对话步骤使用 AI Copilot 助手功能自定义表以满足你的需求。

![建议的 Dataverse 表](./images/copilot-dataverse-table.png?WT.mc_id=academic-105485-koreyst)

1. 财务团队想要发送一封电子邮件给供应商，以向他们更新其发票的当前状态。你可以使用 Copilot 向表中添加一个新字段来存储供应商电子邮件。例如，你可以使用以下提示向表中添加新字段：**_我想添加一个用于存储供应商电子邮件的列_**。点击 **发送（Send）** 按钮将提示发送给 AI Copilot。
1. AI Copilot 将生成一个新的字段，然后你可以自定义该字段以满足你的需求。
1. 完成表后，点击 **创建（Create）** 按钮来创建表。

## 使用 AI Builder 的 Power Platform 中的 AI 模型

AI Builder 是 Power Platform 中可用的低代码 AI 能力，使你能够使用 AI 模型来帮助自动化流程并预测结果。借助 AI Builder，你可以将 AI 引入到你的应用和流中，连接到你在 Dataverse 或各种云数据源（如 SharePoint、OneDrive 或 Azure）中的数据。

## 预构建 AI 模型 vs 自定义 AI 模型

AI Builder 提供两类 AI 模型：预构建（Prebuilt）AI 模型和自定义（Custom）AI 模型。预构建 AI 模型是由微软训练并可在 Power Platform 中使用的即用型 AI 模型。这些有助于你为应用和流添加智能，而无需收集数据然后构建、训练和发布你自己的模型。你可以使用这些模型来自动化流程并预测结果。

Power Platform 中可用的一些预构建 AI 模型包括：

- **关键信息提取（Key Phrase Extraction）**：此模型从文本中提取关键短语。
- **语言检测（Language Detection）**：此模型检测文本的语言。
- **情感分析（Sentiment Analysis）**：此模型检测文本中的正面、负面、中性或混合情感。
- **名片读取器（Business Card Reader）**：此模型从名片中提取信息。
- **文本识别（Text Recognition）**：此模型从图像中提取文本。
- **对象检测（Object Detection）**：此模型检测并从图像中提取对象。
- **文档处理（Document processing）**：此模型从表单中提取信息。
- **发票处理（Invoice Processing）**：此模型从发票中提取信息。

使用自定义 AI 模型，你可以将自己的模型引入 AI Builder，使其可以像任何 AI Builder 自定义模型一样工作，允许你使用自己的数据训练模型。你可以使用这些模型在 Power Apps 和 Power Automate 中自动化流程并预测结果。使用你自己的模型时有适用的限制。请阅读这些 [限制](https://learn.microsoft.com/ai-builder/byo-model#limitations?WT.mc_id=academic-105485-koreyst)。

![AI builder 模型](./images/ai-builder-models.png?WT.mc_id=academic-105485-koreyst)

## 作业 #2 - 为我们的初创公司构建发票处理流

财务团队一直在努力处理发票。他们一直在使用电子表格来跟踪发票，但随着发票数量的增加，这变得难以管理。他们请你构建一个工作流，使用 AI 帮助他们处理发票。该工作流应该使他们能够从发票中提取信息并将信息存储在 Dataverse 表中。该工作流还应该使他们能够向财务团队发送一封带有提取信息的电子邮件。

既然你知道了什么是 AI Builder 以及为什么要使用它，让我们看看如何使用我们前面介绍的发票处理 AI 模型，在 AI Builder 中构建一个工作流，帮助财务团队处理发票。

要构建一个工作流，使用 AI Builder 中的发票处理 AI 模型帮助财务团队处理发票，请遵循以下步骤：

1. 导航到 [Power Automate](https://make.powerautomate.com?WT.mc_id=academic-105485-koreyst) 主屏幕。
1. 使用主屏幕上的文本区域来描述你想要构建的工作流。例如，**_当我邮箱收到发票时处理它_**。点击 **发送（Send）** 按钮将提示发送给 AI Copilot。

   ![Copilot power automate](./images/copilot-chat-prompt-powerautomate.png?WT.mc_id=academic-105485-koreyst)

1. AI Copilot 将建议你执行想要自动化的任务所需的操作。你可以点击 **下一步（Next）** 按钮进入下一步。
1. 在下一步中，Power Automate 将提示你设置流所需的连接。完成后，点击 **创建流（Create flow）** 按钮创建流。
1. AI Copilot 将生成一个流，然后你可以自定义以满足你的需求。
1. 更新流的触发器，并将 **文件夹（Folder）** 设置为发票将存储的文件夹。例如，你可以将文件夹设置为 **收件箱（Inbox）**。点击 **显示高级选项（Show advanced options）** 并将 **仅带附件（Only with Attachments）** 设置为 **是（Yes）**。这将确保流仅在该文件夹中收到带有附件的电子邮件时运行。
1. 从流中移除以下操作：**HTML 转文本（HTML to text）**、**Compose**、**Compose 2**、**Compose 3** 和 **Compose 4**，因为你将不会使用它们。
1. 从流中移除 **条件（Condition）** 操作，因为你将不会使用它。它应该看起来像下面的截图：

   ![power automate，移除操作](./images/powerautomate-remove-actions.png?WT.mc_id=academic-105485-koreyst)

1. 点击 **添加操作（Add an action）** 按钮并搜索 **Dataverse**。选择 **添加新行（Add a new row）** 操作。
1. 在 **从发票提取信息（Extract Information from invoices）** 操作中，将 **发票文件（Invoice File）** 更新为指向来自电子邮件的 **附件内容（Attachment Content）**。这将确保流从发票附件中提取信息。
1. 选择你前面创建的 **表（Table）**。例如，你可以选择 **发票信息（Invoice Information）** 表。选择前一个操作的动态内容来填充以下字段：

    - ID
    - 金额（Amount）
    - 日期（Date）
    - 名称（Name）
    - 状态（Status）- 将 **状态（Status）** 设置为 **待处理（Pending）**。
    - 供应商电子邮件（Supplier Email）- 使用来自 **当新电子邮件到达（When a new email arrives）** 触发器的 **发件人（From）** 动态内容。

    ![power automate 添加行](./images/powerautomate-add-row.png?WT.mc_id=academic-105485-koreyst)

1. 完成流后，点击 **保存（Save）** 按钮保存流。然后你可以通过向你在触发器中指定的文件夹发送一封带发票的电子邮件来测试流。

> **你的作业**：你刚刚构建的流是一个良好的开端，现在你需要思考如何构建一个自动化，使我们的财务团队能够发送一封电子邮件给供应商，以向他们更新其发票的当前状态。你的提示是：流必须在发票状态改变时运行。

## 在 Power Automate 中使用文本生成 AI 模型

AI Builder 中的“使用 GPT 创建文本（Create Text with GPT）”AI 模型使你能够基于提示生成文本，由 Microsoft Azure OpenAI 服务提供支持。借助此能力，你可以将 GPT（生成式预训练 Transformer）技术整合到你的应用和流中，以构建各种自动化流和有洞见的应用。

GPT 模型在大量数据上进行了广泛训练，使其能够在提供提示时生成非常接近人类语言的文本。当与工作流自动化集成时，像 GPT 这样的 AI 模型可以被利用来精简和自动化各种任务。

例如，你可以构建流来为各种用例自动生成文本，如电子邮件草稿、产品描述等。你也可以使用模型为各种应用生成文本，如聊天机器人和客户服务应用，使客户服务代理能够有效且高效地响应客户询问。

![创建提示](./images/create-prompt-gpt.png?WT.mc_id=academic-105485-koreyst)

要了解如何在 Power Automate 中使用此 AI 模型，请学习 [使用 AI Builder 和 GPT 添加智能](https://learn.microsoft.com/training/modules/ai-builder-text-generation/?WT.mc_id=academic-109639-somelezediko) 模块。

## 干得漂亮！继续你的学习

完成本课后，请查看我们的 [生成式 AI 学习合集](https://aka.ms/genai-collection?WT.mc_id=academic-105485-koreyst)，继续提升你的生成式 AI 知识！

想要定制化并更好地利用 Copilot？探索 [Awesome Copilot](https://github.com/github/awesome-copilot?WT.mc_id=academic-105485-koreyst)——一个社区贡献的指令、智能体、技能和配置集合，帮助你充分利用 GitHub Copilot。

前往第 11 课，我们将了解如何 [将生成式 AI 与函数调用集成](../11-integrating-with-function-calling/README.md?WT.mc_id=academic-105485-koreyst)！
