# 高级感网页交互技法分类学 — "它们是怎么做到的"

> 面向:给 Codex/Claude 搭建"设计高级感网页"能力库。把美的交互拆成可归纳行为,每种讲清实现路径。
> 结构:每类 = ①代表案例 ②视觉效果 ③实现原理(浏览器技术层) ④主流技术栈/库 ⑤开源可抄来源。
> 日期:2026-07-07。标「未验证」= 未逐一打开验证,凭搜索/常识判断,不作事实断言。

---

## 0. 全局技术地图(先看这张)

大多数 Awwwards 级站点 = **平滑滚动引擎 + 动画时间线库 + WebGL 图层** 三件套叠加:
- **平滑滚动层**:Lenis(接管原生滚动 → 丝滑 + 可编程,给下面两层喂统一的 scroll 进度)。
- **动画编排层**:GSAP + ScrollTrigger(把 scroll 进度映射到任意属性/时间线);或原生 CSS scroll-driven animations(免 JS,跑在合成线程)。
- **视觉图层**:Three.js / R3F / OGL / curtains.js,把 DOM 位置同步到 WebGL 平面做 shader 效果。
- **微交互层**:Framer Motion(Motion) 弹簧 / GSAP quickTo 做低延迟指针跟随。

理解这个分层,下面 9 类只是它的不同切面。

---

## 1. 滚动叙事(Scrollytelling / pin & scrub / 滚动进度驱动)

**①代表案例**
- Apple AirPods Pro 产品页 — 滚动触发的产品序列动画 + 单页滚动导航(WebGL/GLSL) <https://www.awwwards.com/inspiration/product-scroll-triggered-animation-apple-airpods-pro>
- Igloo Inc(SOTD)— 滚动驱动的相机在冰晶场景间漂移 <https://www.awwwards.com/sites/igloo-inc>
- Codrops "Cinematic 3D Scroll" case study — scroll 驱动相机路径/灯光/shader <https://tympanus.net/codrops/2025/11/19/how-to-build-cinematic-3d-scroll-experiences-with-gsap/>

**②视觉效果**
元素被"钉住"(pinned)不动,内容/图表/相机随滚动条像擦洗磁带(scrub)一样正反向推进;文字块滑过固定的可视化;章节之间无缝过渡。

**③实现原理**
- **Pin**:滚动到触发点时把元素 `position:fixed`(或用 transform 抵消滚动位移)冻结,底下内容继续滚。
- **Scrub**:把滚动条位置线性映射到动画 timeline 的 playhead(进度 0→1),`scrub:0.5` 让 playhead 用 0.5s 缓动追上滚动条(避免生硬)。
- **进度驱动**:本质是 `scrollY / (sectionHeight)` → 归一化进度 → 驱动任意属性。
- 原生等价物见第 7 节(`animation-timeline: scroll()`),跑在合成线程,零 JS。

**④主流技术栈/库**
- **GSAP ScrollTrigger** — 事实标准,scrub/pin/snap/toggleActions 一把梭 <https://gsap.com/docs/v3/Plugins/ScrollTrigger/>
- **Lenis** — 平滑滚动引擎,给 ScrollTrigger/WebGL 喂统一 scroll 值,业界默认(连 Locomotive 都在用) <https://www.lenis.dev/>
- **原生 CSS scroll-driven animations** — 无库、跑合成线程(见第 7 节)

**⑤开源可抄来源**
- Lenis 源码(MIT,darkroom.engineering,14k★) <https://github.com/darkroomengineering/lenis>
- Codrops 无限视差网格教程 <https://tympanus.net/codrops/2025/06/11/building-an-infinite-parallax-grid-with-gsap-and-seamless-tiling/>
- Codrops GSAP+Lenis 无限滚动 <https://tympanus.net/codrops/2026/05/28/the-never-ending-story-building-a-seamless-infinite-scroll-experience-with-gsap-lenis/>
- Osmo 页面/滚动课(35 个 Awwwards 站的模板,付费但含免费样例) <https://www.osmo.supply/>

---

## 2. WebGL / 3D / Shader(粒子 / 流体 / 噪声渐变 / 3D 产品 / 图片 hover 扭曲)

**①代表案例**
- Bruno Simon 作品集 — Three.js + Cannon.js 开车逛 3D 世界(Awwwards SOTM) <https://www.awwwards.com/sites/igloo-inc>(同榜单参考)
- Igloo Inc — 程序化晶体生长 + shader UI + 体数据 <https://www.webgpu.com/showcase/igloo-inc-procedural-crystals/>
- Awwwards "WebGL Shaders + Code" 合集 <https://www.awwwards.com/awwwards/collections/webgl-shaders-code/>

