+++
author = "FlintyLemming"
title = "Claude Code 最佳实践（2025年5月）"
slug = "gXQuQoJyG28kepCiw9M4tp"
date = "2025-08-05"
description = "Claude Code 使用经验"
categories = ["Coding"]
tags = ["Claude", "AI"]
image = "https://img.mitsea.com/blog/posts/2025/08/Claude%20Code%20%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5%EF%BC%882025%E5%B9%B45%E6%9C%88%EF%BC%89/hirzul-maulana-r-i7U49v4c4-unsplash.avif"
+++

本文来源于官方演讲视频，原地址 [Claude Code best practices - YouTube](https://www.youtube.com/watch?v=gv0WHhKelSE "Claude Code best practices - YouTube") 由 AI 根据内容总结生成

### **1. 理解 Claude Code 的工作原理**

- **本质是代理（Agent）**：Claude Code 是一个基于终端操作的强大编码助手。它通过系统指令和工具链自动运行，持续调用工具来完成任务。
- **不依赖索引机制**：不像其他代码辅助工具会预处理和嵌入整个代码库，Claude Code 是“边走边学”，模拟人类开发者探索代码的方式——通过文件查找、目录浏览等命令理解项目结构。

***

### **2. 充分利用 ****`CLAUDE.md`**** 文件的作用**

- 这个文件用于为 Claude 提供上下文相关的说明或规则。
- 可以包含：
  - 如何运行测试；
  - 项目的组织方式介绍；
  - 编码风格规范；
  - 特殊注意事项或其他团队约定。
- 放置位置灵活：
  - 项目根目录下并提交至版本控制中（方便多人共享）
  - 家目录下配置全局生效的信息

***

### **3. 合理管理权限系统**

- 在进行危险操作前需手动确认：
  - 写入文件（创建、修改）；
  - 执行 shell 命令；
  - 操作敏感数据或环境变量。
- **提高效率的小技巧**：
  - 开启 `auto-accept`（Shift + Tab），让其快速启动；
  - 对于重复性高且安全的操作可设置始终允许（如运行单元测试命令）；
  - 学会适时打断与交互干预。

***

### **4. 充分发挥终端优势 & 工具整合能力**

- 因为其擅长执行 CLI 工具指令，因此推荐集成多种开发相关的 CLI 工具：
  - Git、Docker、GitHub CLI (`gh`)等；
  - 自定义脚本工具亦可通过 MCP 协议接入。
- 注意优先选择稳定公开文档清晰的命令行工具而非复杂自研插件。

***

### **5. 上下文管理和长会话优化**

- 当前模型支持高达 200K tokens 上下文长度。
- 长时间操作后注意检查提示信息，两种常见策略应对增长的内容：
  - `/clear`: 清空当前会话上下文（保留 .claudemd 文件）；
  - `/compact`: 总结历史内容形成摘要后再继续对话。

***

### **6. 利用计划驱动思维（Planning）提升可控性和质量**

- 强烈建议在正式开始编写前先要求 Claude 先输出解决方案规划，避免盲目推进错误方向。
- 使用 To-Do List 功能来追踪阶段性步骤，在关键节点校验其路径是否合理。

***

### **7. 实践智能调试与增量式编程模式**

- 推荐采用小步提交+频繁反馈流程：
  - 修改 -> 测试 -> 格式检查 -> 提交循环（TDD理念延伸）；
  - 遇到难以言表的问题可以用截图或图片辅助描述需求。

***

### **8. 探索高级玩法（进阶技巧）**

#### （1）多实例并行

- 考虑开启多个 Claude 终端窗口同步协作不同子任务；

#### （2）Escape 及回溯功能

- 随时按 Esc 中断当前进程，甚至双击进入回溯界面调整思路；

#### （3）MCP 插件拓展

- 若标准功能无法满足特定场景需要，请考虑添加自定义 MCP Server 来增强功能性；

#### （4）Headless 自动化

- 将其作为程序化的开发助理接入 CI/CD 或 DevOps 生命周期（例如 GitHub Actions）

***

### **9. 关注最新特性与更新动态**

- 新增功能包括：
  - `/model` 查询使用的具体语言模型（Sonnet / Opus 等）；
  - 跨工具间思考过程可视化（Think Hard between Tools）；
  - 更好的 IDE 集成（如 VS Code 和 JetBrains 系列）；
- 官方 GitHub 仓库发布 Change Log，定期查看能及时掌握新技巧。

***

### **10. 处理常见行为挑战的经验分享**

| 场景             | 对策                                                |
| -------------- | ------------------------------------------------- |
| 模型不断生成无意义内联注释？ | 更新到 Claude 4 模型已经大幅改善该问题；同时可强化 CLAUDE.md 中禁止此类行为。 |
| 忽视用户定义规则怎么办？   | 新版更尊重定制约束；也建议清理过期条目保持精简有效。                        |

***

### ✅ 最佳实践总结一句话版：

> “善用 `CLAUDE.md` + 规划先行 + 分阶段验证 + 权限管控结合自动化。”

如果你正打算深度应用 Claude Code，以上这些要素将极大帮助你在实际工作中事半功倍，构建高质量软件成果。

> Photo by [Hirzul Maulana](https://unsplash.com/@ijoelpitulikur?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/colorful-floating-bubbles-against-a-blue-background-r-i7U49v4c4?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
      
