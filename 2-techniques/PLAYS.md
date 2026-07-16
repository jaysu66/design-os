# PLAYS — 9 类交互技法卡

> 用途:RECIPE.md 定完档位后,按类查「效果/原理/库/抄哪里」。全部 URL 复制自 research/03-interaction-techniques.md(2026-07-07);「未验证」沿用原报告标注。

## 1 滚动叙事(Scrollytelling / pin & scrub / 进度驱动)

【效果】元素被钉住(pinned)不动,内容/图表/相机随滚动像擦洗磁带(scrub)一样正反向推进,章节无缝过渡。
【原理】pin = 滚动到触发点把元素 `position:fixed`(或 transform 抵消位移)冻结;scrub = 滚动位置线性映射到 timeline playhead,`scrub:0.5` 让 playhead 用 0.5s 缓动追上滚动条;本质是 `scrollY/sectionHeight` 归一化进度驱动任意属性。原生等价物见第 7 类(合成线程零 JS)。常见坑:pin 场景 `start:"top top"` 而非 `"top center"`(taste-skill 生产实测)。
【库】GSAP ScrollTrigger——事实标准,scrub/pin/snap/toggleActions 一把梭 <https://gsap.com/docs/v3/Plugins/ScrollTrigger/>;Lenis——平滑滚动引擎,喂统一 scroll 值 <https://www.lenis.dev/>
【抄哪里】
- Lenis 源码(MIT,darkroom.engineering,14k★)<https://github.com/darkroomengineering/lenis>
- Codrops 无限视差网格教程 <https://tympanus.net/codrops/2025/06/11/building-an-infinite-parallax-grid-with-gsap-and-seamless-tiling/>
- Codrops GSAP+Lenis 无限滚动 <https://tympanus.net/codrops/2026/05/28/the-never-ending-story-building-a-seamless-infinite-scroll-experience-with-gsap-lenis/>
- Codrops 电影级 3D scroll case study <https://tympanus.net/codrops/2025/11/19/how-to-build-cinematic-3d-scroll-experiences-with-gsap/>
- Osmo 页面/滚动课(35 个 Awwwards 站模板,付费含免费样例)<https://www.osmo.supply/>
- 案例:Apple AirPods Pro <https://www.awwwards.com/inspiration/product-scroll-triggered-animation-apple-airpods-pro> · Igloo Inc(SOTD)<https://www.awwwards.com/sites/igloo-inc>

## 2 WebGL / 3D / Shader(粒子 / 流体 / 噪声渐变 / 3D 产品 / hover 扭曲)

【效果】噪声流动的渐变背景(Stripe/Linear/Vercel 风)、鼠标搅动流体、图片 hover 液态扭曲/毛刺/放大镜膨胀、3D 产品可旋转、粒子跟随指针。
【原理】一块全屏或贴合 DOM 的 plane geometry,像素颜色由 fragment shader(GLSL)逐帧计算。噪声渐变 = simplex/perlin noise 采样 + 时间 uniform 推进 + 颜色 mix;图片扭曲 = `<img>` 当纹理传给 shader,用 `uMouse`/`uHover`/`uVelocity` uniform 位移 UV 或顶点(bulge=径向位移 UV,ripple=正弦波,glitch=分通道偏移);DOM↔WebGL 同步 = 读 `getBoundingClientRect()` 设 plane 位置,让效果盖在真实布局上(curtains.js 核心卖点)。
【库】Three.js——WebGL 事实标准,重但全能;React Three Fiber(R3F)——Three 的 React 声明式封装;drei——R3F 官方 helper,`MeshTransmissionMaterial`(玻璃折射/色散)、`Environment`(HDRI)开箱即用 <https://github.com/pmndrs/drei>;OGL——极小裸 WebGL,单效果/包体敏感;curtains.js——HTML img/video 转 WebGL 纹理平面贴合 DOM(后继 gpu-curtains 走 WebGPU)<https://www.curtainsjs.com/>;ShaderGradient——动画渐变生成器,出 React/Figma/Framer <https://shadergradient.co/>;whatamesh——Stripe 主页 mesh gradient 开源实现 <https://meshgradient.com/>
【抄哪里】
- Olivier Larose R3F/shader 系列(bulge/ripple/wave/glass,完整代码)<https://blog.olivierlarose.com/tutorials/bulge-effect>
- Maxime Heckel「The Study of Shaders with R3F」<https://blog.maximeheckel.com/posts/the-study-of-shaders-with-react-three-fiber/>
- Codrops Creative Hub(含 Gooey Image Hover with Three.js)<https://tympanus.net/codrops/hub/>
- drei 源码 MeshTransmissionMaterial.tsx <https://github.com/pmndrs/drei/blob/master/src/core/MeshTransmissionMaterial.tsx>
- ShaderGradient 原始 React 包 <https://github.com/ruucm/shadergradient>
- 案例:Igloo Inc 程序化晶体 <https://www.webgpu.com/showcase/igloo-inc-procedural-crystals/> · Awwwards WebGL 合集 <https://www.awwwards.com/awwwards/collections/webgl-shaders-code/>

