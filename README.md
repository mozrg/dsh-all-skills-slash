# All skills in the Harness slash menu

A small DeepSeek Harness bundle that enables local Agent Skills in the standard Web GUI `/` menu.

The bundle uses Harness's built-in `dsh-skill-filesystem` provider and `dsh-tool-skill` consumer. It does **not** create one custom command per skill: the supported `/skill-name` gesture remains owned by Harness, so built-in commands are not shadowed and skill instructions are loaded through the canonical skill path.

## What it enables

- Local skills from the normal Harness project and user roots.
- Skills from a checkout's `.agents/skills` directory.
- The nested `ui-ux-pro-max-skill-2.15.0/.claude/skills` and `cli/assets/skills` roots when that checkout is present.
- Filesystem watching, so added or changed `SKILL.md` files can appear after the catalog refreshes.
- The built-in skill loader and `/` suggestions in the Web GUI.

## Requirements

- DeepSeek Harness with the `dsh-skill`, `dsh-skill-filesystem`, `dsh-tool-skill`, and Web UI packages available.
- A profile with permission to install a local bundle.
- Local skills in the default roots or in the directory configured by `DSH_SKILLS_DIR`.

## Install from a clone

Clone this repository, then install the bundle from its directory with the Harness plugin manager:

```powershell
git clone https://github.com/mozrg/dsh-all-skills-slash.git
# In Harness, install the cloned directory as a local bundle:
# plugin_manager(action: "install_bundle", target: "C:\\path\\to\\dsh-all-skills-slash")
```

The plugin manager is the preferred installation path because it writes the profile bundle state and dependencies correctly. Do not copy the patch into the profile by hand.

## Skill checkout path

By default, the patch looks for the skill checkout at:

```text
<user-home>/deepseek-harness/.agents/skills
```

For another location, set `DSH_SKILLS_DIR` before starting or restarting Harness. For example:

```powershell
$env:DSH_SKILLS_DIR = 'D:\\work\\deepseek-harness\\.agents\\skills'
```

The patch evaluates this variable when the profile mounts. Restart the profile after changing it.

## Use it

1. Open the Harness Web GUI.
2. Focus the message composer and type `/`.
3. Select a skill, such as `/frontend-design`, `/ui-ux-pro-max`, or `/dsh-code-review`.
4. Add your request and send it.

Only skills whose frontmatter allows user invocation are shown. A skill directory must contain a valid `SKILL.md` with YAML frontmatter including `name` and `description`.

## Skill file format

```markdown
---
name: example-skill
description: Use this skill for example tasks.
---

Instructions for the skill go here.
```

Use `user-invocable: false` to keep a skill out of the human `/` menu. Use `disable-model-invocation: true` when it should not be available to the model's skill tool.

## Files

- `package.json` — local bundle manifest and display metadata wiring.
- `cordis.patch.yml` — enables the built-in skill provider and loader.
- `locale/` — English and Russian plugin metadata.
- `icon.svg` — original plugin artwork.

## License

MIT. See [LICENSE](LICENSE).
