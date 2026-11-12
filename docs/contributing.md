# Contributing

Contributions should improve an agent's ability to make correct decisions in real Minecraft mod repositories. The project favors a small set of high-value instructions over a large generic manual.

## Acceptable contributions

- A repeated, field-tested failure mode with a concrete prevention strategy.
- A loader/version discovery rule that prevents incorrect API assumptions.
- A better task-routing boundary that reduces irrelevant context.
- A complete-chain checklist for a system not yet covered.
- A more reliable verification decision or failure-classification rule.
- Fixes for broken links, invalid frontmatter, ambiguous instructions, or installation paths.
- Small templates that reduce repeated work without encouraging boilerplate reports.

## What to avoid

- Copying a private mod's gameplay, assets, identifiers, package structure, or implementation.
- A full loader tutorial or API reference that will quickly become outdated.
- Large code examples from one Minecraft version presented as universal.
- Duplicate rules that repeat `SKILL.md` in multiple references.
- Mandatory subagents, broad web research, heavy validators, clean builds, or game launches for every task.
- Claims that a combination of agents/versions is supported without evidence.
- CI or tooling that adds maintenance cost without catching a realistic failure.

## Editing principles

1. Keep `SKILL.md` a dispatcher with essential invariants.
2. Put conditional detail in the most relevant reference.
3. Preserve progressive disclosure: each reference must be readable independently.
4. State version-sensitive facts as version-sensitive.
5. Prefer decision criteria and evidence requirements over copied code.
6. Keep examples loader-neutral unless the file explicitly targets a loader.
7. Do not add a file solely to mirror the suggested directory tree.

## Documentation changes

- Keep `README.md` and `README.zh-CN.md` materially aligned.
- Update `CHANGELOG.md` for user-visible workflow or installation changes.
- Ensure every relative Markdown link resolves.
- Use UTF-8 and standard Markdown/CommonMark.
- Preserve the MIT license and attribution model.

## Skill changes

Before submitting:

- verify YAML frontmatter;
- verify `name: minecraft-mod-developer` matches the folder;
- verify all references linked from `SKILL.md` exist;
- read the full changed reference for contradictions;
- test at least one prompt route conceptually: simple resource task, cross-system task, and verification task;
- keep the main skill from turning into an encyclopedia.

If the validator from a compatible Skill authoring environment is available, run it on `skills/minecraft-mod-developer` and report the exact result. Do not add a bespoke validation framework solely for this repository unless a real maintenance need is demonstrated.

## Pull requests

Describe:

- the concrete problem;
- why the current instruction fails or is expensive;
- the smallest change made;
- evidence from a real project, issue, or observable agent failure;
- checks performed.

Do not include secrets, private project source, licensed assets without permission, or personal filesystem paths. The maintainer decides whether a contribution is sufficiently general.

## Versioning

The project uses semantic versioning for the skill workflow:

- patch: wording, links, small routing or validation fixes;
- minor: new task coverage or supporting references that preserve existing behavior;
- major: incompatible directory/interface changes or a substantially different operating model.

Record changes in `CHANGELOG.md`.
