# AI-TELLS — AI 味禁令(UI 任务必读,负面清单)

> AI 味的本质:模型无约束时输出训练分布的众数 = 所有网站的平均脸。它不是丑,是**谁都不是**。
> 来源:Alan West《Fix the AI look》+ taste-skill v2 AI-Tells 节 + research/02。
> 例外条款:用户明确要求某项时可破,但要提示一句"这是常见 AI 味"。

## 视觉禁令
- ❌ 紫蓝渐变 / indigo-600 / violet 系当默认主色(尤其 `#8b5cf6 → #ec4899` 一族)
- ❌ `rounded-2xl + shadow-lg` 组合批量刷在卡片上
- ❌ 渐变文字标题(gradient text)当默认标题处理
- ❌ emoji 当功能图标(🚀🔒✨ 三件套是重灾区)
- ❌ 玻璃拟态(backdrop-blur)无理由滥用
- ❌ 大黑阴影(blur>16px)当层次工具
- ❌ 随机图库感配图 / 显眼的占位图

## 布局禁令
- ❌ 居中 hero + 等宽三卡纵列(出厂默认构图)
- ❌ 全页每个 section 同 padding、同结构、无节奏变化
- ❌ 万物皆卡片:能用留白和分隔线分组的,不套卡片
- ❌ 完美对称到底 —— 至少一处有意打破

## 文案禁令
- ❌ 赋能 / 无缝 / 一站式 / 开箱即用 / 智能驱动 / 全方位
- ❌ Empower / Seamless / Unlock / Supercharge / Effortless
- ❌ 空洞三特性("极速部署 · 安全可靠 · 智能洞察")
- ✅ 替代:具体主张 + 真实数字("每 3 秒刷新一次全量指标")

## 字体禁令
- ❌ Inter / 系统栈单打 display 标题(正文可用)
- ❌ 全 bold(重量失去层级意义)
- ❌ 大标题不调字距(≥32px 必须收紧)

## 动效禁令
- ❌ `linear` / 默认 `ease` 用于 UI 过渡
- ❌ 全体元素同时飞入(无 stagger)
- ❌ 无限循环的注意力乞讨(弹跳箭头/永动 pulse)
- ❌ hover scale ≥1.1 的夸张缩放
- ❌ 高频操作(菜单/切换/tooltip)加 >150ms 动画

## 交互禁令
- ❌ 只交付静止的完美:无 hover/focus/active/disabled
- ❌ 数据区裸奔:无 loading/empty/error
- ❌ 点击后 >100ms 零反馈

---
**自查口诀:交付前问三句 —— ①这页面像不像"任何一个 AI 生成的网站"?②文案有没有一句是只有这个产品能说的?③有没有一处让人记住的细节?**