**②视觉效果**
噪声流动的渐变背景(Stripe/Linear/Vercel 风)、鼠标搅动的流体、图片 hover 时液态扭曲/毛刺/放大镜膨胀、3D 产品可旋转、粒子跟随指针。

**③实现原理**
- 一块全屏或贴合 DOM 的 `plane` geometry,像素颜色由 **fragment shader(GLSL)** 逐帧计算。
- **噪声渐变** = simplex/perlin noise 采样 + 时间 uniform 推进 + 颜色 mix。
- **图片扭曲** = 把 `<img>` 当纹理传给 shader,用 `uMouse`/`uHover`/`uVelocity` uniform 位移 UV 或顶点(bulge=径向位移 UV,ripple=正弦波,glitch=分通道偏移)。
- **DOM↔WebGL 同步**:读 DOM 元素 `getBoundingClientRect()` → 设 plane 位置,让 shader 效果盖在真实布局上(curtains.js 的核心卖点)。

**④主流技术栈/库**
- **Three.js** — WebGL 事实标准,重但全能
- **React Three Fiber (R3F)** — Three.js 的 React 声明式封装 <https://blog.maximeheckel.com/posts/the-study-of-shaders-with-react-three-fiber/>
- **drei** — R3F 官方 helper 库,`MeshTransmissionMaterial`(玻璃折射/色散)、`Environment`(HDRI 环境光)开箱即用 <https://github.com/pmndrs/drei>
- **OGL** — 极小的裸 WebGL 库,适合单一 shader 效果、包体敏感场景
- **curtains.js** — 把 HTML img/video/canvas 转成 WebGL 纹理平面、贴合 DOM 位置(作者 Martin Laxenaire);后继 gpu-curtains 走 WebGPU <https://www.curtainsjs.com/>
- **ShaderGradient** — 动画渐变生成器,输出 React 组件/Figma/Framer,URL 即配置 <https://shadergradient.co/>
- **Stripe Mesh Gradient(whatamesh)** — Stripe 主页那种网格渐变的开源实现(mesh gradient) <https://meshgradient.com/>

**⑤开源可抄来源**
- Olivier Larose R3F/shader 系列教程(bulge/ripple/wave/glass,含完整代码) <https://blog.olivierlarose.com/tutorials/bulge-effect>
- Codrops "Gooey Image Hover with Three.js" 及 Creative Hub <https://tympanus.net/codrops/hub/>
- drei 源码 `MeshTransmissionMaterial.tsx` <https://github.com/pmndrs/drei/blob/master/src/core/MeshTransmissionMaterial.tsx>
- ShaderGradient 原始 React 包 <https://github.com/ruucm/shadergradient>

---

## 3. 页面过渡(SPA 转场 / 共享元素过渡)

**①代表案例**
- Osmo 页面过渡课 — 基于 BarbaJS + GSAP,声称覆盖 35 个 Awwwards 站 <https://www.osmo.supply/product/page-transition-course>
- (原生)MDN / Chrome 官方 View Transition 演示 <https://developer.chrome.com/docs/web-platform/view-transitions>

**②视觉效果**
点链接不白屏,旧页优雅淡出/滑走、新页滑入;某张图/标题在两页间"飞"到新位置(共享元素);全屏遮罩擦除转场。

**③实现原理**
- **原生 View Transitions API**:`document.startViewTransition(cb)` 对旧/新 DOM 各截一张快照,浏览器自动做硬件加速的交叉淡化;`view-transition-name` 标同一元素 → 自动 morph 位置/尺寸(共享元素)。分**同文档(SPA)**与**跨文档(MPA,纯 CSS 即可 `@view-transition{navigation:auto}`)**两种。
- **库方案(前 API 时代 / 需精细控制)**:Barba.js/Swup 拦截导航 → 阻止默认跳转 → ajax 取新页 → 用 GSAP 做进出场时间线;或 FLIP 手动算首末位置差补间。

**④主流技术栈/库**
- **View Transitions API(原生)** — 2026 现状:Chromium 111+/Edge/Opera、Safari 18+、Firefox 144+ 已支持同文档;跨文档在 Chromium + Safari 18.2+ <https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API>
- **Barba.js** — 经典 SPA 转场路由器(拦截+编排),配 GSAP
- **Framer Motion (Motion) layout / AnimatePresence** — React 里 `layoutId` 做共享元素、`AnimatePresence` 做进出场
- **Astro / Next 内置** — 框架级 View Transitions 封装(未验证具体版本行为)

