# CSS 变量完整参考

本文档列出 MusicFree 桌面版全部可被主题覆盖的 CSS Custom Properties。

## 覆盖等级说明

| 等级 | 含义 |
|------|------|
| **必须** | 不覆盖就没有换肤效果，所有主题都应覆盖 |
| **建议** | 覆盖后主题才完整；浅色主题不覆盖会出现暗色残留 |
| **风格** | 风格性 token，根据设计意图决定是否覆盖 |
| **慎重** | 默认不覆盖；仅当用户明确要求极端风格时才可调整 |
| **禁止** | 结构性 token，覆盖会破坏布局或功能 |

---

## 1. Background（背景色）— 按海拔递增

按覆盖优先级排列。暗色主题至少覆盖前 3 个，浅色主题应全部覆盖。

| 变量 | 默认值（暗色） | 说明 | 等级 |
|------|---------------|------|------|
| `--color-bg-base` | `rgb(18, 18, 18)` | 页面底层 body 背景 | **必须** |
| `--color-bg-sidebar` | `rgba(30, 30, 30, 0.4)` | 左侧导航栏背景 | **建议** |
| `--color-bg-player` | `rgba(25, 25, 25, 0.9)` | 底部播放器栏背景 | **建议** |
| `--color-bg-surface` | `rgba(255, 255, 255, 0.04)` | 卡片、列表行 hover、设置区块 | **建议** |
| `--color-bg-surface-raised` | `rgba(255, 255, 255, 0.08)` | 输入框、标签、高亮元素 — 需从 surface 中突出 | **建议** |
| `--color-bg-drawer` | `rgba(24, 24, 27, 0.95)` | 队列抽屉面板 | **建议** |
| `--color-bg-overlay` | `rgba(0, 0, 0, 0.6)` | 弹窗遮罩层 | **建议** |
| `--color-bg-modal` | `rgb(30, 30, 30)` | 弹窗容器背景 | **建议** |
| `--color-bg-popover` | `rgba(30, 30, 30, 0.95)` | 右键菜单、下拉菜单等浮层 | **建议** |
| `--color-bg-fullscreen` | `rgb(18, 18, 18)` | 全屏播放器底色 | **建议** |
| `--color-bg-placeholder` | `rgb(39, 39, 42)` | 封面/头像占位背景 | **建议** |

## 2. Fill（填充色）— 按交互强度分级

### 品牌填充

| 变量 | 默认值（暗色） | 说明 | 等级 |
|------|---------------|------|------|
| `--color-fill-brand` | `rgb(52, 211, 153)` | 品牌主色 — 主按钮、进度条、高亮等 | **必须** |
| `--color-fill-brand-hover` | `rgba(52, 211, 153, 0.85)` | 品牌色悬停态 | **建议** |
| `--color-fill-brand-muted` | `rgba(52, 211, 153, 0.15)` | 品牌色弱化 — 选中行背景、标签背景 | **建议** |

### 中性填充

| 变量 | 默认值（暗色） | 说明 | 等级 |
|------|---------------|------|------|
| `--color-fill-neutral` | `rgba(255, 255, 255, 0.1)` | 中性按钮/控件默认填充 | **建议** |
| `--color-fill-neutral-hover` | `rgba(255, 255, 255, 0.16)` | 中性填充悬停态 | **建议** |
| `--color-fill-neutral-active` | `rgba(255, 255, 255, 0.2)` | 中性填充按下态 | **建议** |

### 弱填充

| 变量 | 默认值（暗色） | 说明 | 等级 |
|------|---------------|------|------|
| `--color-fill-subtle` | `rgba(255, 255, 255, 0.05)` | 最轻微的背景区分 | **建议** |
| `--color-fill-subtle-hover` | `rgba(255, 255, 255, 0.1)` | 弱填充悬停态 | **建议** |

## 3. Text（文本色）

| 变量 | 默认值（暗色） | 说明 | 等级 |
|------|---------------|------|------|
| `--color-text-primary` | `rgb(255, 255, 255)` | 主文本色 — 标题、正文 | **必须** |
| `--color-text-secondary` | `rgb(161, 161, 170)` | 次要文本 — 副标题、辅助信息 | **建议** |
| `--color-text-muted` | `rgba(255, 255, 255, 0.6)` | 弱化文本 | **建议** |
| `--color-text-placeholder` | `rgba(255, 255, 255, 0.5)` | 占位符文本 | **建议** |
| `--color-text-disabled` | `rgba(255, 255, 255, 0.3)` | 禁用态文本 | **建议** |
| `--color-text-brand` | `rgb(52, 211, 153)` | 品牌色文本 — 链接、高亮文字 | **必须** |
| `--color-text-on-brand` | `rgb(18, 18, 18)` | 品牌色按钮上的文本（需与 fill-brand 对比够大） | **必须** |

## 4. Border（边框色）

