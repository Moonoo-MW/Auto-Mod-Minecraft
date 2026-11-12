# Supported Platforms

## Open Agent Skill format vs. host support

This repository uses the open folder-based Agent Skills shape:

- one directory per skill;
- a required `SKILL.md` with YAML frontmatter;
- `name` matching the directory;
- optional references, templates, scripts, and assets used by the workflow.

That format is portable. Skill discovery, invocation policy, metadata extensions, installation paths, and available tools are host-specific.

## Agent hosts

### Codex

Documented Codex discovery includes user-level `$HOME/.agents/skills` and repository-level `.agents/skills` locations. Codex also supports automatic skill selection and explicit invocation in supported versions.

Official reference: <https://developers.openai.com/codex/skills>

The documentation for this repository targets that general behavior. It does not claim compatibility with every Codex release or every organization-managed discovery scope.

### DeepSeek Harness

The documented target locations are:

- user-level preferred: `~/.agents/skills/`
- additional user-level location: `.dsh/skills/`
- project-level: `.agents/skills/`

Host versions can differ. Verify that the installed version discovers the chosen root.

### Other compatible agents

Any host that can ingest an Agent Skill folder may install it. Examples include hosts that support filesystem skill directories, a Skills import UI, or a system prompt packaged from a selected skill.

The following are host responsibilities, not guarantees made by this repository:

- automatic trigger selection;
- support for the optional `license` frontmatter field;
- whether referenced files are readable on demand;
- whether external repository access is available;
- whether project-level skill roots are allowed.

## Minecraft and mod-loader support

The skill is loader-adaptive rather than API-universal. It can guide work in Forge, NeoForge, Fabric, and related projects by discovering the actual toolchain and following existing project code.

It does not ship mod implementations or compile-time libraries. Supported behavior depends on:

- the target Minecraft version;
- the loader and loader version;
- mappings;
- Java toolchain;
- source-set/Mixin/access-widener setup;
- availability of exact-version documentation or project examples.

Forge, NeoForge, Fabric, Quilt, and different Minecraft versions may use different registration, networking, data, rendering, and lifecycle APIs. Cross-loader code must use the project's own abstraction or build setup; this skill does not assume source compatibility.

## Validation status

The files in this repository are statically validated for structure and links. The skill has not been runtime-tested in every listed agent, Minecraft version, loader, or operating system. Claims about a specific environment should be treated as compatibility targets until verified there.

## Reporting a platform issue

Include:

- agent/host name and version;
- operating system;
- chosen skill root and actual detected path;
- whether `SKILL.md` is listed;
- the shortest prompt that triggers the problem;
- whether references can be opened;
- relevant error text with secrets removed.
