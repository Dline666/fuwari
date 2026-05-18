---
title: cc配置教程
published: 2026-05-13
description: 'Claude Code配置攻略，学习中...'
image: ''
tags: [CC教程]
category: 'AI'
draft: false 
lang: ''
---

## 部署前准备

1. 必备
   * node.js
   * git
2. 可选
   * Agent IDE(智能体集成开发环境)

> 🤩需要准备的环境好简单

## 安装并配置APIKEY

一、进入cmd窗口，使用npm包管理工具进行安装

```bash
# 因为cc只是一个node脚本，直接使用npm安装（-g参数直接全局安装）
npm install -g @anthropic-ai/claude-code
```

二、使用cc-switch配置APIKEY和base_url

下载地址：[CC Switch 官方网站 - AI 编程 CLI 统一管理工具](https://ccswitch.io/zh/)

在里面配置好LLM的APIKEY和base_url即可开始使用cc了

## 了解cc的基本操作

### 1. 三种模式选择

|       模式       |                           功能                           |
| :--------------: | :------------------------------------------------------: |
|     默认模式     |                     CC默认的交互状态                     |
|     计划模式     | CC进行复杂的代码修改时，会进入计划模式分析需求，不动代码 |
| Accept Edits模式 |        “代码落地”阶段，执行代码变更和最终确认写入        |

> 使用`shift + tab`手动进行模式切换

*tips1:*

如果希望CC无需询问，直接进行所有操作，需要在启动前加上一行参数`--dangerously-skip-permissions`
```bash
claude --dangerously-skip-permissions
```

### 2. 提供文件（节省token，CC提效）

**方式一：本地文件**

使用@命令让CC进行本地文件查找
```bash
# 例子（filename 是具体文件名）
@filename 帮我看看这个文件内容
```

**方式二：图片**

直接拖拽图片至对话框，或复制粘贴：

* **Windows**:`Alt + V`
* **macOS**：`Command + V`

**方式三：多行文本输入**

在CC文本框内换行快捷键（不是Shift + Enter）:

* **Windows:**`Ctrl  + Enter`
* **macOS:**`Option + Enter`

### 3. 常用指令大全

|   CC指令    |                           功能说明                           |
| :---------: | :----------------------------------------------------------: |
|   `/help`   |                           帮助手册                           |
|  `/model`   |                           切换模型                           |
|   `/btw`    |       进入临时对话，隔离当前上下文，`ESC`退出临时对话        |
| `/simplify` | 派生出三个agent，从代码质量、运行效率和复用性<br />三个角度进行代码审查，自动优化 |
|  `/rewind`  |                         进入回滚界面                         |
| `/compact`  |                          压缩上下文                          |
|  `/clear`   |                          清空上下文                          |
| `/context`  |                 展示上下文信息（窗口使用率）                 |
|  `/resume`  |                        恢复之前的对话                        |
|   `/init`   |                  初始化创建项目级Claude.md                   |
|  `/memory`  |   对Claude的全局、项目记忆，以及auto memory进行操作和管理    |
|  `/agents`  |                   创建、调用、管理子agent                    |
|  `/plugin`  |           发现新插件，管理已下载插件，新增插件生态           |

## 个性化

### 1. Claude.md配置

**项目级 Claude.md**创建方式

```bash
/init
```

**全局级 Claude.md**创建方式
**方式1：提示词交互**

```bash
永远说中文，写进全局claude.md
```

**方式2：使用指令**
输入`memory`后，选择[User Memory]进入

### 2. Auto Memory

* 打开Auto Memory，输入`/memory`,选择[Auto-memory]并输入回车开启

> Auto Memory是CC自动记忆一些习惯、错误、经验，都会以自动记忆的形式记录，仅限于当前项目，不会跨项目产生影响

## 能力扩展

### 1. Skill能力扩展

1. Claude Code优质skill

   * 直接将skill链接给cc，让cc自动安装

   | skill名称       | 功能                                                  | 下载链接                                                     |
   | --------------- | ----------------------------------------------------- | ------------------------------------------------------------ |
   | Find-Skill      | 根据用户需求，查找和安装来自agent skill开放生态的技能 | [GitHub](https://github.com/vercel-labs/skills/tree/main/skills/find-skills) |
   | Frontend-Design | 创建具有独特风格，生产级品质且设计精良的前端界面      | [GitHub](https://github.com/anthropics/skills/tree/main/skills/frontend-design) |
   | Skill-Creator   | 创建新skill、修改和改进现有skill、并衡量skill表现     | [GitHub](https://github.com/anthropics/skills/tree/main/skills/skill-creator) |
   | 卡帕西skill     | 一个依据卡帕西经验总结，提升CC编码表现的skill         | [GitHub](https://github.com/forrestchang/andrej-karpathy-skills) |

2. skill合集网站：**[lobehub](https://lobehub.com/zh/skills)**

3. 自定义skill

4. 下载后如何装

   * **全局skill位置：**`/Users/username/.claude/skills/`
   * **项目skill位置：**`./claude/skills/`

### 2.MCP扩展

由于对于大模型来说，CLI工具比GUI操作的效率更高，因此这里暂时跳过MCP部分

### 3.CLI命令行工具

**优质CLI推荐，**将下载链接给CC，CC全权负责

|  CLI名称   | 功能                                                         | 下载                                                         |
| :--------: | ------------------------------------------------------------ | ------------------------------------------------------------ |
|  飞书CLI   | 飞书官方CLI工具，覆盖消息、文档、多维表格、电子表格、幻灯片、日历、邮箱、任务、会议等核心业务域，提供200+命令及24个AI Agent Skills | [GitHub](https://github.com/larksuite/cli/blob/main/README.zh.md) |
|  OpenCLI   | 万能命令行工具箱，通用命令行中心与AI原生运行平台，能将任何网站、桌面应用或本地程序变成统一命令行操作界面 | [GitHub](https://github.com/jackwener/opencli)               |
|    CLI     | GitHub 的官方命令行工具。它将拉取请求、问题和其他 GitHub 概念带到终端中，与你已经在使用 `git` 和代码的地方并排显示。 | [GitHub](https://github.com/cli/cli)                         |
| gemini-CLI | Gemini CLI 可将 Gemini 的功能直接引入终端。它提供轻量级的 Gemini 访问方式，能够以最直接的方式从终端命令访问 Gemini 模型。 | [GitHub](https://github.com/google-gemini/gemini-cli)        |

> GitHub上CLI主题推荐网页：[Command-line interface](https://github.com/topics/cli)

### 子Agent（SubAgent）

**创建Agent的两种方式**

1. **自动触发：**任务复杂且存在并行可能时，CC会自动派生子Agent并行推进
2. **手动创建：**通过指令 `/agents` ，在Library界面进行创建
