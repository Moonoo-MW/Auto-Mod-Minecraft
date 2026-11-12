---
name: minecraft-mod-developer
description: Develop, extend, debug, and verify Minecraft mods in an existing project by discovering the exact loader and Minecraft version first, then implementing the needed Java, data, assets, UI, networking, and build checks. Use for Forge, NeoForge, Fabric, and related mod work where behavior must fit the repository's existing architecture.
license: MIT
---

# Minecraft Mod Developer

Turn a natural-language mod request into a working, repository-consistent change. Discover the project before choosing APIs, then implement the smallest complete feature path and verify only what the change requires.

## Operating contract

- Read the target repository's agent instructions and inspect its current state before editing.
- Preserve uncommitted work. Never discard or overwrite likely user changes.
- Treat the project's Minecraft version, mod loader, mappings, Java version, wrapper, and existing conventions as authoritative. Forge, NeoForge, Fabric, and different Minecraft versions do not share stable APIs.
- Prefer extending existing registration, data, UI, networking, and asset patterns. Do not introduce a new framework or architecture to solve a local problem.
- Do not upgrade dependencies, change versions, start a game client/server, or perform broad refactors unless the user explicitly asks.
- Implement the whole required chain. A registered item without a usable model, texture, language entry, and intended data is usually incomplete.
- Keep client-only classes and server-authoritative logic separated. Validate untrusted network input on the server.
- Report only checks actually run. Compilation, static resource checks, data generation, client launch, and in-game behavior are different evidence levels.

## Start here

1. Look for `.moddev/PROJECT_CONTEXT.md`. If present, trust it only where it still matches the current build files and source.
2. For a new or materially changed project, read [project-discovery.md](references/project-discovery.md) and perform a bounded architecture scan.
3. Read [development-workflow.md](references/development-workflow.md) for the shared execution loop.
4. Inspect the closest existing implementation and its call sites before designing the change.

Do not read every reference by default. Load only the references that match the task.

## Route by task

| Task signal | Read |
| --- | --- |
| Unknown project, first request, build-system uncertainty | [project-discovery.md](references/project-discovery.md) |
| Any feature, fix, integration, or refactor | [development-workflow.md](references/development-workflow.md) |
| Item, block, block entity, entity, effect, interaction, world behavior, command, advancement | [gameplay-systems.md](references/gameplay-systems.md) |
| Recipe, loot, tag, model, blockstate, data generation, language, sound registration | [minecraft-resources.md](references/minecraft-resources.md) |
| Screen, menu, HUD, renderer, animation, texture, shader, visual/sound polish | [assets-and-ui.md](references/assets-and-ui.md) |
| Build, test selection, packaging, compatibility review, release evidence | [verification.md](references/verification.md) |
| Architecture tradeoff, repeated failure, scope pressure, or extracting lessons | [field-tested-principles.md](references/field-tested-principles.md) |

When a task spans systems, read the minimum set and follow the data flow across them: registration -> runtime behavior -> synchronization/persistence -> resources -> verification.

## Decision rules

### Reuse before invention

Search for the same concept before creating a new registry, packet, menu, config, data codec, renderer, or utility. If an existing pattern is incomplete, extend it at the narrowest shared point. If a new shared abstraction would replace unrelated systems, stop at the requested scope unless the refactor is necessary for correctness.

### Implement for the real game loop

Identify who initiates the action, where authority lives, how state is stored, what the client needs to render it, how it is localized, and what happens on load, unload, death, respawn, dimension change, disconnect, and multiplayer. Add only the lifecycle cases that the feature can actually encounter.

### Use version-correct sources

Prefer, in order: working code in the target repository, APIs already used there, official documentation/source for the exact version and loader, then trusted same-version examples. Do not infer an API from another loader or a newer Minecraft version.

### Ask only when the answer changes the implementation

Use reasonable low-risk defaults from the repository. Ask before irreversible operations, dependency/version changes, destructive migrations, or choices that materially alter gameplay or public compatibility.

## Completion standard

Before finishing:

- Re-read the changed files and relevant call sites.
- Check registration IDs, resource paths, language keys, data references, and client/server boundaries touched by the change.
- Run the project wrapper's relevant targeted checks and, when the change can affect compilation or packaging, the normal build.
- Review the final diff for unrelated edits.
- State what works from code/build evidence and what still needs a real game session.

Keep the delivery concise: implemented behavior, important files/systems, usage, verification result, and remaining risk.
