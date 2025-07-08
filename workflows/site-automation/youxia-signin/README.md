# 游侠源码网自动签到工作流

## 功能简介

本工作流实现了游侠源码网的自动签到，并通过飞书机器人推送签到结果通知。

- 自动定时签到
- 支持cookie配置
- 支持签到成功/失败的飞书通知

## 主要节点

1. puppeteer 社区节点（浏览器自动化，执行签到）
2. HTTP节点（飞书通知）
3. 定时触发器
4. cookie配置

## 工作流结构

- 定时触发器：定时执行签到任务
- cookie节点：设置自己的cookie
- puppeteer节点：实现自动签到（依赖webSocketDebuggerUrl）
- if节点：判断签到结果
- EditFile节点：构建签到成功/失败消息
- HTTP节点：发送飞书通知

## 如何获取Chrome webSocketDebuggerUrl

1. 打开cmd，执行如下命令（以Windows为例，chrome安装路径请根据实际调整）：

   ```
   "C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222 --remote-debugging-address=0.0.0.0 --user-data-dir="C:\chrome-remote-profile"
   ```

2. 在上面命令行执行后，访问浏览器输出的地址：

   ```
   http://localhost:9222/json/version
   ```

3. 找到 `webSocketDebuggerUrl` 字段，将 `localhost` 替换成 `host.docker.internal`（如n8n运行在docker中）：

   ```
   ws://host.docker.internal:9222/devtools/browser/xxxxxx
   ```

## 如何获取cookieStr信息

1. 进入游侠源码网，完成登录
2. 打开浏览器控制台，切换到 network，找到一个请求并点击，点 Headers，在里面找到 cookie 字段，复制其值

## 配置说明

请将 `config.example.json` 复制为 `config.json`，并根据实际情况填写：

- `cookieStr`：你的游侠源码网cookie
- `webSocketDebuggerUrl`：你的chrome调试地址
- `feishuWebhook`：你的飞书机器人webhook地址

## 示例配置

详见 `config.example.json`

## 常见问题

- puppeteer节点无法连接：请检查webSocketDebuggerUrl是否正确，chrome是否已启动远程调试
- 签到失败：请检查cookie是否有效
- 飞书通知失败：请检查webhook地址是否正确

---

**如有问题欢迎在[项目Issues](https://github.com/Qinjianbo/n8n_workflow/issues)反馈！**