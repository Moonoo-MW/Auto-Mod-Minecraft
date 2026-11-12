# Repository Instructions

This repository packages a reusable Agent Skill. Preserve its portability and progressive-disclosure design.

## Required reading for changes

Read `skills/minecraft-mod-developer/SKILL.md` and only the references relevant to the requested edit. Keep root documentation aligned with the skill's actual behavior.

## Rules

- Do not copy private gameplay, identifiers, package names, source, textures, sounds, or data from the project that inspired this skill.
- Do not make Forge, NeoForge, Fabric, or a single Minecraft version the implicit default. Version-sensitive guidance must be labeled and verified.
- Keep `SKILL.md` concise. Move conditional detail into a reference and link it from the task router.
- Do not add a file unless the workflow needs it.
- Keep `README.md` and `README.zh-CN.md` materially equivalent.
- Maintain valid Agent Skill frontmatter; `name` must match the skill directory.
- Use UTF-8 text.
- Do not claim runtime validation, agent compatibility, or in-game verification without evidence.
- Installation instructions must not overwrite an existing same-named skill or execute unreviewed scripts.
- Do not add CI, generated scaffolds, or full API catalogs unless the user explicitly requests them and a concrete need exists.
- Do not commit, push, publish, or create releases unless the user explicitly requests that external action.

## Validation

For a normal skill change:

1. Check YAML frontmatter and directory naming.
2. Check every relative link in `SKILL.md` and READMEs.
3. Read changed references for contradictions or duplicated policy.
4. If a compatible Skill quick validator is available, run it.
5. Review `git status` and `git diff` before reporting.

No game or Minecraft build is required to validate this documentation repository.
