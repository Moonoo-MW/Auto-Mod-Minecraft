# Development Workflow

Use this loop for feature work, fixes, integrations, and scoped refactors. Keep the loop proportional to the task.

## A. Project identification

For a new project or changed toolchain, follow [project-discovery.md](project-discovery.md). For a local follow-up, reuse the verified project context and inspect only changed or newly relevant files.

The minimum working set is the target implementation, its registration/call sites, directly used resources/data, and the project's verification path.

## B. Understand the request

Extract the observable outcome:

- What the player/developer does.
- What should happen immediately and over time.
- What triggers, limits, costs, cooldowns, permissions, or failure states exist.
- What must persist or synchronize.
- Which visible text, sounds, models, particles, or UI states are needed.
- Whether multiplayer, dedicated server, old saves, or existing datapacks matter.

Resolve discoverable details from code. Use sensible defaults for low-risk gaps. Ask only when an unresolved choice changes behavior, compatibility, data safety, or irreversible actions.

For a simple request, do not write a formal design document. A short change map is enough.

## C. Plan the smallest complete change

1. Find existing implementations of the same concept.
2. Select the closest pattern.
3. List the required layers: registration, runtime, persistence/sync, client rendering, resources/data, verification.
4. Identify persistent IDs and compatibility risks.
5. Decide what is intentionally out of scope.

Use a compact change map:

| Question | Answer |
| --- | --- |
| Closest existing pattern | File/system to extend |
| Entry point/registration | Where the feature joins the game |
| Authority/state owner | Server/common/client and persistence |
| Player-visible resources | Model, texture, lang, sound, UI, data |
| Required validation | Targeted checks and/or build |

For unfamiliar cross-system work, send a brief progress update before a long implementation pass.

## D. Implement end to end

Work in coherent increments:

- Add stable identifiers and registration.
- Implement runtime behavior and validation.
- Add persistence and synchronization where needed.
- Add client handlers/renderers only on the client side.
- Wire models, textures, language, recipes, loot, tags, sounds, or data.
- Add or update the nearest existing tests/validators when the project has them.

Keep unrelated formatting and renames out of the diff. Do not delete user-authored files to simplify the change.

### Preserve existing behavior

When modifying shared code, search all callers. If changing a shared contract, update every in-scope caller and consider old saves, network protocol compatibility, config values, and resource IDs. If the shared change is not necessary for the request, use a narrower local extension.

### Handle unknown APIs

When an API is uncertain:

1. Find an existing target-project call.
2. Inspect the exact dependency/source available to Gradle.
3. Check official documentation/source for the same loader and Minecraft version.
4. Use a trusted same-version example only if the first three are insufficient.

Do not guess a method signature and iterate blindly through compiler errors when the exact type/source can be inspected.

## E. Verify with evidence

Follow [verification.md](verification.md). At minimum:

- Review the diff and direct call sites.
- Check IDs, paths, language keys, and data references touched by the change.
- Compile/build when Java, registrations, dependencies, or packaging can be affected.
- Run project-specific validators when they exist and fit the change.

Do not run `runClient`, `runServer`, `runData`, or equivalent game/data processes by default. Ask for explicit authorization when a real session is needed or the task specifically requests it.

## F. Deliver

Report:

- Implemented behavior.
- Important systems/files or architecture points changed.
- How to use it.
- Verification actually performed and its result.
- What still needs in-game, multiplayer, visual, or audio testing.

Include paths or artifact locations when the user requested a build/release. Do not call a compile-only result gameplay-verified.

## Change-depth guidance

### Local edit

Examples: fix one interaction, add a language key, adjust a texture reference.

Inspect the class, its callers, and the linked resource. Use targeted checks and a build only when the change can affect code or registration.

### Feature extension

Examples: new item, entity ability, recipe type, config option.

Map the complete feature chain, copy the closest in-project pattern, and build/validate the affected layers.

### Cross-system change

Examples: networking protocol, data component/storage model, menu architecture, renderer pipeline.

Trace producer and consumer, lifecycle, compatibility, and failure handling. Expand investigation only as evidence requires.

### Unknown defect

Start from the error, stack trace, reproduction conditions, and recent diff. Narrow the failing stage before reading unrelated systems. Do not “fix” by upgrading versions or disabling features.
