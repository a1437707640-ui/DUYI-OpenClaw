# /factory -- AI 一人公司工厂架构图

> 自动探索系统，用「工厂」类比生成 HTML 全景图。

## 触发词
- /factory
- 画架构图
- 工厂架构图
- 我的系统长什么样

## 执行流程

收到触发后，**不问用户任何问题**，直接按以下步骤执行：

### 第一步：自动探索

用 Bash 工具依次执行以下探索，记录发现的信息：

#### 1.1 Agent 系统（工厂在哪）
```bash
ls -la ~/.openclaw/ 2>/dev/null
ls -la ~/.claude/ 2>/dev/null
ls -la ~/dify/ 2>/dev/null
ls -la ~/coze/ 2>/dev/null
find . -maxdepth 3 -name "SOUL.md" -o -name "WORKFLOWS.md" -o -name "RULES.md" 2>/dev/null
```
- OpenClaw: 读 `openclaw.json`、`agents/`、`cron/jobs.json`、`node.json`、`exec-approvals.json`
- Claude Code: 读 `CLAUDE.md`、`settings.json`、`skills/`

#### 1.2 Agent 定义（工人）
- 读每个 Agent 的 SOUL.md / WORKFLOWS.md / RULES.md
- 识别：名称、角色、模型、触发方式、产出

#### 1.3 模型配置（大脑）
```bash
grep -r "model" ~/.openclaw/openclaw.json 2>/dev/null
grep -r "model" ~/.claude/settings.json 2>/dev/null
```

#### 1.4 数据/笔记系统（仓库）
```bash
ls -la .
find . -maxdepth 2 -name "*.md" | head -20
find ~ -maxdepth 2 -name ".obsidian" -type d 2>/dev/null
```

#### 1.5 定时任务（排班表）
```bash
cat ~/.openclaw/cron/jobs.json 2>/dev/null
crontab -l 2>/dev/null
```

#### 1.6 通信和发布渠道
```bash
grep -r "telegram\|bot_token\|chat_id" ~/.openclaw/ 2>/dev/null | head -5
grep -r "wechat\|weixin\|slack\|discord" ~/.openclaw/ ~/.claude/ 2>/dev/null | head -5
```

#### 1.7 MCP 工具（车间设备）
```bash
cat ~/.claude.json 2>/dev/null | grep -A 2 "mcpServers"
```

#### 1.8 Gateway / ACP（总机）
```bash
cat ~/.openclaw/node.json 2>/dev/null
```

#### 1.9 安全配置
```bash
cat ~/.openclaw/exec-approvals.json 2>/dev/null
```

#### 1.10 记忆系统
```bash
ls ~/.claude/projects/*/memory/ 2>/dev/null
find . -maxdepth 3 -type d -name "*记忆*" -o -name "*memory*" 2>/dev/null
```

### 第二步：映射到工厂框架

把所有发现映射到这套固定类比：

| 真实系统 | 工厂类比 |
|---------|---------|
| 用户 | 厂长 |
| Agent 系统 | 工厂 |
| 数据/笔记目录 | 仓库 |
| 每个 Agent | 工人 |
| 大模型 | 工人的大脑 |
| Fallback 链 | 顶班策略 |
| Gateway | 总机 |
| 外部 AI 调用 | 外聘顾问/外包 |
| Claude Code | 厂长秘书 |
| 通知渠道 | 对讲机 |
| 发布平台 | 门店 |
| 付费产品 | 会员店/俱乐部/培训中心 |
| 社群 | 客户交流区 |
| Cron | 排班表 |
| CLAUDE.md/RULES.md | 企业宪法 |
| WORKFLOWS.md | SOP 手册 |
| 权限配置 | 权限管理 |
| 记忆系统 | 企业记忆 |
| 知识库 | 零件仓 |
| MCP 工具 | 车间设备 |

**模型智商类比：**
- 推理型 (GPT-4/Claude Opus/Gemini Pro) = 985 本科生
- 执行型 (GPT-4o-mini/Haiku/Flash) = 熟练技工
- 编码型 (Kimi/Deepseek) = 程序员
- 专业型 (Gemini 2.5 Pro) = 老学究

没找到的组件标「待搭建」。

### 第三步：生成 HTML

**文件路径:** 当前工作目录下的 `my-factory.html`

**视觉规范：**
- 深色主题（背景 #0a0a12，卡片 #12121e）
- 字体：-apple-system 系统字体，代码用 SF Mono
- 卡片 border-radius: 14px，顶部 3px 渐变色条
- hover: translateY(-2px) + box-shadow
- 响应式 max-width: 1400px
- 颜色体系：厂长=#facc15, 工厂=#f43f5e, 仓库=#38bdf8, 工人=#4ade80, 大脑=#a78bfa, 渠道=#fb923c, 外部=#22d3ee, 总机=#818cf8

**必须包含的区块（按顺序，不可省略）：**

1. **标题 + 统计概览** -- N个工人 / N个定时任务 / N个MCP工具
2. **完整对照表** -- 发现的每个组件 vs 工厂类比
3. **厂长区** -- 用户 + 秘书(Claude Code) + 对讲机(通知渠道)
4. **工厂区**（红色大边框包裹）:
   - 工人卡片（每个 Agent 一张，含岗位/大脑/排班/产出）
   - 管理层（主控 Agent，如果有）
   - 总机（Gateway，如果有）
   - 排班表（Cron 时间轴）
   - 权限管理
   - 内部制度（SOP/RULES）
5. **大脑区** -- 模型对比卡片 + Fallback 链
6. **外部协作** -- 外聘顾问 + 外包 + 用工模式
7. **仓库区**（蓝色大边框包裹）:
   - 各货架（按发现的目录结构）
   - 企业宪法（规则文件）
   - 企业记忆（记忆系统）
8. **渠道区** -- 内部通信 / 销售渠道 / 社区
9. **产品漏斗**（如有付费产品）
10. **全景运转图** -- monospace 字体画的一天时间线
11. **一句话总结**

**「待搭建」区块样式：**
- border: 2px dashed rgba(255,255,255,0.1)
- 内容居中：「待搭建」+ 一句建议
- 建议文案：
  - 没有 Agent → "可以用 OpenClaw 搭建你的第一个 AI 员工"
  - 没有 Cron → "给 Agent 排个班，让它每天自动干活"
  - 没有 Fallback → "加个备用模型，主力挂了工厂不停工"
  - 没有通知 → "接个 Telegram Bot，让 Agent 能向你汇报"
  - 没有记忆 → "给系统加个记忆，下次对话不用重新交代"
  - 没有安全 → "给工人限定权限，别让它们乱动"
  - 没有知识库 → "把收藏的文章拆成知识卡片，写作时随时调用"
  - 没有渠道 → "内容生产出来要有地方发，选一个主阵地"
  - 没有漏斗 → "从免费内容到付费产品，设计一条转化路径"

### 第四步：打开浏览器并截图给用户

```bash
open my-factory.html  # macOS
# 或 xdg-open my-factory.html  # Linux
# 或 start my-factory.html  # Windows
```

等待 3 秒让页面加载完成，然后用浏览器工具对页面截图（全页截图），把截图展示给用户看。

## 铁律

- ⛔ 不问用户任何问题，直接探索直接生成
- ⛔ 不编造数据，找到什么画什么
- ⛔ 不省略区块，空的也要显示（虚线框 + 建议）
- ⛔ 路径用真实路径，不用占位符
- ⛔ HTML 必须独立，不依赖外部文件
