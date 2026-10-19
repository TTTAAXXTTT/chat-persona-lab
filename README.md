# 聊语实验室 · Chat Persona Lab

**从聊天记录了解自己的表达习惯，用自己的风格写出可以亲自审阅的回复草稿。**

以本地处理为起点的 Python 项目：导入聊天文本与结构化人格档案，建立按联系人隔离的历史索引，统计本人表达风格，再根据当前对话生成候选回复。可先完全离线体验，再自行配置云端模型。

## 下载完整项目

**[下载 v0.1.0 完整源码包](chat-persona-lab-v0.1.0-source.zip?raw=true)** · [源文件清单](source-manifest.json) · [SHA-256 校验](SHA256SUMS.txt) · [English](README.en.md)

本仓库当前以完整源码 ZIP 分发。下载并解压后，进入其中的 `chat-persona-lab` 目录运行；以下命令均在解压后的项目根目录执行。ZIP 包含全部 70 个项目文件：程序、中文网页、回归测试、中英文介绍、详细说明、虚构示例、配置模板及维护脚本。

版本 **0.1.0 / 实验阶段**。默认关闭云端处理与真实发送；离线演示使用固定测试模型，只验证流程。公开项目不含真实聊天、私人人格档案、账号配置、密钥、数据库或原项目 Git 历史。

## 功能

| 功能 | 实现 |
| --- | --- |
| 聊天导入 | 约定格式的 UTF-8 文本，保留时间、说话方、会话与来源 |
| 本地分析 | 消息数量、本人/对方占比、回复示例、时间范围及风格统计 |
| 人格资料 | 本地读取 XLSX，按用途、关系和确认状态选取档案条目 |
| 风格记忆 | 从本人消息统计长度、标点、节奏及筛选后的常用表达 |
| 联系人隔离 | 只取当前联系人的上下文和历史示例，关系由本人标注 |
| 草稿工作台 | 中文本地网页，输入来信、修改草稿、逐条确认或拒绝 |
| 可选云端 | DeepSeek 官方 / OpenCode Go，自行提供 Key 并明确启用 |
| 消息集成 | JSONL inbox/outbox，默认不向外发送 |
| 微信实验 | 可选 Windows 桌面读取、受控单联系人发送、全局只读隔离 |

不包含微信数据库解密、自动提取历史、语音识别或模型训练。聊天分析使用统计与检索，人格资料由使用者填写；不把模型推测当作已确认事实。

## 三步体验

安装 **Python 3.11+**，解压源码并进入项目根目录。核心功能仅使用 Python 标准库。

```sh
python scripts/create_demo.py
python -m persona_agent --analyze-only
python -m persona_agent --offline-fake-model
```

打开 [本地工作台](http://127.0.0.1:8765)，选择虚构联系人，输入“早上好”，尝试修改、确认和拒绝草稿。默认确认结果为 `approved_not_sent`，不会发送到微信。退出服务按 `Ctrl+C`。

Windows 可在生成演示资料后双击 `run-offline.cmd`。生成器不会覆盖已有 `.env` 或档案。全部示例均为专门编写的虚构内容。

## 使用自己的资料

将聊天文本和 XLSX 放入本地 `private/` 或仓库外，修改 `.env`：

```dotenv
PROFILE_XLSX_PATH=private/profile.xlsx
CHAT_EXPORT_DIR=private/chats
CHAT_ADAPTER=jsonl
SEND_ENABLED=false
ALLOW_CLOUD_PROCESSING=false
```

目录只识别 `*_messages.txt`，也可用 `CHAT_EXPORT_FILES` 以分号列出文件。需符合源码包 `docs/DATA_FORMAT.md` 的约定格式，不能直接传入任意平台数据库。先运行分析核对数量，再启动页面检查联系人。

## 可选云端生成

在自己的本地 `.env` 配置密钥，并明确开启云端处理，再运行 `python -m persona_agent`：

```dotenv
MODEL_PROVIDER=deepseek_official
DEEPSEEK_API_KEY=在本地填写自己的密钥
ALLOW_CLOUD_PROCESSING=true
```

代码当前允许 `deepseek-v4-flash`、`deepseek-v4-pro`；实际模型权限和可用性以供应商为准。OpenCode Go 使用独立密钥与供应商确认配置，详见源码包配置说明。

开启后，选中的当前消息、相关人格条目、当前联系人上下文和示例、风格统计可能发送给供应商。正则过滤无法保证匿名化；项目不承诺供应商的留存政策、训练政策或价格。

## 完整项目目录

```text
chat-persona-lab/
  persona_agent/       Python 核心、适配器、HTTP 服务与中文网页
  examples/            虚构聊天、JSONL；XLSX 由生成器创建
  scripts/             演示、HTTP 冒烟测试、隐私检查、源码打包
  tests/               146 项自动化测试，微信和模型网络使用替身
  docs/                上手、格式、配置、隐私、微信、架构、发布文档
  .github/             CI、issue 与 PR 模板
  .env.example         空密钥、安全默认值
  README.md            完整中文说明
  README.en.md         英文介绍
  LICENSE              MIT
```

## 验证与边界

```sh
python -m unittest discover -s tests -v
python scripts/check_public.py
python scripts/smoke_test.py
python scripts/build_release.py
```

首次发布已在本地通过 **146 项回归测试**，并在源码包干净解压后验证了分析、HTTP 启动、令牌拒绝、草稿生成及审批不外发。没有完成新的真实微信账户/客户端或在线供应商验收。源码包内提供 CI 配置；当前 ZIP 分发仓库不直接运行包内工作流。

SQLite、备份和聊天导出可能包含明文隐私，只在本地保管。服务仅回环访问，请勿开放到公网。真实微信自动化受第三方版本、授权和窗口状态影响，属于实验功能；发送结果不确定时禁止自动重试。

详细说明均在完整源码包的 `docs/` 中，包括快速上手、数据格式、配置参考、隐私数据流、微信限制、架构和发布步骤。问题反馈请只使用虚构数据，不附密钥、真实聊天、数据库或账号截图。

后续方向：更多导出格式、分析报告可视化、通用模型接口、加密存储与平台验证。这些尚未作为本版已完成功能。

[更新记录](CHANGELOG.md) · [第三方说明](NOTICE.md) · [MIT License](LICENSE)
