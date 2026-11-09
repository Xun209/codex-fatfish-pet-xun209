---
name: fatfish-pet
description: "Install and drive the blue whale-girl (大肥鱼) desktop companion inside Codex. Use when the user asks to add/remove/enable the blue fat-fish pet, change which pet Codex shows, or make the pet react to what Codex is currently doing (thinking, working, waiting, done)."
---

# 大肥鱼桌宠（Codex 原生宠物）

把「蓝毛小女仆」装进 Codex 桌面版的原生宠物栏 —— 她会待在 Codex 的透明置顶窗里，
跟着模型的工作状态切换动作。

## 安装

宠物资源随插件一起提供，只需一次脚本调用：

```bash
node "<plugin-root>/scripts/install-pet.mjs"
```

脚本会：

1. 校验精灵图规格（**1536×2288**，v2；或 1536×1872，v1）与体积上限（20 MiB）
2. 写入 `<CODEX_HOME>/pets/fatfish-girl/`（`pet.json` + `spritesheet.webp`）
3. 宠物 id 为 `custom:fatfish-girl`

其它命令：

```bash
node "<plugin-root>/scripts/install-pet.mjs" --status     # 查看是否已安装
node "<plugin-root>/scripts/install-pet.mjs" --uninstall  # 卸载
```

`CODEX_HOME` 未设置时默认用 `~/.codex`。

## 安装后必须做的两步

脚本只负责落盘，**还要在 Codex 界面里启用**：

1. 打开 **设置 → 宠物（Pets）**，在列表里找到「蓝毛小女仆」并切换启用
   - 如果列表里看不到她，说明 Codex 还没重新扫描宠物目录 —— 重启 Codex 客户端即可
2. **完全退出并重新打开 Codex**（含系统托盘），原生透明置顶窗才会加载新宠物

## 让宠物反映当前工作状态

Codex 用 `update_running_summary` 驱动宠物旁边的活动条（pet activity pill）。
该工具在 `codex_app` 命名空间下，若未被延迟加载，先用工具检索找到它。

规则（Codex 内置约定）：

- **开始一段实质工作前**调用一次，写明你打算做什么
- **只在高层目标或阶段发生变化时**更新，不要每次工具调用都更新
- **简短问答不要调用**；**不要定时更新**
- `summary` 限 **1–50 个字符、单行**

与宠物动作的对应关系：

| 你在做什么 | summary 写法示例 | 宠物动作 |
|---|---|---|
| 等待用户回答 | `等你确认下一步` | waiting（踱步张望） |
| 正在读文件/搜索 | `正在找线索` | running（忙碌点按） |
| 正在跑命令 | `在跑测试` | running |
| 刚完成一段 | `这一段搞定了` | review（清点归档） |
| 报错卡住了 | `遇到问题，正在排查` | failed（叹气冒汗） |

## 已知限制

- 宠物只在 **Codex 桌面版**（Electron）里有原生窗口；CLI 模式下不显示
- 精灵图是 8 列 × 11 行、每格 192×208 的固定网格，**不要改尺寸**；改了 Codex 会拒绝加载
- 动作按行固定分配：row0 待机 / row1 向右跑 / row2 向左跑 / row3 挥手 / row4 跳跃 /
  row5 失败 / row6 等待 / row7 工作 / row8 整理归档 / row9-10 备用

## 素材来源与许可

- 动作素材来自 [PC2005-cloud/dsh-pet](https://github.com/PC2005-cloud/dsh-pet)（MIT 代码）
- 角色形象为社区二创的 DeepSeek「大肥鱼」（鲸鱼娘），非 DeepSeek 官方形象；
  美术素材按 **CC BY-NC-SA 4.0** 授权，**禁止商用**，署名链见该仓库 README
