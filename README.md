# DUYI-OpenClaw

杜一的 OpenClaw AI 员工系统 -- Skills & 提示词。

## Skills

### /factory -- AI 一人公司工厂架构图

自动探索你电脑上的 AI 系统（Agent、模型、定时任务、MCP 工具、记忆系统等），用「工厂」类比生成一张 HTML 全景图。

不需要提前告诉它你有什么，它自己会找。没有的部分标「待搭建」+ 给你建议。

#### 效果

- 深色主题 HTML 页面，浏览器直接打开
- 包含：厂长区、工厂区、仓库区、渠道区、产品漏斗、全景时间线
- 有 Agent 的显示完整架构，没有的显示「待搭建」+ 下一步建议

#### 安装方法

在 OpenClaw 里安装：

```bash
openclaw skill add a1437707640-ui/DUYI-OpenClaw --path skills/factory-diagram
```

或者手动安装：把 `skills/factory-diagram/` 整个目录复制到你的 OpenClaw Skill 目录下：

```bash
# 复制到 OpenClaw 的 Skill 加载目录
cp -r skills/factory-diagram ~/.openclaw/workspace/skills/factory-diagram
```

安装后在 OpenClaw 里输入 `/factory` 即可执行。

#### 不用 OpenClaw 也能用

打开 `skills/factory-diagram/prompt.md`，把提示词完整复制，粘贴到 Claude Code 或其他 AI 工具里执行。

#### 常见问题

**Q: 我的系统很简单，只有 Claude Code，没有 Agent，能用吗？**

能。没有的部分标「待搭建」+ 给你建议下一步该搭什么。

**Q: 跑出来一片「待搭建」怎么办？**

这就是你的起点。每个「待搭建」都是一个可以努力的方向。

**Q: 我用的不是 OpenClaw，是 Dify/Coze/自建的，能用吗？**

能。提示词会扫描多种 Agent 框架。如果你用的框架没扫到，在提示词最前面加一句「我的 Agent 系统在 [你的路径]」。

**Q: 生成的图可以发朋友圈/公众号吗？**

可以。浏览器打开后截图即可。

## 关于

- 作者：杜一
- 公众号：杜一的人生实验室
- 训练营：OpenClaw AI 员工训练营
