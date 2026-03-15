# iframe 动态背景开发指南

MusicFree 支持通过 iframe 加载 HTML 页面作为全屏动态背景。适合实现粒子效果、动态渐变等动画效果。

## 工作原理

1. 主题包的 `config.json` 中声明 `"iframe": { "app": "@/iframes/app.html" }`
2. 应用在主窗口创建一个全屏 `<iframe>` 元素，覆盖在 body 背景之上、UI 内容之下
3. iframe 带有 `sandbox` 属性，运行在隔离环境中
4. 应用启动时，如果有 `blurHash` 字段，会先渲染一个低像素模糊占位图，iframe 加载完成后淡入替换

## 文件结构

```
my-theme/
├── config.json
├── index.css
└── iframes/
    ├── app.html          # 入口文件
    ├── script.js         # 可选 — 分离的 JS
    └── assets/           # 可选 — 动画资源
```

## 安全约束

iframe 运行在 `sandbox` 环境中，有以下限制：

- **不能**访问主页面 DOM
- **不能**发送网络请求（fetch / XMLHttpRequest 受限）
- **不能**使用 localStorage / sessionStorage
- **可以**执行 JavaScript（允许 `allow-scripts`）
- **可以**使用 Canvas 2D / WebGL
- **可以**使用 CSS 动画和 requestAnimationFrame

## 性能要求

动态背景在应用运行期间**持续渲染**，必须严格控制性能开销：

| 指标 | 要求 |
|------|------|
| 空闲 CPU 占用 | < 3% |
| 帧率 | 建议锁定 30fps（背景不需要 60fps 的流畅度） |
| 内存占用 | < 50 MB |
| GPU 层 | 避免过多合成层 |

**性能优化技巧：**
- 使用 `requestAnimationFrame` + 帧率节流，而非 `setInterval`
- Canvas 绑定时使用合适的尺寸，不要用超过屏幕分辨率的 canvas
- 粒子数量控制在 100 个以内
- 避免每帧创建新对象（预分配 + 对象池）
- CSS 动画优先使用 `transform` 和 `opacity`（触发 GPU 加速，不触发 reflow）
- 页面不可见时（`visibilitychange` 事件）暂停动画

## 常见模式

### 模式 1：动态渐变

纯 CSS 实现，GPU 友好，零 JS 开销。

```html
<!DOCTYPE html>
<html>
<head>
<style>
    * { margin: 0; padding: 0; }
    body {
        width: 100vw;
        height: 100vh;
        overflow: hidden;
        background: linear-gradient(-45deg, #0f0c29, #302b63, #24243e, #0f0c29);
        background-size: 400% 400%;
        animation: gradientShift 15s ease infinite;
    }
    @keyframes gradientShift {
        0%   { background-position: 0% 50%; }
        50%  { background-position: 100% 50%; }
        100% { background-position: 0% 50%; }
    }
</style>
</head>
<body></body>
</html>
```

**要点：**
- 渐变色应与主题 CSS 中的配色协调
- `background-size: 400% 400%` 让渐变有足够的移动空间
- 动画时长建议 10-20 秒（太快会分散注意力）

### 模式 2：粒子效果

Canvas 2D 实现，性能可控。