## 3 页面过渡(SPA 转场 / 共享元素过渡)

【效果】点链接不白屏:旧页优雅淡出/滑走、新页滑入;某张图/标题在两页间「飞」到新位置(共享元素);全屏遮罩擦除转场。
【原理】原生 View Transitions API:`document.startViewTransition(cb)` 对旧/新 DOM 各截一张快照,浏览器自动做硬件加速交叉淡化;`view-transition-name` 标同一元素 → 自动 morph 位置/尺寸。分同文档(SPA)与跨文档(MPA,纯 CSS `@view-transition{navigation:auto}`)。库方案(前 API 时代/需精细控制):Barba.js/Swup 拦截导航 → ajax 取新页 → GSAP 做进出场时间线;或 FLIP 手动算首末位置差补间。
【库】View Transitions API(原生)——2026 现状:同文档 Chromium 111+/Edge/Opera、Safari 18+、Firefox 144+;跨文档 Chromium + Safari 18.2+ <https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API>;Barba.js——经典 SPA 转场路由器,配 GSAP;Motion(Framer Motion)——React 里 `layoutId` 共享元素、`AnimatePresence` 进出场;Astro / Next 内置封装(未验证具体版本行为)。
【抄哪里】
- Chrome 官方指南(含 SPA 无框架实现)<https://developer.chrome.com/docs/web-platform/view-transitions>
- DebugBear「SPA without a framework」<https://www.debugbear.com/blog/view-transitions-spa-without-framework>
- CSS-Tricks 跨文档 View Transitions 的坑 <https://css-tricks.com/cross-document-view-transitions-part-1/>
- Osmo 页面过渡课(BarbaJS + GSAP)<https://www.osmo.supply/product/page-transition-course>

## 4 文字动效(SplitText / 物理掉落 / 变量字体 / 滚动速度文字)

【效果】标题按字符/单词交错浮现(stagger)、滚动时字母碎裂掉出视口、拖动字母其 font-weight 弹性传播到邻居、scramble 乱码归位、滚动越快文字越倾斜/拉伸。
【原理】拆分 = 把文本 DOM 拆成逐字符/逐词 `<span>`(SplitText 干这个),对每个 span 单独 stagger;物理掉落 = ScrollTrigger + Physics2DPlugin/InertiaPlugin 给字符初速度受「重力」;变量字体 = 动画驱动 `font-variation-settings: 'wght' N` 等可变轴;速度驱动 = 读 ScrollTrigger `getVelocity()` 映射到 skew/scale。
【库】GSAP SplitText——拆字/词/行,GSAP 3 已 100% 免费(Webflow 赞助)<https://gsap.com/docs/v3/Plugins/SplitText/>;GSAP ScrambleText / Physics2D / Inertia——乱码归位、物理、惯性;Motion——React stagger children;可变字体——原生 CSS `font-variation-settings` + transition,零库。
【抄哪里】
- GSAP SplitText 官方 demo <https://gsapdemos.com/plugins/text/splittext>
- Codrops SplitText→MorphSVG 5 个 demo <https://tympanus.net/codrops/2025/05/14/from-splittext-to-morphsvg-5-creative-demos-using-free-gsap-plugins/>
- Codrops 滚动驱动双列波浪文字 <https://tympanus.net/codrops/2026/01/15/building-a-scroll-driven-dual-wave-text-animation-with-gsap/>
- freefrontend 20+ SplitText 示例(可看源码)<https://freefrontend.com/split-text-js/>
- Good Fella Lab 2026 SplitText 实操指南 <https://lab.good-fella.com/blog/gsap-text-animation-splittext-guide>
- Frontend.horse GSAP 动画技法合集 <https://frontend.horse/articles/amazing-animation-techniques-with-gsap/>

## 5 微交互(magnetic 按钮 / 自定义光标 / 点击涟漪 / 物理弹簧)

