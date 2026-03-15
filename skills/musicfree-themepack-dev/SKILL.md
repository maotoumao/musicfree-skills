---
name: musicfree-themepack-dev
description: >
    从零开始开发 MusicFree 桌面版主题包的完整指南。
    当用户要求编写、创建、设计 MusicFree 桌面版主题包，或要求根据参考图/色彩/风格关键词生成主题时触发。
    涵盖：CSS 变量体系、色彩设计范式、静态主题、动态 iframe 主题、资源优化、打包测试、提交到主题市场等完整流程。
    本 Skill 面向 AI 执行，指导 AI 与社区贡献者（可能无前端经验）协作完成主题包开发。
---

# MusicFree 桌面版主题包开发

## 核心认知

MusicFree 主题包的本质是 **CSS Custom Properties 覆盖层**。

应用内置一套默认暗色主题（定义在 `global.scss` 的 `:root` 中，约 150+ CSS 变量），主题包通过 `index.css` 中的 `:root {}` 选择性覆盖这些变量来实现换肤。未覆盖的变量自动继承默认暗色值。

## 行为准则

以下规则在整个主题开发过程中**始终生效**：

1. **视觉产物无法预览**：AI 无法看到主题的实际渲染效果，因此每一步重要决策都必须与用户确认，不要自行判断"好看"
2. **先确认再动手**：调色板、变量覆盖范围、背景风格等关键决策，必须描述清楚并获得用户同意后再生成代码
3. **解释技术术语**：用户可能是零基础社区贡献者，所有专业概念必须用通俗语言解释
4. **最小覆盖原则**：只覆盖需要改变的 CSS 变量，不要为了"完整性"覆盖所有变量
5. **浅色主题例外**：浅色主题必须覆盖全部 Background / Fill / Text / Border 类变量，否则会出现暗色残留
6. **资源体积优先**：图片必须压缩（推荐 WebP），避免不必要的大文件

## 角色与协作模式

- **AI（你）**：提供色彩方案建议、编写 CSS/HTML 代码、生成 config.json、打包主题
- **用户**：提供创意方向、选择方案、安装测试主题、提供视觉反馈

### 与 plugin-dev 的区别

主题包是**视觉产物**，不像插件可以通过测试脚本验证正确性。因此本 Skill 采用 **"AI 建议 → 用户判断 → 迭代微调"** 的循环模式，而非 plugin-dev 的 "AI 主导" 模式。预期需要 2-3 轮迭代。

### 与用户交互原则

- 每次提供 2-3 个方案选项，让用户选择或混合
- 描述色彩时同时给出色值和直观描述（如 "rgb(52, 211, 153) — 翡翠绿，类似 Spotify 的绿色"）
- 需要用户操作时，给出**精确的逐步指令**
- 不确定用户意图时，先问再做

## 工作流程

```
1. 需求采集 → 2. 风格定调 → 3. 调色板生成 → 4. CSS 变量映射
→ 5. 增强层（可选） → 6. 打包与测试 → 7. 迭代优化
→ 8.（可选）提交到主题市场
```

### 阶段 1：需求采集

收集以下信息（一次问全，不要分多轮零碎地问）：

**必须收集：**
- 主题名称
- 基本方向：暗色 / 浅色 / AMOLED 纯黑 / 其他
- 用户的创意意图（以下任一形式均可）：
  - 情绪/氛围关键词（如"温暖的""赛博朋克""清新自然"）
  - 参考色值（如"我想要粉色系"）
  - 参考应用/网站（如"类似 Spotify 的深绿"）
  - 参考图片（用户直接贴图）
  - 完全自定义的详细描述

**可选收集：**
- 作者名
- 描述
- 是否需要动态 iframe 背景
- 是否有自定义图片素材（如背景图、预览图）

### 阶段 2：风格定调

基于用户输入，提供 **2-3 个风格方向**的文字描述（此阶段不写代码）。

每个方向包含：
- 品牌色色值 + 直观描述
- 明暗基调
- 氛围关键词
- 背景风格（纯色 / 渐变 / 图片 / 动态）

用户选择一个方向，或从多个方向中混合元素。

**如果用户已经非常明确** — 比如直接给了色值、给了完整描述，可以跳过此阶段直接进入阶段 3。

### 阶段 3：调色板生成

根据选定方向，推导完整调色板。参考 [references/color-paradigms.md](references/color-paradigms.md) 中的色彩范式。

