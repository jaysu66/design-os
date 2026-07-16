# design-os — 给 AI agent 用的个人设计操作系统

> 让 Codex / Claude / 任意能读本地文件的 coding agent,在任何项目里做 UI 时:
> **带着判别标准开工、借高质量参考突破上限、在用户的直觉反馈中越来越懂他。**
> 你是创意总监,agent 是设计师,**翻译工作归 agent**。

## 这是什么 / 解决什么

AI 做前端的通病是「模板脸」——居中 hero + 渐变字 + 三卡 + emoji,谁都不是。根因是**无参考裸奔生成**:模型凭训练分布的平均值出图,自然是平均的 AI 味。

design-os 是一套**纯文本、agent 无关**的操作系统,用四件事把这个问题按住:

1. **判别力**(`1-judgment/`)——先给 agent 一套「什么是好、什么是 AI 味」的硬标准和出厂自检,让它自己认得出烂。
2. **手艺**(`2-techniques/`)——四层配方 + 9 类交互技法卡,让它知道好效果怎么做出来。
3. **见识**(`3-references/`)——参考引擎:先定高质量参考再动手,禁止裸奔;支持挂接你自己的本地风格库。
4. **流程 + 记忆**(`4-workflow/`)——固定协作流程 + 一份 agent 自动维护的偏好档案,你每纠错一次它就更懂你一点。

它不是组件库、不是 CSS 框架、不绑任何技术栈——它是喂给 agent 的**判断力和工作方式**。

## 快速上手

装到你的 agent 只需三步(完整说明见 **[DEPLOY.md](DEPLOY.md)**):

1. `git clone` 本仓库到本地任意路径。
2. 把 `agent-skill/SKILL.md`(通用薄壳)装进你 agent 的 skills 目录,把里面的 `<DESIGN_OS_PATH>` 占位符替换成第 1 步的绝对路径。
3. 对 agent 说「用 design-os 起步,做一个 ××」——它会自动加载路由、先给你 3 个参考候选。

> 双端/多端接入(Claude Code / Codex / 其他)、可选外部依赖、字体获取,全在 [DEPLOY.md](DEPLOY.md)。

## 目录

```
1-judgment/    判别力:STANDARDS(标准) AI-TELLS(禁令) PREFLIGHT(出厂自检)
2-techniques/  手艺:RECIPE(四层配方+选型) PLAYS(9 类技法卡)
3-references/  见识:ENGINE(参考引擎) EXTRACT(网站提取协议) PERSONAL/(你的精选库)
4-workflow/    流程:WORKFLOW(5 入口) FEEDBACK-PROTOCOL(直觉反馈协议)
               DESIGN-TEMPLATE(项目指南针模板) preferences(偏好档案,agent 自动维护)
5-human/       给人的:审美训练页 使用手册 沟通速查(你可选学,不学系统照样转)
research/      调研底稿(全部结论的出处,177+ 来源)
agent-skill/   给 agent 装的通用薄壳(SKILL.md 模板)
```

## 加载路由(上下文预算铁律:按任务读,不全读)

| 任务 | 必读 | 按需 |
|---|---|---|
| 任何 UI 实现/修改 | 1-judgment/STANDARDS + AI-TELLS + 项目 DESIGN.md | 2-techniques/PLAYS(用到该类技法时) |
| 交付前 | 1-judgment/PREFLIGHT(浏览器自检) | — |
| 新项目起步 | 4-workflow/WORKFLOW 入口① + 3-references/ENGINE | DESIGN-TEMPLATE |
| 用户表达感受/不满 | 4-workflow/FEEDBACK-PROTOCOL | 5-human/沟通速查(映射表同源) |
| 用户丢链接说"存进设计库" | 3-references/EXTRACT | — |
| 选技术方案(要不要动效/上不上 WebGL) | 2-techniques/RECIPE | — |
| 复盘/治理 | 4-workflow/WORKFLOW 治理节 + preferences.md | — |

## 两条铁律

1. **先参考后实现**:任何新界面,先定参考和 DESIGN.md,用户点头再写码。禁止无参考裸奔生成。
2. **偏好自动记录**:用户每次明确纠错/明确认可,当场记入 `4-workflow/preferences.md`(用户零维护)。

## 治理(防系统腐烂)

- 本仓库独立 git:每次偏好/规则变更都留痕,`git log` 就是"系统学习史"。
- STANDARDS / AI-TELLS 各一页封顶:新规则进来,同类合并;90 天未触发的进休眠区。
- 偏好两区制:观察区(单次记录)→ 总偏好(跨项目 ≥2 次确认才转正)。
- 用户说「design-os 复盘」触发整合(详见 WORKFLOW 治理节)。

## 参考引擎与外部依赖

`3-references/ENGINE.md` 的参考引擎支持三级来源:你的个人精选库 > 可选的本地风格档案库 > 外部实时源。**本仓库不打包任何第三方设计资产**(风格库、字体、截图都有版权),这些都是可选的、自备的外部依赖——你可以用 `EXTRACT.md` 协议从任意公开站自建你自己的库。详见 [DEPLOY.md](DEPLOY.md) 的「可选外部依赖」。

## License

[MIT](LICENSE)。方法论、协议、技法卡、研究底稿均可自由使用/修改/再分发,保留版权声明即可。

> 注:`5-human/` 下的 HTML 训练页引用的字体、以及你自建风格库里收藏的第三方设计资产,各有其自身授权,不在本仓库 MIT 覆盖范围内——按各自来源的许可使用。
