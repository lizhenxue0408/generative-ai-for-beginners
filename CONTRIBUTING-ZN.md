# 贡献指南（中文版）

本项目欢迎各种贡献和建议。大多数贡献都要求你同意一份贡献者许可协议（Contributor License Agreement，CLA），声明你有权且确实授予我们使用你贡献内容的权利。详情请访问 <https://cla.microsoft.com>。

> 重要提示：翻译本仓库中的文本时，请确保不要使用机器翻译。我们将通过社区来校验翻译，因此请仅在你所精通的语言方向上认领翻译任务。

当你提交 pull request 时，CLA 机器人会自动判断你是否需要提供 CLA，并相应地为 PR 添加标记（例如标签、评论）。只需按照机器人给出的指示操作即可。在所有使用我们 CLA 的仓库中，你只需完成这一次。

## 行为准则（Code of Conduct）

本项目采用了 [Microsoft 开源行为准则](https://opensource.microsoft.com/codeofconduct/?WT.mc_id=academic-105485-koreyst)。如需更多信息，请阅读 [行为准则 FAQ](https://opensource.microsoft.com/codeofconduct/faq/?WT.mc_id=academic-105485-koreyst)，或通过 [opencode@microsoft.com](mailto:opencode@microsoft.com) 联系我们，提出任何额外的问题或意见。

## 有疑问或问题？

请不要为一般性的支持问题开具 GitHub issue，因为 GitHub 列表应当用于功能请求和 bug 报告。这样我们才能更轻松地跟踪代码中的真实问题或 bug，并将一般性讨论与实际代码区分开来。

## 笔误、问题、Bug 和贡献

无论何时，当你要向 Generative AI for Beginners 仓库提交任何改动时，请遵循以下建议。

* 在做出修改之前，请先将仓库 fork 到你自己的账户
* 不要将多个改动合并到一个 pull request 中。例如，请用单独的 PR 分别提交 bug 修复和文档更新
* 如果你的 pull request 出现了合并冲突，请务必先将你本地的 main 分支更新为与上游主仓库完全一致，然后再进行修改
* 如果你要提交翻译，请为所有翻译文件创建一个 PR，因为我们不接受内容的不完整翻译
* 如果你要提交笔误或文档修复，可以在合适的情况下将修改合并到一个 PR 中

## 写作通用指南

- 确保你所有的 URL 都用方括号包裹，后跟圆括号，且括号内和周围不要有多余空格 `[]()`。
- 确保任何相对链接（即指向仓库中其他文件和文件夹的链接）以 `./`（指代当前工作目录中的文件或文件夹）或 `../`（指代父工作目录中的文件或文件夹）开头。
- 确保任何相对链接（即指向仓库中其他文件和文件夹的链接）末尾都带有跟踪 ID（即 `?` 或 `&` 后接 `wt.mc_id=` 或 `WT.mc_id=`）。
- 确保来自以下域名的任何 URL _github.com、microsoft.com、visualstudio.com、aka.ms 和 azure.com_ 末尾都带有跟踪 ID（即 `?` 或 `&` 后接 `wt.mc_id=` 或 `WT.mc_id=`）。
- 确保你的链接中不包含特定国家的区域设置（即 `/en-us/` 或 `/en/`）。
- 确保所有图片都存储在 `./images` 文件夹中。
- 确保图片使用英文、数字和连字符命名，具有描述性名称。

## GitHub 工作流

当你提交 pull request 时，会触发四个不同的工作流来验证上述规则。只需按照此处列出的说明操作，即可通过这些工作流检查。

- [检查失效的相对路径](#check-broken-relative-paths)
- [检查路径是否带有跟踪](#check-paths-have-tracking)
- [检查 URL 是否带有跟踪](#check-urls-have-tracking)
- [检查 URL 是否不含区域设置](#check-urls-dont-have-locale)

### 检查失效的相对路径（Check Broken Relative Paths）

该工作流确保你文件中的任何相对路径都有效。本仓库部署在 GitHub Pages 上，因此在输入用于串联各部分内容的链接时要非常小心，以免将任何人引导到错误的地方。

要确保链接正常工作，只需使用 VS Code 来检查即可。

例如，当你将鼠标悬停在文件中的任何链接上时，会提示你通过按下 **ctrl + 点击** 来跟随该链接。

![VS code 跟随链接截图](./images/vscode-follow-link.png?WT.mc_id=academic-105485-koreyst "VS Code 悬停在链接上提示跟随链接的截图。")

如果你点击了一个链接，而它在本地无法工作，那么它肯定会在 GitHub 上触发工作流并报错。

要修复此问题，请尝试借助 VS Code 输入链接。

当你输入 `./` 或 `../` 时，VS Code 会根据你输入的内容提示你从可用选项中选择。

![VS code 选择相对路径截图](./images/vscode-select-relative-path.png?WT.mc_id=academic-105485-koreyst "VS Code 从弹出的列表中选择相对路径的截图。")

通过点击所需的文件或文件夹跟随路径，你就能确保路径没有断开。

一旦你添加了正确的相对路径，保存并推送你的改动，工作流会再次被触发以验证你的更改。如果通过了检查，你就可以继续了。

### 检查路径是否带有跟踪（Check Paths Have Tracking）

该工作流确保任何相对路径都带有跟踪。本仓库部署在 GitHub Pages 上，因此我们需要跟踪不同文件和文件夹之间的跳转情况。

要确保相对路径带有跟踪，只需检查路径末尾是否存在以下文本 `?wt.mc_id=`。如果它附加在你的相对路径后，你就能通过这项检查。

如果没有，你可能会收到如下错误。

![GitHub 检查路径缺少跟踪的评论截图](./images/github-check-paths-missing-tracking-comment.png?WT.mc_id=academic-105485-koreyst "GitHub 评论显示相对路径缺少跟踪的截图")

要修复此问题，请尝试打开工作流高亮的文件路径，并将跟踪 ID 添加到相对路径的末尾。

一旦你添加了跟踪 ID，保存并推送你的改动，工作流会再次被触发以验证你的更改。如果通过了检查，你就可以继续了。

### 检查 URL 是否带有跟踪（Check URLs Have Tracking）

该工作流确保任何 Web URL 都带有跟踪。本仓库对所有人开放，因此你需要确保对访问进行跟踪，以了解流量来自何处。

要确保 URL 带有跟踪，只需检查 URL 末尾是否存在以下文本 `?wt.mc_id=`。如果它附加在你的 URL 后，你就能通过这项检查。

如果没有，你可能会收到如下错误。

![GitHub 检查 URL 缺少跟踪的评论截图](./images/github-check-urls-missing-tracking-comment.png?WT.mc_id=academic-105485-koreyst "GitHub 评论显示 URL 缺少跟踪的截图")

要修复此问题，请尝试打开工作流高亮的文件路径，并将跟踪 ID 添加到 URL 的末尾。

一旦你添加了跟踪 ID，保存并推送你的改动，工作流会再次被触发以验证你的更改。如果通过了检查，你就可以继续了。

### 检查 URL 是否不含区域设置（Check URLs Don't Have Locale）

该工作流确保任何 Web URL 都不包含特定国家的区域设置。本仓库面向全世界所有人开放，因此你需要确保不要在 URL 中加入你自己国家的区域设置。

要确保 URL 不含国家区域设置，只需检查 URL 中任何位置是否存在以下文本 `/en-us/` 或 `/en/` 或任何其他语言区域设置。如果 URL 中不存在这些内容，你就能通过这项检查。

如果没有，你可能会收到如下错误。

![GitHub 检查国家区域设置的评论截图](./images/github-check-country-locale-comment.png?WT.mc_id=academic-105485-koreyst "GitHub 评论显示 URL 被添加了国家区域设置的截图")

要修复此问题，请尝试打开工作流高亮的文件路径，并从 URL 中移除国家区域设置。

一旦你移除了国家区域设置，保存并推送你的改动，工作流会再次被触发以验证你的更改。如果通过了检查，你就可以继续了。

恭喜！我们会尽快就你的贡献给出反馈。