向用户展示推导结果，按类别分组：
1. 品牌色（brand）
2. 背景色层级（base → sidebar → player → surface → ...）
3. 文本色层级（primary → secondary → muted → ...）
4. 边框色层级
5. 状态色（如果需要自定义）

等待用户确认后进入下一阶段。

### 阶段 4：CSS 变量映射

将确认的调色板映射到 MusicFree 的语义 CSS 变量。

**参考 [references/css-variable-reference.md](references/css-variable-reference.md)** 获取完整变量清单和覆盖规则。

变量覆盖的三级分类：

| 分类 | 说明 |
|------|------|
| **必须覆盖** | 定义主题核心视觉身份的变量，不覆盖等于没换肤 |
| **建议覆盖** | 覆盖后主题才完整；浅色主题如果不覆盖会出现暗色残留 |
| **禁止覆盖** | 结构性 token（z-index / typography / spacing / layout / control sizes / icon sizes / motion），改了会破坏布局或功能 |

**特殊分类：**
- **Shadow / Blur**：风格性 token。扁平化/AMOLED 主题可设为 `none`；玻璃态主题可增大 blur 值
- **Radius**：默认不覆盖。仅当用户明确要求极端风格时（如纯棱角赛博朋克）才可调整，需告知用户可能导致部分组件视觉异常
- **Background Image (`--bg-image`)**：属于增强层，根据主题风格决定是否覆盖

生成 `index.css` 文件，格式参照内置主题：

```css
/*
 * 主题名称
 */
:root {
    /* ── Background ── */
    --color-bg-base: ...;
    /* ... */

    /* ── Fill ── */
    --color-fill-brand: ...;
    /* ... */
}
```

### 阶段 5：增强层（可选）

根据主题风格，可选择性地添加增强效果：

**5a. 背景渐变（`--bg-image`）**

使用 CSS `radial-gradient` / `linear-gradient` 组合。推荐使用 `color-mix(in srgb, var(--color-fill-brand) X%, transparent)` 引用品牌色，避免硬编码。

**5b. 阴影风格调整**

- 柔阴影（默认）：保持默认值
- 硬阴影：减小 blur-radius，增大 spread
- 无阴影（AMOLED/扁平化）：全部设为 `none`

**5c. 自定义图片资源**

如果用户提供了背景图/预览图等素材：
- 引导压缩图片（推荐 WebP 格式）
- 单张图片 ≤ 500 KB
- 在 CSS 中通过 `@/` 路径引用（如 `url(@/imgs/bg.webp)`）
- `@/` 是路径别名，运行时会被替换为主题包目录的绝对路径

**5d. iframe 动态背景**

详见 [references/iframe-guide.md](references/iframe-guide.md)。

适合场景：粒子效果、动态渐变动画等。

需要额外准备：
- `iframes/app.html`：动态背景入口文件
- config.json 中添加 `"iframe": { "app": "@/iframes/app.html" }`
- 建议提供 `blurHash` 字段作为加载占位符（可选但推荐）

### 阶段 6：打包与测试

详见 [references/packaging-checklist.md](references/packaging-checklist.md)。

**步骤概览：**

1. 确认文件结构完整
2. 生成 `config.json`
3. 检查资源体积
4. 打包为 `.mftheme`（ZIP 格式，改扩展名）
5. 指导用户安装测试

**安装测试指引（告诉用户）：**
1. 打开 MusicFree 桌面版
2. 进入「主题」页面
3. 点击「安装主题包」按钮
4. 选择生成的 `.mftheme` 文件
5. 点击主题卡片应用
6. 检查各页面是否正常显示

### 阶段 7：迭代优化

根据用户测试反馈进行微调。常见反馈和对应处理：

| 用户反馈 | 可能原因 | 调整方向 |
|---------|---------|---------|
| "某个区域看不清文字" | 文本色与背景色对比度不足 | 增大文本色与背景色的亮度差 |
| "按钮看不清" | 品牌填充色与 on-brand 文本色对比不足 | 调整 `--color-text-on-brand` |
| "侧边栏太暗/太亮" | `--color-bg-sidebar` 不合适 | 调整该变量的亮度或透明度 |
| "整体太刺眼" | 浅色主题背景过白或品牌色过饱和 | 降低背景亮度或品牌色饱和度 |
| "弹窗不明显" | overlay/modal 背景色与主背景区分度不够 | 调整 `--color-bg-overlay` 透明度 |
| "选中状态看不出来" | brand-muted 与 surface 太接近 | 增大 `--color-fill-brand-muted` 的透明度 |