| 变量 | 默认值（暗色） | 说明 | 等级 |
|------|---------------|------|------|
| `--color-border-default` | `rgba(255, 255, 255, 0.08)` | 默认边框 | **建议** |
| `--color-border-subtle` | `rgba(255, 255, 255, 0.05)` | 弱边框 — 分隔线 | **建议** |
| `--color-border-strong` | `rgba(255, 255, 255, 0.16)` | 强边框 | **建议** |
| `--color-border-focus` | `rgba(255, 255, 255, 0.2)` | 焦点态边框 | **建议** |
| `--color-border-brand` | `rgba(52, 211, 153, 0.3)` | 品牌色边框 | **建议** |

## 5. Focus Ring（焦点环）

| 变量 | 默认值（暗色） | 说明 | 等级 |
|------|---------------|------|------|
| `--focus-ring` | `0 0 0 2px var(--color-fill-brand)` | 焦点环样式 | **建议** |
| `--focus-ring-offset` | `2px` | 焦点环偏移 | **建议** |

## 6. Status（状态色）

通常不需要覆盖，除非品牌色与某个状态色冲突。

### Info（信息 — 蓝青色调）

| 变量 | 默认值（暗色） | 说明 | 等级 |
|------|---------------|------|------|
| `--color-status-info-bg` | `rgba(23, 29, 34, 0.95)` | 信息提示背景 | **建议** |
| `--color-status-info-border` | `rgba(34, 211, 238, 0.3)` | 信息提示边框 | **建议** |
| `--color-status-info-text` | `rgb(34, 211, 238)` | 信息提示文本 | **建议** |

### Warning（警告 — 琥珀色调）

| 变量 | 默认值（暗色） | 说明 | 等级 |
|------|---------------|------|------|
| `--color-status-warn-bg` | `rgba(32, 23, 20, 0.95)` | 警告背景 | **建议** |
| `--color-status-warn-border` | `rgba(251, 191, 36, 0.3)` | 警告边框 | **建议** |
| `--color-status-warn-text` | `rgb(251, 191, 36)` | 警告文本 | **建议** |
| `--color-status-warn-fill` | `rgb(251, 191, 36)` | 警告填充 | **建议** |

### Danger（危险 — 红色）

| 变量 | 默认值（暗色） | 说明 | 等级 |
|------|---------------|------|------|
| `--color-status-danger-bg` | `rgba(239, 68, 68, 0.1)` | 危险背景 | **建议** |
| `--color-status-danger-border` | `rgba(239, 68, 68, 0.2)` | 危险边框 | **建议** |
| `--color-status-danger-text` | `rgba(239, 68, 68, 0.8)` | 危险文本 | **建议** |
| `--color-status-danger-fill` | `rgb(239, 68, 68)` | 危险填充（如删除按钮） | **建议** |

### Success（成功）

| 变量 | 默认值（暗色） | 说明 | 等级 |
|------|---------------|------|------|
| `--color-status-success-text` | `rgb(110, 231, 183)` | 成功文本 | **建议** |

### Specific（专用色）

| 变量 | 默认值（暗色） | 说明 | 等级 |
|------|---------------|------|------|
| `--color-favorite` | `rgba(239, 68, 68, 0.8)` | 收藏红心 — 独立于 danger，不受状态色变更影响 | **建议** |

## 7. Background Image（背景图）

| 变量 | 默认值（暗色） | 说明 | 等级 |
|------|---------------|------|------|
| `--bg-image` | 三色渐变（品牌绿 / 蓝 / 粉） | body 背景图 — 可用渐变或 `url(@/imgs/bg.webp)` | **风格** |

默认值的详细内容：
```css
--bg-image:
    radial-gradient(at 0% 0%, color-mix(in srgb, var(--color-fill-brand) 15%, transparent) 0px, transparent 50%),
    radial-gradient(at 100% 0%, rgba(59, 130, 246, 0.15) 0px, transparent 50%),
    radial-gradient(at 100% 100%, rgba(236, 72, 153, 0.15) 0px, transparent 50%);
```

**设计建议：**
- 推荐使用 `color-mix(in srgb, var(--color-fill-brand) X%, transparent)` 引用品牌色，避免硬编码色值
- 设为 `none` 可禁用背景渐变（如 AMOLED 纯黑主题）

## 8. Shadow（阴影）

| 变量 | 默认值（暗色） | 说明 | 等级 |
|------|---------------|------|------|
| `--shadow-xs` | `0 1px 2px 0 rgba(0, 0, 0, 0.05)` | 最小阴影 | **风格** |
| `--shadow-sm` | `0 6px 16px rgba(0, 0, 0, 0.25)` | 小阴影 | **风格** |
| `--shadow-md` | `0 10px 30px rgba(0, 0, 0, 0.3)` | 中阴影 | **风格** |
| `--shadow-lg` | `0 20px 40px rgba(0, 0, 0, 0.35)` | 大阴影 | **风格** |
| `--shadow-xl` | `0 25px 50px -12px rgba(0, 0, 0, 0.5)` | 超大阴影 | **风格** |
| `--shadow-up` | `0 -8px 16px rgba(0, 0, 0, 0.24)` | 上方阴影 | **风格** |
| `--shadow-left` | `-10px 0 30px rgba(0, 0, 0, 0.3)` | 左侧阴影 | **风格** |

