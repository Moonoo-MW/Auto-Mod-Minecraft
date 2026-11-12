# Minecraft Resources

Minecraft mods combine code registries with resource and data file trees. Exact folder names and JSON schemas change by Minecraft version and loader. Inspect the target project before writing a path.

## Find the authoritative patterns

Search for one working example of each needed type:

- item registration and its model/texture/lang;
- block registration, blockstate, block model, item model, and loot;
- sound-event registration, `sounds.json`-equivalent definition, audio file, and subtitle;
- recipe/loot/tag JSON in the project’s supported schema;
- particle or effect registration and visuals;
- data-generator providers, if present.

Reuse the project’s namespace, directory depth, asset naming, indentation, and data conventions.

## Keep the identifier spine aligned

For namespace `modid` and content path `example_path`, expect consumers such as:

| Layer | Typical identity |
| --- | --- |
| Java registration | `modid:example_path` |
| Item/block model | `assets/modid/models/.../example_path.json` |
| Texture | `assets/modid/textures/.../example_path.png` |
| Blockstate | `assets/modid/blockstates/example_path.json` |
| Translation | `item.modid.example_path`, `block.modid.example_path`, etc. |
| Data | `data/modid/<data-type>/example_path.json` |

The exact tree may differ. Check the actual project and version; do not rename one layer in isolation.

## Item checklist

Use only the rows that apply:

- Java registration and creative-tab placement.
- Item model.
- PNG texture with the expected format and transparency.
- Name and tooltip translation keys.
- Recipe, loot, trades, tags, advancement, or world sources.
- Interaction, durability, container, cooldown, sound, or particle behavior.
- Client model properties, layers, or custom renderer.

For a simple generated item model, inspect an existing sibling rather than copying a model from another mod.

## Block checklist

Use only the rows that apply:

- Block and optional block-item registration.
- Blockstate variants for every state value that can exist.
- Block model and item model.
- Texture files and transparency/render-layer requirements.
- Loot table and required tool tags.
- Recipe and creative-tab placement.
- Collision/selection shape, facing/rotation, and placement rules.
- Block entity type, ticker, renderer, sync, and saves.
- Sounds, particles, interactions, GUI, or comparator behavior.

Missing blockstate combinations can make placed blocks appear missing or crash during state baking. Enumerate the actual property combinations.

## Models and blockstates

- Keep model parent paths valid for the project version.
- Ensure every blockstate `model` and model `parent` points to an existing JSON or a known vanilla resource.
- Ensure model `textures` point to existing PNG files unless the parent supplies them.
- Check rotation/mirroring and item transforms on custom geometry.
- For multipart/multi-block content, verify each state, anchor, and item model.

## Language resources

Store all fixed player-visible strings in language files, not in Java/UI code. Maintain every language the project supports.

- Naming: item/block/entity/effect/menu keys follow the loader/version convention.
- Tooltips: add explicit keys and use the project’s translatable component helper.
- UI: localize labels, buttons, errors, empty states, and config text.
- Dynamic values: use translation placeholders instead of concatenating complete sentences.
- Subtitles: add a key for every custom sound event.

When comparing language files, compare keys as well as JSON validity. Different key sets are usually a bug; different values are expected.

## Sound resources

Use the project’s existing custom sound-event path. Typically this requires:

1. A registered sound event.
2. The namespace’s sound definition file.
3. Valid audio files under the expected sounds directory.
4. A subtitle/translation key.
5. A world or UI playback call appropriate to the context.

Do not copy one audio file to fabricate variants. High-frequency actions may deserve distinct variants if the project’s sound system supports random selection. Keep world sounds spatial; use UI playback APIs for menu sounds. Avoid restarting looped music every tick.

## Recipe, loot, and tag data

Data paths and formats are version-sensitive:

- Inspect a working project file of the same type.
- Confirm registry names in result/output and ingredient/tag references.
- Verify tags use the right item/block/entity registry and directory.
- Verify loot table references from block/entity definitions.
- If data generation exists, modify providers and run the project’s generator rather than editing both source and generated output inconsistently.

Do not assume newer component, recipe, or loot schemas apply to older Minecraft versions.

## Particles and effects

For a custom particle, wire registration, client factory/provider, definition JSON if required, texture, and emission. For an effect, wire registration, translation, icon/texture, application/removal, color/category as supported, and stacking behavior.

## Asset quality and provenance

- Keep candidate/source art separate from production assets when the project does.
- Verify dimensions, PNG validity, and actual alpha data when transparency matters.
- Match the repository’s pixel-art and scaling conventions; do not assume every texture type has one universal size.
- Record source, author, license/permission, and purpose for third-party assets.
- Do not claim generated concept art is a reviewed 3D/game asset.

## Resource verification

For the changed resource set, verify:

- JSON syntax and no accidental duplicate keys;
- referenced model/texture/data files exist;
- language keys exist in every supported locale;
- registered IDs have the expected models/definitions;
- sound/audio files are valid;
- textures decode correctly and match their intended transparency;
- generated data is current if the project uses data generation.

Use the repository’s validator when present. Otherwise write only a small targeted check if it materially improves confidence.
