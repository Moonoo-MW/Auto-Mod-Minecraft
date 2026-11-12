# Assets and UI

Use this reference for visual, audio, GUI, renderer, animation, and shader work. Follow the target project’s style and APIs; do not transplant visuals or implementation from the skill’s source project.

## Visual asset workflow

1. Inspect existing assets of the same type and the exact rendering use.
2. Confirm the required dimensions, background/alpha behavior, color style, and file format.
3. Create or edit the asset.
4. Verify the file at original scale before integrating it.
5. Wire the model/blockstate/atlas/renderer reference.
6. Check the final packaged resource path.

For pixel art:

- draw at the intended logical resolution instead of downscaling an anti-aliased image;
- keep intentional hard edges and limited colors;
- check transparent pixels in the actual alpha channel;
- use integer scaling for previews;
- distinguish ordinary item icons, effect icons, GUI elements, particles, and custom model textures; their conventions can differ.

Do not assume square textures or power-of-two dimensions are required everywhere. Determine the renderer/format constraint.

## UI discovery

Find the closest existing menu/screen pair or HUD renderer. Identify:

- which data is server-owned;
- how the menu/capability state reaches the client;
- how actions travel back to the server;
- how the screen handles inventory slots, labels, scrolling, hover, and keyboard input;
- how the project defines colors, fonts, backgrounds, and scale;
- whether a custom renderer or vanilla widget pattern is preferred.

## GUI layout

Build for the actual screen-size range supported by Minecraft and the project.

- Calculate positions relative to the screen or menu origin; avoid hardcoded assumptions that only work at one scale.
- Keep hitboxes and rendering from diverging.
- Clip or scroll long lists.
- Handle empty, loading, disabled, and error states.
- Use explicit tooltips for icon-only actions where appropriate.
- Keep server-side menus authoritative for slot movements and mutations.
- Close or synchronize safely if a server-side container/object disappears.

Include all user-visible strings in language files. Do not hide missing localization by constructing English/Chinese sentences inside render code.

## HUD and client state

- Read compact synchronized state, not server-only objects.
- Avoid expensive font layout, texture allocation, recipe scans, or data parsing every frame; cache stable results.
- Respect GUI scale, hidden/debug states, and project config toggles.
- Clear transient state on disconnect/world unload.
- Keep rendering deterministic and side-effect-free.

## Entity and block entity renderers

- Register the renderer on the client only.
- Ensure model/texture paths and layer definitions exist.
- Separate animation state from authoritative gameplay state.
- Handle missing entities/block entities and render distances safely.
- Avoid mutating saved data from a renderer.
- Validate custom geometry visually or with model inspection; compilation does not validate shape/appearance.

## Particles and animation

Define the event cadence and visual owner. Client-only particles should not be emitted through server calls if the project pattern sends an effect packet instead. Bound particle counts and lifetimes, especially for area attacks or high-frequency interactions.

Synchronize only the seed/phase/position/data needed for a deterministic effect. Avoid duplicating identical animations for every nearby player unless intended.

## Screen effects and camera work

Respect client settings and accessibility. Keep effects short, layered intentionally, and restorable on screen close/world change. Do not permanently modify camera/FOV state. If multiple systems alter the camera, compose offsets rather than overwriting one another blindly.

## Shaders

Confirm the exact render pipeline and resource location support for the project’s Minecraft version. Verify shader files, uniforms, fallback behavior, and interaction with common renderer mods when relevant. A shader that compiles in another version/pipeline is not a reliable template.

## Sounds and music

- Use registered custom sound events where the project requires them.
- Put playback on the correct side and source/category.
- World sounds should preserve spatial attenuation unless intentionally UI/global.
- UI sounds should use the client UI path.
- Music loops should be owned by one manager and stop/transition cleanly.
- Provide subtitles for custom sounds.

## Visual and audio verification limits

Static checks can prove a file decodes, a path exists, and a model reference is syntactically valid. They cannot prove:

- the icon is readable at game scale;
- the GUI fits every supported resolution;
- the model is aligned;
- animation timing feels correct;
- audio has the right loudness or no clipping;
- the feature works in a real client.

Label those items as needing in-game or human review unless they were actually checked.
