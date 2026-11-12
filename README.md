# Auto-Mod-Minecraft

An open Agent Skill for developing Minecraft mods through natural language without losing sight of the actual repository.

Project site: <https://moonoo-mw.github.io/Auto-Mod-Minecraft/>

Auto-Mod-Minecraft teaches a compatible coding agent to inspect the project first, identify the exact Minecraft/loader/toolchain combination, reuse the project's architecture, implement the full gameplay and resource chain, and report verification honestly. It is a development workflow, not a collection of copy-paste mod classes or a Minecraft tutorial.

## Why this exists

Minecraft mod work breaks easily when an agent assumes the wrong API, adds Java without models/translations/data, or treats a successful compile as proof that a feature works in game. This skill turns those project-specific lessons into a reusable workflow:

- discover before coding;
- follow the repository's registration and resource conventions;
- implement code, data, localization, UI, sounds, and lifecycle behavior as one feature;
- keep server authority and client presentation separate;
- run targeted checks instead of indiscriminate scans;
- distinguish static validation, build success, game launch, and observed gameplay.

The workflow was distilled from a real Forge mod project, but this repository contains no mod-specific source, assets, package names, or private gameplay content. MoonooMod is not a dependency.

## What it can do

The skill routes tasks across:

- items, blocks, block entities, entities, effects, AI, attributes, combat, and interactions;
- menus, screens, HUDs, client state, renderers, animation, particles, and shaders;
- recipes, loot, tags, models, blockstates, language files, sounds, and data generation;
- config, persistence, capabilities/attachments, commands, advancements, worldgen, and structures;
- client/server architecture, networking, multiplayer behavior, and save compatibility;
- project-specific build and validation workflows across Forge, NeoForge, Fabric, and related environments.

APIs are not assumed to be portable. The skill reads the target project's Minecraft version and loader before choosing an implementation pattern.

## Install

The recommended installation is user-level so the same skill can serve multiple mod projects.

Paste this request into a compatible agent:

> Please install the Minecraft mod development Skill provided by this GitHub repository: [https://github.com/Moonoo-MW/Auto-Mod-Minecraft](https://github.com/Moonoo-MW/Auto-Mod-Minecraft). Read the README first. Based on the current agent environment, install `skills/minecraft-mod-developer` into the user-level Skills directory. Do not copy the entire repository into my mod project, do not execute unreviewed scripts, and do not overwrite an existing skill with the same name.

Common user-level roots:

| Agent/environment | User-level root |
| --- | --- |
| Codex | `~/.agents/skills/` (`%USERPROFILE%\.agents\skills\` on Windows) |
| DeepSeek Harness | `~/.agents/skills/` preferred; `.dsh/skills/` is also documented |

Project-level discovery commonly uses `.agents/skills/`. See [installation.md](docs/installation.md) for manual steps, collision checks, verification, and failure handling.

## Use

After installation, start a new agent session in the Minecraft mod repository and describe the outcome, constraints, and any expected interaction.

Examples:

```text
Add a block that stores a small inventory, opens a menu, and works on a dedicated server.
Add an entity that chases nearby hostile mobs, but only within 16 blocks and with a combat cooldown.
Review my current networking code for authority and multiplayer bugs, then fix the confirmed issues.
Implement the item described in the attached design and wire all required resources.
```

The skill's first pass is intentionally bounded: entry points, build configuration, one or two existing implementations of the same concept, and the relevant resource paths. It then follows the project's own pattern and uses the repository's wrapper for validation.

## Supported platforms

The repository follows the open Agent Skills layout: a folder named after the skill containing `SKILL.md` with YAML frontmatter and optional supporting files. Each agent still controls its discovery paths, invocation behavior, supported frontmatter, and installation mechanism.

This repository targets:

- Codex-style user/project Skills discovery;
- DeepSeek Harness user/project installations where documented;
- other agents that support importing an Agent Skill folder.

It does **not** claim universal compatibility with every agent, Minecraft version, loader, or mod template. See [supported-platforms.md](docs/supported-platforms.md).

## How the skill works

`skills/minecraft-mod-developer/SKILL.md` is the compact dispatcher. It establishes safety and version rules, then links only the references relevant to the current task:

| Reference | Loaded when |
| --- | --- |
| [Project discovery](skills/minecraft-mod-developer/references/project-discovery.md) | The project or build environment is unfamiliar or changed |
| [Development workflow](skills/minecraft-mod-developer/references/development-workflow.md) | Any feature, fix, or scoped implementation |
| [Gameplay systems](skills/minecraft-mod-developer/references/gameplay-systems.md) | Items, blocks, entities, UI behavior, networking, data, config, persistence, world behavior |
| [Minecraft resources](skills/minecraft-mod-developer/references/minecraft-resources.md) | Models, textures, language, sounds, recipes, loot, tags, data generation |
| [Assets and UI](skills/minecraft-mod-developer/references/assets-and-ui.md) | Screens, HUDs, renderers, animation, visual effects, audio |
| [Verification](skills/minecraft-mod-developer/references/verification.md) | Choosing checks, reviewing failures, building, packaging |
| [Field-tested principles](skills/minecraft-mod-developer/references/field-tested-principles.md) | Architecture tradeoffs and repeated failure modes |

The optional `.moddev/PROJECT_CONTEXT.md` cache records only stable project facts such as loader/version, entry points, registration style, and build commands. It is designed to be updated selectively, not regenerated on every task.

## Token-consumption design

The skill is optimized for useful context rather than short instructions at any cost:

1. The dispatcher is small and route-based.
2. Detailed procedures live in task-specific references.
3. Project facts are cached rather than rediscovered indefinitely.
4. Search starts with symbols, file names, and direct call paths.
5. Simple tasks do not require a formal design report.
6. No subagents, broad internet research, or large validators are required by default.
7. Builds and game launches are selected by risk and evidence value.

This reduces repeated scanning and boilerplate while preserving the code, resource, and verification work required for a real feature.

## Known limitations

- Loader and Minecraft APIs change; the skill directs the agent to verify exact-version APIs but cannot guarantee every third-party integration.
- Data generation, client launch, dedicated-server testing, multiplayer behavior, visual polish, and audio quality ordinarily require the target project and sometimes explicit authorization.
- Installation paths and skill-discovery behavior vary by agent version.
- The skill improves decision-making and workflow scope; it does not replace project knowledge, compiler diagnostics, or real gameplay testing.

## Contributing

See [docs/contributing.md](docs/contributing.md). Contributions should add transferable, non-obvious guidance, preserve progressive disclosure, and avoid copying private mod content or loader-specific code without a demonstrated need.

## License

MIT. See [LICENSE](LICENSE).
