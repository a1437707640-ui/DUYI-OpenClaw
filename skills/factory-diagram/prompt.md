# AI 一人公司工厂架构图 -- 通用提示词

> 把下面这段提示词完整复制，粘贴到 Claude Code 或 Codex 里执行。
> 它会自动探索你电脑上的 AI 系统，生成一张「工厂类比」的 HTML 全景图。
> 不需要提前告诉它你有什么，它自己会找。

## 使用方法

1. 打开 Claude Code（终端里输入 `claude`）
2. 把下面 `---提示词开始---` 到 `---提示词结束---` 之间的内容完整粘贴进去
3. 等它跑完，会自动用浏览器打开生成的 HTML 文件
4. 截图发群里，大家对比各自的「工厂」长什么样

---

## ---提示词开始---

```
你是一个系统架构分析师。你的任务是：自动探索我电脑上的 AI 系统，然后用「工厂」类比画一张 HTML 全景图。

## 核心类比框架

在你探索之前，先理解这套类比（这是固定的，不管用户有什么系统都用这套框架）：

| 真实系统 | 工厂类比 | 一句话 |
|---------|---------|--------|
| 用户本人 | 厂长 / CEO | 不在车间干活，拍板决策 |
| Agent 管理系统 (OpenClaw/Dify/Coze/自建等) | 工厂 | 包含车间、工人、排班表、制度、设备 |
| 数据/笔记目录 (Obsidian/Notion/本地文件夹等) | 仓库 | 存原材料、零件、成品、情报、日志 |
| 每个 AI Agent | 工人 | 有岗位、有工作流程、有权限范围 |
| 大模型 (GPT/Claude/Gemini/Kimi/Deepseek等) | 工人的大脑 | 决定这个岗位的人有多聪明，岗位不变大脑可换 |
| 模型 Fallback 链 | 顶班策略 | 主力请假时谁来顶，保证工厂不停工 |
| Gateway / API 网关 | 工厂总机 | 对外通信的统一入口 |
| 外部 AI 调用 (ACP/API) | 外聘顾问或外包团队 | 自己人搞不定时请外援 |
| Claude Code (交互式) | 厂长的贴身秘书 | 用户直接操作系统的界面 |
| Coding Plan (月薪制) | 把外聘顾问转正为全职员工 | 包月无限用 |
| API 按量付费 | 外聘按小时收费 | 用一次付一次 |
| 通知渠道 (Telegram/微信/邮件/Slack等) | 对讲机 / 内部通信 | Agent 向厂长汇报的通道 |
| 内容发布平台 (公众号/小红书/Twitter等) | 门店 / 销售渠道 | 成品卖给客户的地方 |
| 付费产品 (课程/会员/社群等) | 会员店 / 俱乐部 / 培训中心 | 不同价位的产品 |
| 社群 (微信群/Discord/飞书群等) | 客户交流区 | 用户互动的地方 |
| 定时任务 (Cron/调度器) | 排班表 | 谁几点上班干什么活 |
| 规则/配置文件 (CLAUDE.md/RULES.md等) | 企业宪法 / 规章制度 | 约束所有人的行为准则 |
| 工作流文件 (WORKFLOWS.md/SOP等) | SOP 手册 | 每个岗位怎么干活 |
| 权限配置 (exec-approvals等) | 权限管理 | 每个工人只能用自己岗位的工具 |
| 记忆系统 (memory/记忆库等) | 企业记忆 | 会议纪要、经验手册、制度 |
| 知识库/卡片系统 | 零件仓 | 加工好的标准零件，随时可用 |
| MCP 工具 | 车间设备 | Agent 可调用的工具 |
| 产品漏斗 (免费→付费) | 从路人到学员的转化链 | 传单→门店→会员→培训 |

## 第一步：自动探索（不要问我，自己找）

依次执行以下探索，把发现的信息记下来：

### 1.1 找 Agent 系统（工厂在哪）
```bash
# 检查常见 Agent 框架目录
ls -la ~/.openclaw/ 2>/dev/null      # OpenClaw
ls -la ~/.claude/ 2>/dev/null         # Claude Code 配置
ls -la ~/dify/ 2>/dev/null            # Dify
ls -la ~/coze/ 2>/dev/null            # Coze
# 也找一下当前工作目录下有没有 agent 相关配置
find . -maxdepth 3 -name "*.json" -path "*/agent*" 2>/dev/null
find . -maxdepth 3 -name "SOUL.md" -o -name "WORKFLOWS.md" -o -name "RULES.md" 2>/dev/null
```
- 如果找到 OpenClaw：读 `~/.openclaw/openclaw.json`、`agents/` 目录、`cron/jobs.json`、`node.json`、`exec-approvals.json`
- 如果找到 Claude Code：读 `~/.claude/CLAUDE.md`、`~/.claude/settings.json`、`~/.claude/skills/` 目录
- 如果都没找到：标记「工厂：待搭建」

### 1.2 找 Agent 定义（工人有哪些）
- 读每个 Agent 的配置（SOUL.md / WORKFLOWS.md / RULES.md 或等效文件）
- 识别：名称、角色描述、使用的模型、触发方式（cron/手动/事件）
- 如果没有 Agent：标记「工人：0 个，全靠厂长亲自干」

### 1.3 找模型配置（大脑是什么）
```bash
# 在配置文件中找模型相关字段
grep -r "model" ~/.openclaw/openclaw.json 2>/dev/null
grep -r "model" ~/.claude/settings.json 2>/dev/null
# 找 API key 配置（不读内容，只确认存在）
ls ~/.config/ 2>/dev/null
find ~ -maxdepth 2 -name ".env" 2>/dev/null | head -5
```
- 识别主力模型、备用模型、Fallback 链
- 区分模型等级：推理型（贵/聪明）vs 执行型（便宜/快）

### 1.4 找数据/笔记系统（仓库在哪）
```bash
# 检查当前工作目录结构
ls -la .
# 常见笔记系统
find . -maxdepth 2 -name "*.md" | head -20
find ~ -maxdepth 2 -name ".obsidian" -type d 2>/dev/null
```
- 识别目录结构和分类逻辑
- 找 CLAUDE.md / README.md 等规则文件

### 1.5 找定时任务（排班表）
```bash
# OpenClaw cron
cat ~/.openclaw/cron/jobs.json 2>/dev/null
# 系统 crontab
crontab -l 2>/dev/null
# Claude Code scheduled tasks
ls ~/.claude/scheduled_tasks.json 2>/dev/null
```

### 1.6 找通信和发布渠道（对讲机和门店）
```bash
# Telegram bot 配置
grep -r "telegram\|bot_token\|chat_id" ~/.openclaw/ 2>/dev/null | head -5
grep -r "telegram" ~/.claude/ 2>/dev/null | head -5
# 微信/公众号相关
grep -r "wechat\|weixin\|appid" ~/.openclaw/ 2>/dev/null | head -5
# 其他平台
grep -r "slack\|discord\|email\|smtp" ~/.openclaw/ ~/.claude/ 2>/dev/null | head -5
```

### 1.7 找 MCP 工具（车间设备）
```bash
cat ~/.claude.json 2>/dev/null | grep -A 2 "mcpServers"
cat ~/.claude/settings.json 2>/dev/null | grep -A 2 "mcp"
```

### 1.8 找 Gateway / ACP（总机）
```bash
cat ~/.openclaw/node.json 2>/dev/null
grep -r "gateway\|acp\|subagent" ~/.openclaw/ 2>/dev/null | head -10
```

### 1.9 找安全配置（权限管理）
```bash
cat ~/.openclaw/exec-approvals.json 2>/dev/null
grep -r "security\|permission\|approval" ~/.openclaw/ ~/.claude/ 2>/dev/null | head -10
```

### 1.10 找记忆系统（企业记忆）
```bash
# Claude Code auto-memory
ls ~/.claude/projects/*/memory/ 2>/dev/null
# 项目内记忆
find . -maxdepth 3 -type d -name "*记忆*" -o -name "*memory*" 2>/dev/null
```

## 第二步：映射到工厂框架

把探索到的所有信息，按以下结构整理（内部思考，不需要输出给用户）：

```
厂长: [用户名或"你"]
  秘书: [Claude Code / 其他交互工具]
  对讲机: [Telegram / 微信 / 邮件 / 无]

工厂: [OpenClaw / Dify / 自建 / 无(厂长亲自干)]
  位置: [目录路径]
  工人:
    - [Agent名] = [岗位类比] | 大脑=[模型] (类比=[985/技工/学究]) | 排班=[cron时间] | 产出=[输出路径]
    - ...
  COO/管理层: [主控Agent / 无]
  总机: [Gateway地址 / 无]
  外聘顾问: [Claude ACP / 其他外部AI / 无]
  外包团队: [Codex / 其他 / 无]
  排班表: [N个定时任务]
  权限管理: [有/无]
  顶班策略: [Fallback链 / 无]
  车间设备(MCP): [列表]

仓库: [笔记/数据目录路径]
  原材料仓: [收藏/采集的内容]
  零件仓: [知识库/卡片]
  成品仓: [发布的内容]
  情报柜: [研究/分析报告]
  生产日志: [日志/日记]
  企业宪法: [CLAUDE.md / 规则文件]
  产品标准: [写作风格 / 质检规则]
  企业记忆: [记忆系统]

门店/渠道:
  内部通信: [Telegram / ...]
  免费渠道: [公众号 / 小红书 / Twitter / ...]
  付费产品: [课程 / 会员 / 社群 / ...]
  产品漏斗: [免费 → ... → 最高价]

用工模式: [API按量 / Coding Plan / 混合 / 不确定]
```

对于没有找到的组件，标记为「待搭建」。

## 第三步：生成 HTML

用下面的 HTML 模板，把第二步整理好的数据填进去。

**要求：**
1. 深色主题 (背景 #0a0a12)
2. 卡片式布局，每个区块一个卡片
3. 不同类别用不同颜色区分（厂长=金色, 工厂=红色, 仓库=蓝色, 工人=绿色, 大脑=紫色, 渠道=橙色, 外部=青色）
4. 「待搭建」的区块用虚线边框 + 灰色，不隐藏
5. 包含以下区块（按顺序）：
   a. 标题 + 统计概览（N个工人 / N个定时任务 / N个MCP工具 等）
   b. 完整对照表（真实系统 vs 工厂类比）
   c. 厂长区（用户 + 秘书 + 对讲机）
   d. 工厂区（用大边框包裹，内含：工人卡片 + 管理层 + 总机 + 排班表 + 权限 + 制度）
   e. 工人的大脑（模型对比 + Fallback 链）
   f. 外部协作（外聘顾问 + 外包团队 + 用工模式对比）
   g. 仓库区（用大边框包裹，内含：各货架 + 企业宪法 + 企业记忆）
   h. 渠道区（内部通信 + 销售渠道 + 培训社区）
   i. 产品漏斗（如果有付费产品的话）
   j. 全景运转图（一天的时间线，用 monospace 字体画）
   k. 一句话总结
6. 文件保存到当前工作目录下的 `my-factory.html`
7. 生成完毕后自动用浏览器打开（`open my-factory.html`）

**HTML 视觉规范：**
- 字体：-apple-system, BlinkMacSystemFont, 'SF Pro Display', sans-serif
- 代码/路径：'SF Mono', 'Fira Code', monospace
- 卡片：border-radius: 14px, 背景 #12121e, 边框 rgba(255,255,255,0.06)
- 每个卡片顶部有 3px 的渐变色条标识类别
- hover 效果：translateY(-2px) + box-shadow
- 响应式：max-width 1400px，移动端单列
- 「待搭建」区块样式：border: 2px dashed rgba(255,255,255,0.1), 内容居中显示「待搭建」+ 一句建议

**「待搭建」建议文案：**
- 没有 Agent 系统 → "可以用 OpenClaw 搭建你的第一个 AI 员工"
- 没有定时任务 → "给 Agent 排个班，让它每天自动干活"
- 没有 Fallback → "加个备用模型，主力挂了工厂不停工"
- 没有通知渠道 → "接个 Telegram Bot，让 Agent 能向你汇报"
- 没有记忆系统 → "给你的系统加个记忆，下次对话不用重新交代"
- 没有安全配置 → "给工人限定权限，别让它们乱动"
- 没有知识库 → "把收藏的文章拆成知识卡片，写作时随时调用"
- 没有发布渠道 → "内容生产出来要有地方发，选一个主阵地"
- 没有产品漏斗 → "从免费内容到付费产品，设计一条转化路径"

## 重要原则

1. **不要问我任何问题**，直接探索直接生成
2. **不要编造数据**，找到什么画什么，没找到标「待搭建」
3. **不要省略区块**，即使是空的也要显示（虚线框 + 建议）
4. **路径用真实路径**，不要用占位符
5. **模型类比要准确**：推理型(GPT-4/Claude Opus/Gemini Pro) = 985本科生，执行型(GPT-4o-mini/Claude Haiku/Gemini Flash) = 熟练技工，编码型(Kimi/Deepseek) = 程序员
6. HTML 必须是一个完整的独立文件，不依赖外部 CSS/JS

现在开始执行。不要输出分析过程，直接探索 → 生成 HTML → 打开浏览器 → 等待3秒页面加载完成后对页面全页截图展示给我看。
```

## ---提示词结束---

---

## 常见问题

**Q: 我的系统很简单，只有 Claude Code，没有 Agent，能用吗？**
A: 能。它会自动识别你有什么，没有的部分标「待搭建」+ 给你建议下一步该搭什么。

**Q: 跑出来一片「待搭建」怎么办？**
A: 这就是你的起点。每个「待搭建」都是一个可以努力的方向。对比群里其他朋友的架构图，看看他们多了什么。

**Q: 我用的不是 OpenClaw，是 Dify/Coze/自建的，能用吗？**
A: 能。提示词会扫描多种 Agent 框架。如果你用的框架它没扫到，可以在提示词最前面加一句「我的 Agent 系统在 [你的路径]」。

**Q: 生成的图可以发朋友圈/公众号吗？**
A: 可以。用浏览器打开后截图即可。深色主题很适合发社交媒体。
