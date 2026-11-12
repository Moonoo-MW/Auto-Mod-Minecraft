# Auto-Mod-Minecraft

一个面向 Minecraft 模组开发的开放 Agent Skill。它让兼容的 AI 智能体通过自然语言完成模组需求，同时坚持先读项目、后选 API，并把代码、资源、数据与必要验证作为一个完整功能来处理。

这不是 Minecraft 教程，也不是一组可以盲目复制的 Java 类。它是一套经过真实模组项目验证、再去项目化后的开发工作流。

## 解决什么问题

Minecraft 模组开发常见失败并不是“不会写一个类”，而是：

- 把 Forge、NeoForge、Fabric 或不同 Minecraft 版本的 API 混在一起；
- 注册了物品，却遗漏模型、贴图、语言、配方或战利品；
- 客户端直接改服务端权威状态，多人游戏出现不同步或漏洞；
- 为了修一个局部问题而升级依赖、重写架构或扫描整个工程；
- 编译成功后就宣称功能已在游戏内验证。

Auto-Mod-Minecraft 将这些经验抽象成可迁移的工作流：识别项目与版本、复用现有实现、补齐完整链路、按风险验证、如实汇报证据。

底层经验来自真实 Forge 项目，但本仓库不包含该模组的源码、贴图、资源、Mod ID、包名或私有玩法。MoonooMod 不是本 Skill 的依赖项目。

## 能力范围

Skill 可按任务处理：

- Item、Block、BlockEntity、Entity、Effect、AI、属性、战斗与交互；
- GUI、Screen、Menu、HUD、Renderer、Animation、Particle、Shader；
- Recipe、Loot、Tag、Model、Blockstate、Language、Sound、Data Generation；
- Config、存档数据、Capability/Attachment、Command、Advancement、Worldgen、Structure；
- Client/Server 边界、网络同步、多人兼容与旧存档兼容；
- 当前项目自身的构建、资源校验与打包流程。

它不会假定加载器 API 通用，而是先从工程文件、入口类、依赖和现有调用点确认实际环境。

## 安装

推荐用户级安装，让多个模组工程共用同一个 Skill。

可直接把下面请求交给兼容的智能体：

> 请安装这个 GitHub 仓库提供的 Minecraft Mod 开发 Skill：https://github.com/Moonoo-MW/Auto-Mod-Minecraft 。先阅读 README，根据当前智能体环境，将 `skills/minecraft-mod-developer` 安装到用户级 Skills 目录。不要把整个仓库复制到我的模组项目，不要执行未经审查的脚本，也不要覆盖已有同名技能。

常见用户级目录：

| 智能体/环境 | 用户级目录 |
| --- | --- |
| Codex | `~/.agents/skills/`；Windows 为 `%USERPROFILE%\.agents\skills\` |
| DeepSeek Harness | 优先 `~/.agents/skills/`；文档化路径还包括 `.dsh/skills/` |

项目级安装通常使用 `.agents/skills/`。详细的手动步骤、同名检查和失败处理见 [installation.md](docs/installation.md)。

## 使用

安装后，在模组仓库中开启新会话，直接描述目标、约束和期望交互，例如：

```text
增加一个带小型存储空间和菜单的方块，必须能在专用服务端正常工作。
增加一个会在 16 格内追击敌对生物、带攻击冷却的实体。
检查现有网络包是否存在权限、同步或多人问题，并修复已确认的问题。
按照附件设计实现物品，并把必要的代码和资源全部接好。
```

Skill 第一次进入陌生项目时会做有限调查：构建配置、入口、注册方式、同类实现和对应资源。之后使用项目自己的 Wrapper 与验证方式，不启动未经授权的游戏进程。

## Skill 的工作方式

`skills/minecraft-mod-developer/SKILL.md` 是精简调度器，不会一次性加载全部知识。它会按任务选择相关参考：

| 参考文件 | 适用场景 |
| --- | --- |
| [project-discovery.md](skills/minecraft-mod-developer/references/project-discovery.md) | 陌生项目、构建环境或版本不确定 |
| [development-workflow.md](skills/minecraft-mod-developer/references/development-workflow.md) | 功能、修复、有限范围重构 |
| [gameplay-systems.md](skills/minecraft-mod-developer/references/gameplay-systems.md) | 玩法、UI 行为、网络、数据、配置、持久化 |
| [minecraft-resources.md](skills/minecraft-mod-developer/references/minecraft-resources.md) | 模型、贴图、语言、声音、配方、战利品、标签 |
| [assets-and-ui.md](skills/minecraft-mod-developer/references/assets-and-ui.md) | GUI、HUD、渲染、动画、视觉与声音 |
| [verification.md](skills/minecraft-mod-developer/references/verification.md) | 选择检查、定位失败、构建、打包 |
| [field-tested-principles.md](skills/minecraft-mod-developer/references/field-tested-principles.md) | 架构取舍与重复故障 |

需要时可生成简短的 `.moddev/PROJECT_CONTEXT.md`，只缓存版本、入口、注册方式、资源根目录与构建命令等稳定事实，并在架构变化时局部更新。

## Token 控制

1. 主 `SKILL.md` 只保留路由、底线和统一流程。
2. 详细内容按任务读取，不默认加载全部 references。
3. 项目事实按需缓存，减少反复扫描。
4. 优先按符号、文件名和调用关系检索。
5. 简单任务不写正式设计报告。
6. 默认不创建子 Agent、不做广泛联网搜索、不跑大型无关验证器。
7. 构建与游戏进程按证据价值选择，而不是每次机械全测。

节省 Token 不等于省略必要代码、资源接线或关键验证。

## 支持范围

本仓库遵循开放 Agent Skills 目录结构：技能目录内包含带 YAML frontmatter 的 `SKILL.md`，并可附带 references/templates。各智能体仍可能使用不同的发现路径、调用策略和 frontmatter 扩展。

目标场景包括 Codex 风格的用户级/项目级 Skills、DeepSeek Harness 已文档化的 Skills 位置，以及支持导入 Agent Skill 文件夹的其他环境。本仓库不宣称兼容所有 Agent、所有 Minecraft 版本或所有模组模板。详见 [supported-platforms.md](docs/supported-platforms.md)。

## 已知局限

- API 会随 Minecraft、加载器和映射变化；Skill 要求针对实际版本核对，但不能消除第三方兼容问题。
- 数据生成、客户端启动、专用服务端、多人同步、视觉与音频质量通常需要目标工程和用户授权。
- 不同智能体的安装路径与 Skill 发现机制可能变化。
- 本 Skill 提升流程与决策质量，不替代项目知识、编译器诊断或真实游戏测试。

## 贡献与许可

贡献要求见 [docs/contributing.md](docs/contributing.md)。请提交可迁移、非显而易见、真正改变实现决策的经验，避免复制私有模组内容或堆叠重复规则。

采用 MIT License，见 [LICENSE](LICENSE)。
