# EXTRACT — 网站设计语言提取协议(自家版 web-to-design-md)

> 触发:用户丢一个 URL 说「存进设计库 / 收藏这个 / 学一下这个风格」。
> 产出:一份别的 agent **不看原站也能复刻其设计语言**的 DESIGN.md + 截图,落盘 PERSONAL。
> 方法论借自 Paidax01/web-to-design-md <https://github.com/Paidax01/web-to-design-md>(该仓库未选 license:只借结构与规则,不复制其模板文件原文);工具链换成本机 Playwright。

## 原则(evidence-first)

- 证据主体 = DOM + computed styles + CSS 变量 + 可读 stylesheet 规则 + 交互态 diff;**截图只作最后交叉核对与存档,不许用截图替代 eval 提取**。
- 区分「观察到的事实」vs「推断」,推断必须显式标注。
- 颜色必须绑角色,带精确 hex:
  - Good:`Primary actions use a cool electric blue #3B82F6 on dark charcoal surfaces.`
  - Bad:`Button background is rgb(59, 130, 246).`(只有数值、无角色)
- 目标是文档化设计语言,不是克隆页面。

## 工具链(本机 Playwright)

用 `mcp__playwright__browser_*` 工具(或项目内置 Playwright Chromium 写脚本跑),顺序:

1. `browser_navigate` 打开 → `browser_wait_for` 等加载与 hydration。
2. `browser_evaluate` 全页慢滚一遍——触发懒加载、sticky 态、延迟动画,滚完再动笔。
3. `browser_evaluate` 提取:代表节点 outerHTML、`:root` CSS 变量、可读 stylesheet 规则、headings/buttons/cards/nav/section 的 `getComputedStyle`(颜色/字体/字号/行高/字距/间距/圆角/边框/阴影)、可见文案与 CTA。
4. `browser_hover` / `browser_click` 探 hover/active/expanded/sticky 态,取 computed-style diff。
5. `browser_resize` 切 tablet/mobile 各看一轮(布局有实质变化才细记)。
6. 站点有明暗切换:点 toggle,两种模式分别取证,记录哪些 token 反转、哪些 accent 不变。
7. 最后 `browser_take_screenshot` 存档为 `screenshot.png`。

## 六个提取 pass(顺序执行)

1. **Scope**:页面目的(marketing/product/dashboard/docs/ecommerce/editorial),从上到下列出可见 section;拿不到的区域(登录墙等)如实标注,不编。
2. **Baseline**:desktop 优先,tablet/mobile 补充;先滚完全页再写。
3. **Design System**:颜色角色 / 明暗双主题 / 字阶层级 / 间距节律与栅格 / 圆角语言 / 边框处理 / 阴影与表面分层 / 图像风格 / 图标风格 / 动效与过渡风格 / 密度与留白哲学 / CSS 变量约定 / 重复组件的 DOM 结构模式。
4. **Components & States**:nav / 公告条 / hero / 按钮与链接 / 卡片 / badge / 表单输入 / tabs / accordion / 表格 / footer;每个重要组件记 default / hover / active / focus / disabled / sticky / expanded / 明暗 各可见态。
5. **Interaction Behavior**:滚动改什么、hover 改什么、点击改什么、什么被动画进场;轮播/tabs/sticky 是点击驱动、滚动驱动还是时间驱动;动效手感(subtle / crisp / cinematic / playful / restrained)。
6. **Content & Brand Voice**:标题风格 / CTA 措辞 / 句子密度 / 产品框架 / 信任信号 / 功能命名模式;文案是 technical / playful / premium / direct / academic / conversational——好的 DESIGN.md 同时指导视觉与表达。

## 输出契约(9 节 DESIGN.md)

落盘目录:`3-references/PERSONAL/<站名>-<YYYYMMDD>/`,内含 `DESIGN.md` + `screenshot.png`。

固定 9 节(借 w2d 结构):

1. **Visual Theme & Atmosphere** — 至少一段高密度定性描述(Key Characteristics)
2. **Color Palette & Roles** — 分组:Primary / Interactive / Neutral Scale / Surface & Overlay;精确 hex + 功能角色
3. **Typography Rules** — Font Family / Hierarchy 表(size / weight / line-height / tracking)/ Principles
4. **Component Stylings** — Buttons / Cards & Containers / Inputs & Forms / Navigation / Image Treatment / 特色组件
5. **Layout Principles** — Spacing System / Grid & Container / Whitespace Philosophy / Border Radius Scale
6. **Depth & Elevation** — 多层阴影公式;身份级 pattern(如 shadow-as-border)写出一句 rationale
7. **Do's and Don'ts**
8. **Responsive Behavior** — Breakpoints / Touch Targets / Collapsing Strategy
9. **Agent Prompt Guide** — 至少 2-4 条 prompt-ready 例子(hero / card / button / nav)+ Iteration Guide

可选附录(有真价值才加):Interaction Patterns / Content & Messaging Patterns / Observed Pages / Evidence Notes。
支持明暗双主题的站:必须有显式 Theme Modes 记录(哪些 token 反转、哪些不变)。

## 合成规则

- 语义名优先于原始 token 名;颜色精确 hex 且绑功能角色。
- radius / spacing / shadow / type scale 翻译成另一个 agent 能直接用的普通话,不倒 CSS 原文、不做数据倾倒。
- 观点鲜明、可用优先;重复 pattern 归纳成系统;站点混两种视觉模式就都描述 + 各自出现场景。
- 可观察就给精确数值(hex / font-size / line-height / letter-spacing / radius / spacing 单位 / 多层 shadow 公式);推断值保持标注但仍要有用。

## 质量门(写完自检,任一 no 就回去再看一遍站)

- 另一个 agent 只凭这份文档能复刻整站气质吗?
- 主色绑到具体功能了吗?字阶具体到能直接复用吗?
- 主要组件的状态细节够吗?响应式的实质变化记了吗?
- 文案音色抓到了吗?有没有编造看不到的页面/状态?
- spacing / radius / shadow 规律够另一个 agent 建一套匹配组件库吗?
- 有 2-4 条 prompt-ready 例子吗?

## 版权边界

- **借**:设计决策——tokens / 色彩角色 / 间距节律 / 布局逻辑 / 交互手感 / 动效风格。
- **不借**:品牌资产——logo / 文案原文 / 摄影图 / 插画 / 商标构图。DESIGN.md 引用文案只为记音色,复刻时必须重写。
- 站点 robots 标 `ai-train=no`(如 recent.design):只可 reference 用途,不入训练语料。

## 边界情况

- 多 URL 同产品:先提共享规则,再记每页偏差,不逐页重复同一套系统。
- 登录墙 / 部分不可达:只记录看得到的,显式标缺口,不发明隐藏组件或 flow。
- 复杂 app(非营销页):重点记导航模型 / 密度 / 表格与面板 / 筛选 / 表单 / 空态 / 状态变化,app shell 逻辑写进 Layout Principles。

## 心法

写不下去时用这个框架:**「写出一份强设计工程 agent 在用同一产品语言新建页面之前,最想拿到的设计系统文档。」**
