# 云端配置 ☁️ – GitHub Codespaces

**如果你不想在本地安装任何东西，请使用本指南。**  
Codespaces 为你提供一个免费的、基于浏览器的 VS Code 实例，所有依赖项均已预先安装。

---

## 1. 为什么选择 Codespaces？

| 优势 | 对你意味着什么 |
|------|----------------|
| ✅ 零安装 | 可在 Chromebook、iPad、学校机房电脑上使用…… |
| ✅ 预构建的开发容器 | 已内置 Python 3、Node.js、.NET、Java |
| ✅ 免费额度 | 个人账号每月可获得 **120 核心小时 / 60 GB 小时** 的免费额度 |

> 💡 **小贴士**  
> 通过**停止**或**删除**空闲的 codespace 来保持健康额度  
> （查看 ▸ 命令面板 ▸ *Codespaces: Stop Codespace*）。

---

## 2. 创建 Codespace（一键操作）

1. **Fork** 本仓库（右上角的 **Fork** 按钮）。  
2. 在你的 Fork 仓库中，点击 **Code ▸ Codespaces ▸ Create codespace on main**。  
   ![显示创建 codespace 按钮的对话框](./images/who-will-pay.webp?WT.mc_id=academic-105485-koreyst)

✅ 浏览器中会打开一个 VS Code 窗口，开发容器开始构建。
首次构建大约需要 **2 分钟**。

## 3. 添加你的 API 密钥（安全方式）

### 方案 A Codespaces 密钥——推荐

1. 点击 ⚙️ 齿轮图标 -> 命令面板（Command Pallete）-> Codespaces：管理用户密钥 -> 添加新密钥。
2. 名称：OPENAI_API_KEY
3. 值：粘贴你的密钥 → 添加密钥

就这样——我们的代码会自动读取它。

### 方案 B .env 文件（如果你确实需要一个）

```bash
cp .env.copy .env
code .env         # 填写 OPENAI_API_KEY=your_key_here
```
