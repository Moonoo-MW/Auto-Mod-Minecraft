# Gameplay Systems

Use the sections relevant to the requested behavior. The architecture concepts are stable; class names, packages, registration APIs, data paths, and lifecycle methods are version- and loader-specific.

## Common rule

Before creating a system, locate the project's existing registration and runtime pattern. Determine:

- who owns state;
- what runs only on the logical server;
- what the client may predict or render;
- what persists across saves or reconnects;
- what packets or synchronized data are needed;
- what happens when the owner unloads, dies, changes dimension, or disconnects.

## Items

Implement the item only if its behavior needs one. Check:

- registration and creative visibility;
- `use`, use duration/animation, stack size, durability, rarity, and tooltip;
- right-click on block/entity/air and both interaction hands;
- server-side authorization and inventory mutation;
- cooldowns, sound/particles, advancements, and recipes;
- item model, texture, language entries, tags, loot if obtainable from world data.

For consumables, account for creative mode, remaining/returned containers, partial use, and server authority. For equipment/wearables, inspect how the target version exposes slots/attributes and how client rendering is synchronized.

## Blocks and block entities

For a block, identify:

- block properties, collision/interaction shape, state properties, placement/rotation;
- render type and sound type;
- block entity necessity;
- ticker side and lifecycle;
- drops/loot, tool tags, recipes, worldgen suitability;
- comparator/container/redstone behavior if applicable.

For a block entity:

- registration and valid-block set;
- save/load format and versioning;
- client synchronization and update packets;
- tick scheduling and removal;
- inventory/item-handler behavior if it stores items;
- save compatibility and safe handling of missing/invalid data.

For multi-block structures, define one authoritative anchor, placement validation, break/cleanup behavior, duplicate-drop prevention, and what happens if a part is missing or unloaded.

## Entities and AI

Define:

- entity type, category, dimensions, tracking range, update interval;
- attributes and spawn placement;
- goals/behaviors and priority conflicts;
- persistence, owner/team data, NBT or codec serialization;
- damage, death/drop, despawn, chunk unload, dimension transfer;
- client model, renderer, animation, sounds, particles, and spawn egg if needed.

Keep navigation/goal mutation on the server. Limit expensive target scans and pathfinding. Test behavior in packs or with other entities where relevant.

## Effects, attributes, damage, and interactions

Inspect the exact version’s effect and attribute APIs. Decide whether the feature is a mob effect, attribute modifier, timed state, or persistent custom mechanic; they differ in stacking, removal, persistence, and UI.

For damage or combat:

- identify damage source/type and whether vanilla protections should apply;
- avoid repeated per-tick application without a clear cadence;
- prevent recursion and feedback loops;
- distinguish attacker, owner, projectile, and environmental attribution;
- handle death, invulnerability frames, PvP rules, and multiplayer.

## Menus, screens, and HUDs

Use the project’s existing menu/screen pattern.

- Open server-side from the correct context.
- Send only the data the client needs; do not send a whole inventory/disassembled world if a compact payload suffices.
- Validate button actions and slot changes server-side.
- Keep screen classes in the client source set.
- Update localized labels and handle resize, mouse coordinates, keyboard behavior, and controller input where the project supports it.
- Restore GUI scale/state safely and close server-driven screens when the backing object disappears.

HUDs should derive from synchronized client state and avoid rebuilding expensive resources every frame.

## Networking

For every packet/payload, define:

- direction (client to server, server to client, both);
- owner and lifecycle;
- schema and size limits;
- validation performed on receipt;
- thread handoff to the game/main thread;
- protocol-version impact and backward compatibility;
- what happens if the sender or target is absent.

Never trust coordinates, IDs, amounts, or permissions from the client. Confirm the player can access the referenced block/entity, owns the capability, is close enough, and satisfies cooldowns/config constraints.

Use the loader-appropriate registration and codec system already present in the project. Avoid adding a second networking stack.

## Recipes, loot, tags, and data generation

First determine whether the project hand-writes JSON or uses a data generator.

- A custom recipe/serializer needs registration, codec/parse-serialization, matching logic, and recipe JSON.
- Loot must align with block/entity registration and drop ownership.
- Tags use the correct registry namespace and data path for the exact Minecraft version.
- Data generation requires running the project’s generator and committing/using the generated resources according to repository policy.

Do not invent a data format from a different version. Inspect an existing file and the deserializer/codec.

## Config and persistent data

Match the loader’s current config system. Decide whether the value is common, server, client, startup, or reloadable, then verify reads after reload.

For player/world/entity persistence, prefer the target version’s current data attachment/capability/codec mechanism instead of adding an unrelated global map. Include:

- unique owner key;
- initialization and copy-forward behavior;
- save/load and missing-field defaults;
- cleanup to avoid leaks;
- synchronization boundaries.

Capabilities/attachment substitutes must be registered at the correct lifecycle point and copied on respawn if the data should survive it.

## Commands, advancements, and events

Commands should validate permissions, arguments, target existence, and return useful localized failure messages. Avoid blocking world scans; bound search radius/count.

Advancement triggers/conditions and event listeners must be registered once. Search for existing dispatch points before adding a per-tick global scan.

Prefer the narrowest event with the correct side and phase. Beware cancellation semantics and running destructive logic for both client and server.

## World generation and structures

Identify whether the project uses JSON worldgen, code-based features, or both. Verify registration IDs, biome modifiers/tags, placement values, structure templates, loot, and dimension restrictions against the exact version.

For imported structures:

- record source and license/usage notes;
- inspect and remove hostile spawners/entities, command blocks, and unintended technical blocks;
- adapt loot and palette to the project;
- test rotation/mirroring and placement collisions where possible from files and tooling.

Do not claim generated world content is in-game verified without launching the relevant world.