**⑤开源可抄来源**
- Chrome 官方指南(含 SPA 无框架实现) <https://developer.chrome.com/docs/web-platform/view-transitions>
- DebugBear "SPA without a framework" <https://www.debugbear.com/blog/view-transitions-spa-without-framework>
- CSS-Tricks "跨文档 View Transitions 的坑" <https://css-tricks.com/cross-document-view-transitions-part-1/>

---

## 4. 文字动效(字符逐现 / SplitText / 滚动速度文字 / kinetic typography / 变量字体)

**①代表案例**
- Codrops "SplitText→MorphSVG 5 个 demo" <https://tympanus.net/codrops/2025/05/14/from-splittext-to-morphsvg-5-creative-demos-using-free-gsap-plugins/>
- Codrops "滚动驱动双列波浪文字" <https://tympanus.net/codrops/2026/01/15/building-a-scroll-driven-dual-wave-text-animation-with-gsap/>
- Frontend.horse GSAP 动画技法合集 <https://frontend.horse/articles/amazing-animation-techniques-with-gsap/>

**②视觉效果**
标题按字符/单词交错浮现(stagger)、滚动时字母"碎裂"掉出视口、拖动一个字母其重量(font-weight)弹性传播到邻居、scramble 乱码归位、滚动速度越快文字越倾斜/拉伸。

**③实现原理**
- **拆分**:把一段文本 DOM 拆成逐字符/逐词的 `<span>`(SplitText 干这个),再对每个 span 单独 stagger 动画。
- **物理掉落**:ScrollTrigger + Physics2DPlugin/InertiaPlugin,给字符初速度让它受"重力"掉落/带惯性滑停。
- **变量字体**:动画驱动字体的可变轴(`font-variation-settings: 'wght' N`),weight/宽度/斜度随交互连续变化。
- **速度驱动**:读 ScrollTrigger 的 `getVelocity()` → 映射到 skew/scale。

**④主流技术栈/库**
- **GSAP SplitText** — 拆字/词/行,GSAP 3 已 100% 免费(Webflow 赞助) <https://gsap.com/docs/v3/Plugins/SplitText/>
- **GSAP ScrambleText / Physics2D / Inertia** — 乱码归位、物理掉落、惯性
- **Motion (Framer Motion)** — React 里 stagger children 做逐字浮现
- **可变字体(原生 CSS)** — `font-variation-settings` + transition,零库

**⑤开源可抄来源**
- GSAP SplitText 官方 demo <https://gsapdemos.com/plugins/text/splittext>
- freefrontend 收录 20+ SplitText 示例(可直接看源码) <https://freefrontend.com/split-text-js/>
- Good Fella Lab 2026 SplitText 实操指南 <https://lab.good-fella.com/blog/gsap-text-animation-splittext-guide>

---

## 5. 微交互(magnetic 按钮 / 自定义光标 / hover 艺术 / 点击涟漪 / 物理弹簧)

**①代表案例**
- Olivier Larose "2 种 magnetic 按钮"(GSAP vs Framer Motion 对照) <https://blog.olivierlarose.com/tutorials/magnetic-button>
- Olivier Larose "sticky cursor"(三角函数算跟随) <https://blog.olivierlarose.com/tutorials/sticky-cursor>
- Motion+ Cursor(官方磁吸/分区光标) <https://motion.dev/magazine/introducing-magnetic-cursors-in-motion-cursor>

**②视觉效果**
按钮在 hover 半径内被指针"吸"过去、松开弹回;自定义圆形光标带阻尼滞后跟随、悬停链接时膨胀成"View"徽章;点击处扩散涟漪;拖拽后弹簧回位。

**③实现原理**
- **Magnetic**:监听 mousemove,算指针到元素中心的偏移向量 × 系数 → `transform: translate(dx*k, dy*k)`;离开时回 0。
- **低延迟跟随**:每帧把光标位置向目标 lerp(线性插值)一小步 = 阻尼滞后。GSAP 用 `quickTo`(预编译的高频 setter,比每次 `gsap.to` 省开销);Framer Motion 用 `useMotionValue` + `useSpring`(不触发 React re-render)。
- **弹簧**:用弹簧方程(stiffness/damping/mass)解算而非固定曲线 → 物理真实的过冲回弹。
- **涟漪**:点击点生成一个从 0 放大 + 淡出的圆(伪元素或 span)。

