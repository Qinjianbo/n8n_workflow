# GitLab MR 自动代码 Review 工作流

## 功能简介

本工作流实现了对 GitLab Merge Request（MR）自动化代码 review 的流程。通过集成 AI 代码分析、自动评论、飞书机器人通知等功能，极大提升团队代码质量和协作效率。

- 支持 GitLab Webhook 触发
- 自动解析 MR 变更内容
- 调用 AI 模型自动生成代码 review 建议
- 自动将 review 结果以评论形式写回 MR
- 支持飞书机器人推送 review 进度和合并通知

## 主要节点说明

1. **Webhook 节点**  
   接收来自 GitLab 的 MR 事件（如新建、评论、合并等）。

2. **Need Review/If 节点**  
   判断当前事件是否需要进入 review 流程。

3. **Get Changes/Split Out/Skip File Changes**  
   拉取 MR 变更详情，拆分并过滤需要 review 的文件内容。

4. **Parse Last Diff Line/Code**  
   解析 diff，提取原始代码和新代码片段。

5. **AI Agent/DeepSeek Chat Model**  
   调用 AI 模型自动分析代码变更，生成 review 建议。

6. **Post Discussions1**  
   自动将 AI 生成的 review 建议以评论形式写回 MR。

7. **Feishu Notify/发送可合并通知/发送已合并通知**  
   通过飞书机器人推送 review 完成、可合并、已合并等通知。

## 使用方法

### 1. 配置 GitLab Webhook

- 在你的 GitLab 项目设置中，添加 Webhook，指向 n8n 的 Webhook 节点地址。
- 触发事件选择：Merge request events、Comments。

### 2. 配置参数

- 在 HTTP 节点中，将 `url` 字段中的 `https://open.feishu.cn/open-apis/bot/v2/hook/填写飞书机器人ID` 替换为你的飞书机器人 Webhook 地址。
- 在 HTTP 节点中，将 `PRIVATE-TOKEN` 替换为你的 GitLab API Token。

### 3. 导入工作流

1. 在 n8n 中点击“Import from file”，选择本目录下的 `workflow.json` 文件导入。
2. 检查并配置上述参数。

### 4. 启动工作流

- 激活工作流后，GitLab MR 事件会自动触发 review 流程，AI 自动分析并评论，飞书同步通知。

## 5. 在gitlab项目中提交MR

- 在MR的评论区评论`+0`, 将发起review流程
- 在MR的评论区评论`+1`, 将发起MR合并通知
- 合并MR后，将发起MR合并完成通知。

## 常见问题

- **飞书推送失败**：请检查 Webhook 地址是否正确，机器人是否有权限。
- **GitLab API Token 无效**：请检查 Token 权限，建议使用项目 Owner 级别 Token。
- **AI 代码分析失败**：请检查 AI 节点配置和 API Key 是否有效。
- **Webhook 无法触发**：请检查 n8n Webhook 节点地址是否公网可访问，GitLab Webhook 配置是否正确。

## 贡献说明

如需扩展更多 review 规则、支持多平台通知等，欢迎提交 PR 或在 [Issues](https://github.com/Qinjianbo/n8n_workflow/issues) 反馈建议！

---

如需进一步定制或有其他问题，欢迎随时联系维护者。