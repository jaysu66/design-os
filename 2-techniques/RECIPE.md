# RECIPE — 交互技法四层配方与 ABC 分级

> 用途:接到 UI/网页任务,先用本文件定「上什么技术、到什么档位」,再去 PLAYS.md 查每类的具体抄法。
> 来源:design-os/research/03-interaction-techniques.md(2026-07-07)。

## 一、四层配方(Awwwards 级站点的通用分层)

大多数高级感站点 = 平滑滚动引擎 + 动画编排库 + WebGL 图层 + 微交互 四层叠加:

| 层 | 干什么 | 默认选型 | 备选 |
|---|---|---|---|
| 1 平滑滚动 | 接管原生滚动,给下面两层喂统一 scroll 进度 | Lenis | 不上(见判据) |
| 2 动画编排 | 把 scroll 进度映射到任意属性/时间线 | GSAP + ScrollTrigger | 原生 CSS scroll-driven animations |
| 3 WebGL 图层 | 把 DOM 位置同步到 WebGL 平面做 shader 效果 | Three.js / R3F | OGL / curtains.js / 现成生成器 |
| 4 微交互 | 指针跟随 / 弹簧 / hover 手感 | Motion(Framer Motion) | GSAP quickTo |

### 每层选型判据

**1 平滑滚动(Lenis)**
- 上:桌面端叙事/营销/作品集站,或下层要接 ScrollTrigger/WebGL(需要统一 scroll 值)。一行接管,立刻「贵」。
- 不上:文档/后台/表单密集应用——保留原生滚动手感与可访问性。
- 选 Lenis(darkroom.engineering,MIT,14k★),业界默认,连 Locomotive 都在用;Locomotive Scroll 视为已被取代。

**2 动画编排(GSAP vs 原生 CSS)**
- 轻量进场/视差/滚动进度条 → 原生 CSS scroll-driven animations:零 JS、跑合成线程(主线程再忙也 60fps);约 85% 支持,Firefox 需 flag,必须 `@supports (animation-timeline: view())` 兜底。
- pin / scrub / snap / 多段 timeline / 滚动速度联动等签名级编排 → GSAP ScrollTrigger(事实标准)。
- 生产环境两者并存:复杂动画走 GSAP,轻量交给原生省 JS。

**3 WebGL 上不上(判据从低到高)**
- 只要「高级感背景」→ 不写 shader,用现成生成器:ShaderGradient(URL 即配置,出 React 组件)/ whatamesh(Stripe 风 mesh gradient)。A 档成本。
- 单一效果(图片 hover 扭曲 / 单块 shader)→ OGL(极小裸 WebGL,包体敏感场景)或 curtains.js(DOM↔WebGL 同步)。B 档。
- 内容本体是 3D(产品可旋转 / 沉浸场景 / 流体粒子)→ Three.js / R3F + drei。C 档,专项立项,性能/移动端/可访问性都是硬仗。
- 一句话:**WebGL 只在「效果就是内容」时才上;纯装饰需求一律用生成器或 CSS 替代。**

**4 微交互**
- React 项目 → Motion:`useSpring`/`useMotionValue` 不触发 re-render,声明式弹簧(stiffness/damping/mass)。
- 非 React / 高频指针跟随 → GSAP `quickTo`(预编译高频 setter,极低开销)。

## 二、ABC 难度×收益分级

评分口径:难度 = 实现+调试+可访问性成本;收益 = 用户感知「高级感」增量。

### A 档 — 低成本·高感知:默认掌握,几乎每站都值得上

| 技法 | 为什么低成本 | 默认库 |
|---|---|---|
| Lenis 平滑滚动 | 一行接管 | Lenis |
| SplitText 逐字进场 | 拆字+stagger 模板化 | GSAP SplitText(GSAP 3 全插件已 100% 免费,Webflow 赞助) |
| Magnetic 按钮 / 阻尼自定义光标 | 十几行数学 | Motion / GSAP quickTo |
| Scroll 进场(fade/slide-up) | 原生 `view()` 零 JS,合成线程 60fps | CSS scroll-driven |
| 无限 marquee | 两份 DOM + 平移取模 | 纯 CSS / GSAP |
| Bento grid + hover | 纯 CSS Grid | CSS Grid |
| 页面过渡(交叉淡化/共享元素) | 原生 API,SPA/MPA 都简单 | View Transitions API |
| Stripe 风网格 / shader 渐变背景 | 现成生成器出 React 组件 | ShaderGradient / whatamesh |

### B 档 — 中成本·高感知:需求匹配时上,不难但要调

- Pin & scrub 滚动叙事(GSAP ScrollTrigger)——配方成熟,要调触发点/scrub 手感。
- 横向滚动展区、图片 mouse trail、无限视差网格——Codrops 有可抄 repo(见 PLAYS.md)。
- 图片 hover 液态/膨胀扭曲(curtains.js / OGL 单 shader)——单效果可控。
- 变量字体交互——需要字体资源 + 轴映射。

### C 档 — 高成本·签名级:按需专项立项,不默认承诺

- 全站 WebGL 沉浸世界(Three.js/R3F + 物理 + 自定义 shader 场景,如 Igloo Inc / Bruno Simon)。
- 流体模拟、粒子系统、程序化几何——GPU 编程 + 数学重。
- 相机路径 × 灯光 × shader 全绑滚动的电影级叙事——编排复杂、调试贵。
- WebGPU 前沿(gpu-curtains 等)——兼容性未铺开,实验性质。

**默认策略**:交付 A 档全套(平滑滚动 + 逐字进场 + 微交互 + 原生 scroll 动画 + shader 渐变背景)就已经「高级」;B 档按页面需求点单;C 档明确标注「签名级、需专项」,不轻易承诺。

## 三、性能红线(硬性,任一违反即返工)

1. **只 animate `transform` / `opacity`**(显隐用 GSAP `autoAlpha`);不动 width/height/top/left 等触发 reflow 的属性;`will-change` 只加在真正在动的元素上。
2. **`backdrop-filter` 全页 ≤5 处**——GPU 大户,超了低端机直接掉帧。
3. **`prefers-reduced-motion` 必须响应**:CSS 用 `@media (prefers-reduced-motion: reduce)`;GSAP 用 `gsap.matchMedia()`;React 用 `useReducedMotion`。降级为瞬时切换,不是删内容。
4. **禁止 `window.addEventListener("scroll")` 驱动动画**——用 ScrollTrigger 或原生 `animation-timeline`。
5. React 中 GSAP 必须走 `useGSAP` / `gsap.context()` + revert cleanup;上线前删 ScrollTrigger `markers`。
6. 批量进场用 `ScrollTrigger.batch()`(替代 N 个独立 trigger / 手写 IntersectionObserver);全站共用一个 rAF(参考 darkroom 的 Tempus)。
