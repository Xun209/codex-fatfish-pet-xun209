# Codex 大肥魚桌寵 · 勛

Codex 桌面版原生寵物版大肥魚：把 106 支動畫重新編碼成 Codex 要的 1536×2288 精靈圖（8 列 × 11 行，每格 192×208）。

> 本倉庫為 **勛** 的版本，改編自
> [PC2005-cloud/dsh-pet](https://github.com/PC2005-cloud/dsh-pet)（DeepSeek Harness 的大肥魚桌寵，MIT）與
> [MeteorNOX/DeepSeek-Balance-Whale-Widget](https://github.com/MeteorNOX/DeepSeek-Balance-Whale-Widget)（小鯨魚記賬掛件，MIT）。
> 角色形象為社群二創的 DeepSeek「大肥魚」（鯨魚娘），非官方形象，美術素材 CC BY-NC-SA 4.0（禁止商用）。
> 詳見 [LICENSE](LICENSE)。

---

# 大肥鱼桌宠 · Codex 原生宠物（fatfish-pet）

把「蓝毛小女仆」装进 **Codex 桌面版**的原生宠物栏。她住在 Codex 自己的透明置顶窗里，
会待机、跟着跑、挥手、思考、干活，累了还会躺下睡觉。

## 安装

```bash
node scripts/install.mjs
```

脚本会校验精灵图尺寸（1536×2288）与体积（≤20 MiB），把宠物写入
`~/.codex/pets/fatfish-girl/`，并在 `~/.codex/config.toml` 里注册本地插件市场
`fatfish-pet-local` 且启用插件。**幂等**，可重复执行。

其它参数：`--status` 查看状态、`--uninstall` 移除宠物、`--pet-only` 只装宠物不改配置。

然后在 Codex 里做两步：

1. **设置 → 宠物（Pets）** → 切换启用「蓝毛小女仆」
2. **完全退出并重新打开 Codex**（含系统托盘）

## 目录

```
codex-fatfish-pet/
├── INSTALL.md               ← 给 AI 的安装说明（发给别人时让 AI 读这个）
├── .codex-plugin/
│   ├── plugin.json          插件清单
│   └── unified-skill.md     统一 skill
├── assets/
│   ├── logo.png
│   └── composer-icon.png
├── pet/
│   ├── pet.json             Codex 宠物清单
│   └── spritesheet.webp     1536×2288 精灵图（8 列 × 11 行，每格 192×208）
├── scripts/
│   └── install.mjs          安装宠物 + 注册本地插件市场 + 启用插件
├── skills/fatfish-pet/
│   ├── SKILL.md
│   └── agents/openai.yaml
├── docs/
│   └── codex-pet-spec.md    Codex 原生宠物规格（逆向整理，改动画前必读）
└── tools/
    └── build-spritesheet.py 精灵图生成器（需本地的 dsh-pet 素材；仅自己改动画时用）
```

## 精灵图规格（改了就装不上）

| 项 | 值 |
|---|---|
| 尺寸 | **1536 × 2288**（v2）或 1536 × 1872（v1） |
| 网格 | **8 列 × 11 行**，每格 **192 × 208** |
| 背景 | 必须透明 |
| 体积 | ≤ 20 MiB（当前 2.6 MiB） |
| 格式 | PNG 或 WebP |

动作按行固定分配：

| 行 | 动作 | 帧数 |
|---|---|---|
| 0 | 待机 idle | 6 |
| 1 | 向右跑 running-right | 8 |
| 2 | 向左跑 running-left | 8 |
| 3 | 挥手 waving | 4 |
| 4 | 跳跃 jumping（悬停触发） | 5 |
| 5 | 失败 failed | 8 |
| 6 | 等待 waiting | 6 |
| 7 | 工作 running | 6 |
| 8 | 整理归档 review | 6 |
| 9 | 备用（女仆舞） | 8 |
| 10 | 备用（睡觉） | 8 |

## 素材来源

- 动作素材： [PC2005-cloud/dsh-pet](https://github.com/PC2005-cloud/dsh-pet)（MIT）
- 角色形象：社区二创的 DeepSeek「大肥鱼」（鲸鱼娘），非官方形象；
  美术素材 **CC BY-NC-SA 4.0，禁止商用**，署名链见上游 README
