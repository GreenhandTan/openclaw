# OpenClaw 私有化精简核心版 (Headless Feishu Gateway)

本项目基于原 [OpenClaw](https://github.com/openclaw/openclaw) 深度精简定制，移除了所有与服务端运行无关的臃肿模块。这是一个纯粹的、为 **amd64 Linux 原生部署** 打造的极致轻量化私有聊天机器人后台引擎。

## 🎯 系统特点

- **极简无头 (Headless)**：连同 React UI (ui 文件夹)、交互式配置引导 (wizards) 以及所有的 macOS/iOS/Windows/Android 跨端支持代码已全部剿灭。
- **专一集成 (Monoculture Channel)**：去除了 Slack, Telegram, Discord 等 20 余种冗余通道包，代码库**仅聚焦服务于飞书 (Feishu)** 这一办公渠道。
- **能力完整 (Full Sandbox & Tools)**：全面保留了底层模型桥接库（OpenAI, vLLM, 阿里百炼, Volcengine 等）以及 `browser` 等全套工具组件 (Skills/Tools)。
- **配置即代码 (Config-driven)**：拒绝任何交互式干扰，所有秘钥与模型配置项统一交由外挂 `openclaw.json` 读取。

---

## 🚀 部署指南 (Linux amd64)

### 1. 基础环境
- **Node.js** >= 22
- **npm** / **pnpm**
- **Docker 引擎**（必须安装在 Linux 上：此引擎不用于运行主程序，而是供后台动态拉取沙盒 `Dockerfile.sandbox-*` 防止大模型执行风险代码）

### 2. 编译项目
由于已经移除了重型的图形界面与周边框架，直接运行构建：
```bash
# 1. 安装核心依赖
pnpm install

# 2. 极速全量编译
npx pnpm build
```

### 3. 配置启动
在运行目录（或 `~/.config/openclaw/` 下）配置您的 `openclaw.json`。格式参考：

```json
{
  "models": {
    "providers": {
      "openai": {
        "apiKey": "sk-your-custom-token",
        "baseUrl": "http://your-vllm-or-nim-endpoint:8000/v1"
      }
    }
  },
  "channels": {
    "feishu": {
      "enabled": true,
      "appId": "cli_xxxxxxxx",
      "appSecret": "xxxxxxxxxxxxxxxxxxxx"
    }
  }
}
```

启动守护网关：
```bash
# 启动守护网关（无屏幕输出后台模式）
node dist/index.js gateway run
```