【效果】按钮在 hover 半径内被指针「吸」过去、松开弹回;圆形自定义光标带阻尼滞后跟随、悬停链接膨胀成「View」徽章;点击处扩散涟漪;拖拽后弹簧回位。
【原理】magnetic = mousemove 算指针到元素中心的偏移向量 × 系数 → `transform: translate(dx*k, dy*k)`,离开回 0;低延迟跟随 = 每帧把光标位置向目标 lerp 一小步(阻尼滞后)——GSAP 用 `quickTo`(预编译高频 setter),Motion 用 `useMotionValue` + `useSpring`(不触发 React re-render);弹簧 = 用弹簧方程(stiffness/damping/mass)解算而非固定曲线 → 物理真实过冲回弹;涟漪 = 点击点生成从 0 放大 + 淡出的圆(伪元素或 span)。
【库】Motion(Framer Motion)——`useSpring`/`useMotionValue`/gesture(whileHover/whileTap/drag)声明式弹簧 <https://motion.dev/docs/cursor>;GSAP quickTo——命令式高频指针跟随;Motion+ Cursor——官方成品自定义光标(磁吸/贴合 UI)<https://motion.dev/magazine/introducing-magnetic-cursors-in-motion-cursor>
【抄哪里】
- Olivier Larose magnetic 按钮(GSAP vs Framer Motion 对照)<https://blog.olivierlarose.com/tutorials/magnetic-button>
- Olivier Larose sticky cursor(三角函数跟随)<https://blog.olivierlarose.com/tutorials/sticky-cursor>
- Olivier Larose mask cursor 等微交互全系列 <https://blog.olivierlarose.com/tutorials/mask-cursor-effect>
- GSAP 自定义光标 + 方向感知旋转(Vanilla JS,MIT)<https://github.com/jmarellanes/gsap__change-cursor-hover--01>
- Framer Motion gestures 文档 <https://www.framer.com/motion/gestures/>

## 6 布局花活(bento / 横向滚动 / 破格网格 / sticky 叙事 / 无限 marquee / mouse trail)

【效果】大小不一卡片拼贴(bento);纵向滚动却横向推进的展区;故意错位/重叠的破格网格;左固定右滚的 sticky 叙事;永不停的跑马灯;指针划过甩出一串图片轨迹。
【原理】bento = CSS Grid `grid-template-areas` 或不等 `span` + `gap`,纯布局配 hover/进场;横向滚动区 = pin 一个 section,把纵向滚动进度映射为内部容器 `translateX`(ScrollTrigger 经典配方);无限 marquee = 两份相同内容首尾相接 `translateX` 循环 + 取模,或纯 CSS `@keyframes` + 复制 DOM,进阶版沿 SVG path(Motion);mouse trail = mousemove 从指针位置克隆图片,逐个 stagger 淡入 + 下落回弹(GSAP);无限网格 = 数学取模无缝 tiling + 视差(scroll/drag 都驱动)。
【库】CSS Grid + `:has()`/container queries——布局本体,零库;GSAP ScrollTrigger——横滚 pin+scrub、mouse trail;Motion——SVG path marquee、拖拽惯性;Splide / Embla / Swiper——现成轮播(未验证:偏工程,创意站常自造)。
【抄哪里】
- Codrops ScrollAnimationsGrid(MIT repo)<https://github.com/codrops/ScrollAnimationsGrid>
- Codrops 无限循环滚动创意 <https://tympanus.net/codrops/2023/01/11/getting-creative-with-infinite-loop-scrolling/>
- Codrops 3D 无限轮播 + 反应式背景渐变 <https://tympanus.net/codrops/2025/11/11/building-a-3d-infinite-carousel-with-reactive-background-gradients/>
- Codrops grid / infinite 标签合集 <https://tympanus.net/codrops/tag/grid/>
- 无限视差网格教程同第 1 类 <https://tympanus.net/codrops/2025/06/11/building-an-infinite-parallax-grid-with-gsap-and-seamless-tiling/>

## 7 新 CSS 能力(scroll-driven / @property / anchor positioning / view transitions / :has / 容器查询)

【效果】无 JS 的滚动进度条/视差/进场;tooltip/下拉自动锚定触发元素且智能翻转;`:has()` 父随子状态变样;容器查询按自身宽度自适应(而非视口)。
【原理】scroll-driven animations = 两种时间线:`scroll()`(滚动容器 0→100%)与 `view()`(元素进出视口 0→100%),把 `@keyframes` 的 `animation-timeline` 指向它,`animation-range: entry/exit/cover/contain` 控生效区间,**跑合成线程,主线程再忙也 60fps**;命名时间线 `view-timeline:--t` 定义、`animation-timeline:--t` 消费,跨非后代元素用 `timeline-scope`;约 85% 支持,Firefox 需 flag,`@supports (animation-timeline: view())` 兜底。`@property` = 注册自定义属性类型(`<angle>`/`<color>`)让渐变角度、conic 色标等能被平滑插值。anchor positioning = `anchor-name` + `position-anchor` + `position-area` 锚弹层,`position-try` 定溢出备选位;Chrome 130+/Safari 18+/Firefox 130+。`:has()` = 父/前序选择器纯 CSS 联动。容器查询 = `container-type` + `@container`。
【库】全部原生 CSS,零库;polyfill 存在但 linked timeline 等特性不全。生产环境与 GSAP 并存:签名级复杂动画 GSAP,轻量进场/视差交给原生。
【抄哪里】
- Josh W. Comeau「Scroll-Driven Animations」(机制讲最透)<https://www.joshwcomeau.com/animation/scroll-driven-animations/>
- MDN scroll-driven animations 指南 <https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Scroll-driven_animations>
- MDN anchor positioning「Using」<https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Anchor_positioning/Using>
- Chrome「CSS Wrapped 2025」演示合集 <https://chrome.dev/css-wrapped-2025/>
- Chrome blog scroll-triggered animations(Chrome 145 新增基于偏移触发)<https://developer.chrome.com/blog/scroll-triggered-animations>
- Bram.us 用 View Transitions 动画化 position-area <https://www.bram.us/2026/03/02/animating-css-position-area-with-view-transitions/>
- Interop 2026 进展 <https://css-tricks.com/interop-2026/>

