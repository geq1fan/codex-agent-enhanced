# Codex Agent Enhanced

基于 [dztabel-happy/codex-agent](https://github.com/dztabel-happy/codex-agent) 的增强版 Codex 项目经理操作系统。

---

## 1️⃣ 与原版的区别

| 功能 | 原版 | 增强版 |
|------|------|--------|
| 任务状态管理 | ❌ 无 | ✅ 5 状态机（running/review/committed/blocked/waiting） |
| 质量判断标准 | ❌ 无 | ✅ 明确自主修复 vs 用户拍板的边界 |
| Cron 定时任务协作 | ❌ 无 | ✅ wakeKey 去重机制 |
| 状态文件模型 | ❌ 无 | ✅ JSON schema + notificationRouting |
| 巡检逻辑 | ❌ 不清晰 | ✅ 基于 git diff 的自动化决策流程 |

---

## 2️⃣ 工作流与核心价值

**一句话：用户当老板，OpenClaw 当员工，Codex 当工具。**

传统方式：你坐在电脑前写提示词、盯着 Codex 输出、审批命令、检查结果。

使用本 skill：你说一句话 → OpenClaw 设计最优提示词 → `codex exec --full-auto` 执行 → 完成自动通知你。

### 工作流程

```
用户下任务 → OpenClaw 设计提示词 → codex exec --full-auto → 
notify hook 触发 → Telegram 通知 + Cron 巡检推进 → 用户收到结果
```

### 核心价值

- **智能提示词工程**：根据任务类型选择模型、设计提示词、开启 feature flags，而非转发用户原话
- **全程异步**：任务执行过程中无需守着，Cron 自动巡检状态推进
- **实时可见**：每次 Codex 输出完整内容推送到 Telegram，随时可干预

---

## 3️⃣ 快速安装

### 一键配置

```bash
cd ~/.openclaw/skills/codex-agent
./scripts/setup.sh cron   # 或 simple 仅基础通知
```

### 手动配置

1. **Codex 配置** `~/.codex/config.toml`：

```toml
notify = ["python3", "/root/.openclaw/skills/codex-agent/hooks/on_complete.py"]
```

2. **Cron 配置**（完整工作流）：

```bash
openclaw cron add ./config/codex.cron.yml
```

### 前置条件

- OpenClaw 已安装运行
- Codex CLI 已安装 (`pipx install openai-codex`)
- Telegram 已配置为消息通道
- ⚠️ **关闭 session 自动重置**（默认每天重置会丢失任务上下文）

---

更多信息：[English](README_EN.md) | [INSTALL.md](INSTALL.md) | [SKILL.md](SKILL.md)
