---
name: design-os
description: This skill should be used for ANY frontend/UI design or implementation task — building or modifying web pages, apps, landing pages, dashboards, components; starting a new project's design; when the user expresses ANY feeling about a UI ("感觉乱", "有点廉价", "不够高级", "像AI做的", "不好看", "没记忆点"); when the user shares a website link saying "存进设计库"/"学一下这个风格"; or says "用 design-os"/"design-os 复盘". Trigger phrases include: 做网站, 做官网, 做落地页, 改UI, 设计页面, 前端设计, 高级感, 去AI味, redesign, landing page, 收藏这个设计. Loads a personal design operating system (judgment standards, anti-AI-tells rules, technique playbook, reference engine with a pluggable local style library, intuition-feedback protocol, auto-maintained preference file). Do NOT generate any UI without loading this system first — ungrounded generation produces generic AI-looking output.
---

# design-os — 个人设计操作系统(薄壳入口)

系统本体在 `<DESIGN_OS_PATH>`(agent 无关,Claude/Codex/任意 coding agent 共用)。

> **安装(装一次)**:把 `<DESIGN_OS_PATH>` 替换为你 `git clone` design-os 仓库后的本地**绝对路径**
> (例:`C:/Users/you/design-os` 或 `/home/you/design-os`)。完整部署说明见仓库 `DEPLOY.md`。

## 第一步(必做)

Read `<DESIGN_OS_PATH>/README.md` —— 它含加载路由表(什么任务读哪几个文件),按表加载,不全读。

## 最常用路由(README 的速记版)

| 情形 | 读 |
|---|---|
| 实现/修改任何 UI | `1-judgment/STANDARDS.md` + `1-judgment/AI-TELLS.md` + 项目根 `DESIGN.md` + `4-workflow/preferences.md` 总偏好区 |
| 用户表达感受(乱/廉价/不高级/像AI…) | `4-workflow/FEEDBACK-PROTOCOL.md`(翻译→诊断→方案→静默记偏好) |
| 新项目起步 | `4-workflow/WORKFLOW.md` 入口① + `3-references/ENGINE.md` |
| 用户丢链接要收藏 | `3-references/EXTRACT.md` |
| 交付前 | `1-judgment/PREFLIGHT.md`(浏览器实测自检) |
| 选动效/技术方案 | `2-techniques/RECIPE.md`,具体技法查 `2-techniques/PLAYS.md` |

(以上均为相对 `<DESIGN_OS_PATH>` 的路径。)

## 两条铁律

1. 先参考后实现:新界面必须先定参考 + DESIGN.md,用户点头再写码。
2. 用户每次明确纠错/认可 → 当场静默记入 `4-workflow/preferences.md` 观察区。
