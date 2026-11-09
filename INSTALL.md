# 安装说明 —— 请交给 Codex 里的 AI 执行

> 这份文件是写给 **AI 助手** 的操作指令。人类用户可以只读「人话版」那一节。
> 本插件把「蓝毛小女仆」（DeepSeek 社区二创的「大肥鱼」鲸鱼娘）装进 **Codex 桌面版**的原生宠物栏。

---

## 一、这是什么

一个 **Codex 插件**，提供一只住在 Codex 原生透明置顶窗里的桌宠「蓝毛小女仆」。
她有 11 组动画（待机、左右跑、挥手、跳跃、失败、等待、工作、整理归档、跳舞、睡觉），
并会配合 Codex 的工作状态切换动作。

插件同时提供一个 skill，教 Codex 用 `update_running_summary` 把「正在做什么」显示在宠物旁的活动条上。

**不需要联网下载任何东西**，资源都已经在压缩包里。

---

## 二、前置条件

| 条件 | 说明 |
|---|---|
| Codex **桌面版**（Electron） | CLI 模式没有原生宠物窗，插件装了也看不到宠物 |
| Codex 版本 | 需要 ≥ 26.1027（旧版没有宠物系统，也没有 `update_running_summary`） |
| Node.js | ≥ 18（随 Codex 一起装的话一般已满足；`node --version` 可查） |
| 磁盘 | 约 3 MB |

若对方用的是 Codex CLI 或网页版：**宠物无法显示**，但插件仍可安装（skill 依然可用）。

---

## 三、给 AI 的安装步骤（照做即可）

### 步骤 1 — 确认解压位置

压缩包解开后应该有这个结构：

```
<解压目录>/
├── INSTALL.md
└── marketplace/
    ├── .agents/plugins/marketplace.json
    └── plugins/fatfish-pet/
        ├── .codex-plugin/plugin.json
        ├── pet/{pet.json, spritesheet.webp}
        ├── scripts/install.mjs
        └── skills/fatfish-pet/SKILL.md
```

如果解压出来不是这个结构，**先停下来告诉用户**，不要自行猜测路径。

### 步骤 2 — 执行安装脚本

```bash
node "<解压目录>/marketplace/plugins/fatfish-pet/scripts/install.mjs"
```

把 `<解压目录>` 换成实际路径。脚本会：

1. 校验精灵图尺寸（必须是 **1536×2288**）与体积（≤20 MiB）
2. 把宠物写入 `<CODEX_HOME>/pets/fatfish-girl/`（`CODEX_HOME` 默认 `~/.codex`）
3. 在 `~/.codex/config.toml` 里注册一个本地插件市场 `fatfish-pet-local` 并启用插件
4. 打印后续步骤

**脚本是幂等的**：重复执行不会写重 config.toml（会先清掉旧段落再写）。

若只想装宠物、不想改 config.toml，用 `--pet-only`。

### 步骤 3 — 告诉用户需要手动做的两件事

脚本跑完后，**必须**提示用户：

1. 打开 Codex → **设置 → 宠物（Pets）** → 选中「蓝毛小女仆」并启用
   （如果列表里根本没有她，说明 Codex 还没重新扫描宠物目录 → 需要重启 Codex）
2. **完全退出 Codex 客户端**（含系统托盘图标）再重新打开

原生透明置顶窗需要重启 Codex 才会加载。

### 步骤 4 — 验证

重启后请 AI 执行：

```bash
node "<解压目录>/marketplace/plugins/fatfish-pet/scripts/install.mjs" --status
```

期望看到：`pet.json OK`、`spritesheet.webp OK`、`config.toml 含本插件注册: 是`。

再让用户在 Codex 界面确认宠物出现。

---

## 四、人话版（给用户看的）

1. 解压这个 zip
2. 让 Codex 里的 AI 帮你跑一次安装（或自己跑：`node marketplace/plugins/fatfish-pet/scripts/install.mjs`）
3. Codex 设置里把「蓝毛小女仆」这个宠物打开
4. 重启 Codex

就这四步。

---

## 五、常见问题

| 现象 | 原因 / 处理 |
|---|---|
| 设置里找不到「蓝毛小女仆」 | Codex 还没扫描宠物目录 → 完全退出并重开 Codex |
| 重启后没有宠物窗口 | 确认用的是 **Codex 桌面版**而非 CLI；确认设置里已启用该宠物 |
| 脚本报「精灵图宽度必须是 1536」 | 压缩包解压损坏 → 重新解压 |
| 脚本报「找不到 marketplace 清单」 | 解压结构不对 → 重新解压，保持 `marketplace/` 目录层级 |
| `node: command not found` | 没装 Node.js → 到 https://nodejs.org 装 LTS 版，或改用 Codex 自带的 node |
| 想卸载 | `node .../install.mjs --uninstall`（只删宠物；config.toml 的注册项可手动删） |

---

## 六、素材来源与许可

- 动作素材：[PC2005-cloud/dsh-pet](https://github.com/PC2005-cloud/dsh-pet)（代码 MIT）
- 角色形象：社区二创的 DeepSeek「大肥鱼」（鲸鱼娘），**非 DeepSeek 官方形象**
  - 原型「溟月」出自 B 站 @上善无形
  - DeepSeek 版（女仆鲸鱼娘）设计出自 B 站 @ZipZipPipe
- 美术素材按 **CC BY-NC-SA 4.0** 授权：**署名 · 禁止商用 · 相同方式共享**
- 转发时请保留这份署名信息