```html
<!DOCTYPE html>
<html>
<head>
<style>
    * { margin: 0; padding: 0; }
    body { overflow: hidden; background: transparent; }
    canvas { display: block; }
</style>
</head>
<body>
<canvas id="c"></canvas>
<script>
const canvas = document.getElementById('c');
const ctx = canvas.getContext('2d');

// 响应窗口大小变化
function resize() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
}
window.addEventListener('resize', resize);
resize();

// 预分配粒子数组
const PARTICLE_COUNT = 60;
const particles = Array.from({ length: PARTICLE_COUNT }, () => ({
    x: Math.random() * canvas.width,
    y: Math.random() * canvas.height,
    vx: (Math.random() - 0.5) * 0.5,
    vy: (Math.random() - 0.5) * 0.5,
    r: Math.random() * 2 + 1,
    alpha: Math.random() * 0.5 + 0.1
}));

// 帧率控制：30fps
const FRAME_INTERVAL = 1000 / 30;
let lastFrameTime = 0;

function draw(timestamp) {
    requestAnimationFrame(draw);

    if (timestamp - lastFrameTime < FRAME_INTERVAL) return;
    lastFrameTime = timestamp;

    ctx.clearRect(0, 0, canvas.width, canvas.height);

    for (const p of particles) {
        p.x += p.vx;
        p.y += p.vy;

        // 边界循环
        if (p.x < 0) p.x = canvas.width;
        if (p.x > canvas.width) p.x = 0;
        if (p.y < 0) p.y = canvas.height;
        if (p.y > canvas.height) p.y = 0;

        ctx.beginPath();
        ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(255, 255, 255, ${p.alpha})`;
        ctx.fill();
    }
}

requestAnimationFrame(draw);

// 页面不可见时暂停
document.addEventListener('visibilitychange', () => {
    if (document.hidden) {
        lastFrameTime = Infinity; // 暂停渲染
    } else {
        lastFrameTime = 0; // 恢复渲染
    }
});
</script>
</body>
</html>
```

**要点：**
- 粒子颜色应与主题配色协调（可改为品牌色的低透明度版本）
- `background: transparent` 让 app 底层的 CSS 背景色/渐变透出
- 帧率节流避免不必要的 CPU 开销
- `visibilitychange` 暂停避免后台浪费资源

## blurHash 占位

如果 iframe 内容加载需要时间（尤其是包含较大 JS 或资源的情况），建议在 `config.json` 中提供 `blurHash` 字段。

`blurHash` 是一种将图片编码为短字符串的格式，应用会在 iframe 加载期间将其解码为一个低分辨率模糊占位图，实现平滑过渡。

**生成方法：**

1. 准备一张能代表动态背景整体色调的截图
2. 使用 `blurhash` npm 包生成：

```bash
npm install blurhash sharp
```

```js
const { encode } = require('blurhash');
const sharp = require('sharp');

async function generateBlurHash(imagePath) {
    const { data, info } = await sharp(imagePath)
        .raw()
        .ensureAlpha()
        .resize(32, 32, { fit: 'inside' })
        .toBuffer({ resolveWithObject: true });

    const hash = encode(
        new Uint8ClampedArray(data),
        info.width,
        info.height,
        4, // x components
        3  // y components
    );
    console.log('blurHash:', hash);
}

generateBlurHash('screenshot.png');
```

3. 将生成的字符串填入 config.json 的 `blurHash` 字段

**注意：** 纯 CSS 主题（无 iframe）不需要 blurHash。

## 与主题 CSS 的配合

iframe 背景位于 CSS 背景之上、UI 内容之下。设计时需要注意：

- 如果 iframe 背景本身有颜色，`--bg-image` 可设为 `none`（避免两层背景叠加产生颜色混乱）
- 如果 iframe 背景是半透明粒子效果，底层的 `--color-bg-base` 和 `--bg-image` 仍会透出，可以利用这种叠加效果
- 侧边栏/播放器栏的半透明背景（`--color-bg-sidebar` 等）会让 iframe 背景若隐若现地透出，这是 MusicFree 玻璃态设计的一部分

## 体积控制

| 约束项 | 限制 |
|--------|------|
| iframe HTML + 内联 JS/CSS | 建议 ≤ 500 KB |
| iframe 引用的额外资源 | 计入主题包总体积（≤ 10 MB） |

**优化建议：**
- 优先使用内联 JS/CSS，减少文件数
- 粒子效果不需要外部库，原生 Canvas API 即可
- 动态渐变优先用纯 CSS 实现
- 避免引入大型框架（Three.js 等）——背景效果不需要这些
