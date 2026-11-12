# Project Discovery

Use this reference on the first request in a project, after major build/logic changes, or whenever the loader/version is uncertain. Keep the scan bounded.

## 1. Establish the project boundary and safety state

- Find the repository root and read its agent instructions.
- Check `git status --short`, current branch, and the target files before editing.
- Note uncommitted changes and protect them. Do not reset, clean, or overwrite unrelated work.
- Identify generated, build, cache, and vendored directories; exclude them from source searches when they add noise.

## 2. Read project facts from files

Start with the smallest useful set:

- `gradle.properties`, `build.gradle`, `build.gradle.kts`, `settings.gradle`, `settings.gradle.kts`, wrapper properties.
- `mods.toml`, `neoforge.mods.toml`, `fabric.mod.json`, `quilt.mod.json`, or the loader metadata actually present.
- Root README/agent docs if they describe build or architecture.
- Existing `.moddev/PROJECT_CONTEXT.md`, if present.

Extract:

- Minecraft version and mod loader plus loader version.
- Java toolchain/language level and mappings.
- Wrapper version and canonical build command.
- Mod ID, root package, source-set layout, entry points.
- Client/server entry points and optional integrations.
- Whether data generation, mixins, access wideners/transformers, or custom build steps exist.

Do not infer a loader from a single import. Confirm it from build metadata and entry points.

## 3. Map the architecture by examples

Use file names and symbol search rather than reading every source file.

Look for:

- Registration holders and the event bus registration sequence.
- One representative item, block, block entity, entity, effect, menu/screen, packet, recipe, config, and persistent-data implementation.
- Client-only package boundaries and client event registration.
- Resource roots under `src/main/resources` and, if present, generated resource roots.
- Language, model/blockstate, texture, sound, recipe, loot, tag, advancement, and worldgen trees.
- Existing tests and resource-validation scripts.

Search terms vary by version. Useful signals include `DeferredRegister`, `Registry.register`, `@Mod`, `ModInitializer`, `ClientModInitializer`, `RegisterEvent`, `SimpleChannel`, `PayloadTypeRegistry`, `MenuType`, `BlockEntityType`, `DataGenerator`, and loader-specific config APIs.

## 4. Confirm the runtime chain

For the relevant feature type, trace from entry point to behavior:

1. Where is it registered?
2. Where is it constructed or loaded?
3. What mutates its state?
4. Is the state common, server-only, client-only, saved, or synchronized?
5. Which resource or data IDs make it visible/usable?
6. Which build or validation step catches its most likely breakage?

## 5. Decide whether to cache context

When the project is unfamiliar or likely to receive several tasks, create a short `.moddev/PROJECT_CONTEXT.md` using [../templates/project-context.md](../templates/project-context.md).

Cache stable facts and conventions only:

- version, loader, Java, wrapper/build command;
- mod ID/root package/entry points;
- authoritative registration and resource locations;
- client/server and networking patterns;
- existing validation commands;
- known constraints that are not obvious from file names.

Do not cache secrets, machine-specific personal paths, source dumps, changing TODO lists, or conclusions that have not been verified. Refresh only changed fields when build files or architecture move.

## 6. Avoid over-discovery

Stop when you can confidently answer:

- Am I editing the correct project and version?
- Which existing pattern should this feature follow?
- What files and resources must change?
- How will I verify this class of change?

If those answers are clear, begin implementation instead of producing an architecture report.
