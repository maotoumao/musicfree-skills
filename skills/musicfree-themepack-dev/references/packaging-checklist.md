# 打包、测试与发布检查清单

## 打包前检查

### 文件结构

- [ ] `config.json` 存在
- [ ] `index.css` 存在
- [ ] 所有 `config.json` 中引用的文件（preview / thumb / iframe）实际存在
- [ ] 文件夹命名仅含字母、数字、连字符（`-`）、下划线（`_`）

### config.json 检查

- [ ] `name` 字段已填写
- [ ] `preview` 字段已填写（图片路径或 `#hex` 色值）
- [ ] `author` 字段已填写
- [ ] `version` 字段已填写（语义化版本号如 `0.0.1`）
- [ ] **没有** `id` 字段（ID 由主题市场系统管理）
- [ ] 如有 iframe，`iframe.app` 路径正确指向 HTML 文件
- [ ] 所有 `@/` 路径指向实际存在的文件

### index.css 检查

- [ ] 所有样式写在 `:root { }` 选择器内
- [ ] 只覆盖了应该覆盖的变量（参考 css-variable-reference.md）
- [ ] 没有覆盖禁止类变量（z-index / typography / spacing / layout / motion 等）
- [ ] 浅色主题已覆盖全部 Background / Fill / Text / Border 类变量
- [ ] `@/` 路径引用的资源文件实际存在
- [ ] 没有语法错误（括号、分号完整）

### 资源检查

- [ ] 单张图片 ≤ 500 KB
- [ ] 单个视频 ≤ 5 MB
- [ ] 图片优先使用 WebP 格式
- [ ] 整个主题包 ≤ 10 MB

### iframe 检查（如果有）

- [ ] `iframes/app.html` 可独立打开并正常渲染
- [ ] 不依赖外部网络资源（CDN、在线字体等）
- [ ] 使用了帧率控制（建议 30fps）
- [ ] 实现了 `visibilitychange` 暂停
- [ ] 不使用 localStorage / sessionStorage
- [ ] config.json 中添加了 `"iframe": { "app": "@/iframes/app.html" }`
- [ ] 建议提供 `blurHash` 字段（参考 iframe-guide.md）

## 打包步骤

### 方法 1：命令行打包（推荐）

在主题文件夹的**父目录**执行：

**Windows (PowerShell)：**
```powershell
Compress-Archive -Path "my-theme/*" -DestinationPath "my-theme.zip" -Force
Rename-Item "my-theme.zip" "my-theme.mftheme"
```

**macOS / Linux：**
```bash
cd my-theme
zip -r ../my-theme.mftheme .
```

**注意：** ZIP 内部应直接是 `config.json`、`index.css` 等文件，而非嵌套一层文件夹。

### 方法 2：手动打包

1. 选中主题文件夹内的**所有文件和文件夹**（不要选中外层文件夹本身）
2. 右键 → 压缩为 ZIP
3. 将 `.zip` 扩展名改为 `.mftheme`

## 安装测试

### 测试步骤

1. 打开 MusicFree 桌面版
2. 进入「主题」页面（左侧导航栏）
3. 点击页面右上角的「安装主题包」按钮
4. 选择生成的 `.mftheme` 文件
5. 在主题列表中找到刚安装的主题
6. 点击主题卡片应用

### 验证清单

应用主题后，逐一检查以下区域：

| 区域 | 检查点 |
|------|--------|
| 侧边栏 | 背景色、文字清晰度、选中态 |
| 主内容区 | 背景色、列表文字、hover 效果 |
| 播放器栏 | 背景色、进度条颜色、按钮可见度 |
| 搜索结果 | 文字可读性、标签背景 |
| 弹窗 | 模态框背景、遮罩层、内部文字 |
| 右键菜单 | 浮层背景、文字、悬停色 |
| 设置页面 | 各区块辨识度、开关颜色 |
| 全屏播放器 | 背景色、封面、歌词文字 |
| 队列抽屉 | 抽屉背景、列表文字 |

### 常见问题排查

| 现象 | 可能原因 | 解决方案 |
|------|---------|---------|
| 安装失败 | ZIP 结构不对（多嵌套了一层目录） | 确保 config.json 在 ZIP 根目录 |
| 主题应用无变化 | CSS 语法错误 | 检查 index.css 的括号和分号 |
| 部分区域颜色异常 | 遗漏了必要的变量覆盖 | 参照 css-variable-reference.md 补全 |
| 预览图不显示 | `@/` 路径拼写错误 | 检查 config.json 中的 preview 路径 |
| iframe 背景空白 | HTML 文件路径错误或 JS 报错 | 单独打开 HTML 文件测试 |

## 提交到主题市场（可选）

如果用户希望发布到 MusicFree 主题市场，参考 SKILL.md 阶段 8 的说明，以及 [MusicFreeThemePacks CONTRIBUTING.md](https://github.com/maotoumao/MusicFreeThemePacks/blob/v1/source/CONTRIBUTING.md)。

核心注意事项：
1. Fork [MusicFreeThemePacks](https://github.com/maotoumao/MusicFreeThemePacks) 仓库
2. 基于 `v1/source` 分支创建开发分支
3. 主题放在 `themes/` 目录下
4. config.json 补充 `tags`（1-5 个预定义标签）
5. **不加 `id` 字段**
6. 运行 `npm run validate:theme your-theme-name` 本地校验
7. 向 `v1/source` 分支提交 PR
