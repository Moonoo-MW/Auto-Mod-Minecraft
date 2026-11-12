# Field-Tested Principles

These principles are generalized from a production mod project and are intentionally independent of its mod ID, package names, gameplay, private assets, and loader-specific implementation.

## 1. The repository is the design system

The target project already encodes naming, registration, lifecycle, UI, data, and verification decisions. Consistency with those decisions is usually more valuable than importing a cleaner pattern from elsewhere. Inspect first, then extend.

## 2. A feature is a chain, not a Java class

A gameplay feature commonly spans identifiers, registration, runtime logic, persistence or sync, client presentation, language, models, textures, recipes/loot/tags, and packaging. Decide which layers are genuinely required, then implement all of them together.

## 3. Prefer minimal complete over minimal cosmetic

Adding the visible class while leaving registration, resource wiring, or runtime authority unfinished creates a false sense of completion. The right scope is the smallest set of changes that makes the requested behavior real.

## 4. Preserve one identifier spine

Registry IDs, model directories, texture paths, sound events, translation keys, recipe outputs, and data references should derive from stable, consistent names. Renaming at only one layer creates silent missing-resource and save-compatibility failures.

## 5. Keep authority on the server

Actions that mutate inventory, health, world state, permissions, or other players must be validated and applied server-side. Client packets request actions; they do not grant authority. Client code should present synchronized state.

## 6. Treat client/server separation as an architectural boundary

Do not load client classes on a dedicated server. Separate common payload/data definitions from client handlers, renderers, screens, keys, and visual resources. Test the import graph mentally and with the build.

## 7. Version drift creates false familiarity

Similar classes may have different package names, method signatures, registration APIs, data paths, or lifecycle contracts between loaders and Minecraft versions. Verify the exact project version before copying an example.

## 8. Choose the closest proven pattern

For a new item, inspect existing items; for a screen, inspect an existing menu/screen pair; for data, inspect the project's data gen or handwritten JSON format. This reduces API drift and makes the change easier for maintainers to recognize.

## 9. State has a lifecycle

Persistent state must have a clear owner and survive the events that matter: save/load, entity death or removal, player respawn or dimension change, block unload, server restart, and multiplayer joins. Transient visual state belongs only on the client.

## 10. Localize the interface, not just the item name

Every fixed player-visible string belongs in language resources and is referenced through the localization API. Dynamic text should remain parameterized. Missing one language key is a visible feature defect.

## 11. Use evidence proportional to risk

Resource-only changes may need JSON validation and reference checks. Logic changes may need compilation. Registration, lifecycle, networking, or packaging changes justify a normal build. Actual gameplay correctness still requires a real game session.

## 12. Never upgrade evidence by wording

`BUILD SUCCESSFUL` means the build completed. It does not mean the item appears in-game, the GUI renders, multiplayer sync works, or the behavior is fun. Report exactly what was observed.

## 13. Keep failures diagnostic

When a build fails, classify the failure before editing: source/API, configuration, dependency resolution, network, permissions, or environment. Fix the cause instead of repeatedly rewording code or deleting features to make the error disappear.

## 14. Avoid accidental scope expansion

Unrelated cleanup makes review and rollback harder. Preserve adjacent user edits and limit changes to the requested path unless a shared defect blocks the feature or creates a severe risk.

## 15. Optimize context, not rigor

Cache stable project facts once, search by symbol and call path, read only relevant files, and load guidance on demand. Token efficiency should reduce repeated investigation, not skip required implementation or verification.

## 16. Assets need ownership and provenance

Record where non-original models, textures, sounds, fonts, and structures came from and any conditions of use. Validate actual file format and properties rather than trusting extensions. Do not present generated concept art as an inspected in-game model.

## 17. Compatibility is a claim requiring evidence

Support for a range of Minecraft, loader, or agent versions must be tested or explicitly labeled untested. A skill that discovers versions adaptively is not the same as a library that supports every version.

## 18. Leave a maintainable handoff

The next developer should be able to see what changed, why the approach fits the repository, how to use the feature, what was verified, and what remains uncertain. Concision is useful; omitted uncertainty is not.