**④主流技术栈/库**
- **Framer Motion (Motion)** — `useSpring`/`useMotionValue`/gesture(whileHover/whileTap/drag),声明式弹簧 <https://motion.dev/docs/cursor>
- **GSAP quickTo** — 命令式高频指针跟随,极低开销
- **Motion+ Cursor** — 官方成品自定义光标(磁吸/贴合 UI)

**⑤开源可抄来源**
- Olivier Larose 微交互全系列(magnetic/sticky cursor/mask cursor,含代码) <https://blog.olivierlarose.com/tutorials/mask-cursor-effect>
- GSAP 自定义光标 + 方向感知旋转(Vanilla JS,MIT) <https://github.com/jmarellanes/gsap__change-cursor-hover--01>
- Framer Motion gestures 文档 <https://www.framer.com/motion/gestures/>

---

## 6. 布局花活(bento grid / 横向滚动 / broken-overlap grid / sticky 叙事 / 无限 marquee / 图片轨迹)

**①代表案例**
- Codrops "无限循环滚动创意" <https://tympanus.net/codrops/2023/01/11/getting-creative-with-infinite-loop-scrolling/>
- Codrops "3D 无限轮播 + 反应式背景渐变" <https://tympanus.net/codrops/2025/11/11/building-a-3d-infinite-carousel-with-reactive-background-gradients/>
- Codrops "图片网格滚动动画"(源码 repo) <https://github.com/codrops/ScrollAnimationsGrid>

**②视觉效果**
大小不一的卡片拼贴(bento);纵向滚动却横向推进的展区(horizontal scroll section);故意错位/重叠的破格网格;左固定右滚的 sticky 叙事;永不停的跑马灯;指针划过甩出一串图片轨迹(mouse trail)。

**③实现原理**
- **bento**:CSS Grid `grid-template-areas` 或不等 `span` + `gap`;纯布局,配 hover/进场动画。
- **横向滚动区**:pin 一个 section,把纵向滚动进度映射为内部容器 `translateX`(GSAP ScrollTrigger 经典配方)。
- **无限 marquee**:两份相同内容首尾相接,`transform: translateX` 循环 + 取模;或纯 CSS `@keyframes` 平移 + 复制 DOM;进阶版让 marquee 跟随 SVG path(Motion)。
- **mouse trail**:mousemove 时从指针位置克隆图片,逐个 stagger 淡入 + 下落回弹(GSAP)。
- **无限网格**:数学取模做无缝 tiling + 视差(scroll/drag 都驱动)。

**④主流技术栈/库**
- **CSS Grid + `:has()`/container queries** — 布局本体,零库
- **GSAP ScrollTrigger** — 横向滚动区 pin+scrub、mouse trail
- **Motion** — SVG path 上的 marquee、拖拽惯性
- **Splide / Embla / Swiper** — 现成轮播/横滚(未验证:偏工程,创意站常自造)

**⑤开源可抄来源**
- Codrops ScrollAnimationsGrid(MIT repo) <https://github.com/codrops/ScrollAnimationsGrid>
- Codrops 无限视差网格教程 <https://tympanus.net/codrops/2025/06/11/building-an-infinite-parallax-grid-with-gsap-and-seamless-tiling/>
- Codrops grid / infinite 标签合集 <https://tympanus.net/codrops/tag/grid/>

---

## 7. 新 CSS 能力(scroll-driven / @property / anchor positioning / view transitions / :has / 容器查询)

**①代表案例 / 权威来源**
- Josh W. Comeau "Scroll-Driven Animations"(机制讲最透) <https://www.joshwcomeau.com/animation/scroll-driven-animations/>
- Chrome "CSS Wrapped 2025" 演示合集 <https://chrome.dev/css-wrapped-2025/>
- Bram.us "用 View Transitions 动画化 position-area" <https://www.bram.us/2026/03/02/animating-css-position-area-with-view-transitions/>

**②视觉效果**
无 JS 的滚动进度条/视差/进场动画;tooltip/下拉自动锚定到触发元素且智能翻转;`:has()` 做"父元素随子状态变样";容器查询让组件按自身宽度自适应(而非视口)。

