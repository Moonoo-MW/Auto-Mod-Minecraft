# Installation

Install only the `skills/minecraft-mod-developer` folder. The repository contains public documentation and license files that are useful for review, but the skill does not need them at runtime.

## Preferred natural-language install

Send this to a compatible agent:

> Please install the Minecraft mod development Skill provided by this GitHub repository: https://github.com/Moonoo-MW/Auto-Mod-Minecraft. Read the README first. Based on the current agent environment, install `skills/minecraft-mod-developer` into the user-level Skills directory. Do not copy the entire repository into my mod project, do not execute unreviewed scripts, and do not overwrite an existing skill with the same name.

The agent should:

1. Identify its supported user-level Skills root.
2. Check whether `minecraft-mod-developer` already exists.
3. Stop and report the collision instead of silently overwriting it.
4. Obtain the repository through its supported Git/GitHub method.
5. Copy the single skill folder and its references/templates.
6. Report the final path and any discovery/restart requirement.

Do not grant installation success if download, permissions, or file copying fails.

## Typical discovery roots

### Codex

Codex scans repository and user skill locations. Current Codex documentation lists:

- user: `$HOME/.agents/skills`
- repository: `.agents/skills` from the current working directory toward the repository root.

Official reference: <https://developers.openai.com/codex/skills>

Windows user path: `%USERPROFILE%\.agents\skills`.

Older or customized installations may use another Skills root. Trust the current environment's documented discovery path if it differs.

### DeepSeek Harness

The intended roots are:

- user-level preferred: `~/.agents/skills/`
- additional user-level form: `.dsh/skills/`
- project-level: `.agents/skills/`

Choose the root supported by the installed Harness version. If the environment cannot discover either path, report that rather than pretending installation succeeded.

### Other Agent Skills hosts

Place the folder at the host's documented skill-discovery root. The runtime layout is:

```text
<skills-root>/
└── minecraft-mod-developer/
    ├── SKILL.md
    ├── references/
    └── templates/
```

The containing directory name must remain `minecraft-mod-developer` to match the skill name.

## Manual installation

### 1. Review first

Read:

- `skills/minecraft-mod-developer/SKILL.md`
- the referenced files relevant to your intended use
- `LICENSE`

The core skill has no installer script or executable payload.

### 2. Check for a collision

Inspect `<skills-root>/minecraft-mod-developer`. If it exists:

- review and back it up yourself; or
- choose another compatible root; or
- stop and resolve ownership manually.

Never replace an existing skill merely because the names match.

### 3. Copy the one skill folder

PowerShell example, after reviewing the downloaded repository:

```powershell
$skillsRoot = Join-Path $HOME '.agents\skills'
$target = Join-Path $skillsRoot 'minecraft-mod-developer'
$source = 'C:\path\to\Auto-Mod-Minecraft\skills\minecraft-mod-developer'

if (Test-Path -LiteralPath $target) {
    throw "A skill already exists at $target. Review it before installing."
}

New-Item -ItemType Directory -Path $skillsRoot -Force | Out-Null
Copy-Item -LiteralPath $source -Destination $skillsRoot -Recurse
```

This creates no script and performs no remote write. Do not copy the repository root into a mod source tree.

### 4. Verify discovery

Start a new agent session or refresh the host's skill index. Confirm the skill is listed or can be invoked as `minecraft-mod-developer`. Ask the agent to summarize when it would load the gameplay and verification references; this checks routing without modifying a project.

## Updating

Check for a newer release, review the diff, and replace only the installed skill folder after resolving any local modifications. If the host has no explicit update flow, perform the same collision-aware manual process.

## Troubleshooting

- **Skill not listed:** verify the containing folder name, `SKILL.md` location, frontmatter, and the host's actual discovery root.
- **Host uses a different path:** follow the host's current documentation; do not create duplicate copies in every possible directory.
- **Download or copy denied:** report the exact failure and stop.
- **Existing skill found:** do not overwrite; report the path and ask the user to choose.
- **Skill loads but routes poorly:** report a minimal reproduction and contribute a narrowly scoped fix.