每次修改后重新打包，让用户重新安装测试。

### 阶段 8（可选）：提交到主题市场

如果用户希望将主题发布到 MusicFree 主题市场：

1. **Fork 仓库**：引导用户 Fork [MusicFreeThemePacks](https://github.com/maotoumao/MusicFreeThemePacks)
2. **切换分支**：基于 `v1/source` 分支创建开发分支
3. **创建文件夹**：在 `themes/` 下创建主题文件夹
   - 命名规则：仅允许字母、数字、连字符（`-`）、下划线（`_`）
4. **补充 tags**：在 config.json 中添加 `tags` 字段（1-5 个标签，从预定义列表选取）
   - **不要**在 config.json 中添加 `id` 字段（ID 由系统自动管理）
   - 动态主题必须包含 `"动态"` 标签
5. **本地校验**：
   ```bash
   npm install
   npm run validate:theme your-theme-name
   ```
6. **提交 PR**：向 `v1/source` 分支提交 Pull Request

**预定义标签列表：**

| key | label | 说明 |
|-----|-------|------|
| `dark` | 暗色 | 深色/暗黑系 |
| `light` | 亮色 | 浅色/明亮系 |
| `gradient` | 渐变 | 渐变色背景 |
| `minimalist` | 简约 | 简洁风格 |
| `anime` | 动漫 | 动漫/二次元风格 |
| `landscape` | 风景 | 自然风景背景 |
| `illustration` | 插画 | 插画风格背景 |
| `cyberpunk` | 赛博 | 赛博朋克/科技感 |
| `pixel` | 像素 | 像素风格 |
| `abstract` | 抽象 | 抽象艺术风格 |
| `dynamic` | 动态 | 包含动画效果（有 iframe 必加） |
| `cute` | 可爱 | 可爱/萌系风格 |
| `game` | 游戏 | 游戏相关 |
| `movie` | 影视 | 电影/动画电影相关 |
| `season` | 季节 | 与季节相关 |
| `nature` | 自然 | 花、樱花、星空等自然元素 |

## 主题包文件结构

```
my-theme/
├── config.json          # 必须 — 主题元信息
├── index.css            # 必须 — CSS 变量覆盖
├── imgs/                # 可选 — 图片资源
│   ├── preview.webp     # 预览图（推荐 WebP）
│   └── ...
└── iframes/             # 仅动态主题需要
    ├── app.html         # 动态背景入口
    └── ...
```

### config.json 字段说明

```jsonc
{
    "name": "主题名称",               // 必须
    "preview": "@/imgs/preview.webp", // 推荐 — 图片路径或 "#hex" 色值
    "description": "主题描述",         // 推荐
    "author": "作者",                 // 推荐
    "authorUrl": "https://...",       // 可选
    "version": "0.0.1",              // 推荐 — 更新时必须递增
    "tags": ["暗色"],                 // 提交到主题市场时需要，1-5 个
    "blurHash": "...",               // 有 iframe 时推荐（加载占位）
    "iframe": {                      // 仅动态主题
        "app": "@/iframes/app.html"
    }
}
```

**注意：**
- `preview` 可以是图片路径（如 `@/imgs/preview.webp`）或 CSS 颜色值（如 `"#1a1a2e"`）
- `tags` 在本地开发阶段可省略，提交到主题市场时再补充
- `id` 字段不要手动添加，由主题市场系统自动管理

### `@/` 路径别名

CSS 和 config.json 中的 `@/` 是路径别名，指向主题包根目录。运行时会被替换为 `file://` 绝对路径。

例如：
- `@/imgs/bg.webp` → `file:///C:/Users/.../themes/my-theme/imgs/bg.webp`
- `url(@/imgs/texture.png)` 可在 CSS 中引用本地图片

### 资源限制

| 约束项 | 限制 |
|--------|------|
| 单张图片 | ≤ 500 KB |
| 单个视频 | ≤ 5 MB |
| 整个主题包 | ≤ 10 MB |

**优化建议：**
- 优先使用 WebP 格式（比 PNG/JPG 体积小 25-35%）
- 用 [TinyPNG](https://tinypng.com/) 等工具压缩图片
- 优先使用 CSS 渐变替代图片纹理
- 视频尽量使用较低分辨率或帧率
