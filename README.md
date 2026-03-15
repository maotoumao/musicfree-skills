[English](./README_EN.md) | **中文**

# MusicFree Skills

为 [MusicFree](https://github.com/maotoumao/MusicFree) 音乐播放器打造的 AI Agent Skills 合集。Skills 是打包好的指令与参考文档，用于扩展 AI 编程助手的能力。

Skills 遵循 [Agent Skills](https://agentskills.io/) 格式。

## 可用 Skills

### musicfree-plugin-dev

从零开始开发 MusicFree 插件的完整指南。AI 驱动站点分析、数据对接、代码生成与自动化测试，全流程引导。

适用场景：

- 为 MusicFree 编写/创建音乐源插件
- 将某个音乐网站或 API 适配为 MusicFree 插件
- 分析网站公开接口并生成数据对接代码

涵盖能力：

- 插件协议与完整方法签名（搜索、播放、歌词、歌单、排行榜等 14 种方法）
- 沙箱环境与内置模块（axios / cheerio / crypto-js / dayjs 等）
- 站点分析方法论（静态 HTML 解析 / 公开 API 对接 / Playwright 网络捕获）
- 媒体类型定义（IMusicItem / IAlbumItem / IArtistItem / IMusicSheetItem 等）
- 自动化测试与迭代修复流程

### musicfree-themepack-dev

从零开始开发 MusicFree 桌面版主题包的完整指南。AI 提供色彩方案建议，用户视觉确认，协作完成主题设计。

适用场景：

- 为 MusicFree 桌面版创建/设计自定义主题
- 根据参考图、色彩、风格关键词生成主题包
- 制作包含动态效果（iframe 动画）的高级主题

涵盖能力：

- CSS 变量体系（150+ 语义化变量的完整覆盖规则）
- 色彩设计范式（暗色 / 浅色 / AMOLED 纯黑 / 赛博朋克等）
- 静态主题与动态 iframe 主题开发
- 资源优化与打包测试流程
- 主题市场提交指南（标签、校验、PR 流程）

## 安装

```bash
npx skills add maotoumao/musicfree-skills
```

## 使用

安装后 Skills 自动生效。当 AI 检测到相关任务时会自动调用对应 Skill。

示例提示词：

```
帮我写一个 MusicFree 插件，数据来源是 https://example.com
```

```
帮我做一个赛博朋克风格的 MusicFree 桌面版主题
```

```
我有一个音乐 API 文档，帮我适配成 MusicFree 插件
```

## Skill 结构

每个 Skill 包含：

- `SKILL.md` — AI Agent 的执行指令
- `references/` — 支撑文档（协议定义、变量清单、开发手册等）

```
skills/
├── musicfree-plugin-dev/
│   ├── SKILL.md
│   └── references/
│       ├── media-types.md
│       ├── plugin-protocol.md
│       └── site-analysis-playbook.md
└── musicfree-themepack-dev/
    ├── SKILL.md
    └── references/
        ├── color-paradigms.md
        ├── css-variable-reference.md
        ├── iframe-guide.md
        └── packaging-checklist.md
```

## 许可证

AGPL-3.0
