---
name: fatfish-pet
description: "Install and drive the blue whale-girl (大肥鱼) desktop companion inside Codex. Use when the user asks to add/remove/enable the blue fat-fish pet, change which pet Codex shows, or make the pet react to what Codex is currently doing (thinking, working, waiting, done)."
---

# 大肥鱼桌宠（Codex 原生宠物）

把「蓝毛小女仆」装进 Codex 桌面版的原生宠物栏 —— 她待在 Codex 的透明置顶窗里，
跟着模型的工作状态切换动作。

## 安装

```bash
node "<plugin-root>/scripts/install-pet.mjs"
```

脚本校验精灵图规格（1536×2288 / v2，≤20 MiB），写入 `<CODEX_HOME>/pets/fatfish-girl/`，
宠物 id 为 `custom:fatfish-girl`。

其它：`--status` 查看状态，`--uninstall` 卸载。`CODEX_HOME` 默认 `~/.codex`。

## 安装后必须做的两步

1. 打开 **设置 → 宠物（Pets）**，切换启用「蓝毛小女仆」（列表里看不到就重启 Codex）
2. **完全退出并重新打开 Codex**（含系统托盘），原生透明置顶窗才会加载她

## 让宠物反映当前工作状态

用 `update_running_summary`（`codex_app` 命名空间；若被延迟加载，先用工具检索找到）：

- 开始实质工作前调用一次，写明打算做什么
- 只在高层目标或阶段变化时更新；简短问答不调用；不要定时更新
- `summary` 限 **1–50 字符、单行**

| 场景 | 示例 | 宠物动作 |
|---|---|---|
| 等用户回答 | `等你确认下一步` | waiting（踱步张望） |
| 读文件 / 搜索 | `正在找线索` | running（忙碌点按） |
| 跑命令 | `在跑测试` | running |
| 完成一段 | `这一段搞定了` | review（清点归档） |
| 卡住报错 | `遇到问题，正在排查` | failed（叹气冒汗） |

## 限制

- 仅 Codex 桌面版有原生宠物窗；CLI 不显示
- 精灵图固定 8 列 × 11 行、每格 192×208，**不要改尺寸**
- 素材来自 dsh-pet（MIT 代码）；角色形象为社区二创 DeepSeek「大肥鱼」，
  美术按 CC BY-NC-SA 4.0，**禁止商用**