## 8 组件库生态(agent 可用性视角)

【效果】不手写动效组件,直接从库拉现成的 spotlight/3D card/marquee/文字动效,分钟级出「像有专职设计团队」的区块。
【原理】选型三问:依赖多重?许可能商用?agent 能否直接拿到源码(copy-paste / CLI / MCP / llms.txt)?分发形态决定 agent 可用性。
【库】
| 库 | 定位 | 依赖 | 许可 | 消费方式 | agent 可用性 |
|---|---|---|---|---|---|
| Aceternity UI | 最「炫」的 marketing 动效(200+) | Framer Motion + Tailwind(强制) | MIT | 复制粘贴 | 复制源码即可;无官方 MCP/llms.txt(未验证) |
| Magic UI | landing 动效块,克制些(150+) | Framer Motion + Tailwind | MIT | shadcn CLI | CLI 拉取,shadcn 生态 |
| React Bits | 上升最快(JS Rising Stars 2025 #2,110+) | CSS 为主,按需 GSAP/Three/Matter | **MIT + Commons Clause(禁转售/再分发组件)** | shadcn/jsrepo CLI | 模块化按需依赖,包体友好 |
| 21st.dev | 社区组件市场 + Magic MCP | 视组件 | 视组件 | **MCP(编辑器内生成)** | 最适合 agent,收录 Aceternity 等 |
| UI-Layout | 类 Aceternity 创意组件 | 未验证 | 未验证 | 复制粘贴(未验证) | 未验证 |
| Motion Primitives | Motion 官方向动效原语 | Motion + Tailwind | 未验证 | 复制/CLI(未验证) | 与 shadcn 生态叠加 |
| shadcn/ui | 应用 UI 底座(非动效) | Radix + Tailwind | MIT | CLI + 注册表 | 注册表 + MCP 生态,agent 友好度高 |
【抄哪里】2026 通行搭法:shadcn/ui 底座 → Magic UI 加动效 → 签名区块上 Aceternity <https://www.pkgpulse.com/guides/react-bits-animated-components-2026>;优先 21st.dev Magic MCP <https://21st.dev/>;要「拥有源码、无供应链风险」选 React Bits/Aceternity 复制粘贴(注意 React Bits Commons Clause,详见 ../3-references/ENGINE.md)。

## 9 业界学习源(谁开源了什么 / 按什么顺序学)

【效果】持续跟住 Awwwards 级技法演进:顶级工作室把自用工具直接开源,教程站配可跑 demo + repo,可长期「拆迁」。
【原理】学习优先级:① Codrops(案例+demo+GitHub repo,第一学习源)→ ② GSAP 官方 docs+showcase → ③ Awwwards case study/collections(看获奖站怎么拆)→ ④ Olivier Larose(R3F/shader/微交互带完整代码)→ ⑤ Maxime Heckel(shader 深度)/ Josh Comeau(CSS 机制)。
【库】darkroom.engineering(前 Studio Freight)——开源最狠,46+ repo:Lenis(14k★)、Tempus(全站共用一个 rAF,320★)、Hamo(数学 hook,307★)、Satus(Next.js 内容站 starter,964★)、Aniso(ASCII)、Spargo(WebGL GPU 抖动);Basement Studio——49 repo,多个 SOTD;Locomotive——Locomotive Scroll(经典,现多被 Lenis 取代);Active Theory——极致沉浸 WebGL(偏闭源,看作品学思路);Bruno Simon——Three.js Journey 作者,教学向。
【抄哪里】
- Codrops Creative Hub <https://tympanus.net/codrops/hub/>
- darkroom.engineering GitHub <https://github.com/darkroomengineering>
- Basement Studio GitHub <https://github.com/basementstudio>
- GSAP 官方 <https://gsap.com/>
- Awwwards WebGL Shaders + Code 合集 <https://www.awwwards.com/awwwards/collections/webgl-shaders-code/>
- Olivier Larose blog <https://blog.olivierlarose.com/>
- Locomotive 获奖案例 <https://www.awwwards.com/sites/locomotive-1>