**设计建议：**
- 浅色主题：降低 alpha 值（如 `0.08`~`0.12`），避免阴影过重
- AMOLED/扁平化：全部设为 `none`
- 暗色主题默认值通常已合适

## 9. Blur（模糊）

| 变量 | 默认值（暗色） | 说明 | 等级 |
|------|---------------|------|------|
| `--blur-xs` | `8px` | | **风格** |
| `--blur-sm` | `12px` | | **风格** |
| `--blur-md` | `20px` | | **风格** |
| `--blur-lg` | `30px` | | **风格** |
| `--blur-xl` | `40px` | | **风格** |
| `--blur-overlay` | `12px` | 遮罩层模糊 | **风格** |
| `--blur-ambient` | `100px` | 环境模糊 | **风格** |

**设计建议：**
- 玻璃态（Glassmorphism）主题：可适当增大 blur 值
- AMOLED/省电主题：全部设为 `0px`（应用支持 `no-blur` class 彻底禁用 GPU 合成层）

## 10. Radius（圆角）

| 变量 | 默认值 | 说明 | 等级 |
|------|--------|------|------|
| `--radius-badge` | `4px` | 徽标 | **慎重** |
| `--radius-control` | `8px` | 按钮、输入框 | **慎重** |
| `--radius-card` | `12px` | 卡片 | **慎重** |
| `--radius-panel` | `16px` | 面板 | **慎重** |
| `--radius-modal` | `20px` | 弹窗 | **慎重** |
| `--radius-artwork` | `24px` | 全屏播放器封面 | **慎重** |
| `--radius-pill` | `9999px` | 胶囊按钮 | **慎重** |

**设计建议：** 圆角系统经过精心调试，默认值适用于绝大多数场景。仅在用户明确要求极端风格时调整（如"纯棱角赛博朋克"将所有值设为 `2px`），并告知用户可能导致部分组件视觉异常。

## 11-17. 结构性 Token（禁止覆盖）

以下类别为结构性 token，主题包**不应覆盖**，否则会导致布局错乱或功能异常。

### 11. Z-Index（层级）

`--z-base` · `--z-sticky` · `--z-sidebar` · `--z-drawer` · `--z-header` · `--z-player` · `--z-fullscreen` · `--z-overlay` · `--z-modal` · `--z-toast` · `--z-popover`

### 12. Typography（排版）

`--font-family` · `--text-display-size` · `--text-display-lg-size` · `--text-display-leading` · `--text-title-size` · `--text-title-leading` · `--text-heading-size` · `--text-heading-leading` · `--text-body-size` · `--text-body-leading` · `--text-label-size` · `--text-label-leading` · `--text-caption-size` · `--text-caption-leading` · `--text-overline-size` · `--text-overline-leading` · `--text-badge-size` · `--text-badge-leading` · `--font-weight-normal` · `--font-weight-medium` · `--font-weight-semibold` · `--font-weight-bold` · `--font-weight-extrabold` · `--tracking-tight` · `--tracking-normal` · `--tracking-wide`

### 13. Motion（动效）

`--duration-instant` · `--duration-fast` · `--duration-normal` · `--duration-moderate` · `--duration-slow` · `--duration-glacial` · `--ease-standard` · `--ease-decelerate` · `--ease-accelerate` · `--ease-spring` · `--ease-popup`

### 14. Opacity（透明度）

`--opacity-disabled` · `--opacity-muted` · `--opacity-pressed` · `--opacity-hover-emphasis` · `--opacity-ambient` · `--opacity-overlay`

### 15. Layout（布局尺寸）

`--size-sidebar-w` · `--size-player-h` · `--size-drawer-w` · `--size-modal-w` · `--size-modal-w-sm` · `--size-topbar-h`

### 16. Control Sizes（控件尺寸）

`--size-control-sm` · `--size-control-md` · `--size-control-lg`

### 17. Spacing（间距）

`--space-0` · `--space-0-5` · `--space-1` · `--space-1-5` · `--space-2` · `--space-3` · `--space-4` · `--space-5` · `--space-6` · `--space-7` · `--space-8` · `--space-10` · `--space-12` · `--space-16` · `--space-20`

### 18. Icon Sizes（图标尺寸）

`--icon-xs` · `--icon-sm` · `--icon-md` · `--icon-lg` · `--icon-xl` · `--icon-2xl`