**③实现原理**
- **scroll-driven animations**:两种时间线 —— `scroll()`(滚动容器 0→100%)与 `view()`(元素进出视口 0→100%)。把任意 `@keyframes` 的 `animation-timeline` 指向它,`animation-range: entry/exit/cover/contain` 控生效区间。**跑在合成线程,主线程再忙也 60fps**。命名时间线:`view-timeline:--t` 定义、`animation-timeline:--t` 消费,跨非后代元素用 `timeline-scope`。约 85% 支持;Firefox 需开 flag;`@supports (animation-timeline: view())` 兜底。
- **@property**:注册自定义属性的类型(如 `<angle>`/`<color>`)→ 让本来不可动画的属性(如渐变角度、conic 色标)能被 transition/keyframe 平滑插值。
- **anchor positioning**:`anchor-name` + `position-anchor` + `position-area`,把弹层锚到目标边;`position-try` 定义溢出时的备选位;anchored container query 让箭头方向跟随翻转后的位置。Chrome 130+/Safari 18+/Firefox 130+。
- **:has()**:父/前序选择器,`.card:has(img:hover)` 之类做纯 CSS 联动。
- **容器查询**:`container-type` + `@container`,按容器尺寸而非视口响应式。

**④主流技术栈/库**
- 全部**原生 CSS,零库**;polyfill 存在但 linked timeline 等特性不全。
- 生产环境常与 GSAP 并存:签名级复杂动画仍用 GSAP,轻量进场/视差交给原生省 JS。

**⑤开源可抄来源 / 学习**
- MDN scroll-driven animations 指南 <https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Scroll-driven_animations>
- MDN anchor positioning "Using" <https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Anchor_positioning/Using>
- Chrome for Developers 博客(scroll-triggered animations,Chrome 145 新增基于偏移触发) <https://developer.chrome.com/blog/scroll-triggered-animations>
- Interop 2026 进展 <https://css-tricks.com/interop-2026/>

---

## 8. 组件库生态(agent 可用性视角)

选型三问:**依赖多重?许可能商用?agent 能否直接拿到源码(copy-paste / CLI / MCP / llms.txt)?**

