---
name: skill-authoring
description: >-
  Guide for creating new GitHub Copilot agent skills. Use this skill when asked
  to create, write, or author a new skill for Copilot agents.
---

# Skill Authoring Guide

Use this guide whenever you are asked to create a new GitHub Copilot agent skill.

## What is a skill?

A skill is a folder of instructions (and optionally scripts or resources) that Copilot loads when relevant to improve its performance in specialized tasks. Each skill lives in its own subdirectory under a `skills` directory (e.g., `.github/skills/<skill-name>/`) and must contain a `SKILL.md` file.

## Skill structure

```
.github/skills/<skill-name>/
├── SKILL.md          # required — instructions and frontmatter
└── <optional files>  # scripts, examples, or supplementary markdown files
```

## `SKILL.md` format

Every `SKILL.md` must start with YAML frontmatter followed by a Markdown body.

### Required frontmatter fields

- **`name`** — A unique lowercase identifier using hyphens for spaces. Must match the skill's directory name.
- **`description`** — A concise description of what the skill does and the specific trigger phrase or condition that tells Copilot when to invoke it (e.g., "Use this when asked to…").

### Optional frontmatter fields

- **`license`** — License that applies to the skill content.
- **`allowed-tools`** — List of tools Copilot may use without asking for confirmation each time (e.g., `shell`, `bash`). Only include `shell` or `bash` if you have reviewed all referenced scripts and fully trust them.

### Example frontmatter

```yaml
---
name: my-skill
description: >-
  Describe what the skill does and when to use it. Use this when asked to…
allowed-tools: shell
---
```

## Writing the skill body

The Markdown body contains the instructions Copilot should follow when the skill is invoked. Write instructions that are:

1. **Clear and actionable** — Use numbered steps for processes and bullet points for guidelines.
2. **Self-contained** — The skill body should include everything Copilot needs to complete the task without relying on external context beyond what is in the current repository.
3. **Repo and project agnostic** — Skills must be general-purpose and not reference specific repositories, file paths, company names, internal tools, or project-specific configuration. They should work correctly when copied into any repository with the relevant technology or context.

## Creating a new skill — step by step

1. **Identify the skill's purpose.** Determine the specific task or domain the skill addresses. Keep the scope focused — one skill per concern.

2. **Choose a name.** Use lowercase letters and hyphens only (e.g., `react-best-practices`, `api-documentation`, `database-migrations`). The name should reflect the task, not a project or team.

3. **Create the directory.**
   ```
   .github/skills/<skill-name>/
   ```

4. **Write the `SKILL.md` file.** Include:
   - YAML frontmatter with `name` and `description`.
   - A Markdown body with clear, step-by-step instructions.

5. **Add optional supporting files** if the skill needs scripts or reference material. Reference them by filename in the `SKILL.md` body.

6. **Review for project-specific content.** Before finalizing, scan the skill for:
   - References to specific repository names, organization names, or URLs.
   - Hard-coded file paths or internal tool names.
   - Any assumption that only applies to one project.
   
   Replace any such content with general patterns or placeholders.

## Quality checklist for every new skill

- [ ] `SKILL.md` is present in the skill's directory.
- [ ] Frontmatter includes `name` and `description`.
- [ ] `name` matches the directory name (lowercase, hyphens only).
- [ ] `description` states clearly when Copilot should invoke the skill.
- [ ] The skill body uses clear, numbered or bulleted instructions.
- [ ] No references to specific repositories, organizations, or internal tools.
- [ ] No hard-coded secrets, tokens, or credentials.
- [ ] Any scripts referenced in the body are included in the skill directory.
- [ ] If `allowed-tools` includes `shell` or `bash`, all referenced scripts have been reviewed.

## Example skill

Below is a complete example of a well-formed, project-agnostic skill for writing commit messages.

**File:** `.github/skills/conventional-commits/SKILL.md`

```markdown
---
name: conventional-commits
description: >-
  Guidelines for writing conventional commit messages. Use this when asked to
  write, review, or fix a commit message.
---

# Conventional Commits

Follow the Conventional Commits specification (https://www.conventionalcommits.org) when writing commit messages.

## Format

\`\`\`
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
\`\`\`

## Types

- **feat** — a new feature
- **fix** — a bug fix
- **docs** — documentation changes only
- **style** — formatting, missing semicolons, etc.; no logic change
- **refactor** — code change that neither fixes a bug nor adds a feature
- **test** — adding or correcting tests
- **chore** — build process, dependency updates, tooling

## Rules

1. Use the imperative mood in the description ("add feature" not "added feature").
2. Do not capitalize the first letter of the description.
3. Do not end the description with a period.
4. Keep the subject line under 72 characters.
5. Use the body to explain *what* and *why*, not *how*.
6. Reference issue numbers in the footer using `Closes #<issue>` or `Fixes #<issue>`.
```
