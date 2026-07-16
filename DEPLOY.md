# DEPLOY — 把 design-os 装进你的 AI agent

design-os 是**纯文本、agent 无关**的:本体是一堆 markdown,任何能读本地文件的 coding agent(Claude Code / Codex / Cursor / …)都能用。接入方式统一:**装一个"薄壳"skill,让它指向你本地的 design-os 目录**。薄壳只做一件事——告诉 agent「本体在哪、按 README 路由表加载」。

---

## 0. 前置:把本体放到本地

```bash
git clone <你的仓库地址>/design-os.git
# 记下它的绝对路径,下面全程叫它 <DESIGN_OS_PATH>
# 例:  C:/Users/you/design-os   (Windows,用正斜杠)
#      /home/you/design-os      (macOS / Linux)
```

放哪都行(不必在某个具体项目里),因为它是**跨项目共用**的设计大脑。

---

## 1. Claude Code

Claude Code 的 skill 目录是 `~/.claude/skills/`。

```bash
mkdir -p ~/.claude/skills/design-os
cp <DESIGN_OS_PATH>/agent-skill/SKILL.md ~/.claude/skills/design-os/SKILL.md
```

然后编辑 `~/.claude/skills/design-os/SKILL.md`,把里面的 **`<DESIGN_OS_PATH>` 占位符全部替换**成第 0 步的绝对路径。一条命令替换(把路径换成你的):

```bash
# macOS / Linux
sed -i 's#<DESIGN_OS_PATH>#/home/you/design-os#g' ~/.claude/skills/design-os/SKILL.md
# Windows (Git Bash)
sed -i 's#<DESIGN_OS_PATH>#C:/Users/you/design-os#g' ~/.claude/skills/design-os/SKILL.md
```

触发:说「用 design-os 做一个 ××」,或任意命中 SKILL.md `description` 里触发短语的话(做官网/改UI/像AI做的/存进设计库…),skill 会自动加载。

---

## 2. Codex

同一份薄壳,放进 Codex 的 skills 目录(路径以你的 Codex 版本为准,通常是 `~/.codex/skills/` 或项目内 skills 目录):

```bash
mkdir -p <CODEX_SKILLS_DIR>/design-os
cp <DESIGN_OS_PATH>/agent-skill/SKILL.md <CODEX_SKILLS_DIR>/design-os/SKILL.md
# 同样替换 <DESIGN_OS_PATH> 占位符
```

本体内容 agent 无关,一字不用改。

---

## 3. 其他 agent(Cursor / Windsurf / 自研 harness)

没有标准 skill 机制的 agent,把这句加进它的系统规则 / rules 文件即可:

> 做任何前端/UI 任务前,先 Read `<DESIGN_OS_PATH>/README.md`,按其中的「加载路由表」按需加载对应文件,再开工。遵守两条铁律:先参考后实现;用户每次纠错/认可当场记入 `4-workflow/preferences.md`。

把 `<DESIGN_OS_PATH>` 换成绝对路径。核心就是让 agent 每次动手前先读 README 路由表。

---

## 4. 验证装好了

对 agent 说:

> 用 design-os 起步,帮我做一个某产品的落地页。

装对了的表现:agent **先读 README/ENGINE**,然后**给你 3 个参考风格候选让你挑**,而不是直接开始写代码。如果它上来就裸奔生成 UI,说明薄壳没触发或路径没配对——检查 SKILL.md 里的 `<DESIGN_OS_PATH>` 是否已替换成真实绝对路径。

---

## 5. 可选外部依赖(不装也能转,装了更强)

design-os **不打包任何第三方资产**(有版权),以下都是可选、自备:

### 5.1 本地风格档案库(ENGINE 来源 2)

想让 agent 能"照着 Vercel/Linear/Stripe 这类气质出成套 tokens",挂一个本地风格库:

- **契约**:一个目录 = 一个风格,每风格含 `DESIGN.md`(格式见 `3-references/EXTRACT.md` 的 9 节)+ 可选 tokens 文件。
- **指定库根**:设环境变量 `DESIGN_OS_STYLE_LIB=<你的风格库路径>`。
- **自建**:最省事的方式就是用系统自带的 `EXTRACT.md` 协议——丢任意公开站的链接给 agent 说「存进设计库」,它就提取成一个符合契约的档案,落进 `3-references/PERSONAL/`。收藏久了,`PERSONAL/` 本身就是你的私有风格库。

### 5.2 外挂能力 skill

```bash
# 审美判别 + 质检(三刻度盘 / AI-Tells / Pre-Flight)
npx skills add https://github.com/Leonxlnx/taste-skill
# GSAP 动效正确 API + cleanup
npx skills add https://github.com/greensock/gsap-skills
```

分工:taste 决定「何时/为何用动效」,gsap 供「正确 API 细节」。详见 `3-references/ENGINE.md`。

### 5.3 字体

`5-human/` 的训练页和"Vercel 气质"参考用到 **Geist / Geist Mono**(Vercel 以 SIL OFL 开源,可免费商用)。本仓库不打包字体,按需自取:

- 下载:<https://vercel.com/font>
- 或 npm:`npm i geist`

---

## 6. 更新与治理

- design-os 是活系统:agent 会随你使用不断往 `4-workflow/preferences.md` 写你的偏好。本仓库独立 git,`git log` 就是你的"设计品味学习史"。
- 说「design-os 复盘」触发整合(观察区偏好跨项目 ≥2 次转正、规则同类合并、旧规则休眠)。详见 `4-workflow/WORKFLOW.md` 治理节。

---

## 7. 关于你的私人数据(重要)

- `4-workflow/preferences.md` 随仓库分发的是**空模板**。你用起来后它会被填成你的私人审美偏好——**这是你的个人数据**。
- `3-references/PERSONAL/` 下你收藏的站含第三方设计资产,已被 `.gitignore` 排除。
- 如果你 fork 本仓库私有自用,以上正常提交无妨;**如果你要把改进 PR 回开源上游,不要提交你填充过的 preferences.md 和 PERSONAL 收藏**。