| 库 | 定位 | 依赖 | 许可 | 消费方式 | 组件数 | agent 可用性 |
|---|---|---|---|---|---|---|
| **Aceternity UI** | 最"炫"的 marketing 动效(spotlight/3D card/parallax) | Framer Motion + Tailwind(强制) | MIT | 复制粘贴 | 200+ | 复制源码即可;无官方 MCP/llms.txt(未验证) |
| **Magic UI** | landing 页动效块,克制些 | Framer Motion + Tailwind(强制) | MIT | shadcn CLI | 150+ | CLI 拉取,shadcn 生态 |
| **React Bits** | 上升最快(JS Rising Stars 2025 #2) | CSS 为主,按需 GSAP/Three.js/Matter.js | MIT + Commons Clause(注意:限制转售) | shadcn/jsrepo CLI,"你拥有代码" | 110+ | 模块化按需依赖,包体友好 |
| **21st.dev** | 社区组件市场 + **Magic MCP** | 视组件而定 | 视组件 | **MCP(编辑器内生成)** | 海量 | **最适合 agent**:MCP 直接在编辑器出 UI,收录 Aceternity 等 |
| **UI-Layout** | 类 Aceternity 的创意组件 | 未验证(推测 Framer Motion + Tailwind) | 未验证 | 复制粘贴(未验证) | — | 未验证 |
| **Motion Primitives** | Motion 官方向的动效原语 | Motion + Tailwind | 未验证 | 复制/CLI(未验证) | — | 与 shadcn 生态叠加 |
| **shadcn/ui(底座)** | 应用 UI 底座(非动效) | Radix + Tailwind | MIT | CLI,注册表 | — | **注册表 + MCP 生态**,agent 友好度高 |

**2026 通行搭法**:shadcn/ui 当应用底座 → Magic UI 加动效 → 需要"像有专职设计团队"的区块时上 Aceternity。<https://www.pkgpulse.com/guides/react-bits-animated-components-2026>

**给 agent 的实操建议**:优先 **21st.dev Magic MCP**(直接生成、可编辑器内迭代);要"拥有源码、无供应链风险"选 **React Bits/Aceternity 复制粘贴**;注意 React Bits 的 **Commons Clause**(禁止把它当核心转售)。

---

## 9. 业界怎么学 / 谁开源了什么

**顶级工作室与其公开产出**
- **darkroom.engineering(前 Studio Freight)** — 开源最狠,46+ repo:**Lenis**(平滑滚动,14k★,MIT)、**Tempus**(全站共用一个 rAF,320★)、**Hamo**(数学 hook,307★)、**Satus**(Next.js App Router 内容站 starter,964★)、**Aniso**(ASCII 生成)、**Spargo**(WebGL GPU 抖动)。<https://github.com/darkroomengineering>
- **Basement Studio** — 49 repo,多个 Awwwards SOTD,开源 starter/组件 <https://github.com/basementstudio>
- **Locomotive** — Locomotive Scroll(经典平滑滚动/视差,现多被 Lenis 取代)、多次 Site of the Month <https://www.awwwards.com/sites/locomotive-1>
- **Active Theory** — 极致沉浸式 WebGL(偏闭源,看作品学思路)
- **Bruno Simon** — Three.js Journey 作者 + 开车作品集,教学向

**怎么系统地学(优先级)**
1. **Codrops(tympanus.net)** — 案例+可跑 demo+GitHub repo,交互技法第一学习源 <https://tympanus.net/codrops/hub/>
2. **GSAP 官方 docs + showcase** — ScrollTrigger/SplitText 全套配方 <https://gsap.com/>
3. **Awwwards case study + collections** — 看获奖站怎么拆(Igloo Inc、WebGL collection) <https://www.awwwards.com/awwwards/collections/webgl-shaders-code/>
4. **Olivier Larose blog** — R3F/shader/微交互带完整代码 <https://blog.olivierlarose.com/>
5. **Maxime Heckel / Josh Comeau** — 前者 shader 深度、后者 CSS 机制讲透

---

## 10. 难度 × 收益矩阵(给 agent 的默认/按需分层)

评分:难度=实现+调试+可访问性成本;收益=用户感知"高级感"增量。

### A. 低成本 · 高感知 —— **agent 应默认掌握、几乎每站都值得上**
| 技法 | 为什么低成本 | 默认库 |
|---|---|---|
| Lenis 平滑滚动 | 一行接管,立刻"贵" | Lenis |
| SplitText 逐字进场 | 拆字+stagger 模板化 | GSAP SplitText |
| Magnetic 按钮 / 阻尼自定义光标 | 十几行数学 | Framer Motion / GSAP quickTo |
| Scroll 进场(fade/slide-up)| 原生 `view()` 零 JS,合成线程 60fps | CSS scroll-driven |
| 无限 marquee | 两份 DOM + 平移取模 | 纯 CSS / GSAP |
| Bento grid + hover | 纯 CSS Grid | CSS Grid |
| 页面过渡(交叉淡化/共享元素)| 原生 API,SPA/MPA 都简单 | View Transitions API |
| Stripe 风网格/shader 渐变背景 | 现成生成器出 React 组件 | ShaderGradient / whatamesh |

### B. 中成本 · 高感知 —— **需求匹配时上,不难但要调**
- Pin & scrub 滚动叙事(GSAP ScrollTrigger)—— 配方成熟但要调触发点/scrub 手感。
- 横向滚动展区、图片 mouse trail、无限视差网格 —— Codrops 有可抄 repo。
- 图片 hover 液态/膨胀扭曲(curtains.js / OGL 单 shader)—— 单效果可控。
- 变量字体交互 —— 需要字体资源 + 轴映射。

### C. 高成本 · 签名级 —— **按需,通常需专门时间/专家,agent 别默认**
- 全站 WebGL 沉浸世界(Three.js/R3F + 物理 + 自定义 shader 场景,如 Igloo/Bruno Simon)——性能/移动端/可访问性都是硬仗。
- 流体模拟、粒子系统、程序化几何(晶体生长等)——GPU 编程 + 数学重。
- 相机路径 × 灯光 × shader 全绑滚动的电影级叙事 —— 编排复杂、调试贵。
- WebGPU 前沿(gpu-curtains 等)—— 兼容性未铺开,实验性质。

**一句话策略**:agent 默认交付 A 类(平滑滚动 + 逐字进场 + 微交互 + 原生 scroll 动画 + shader 渐变背景)就已经"高级";B 类按页面需求点单;C 类明确标注"签名级、需专项",不轻易承诺。

---

## 附:最值得优先"拆迁"的 3 个来源
1. **Codrops(tympanus.net)** — 案例+可跑 demo+配套 GitHub repo,交互技法的第一手拆解库 <https://tympanus.net/codrops/hub/>
2. **darkroom.engineering GitHub** — Lenis/Tempus/Hamo/Satus,顶级工作室把自用工具直接开源 <https://github.com/darkroomengineering>
3. **21st.dev(Magic MCP)** — 唯一原生 MCP、能在编辑器内直接给 agent 出可迭代 UI 的组件市场 <https://21st.dev/>
