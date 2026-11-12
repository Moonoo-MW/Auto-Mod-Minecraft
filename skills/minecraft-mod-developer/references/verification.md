# Verification

Use the cheapest evidence that reliably covers the changed risk. More checks are not automatically better; unrun checks must not be described as passing.

## Evidence levels

| Level | What it proves | Typical use |
| --- | --- | --- |
| Inspection | Files and references look consistent | Every change |
| Static validation | Syntax/formats/links pass a checker | Resources, data, generated output |
| Compilation/build | Code compiles and packaging completes | Java, registration, config, dependencies |
| Data generation | Generator completed and outputs exist | Recipes, tags, loot, models, lang when data-gen is used |
| Game launch | The mod loaded without immediate failure | Environment/integration |
| Gameplay check | The requested behavior was observed | Feature acceptance |

Higher levels do not retroactively prove lower-level presentation or feel. A successful build cannot prove a texture looks right or a multiplayer action is authorized correctly.

## Minimum decision matrix

| Change | Minimum verification |
| --- | --- |
| Documentation only | Link/path/content inspection |
| Single JSON/resource | Parse JSON if applicable; check references and local pattern |
| Language-only | Parse files; compare keys across locales |
| Texture/model only | Decode image/model; verify references and expected properties |
| Local Java behavior | Compile or normal build; review call sites |
| Registration/config/lifecycle/network | Normal project build plus targeted compatibility review |
| Data generation | Run the project’s generator; inspect changed output |
| Packaging/release | Build, inspect artifact, verify metadata and critical resources |
| Claims about rendered/gameplay behavior | Real authorized game check |

## Build commands

Read the project rather than assuming syntax:

- use the checked-in Gradle wrapper (`gradlew`, `gradlew.bat`) when Gradle is the build system;
- run from the repository root unless the project documents otherwise;
- preserve the project’s daemon/offline/environment flags;
- do not add `clean` by default; it can remove useful artifacts;
- do not switch to a globally installed build tool merely for convenience.

Common checks include the normal build task and targeted test/resource tasks, but confirm task names from the project.

## Game and data processes

Do not start `runClient`, `runServer`, `runData`, or equivalent processes by default. They can consume significant time/resources and mutate generated files or worlds.

Ask for explicit authorization when:

- the user requests a live test;
- the feature cannot be reasonably validated otherwise;
- generated data must be refreshed and the project policy permits it;
- a crash requires reproduction in the game environment.

Before launching, state what will run, why, and what outputs/files may change.

## Targeted reference checks

For changed content, trace both sides:

1. Registry/ID -> model/blockstate/definition.
2. Model/blockstate -> parent/texture.
3. Sound registration -> sound definition -> audio/subtitle.
4. Item/block -> recipe/loot/tag/advancement references.
5. Packet sender -> receiver -> server validation -> synchronized consumer.
6. Config key -> read path -> reload behavior.
7. Persistent key -> save -> load -> migration/default.

Check only references affected by the change, plus shared keys that would break the packaged feature.

## Failure classification

Classify before modifying:

- **source/API**: compiler error, missing symbol, wrong method signature, loader mismatch;
- **configuration**: Gradle/plugin/toolchain/version or source-set problem;
- **dependency/network**: remote artifact fetch, certificate/connectivity, temporary repository failure;
- **permission/environment**: Java home, file lock, missing tool;
- **resource/data**: JSON, model, texture, language, schema, or reference failure;
- **runtime**: mod load, registry, classloading, or game-logic failure.

Fix according to the cause. Do not downgrade a feature or alter versions to satisfy a transient network issue.

## Final diff review

Review `git diff` and `git status` before reporting. Confirm:

- only intended files changed;
- no secret, personal path, generated cache, or unrelated binary was added;
- naming and paths agree;
- user changes were preserved;
- build artifacts are actually from this run if a release was requested.

## Reporting template

Keep it short:

- Implemented: the behavior.
- Verified: exact check/result.
- Not verified: gameplay, rendering, audio, multiplayer, or real-game state still unobserved.
- Risk: only material unresolved issues.

Never use “fully tested,” “works in game,” or equivalent without the corresponding observed evidence.
