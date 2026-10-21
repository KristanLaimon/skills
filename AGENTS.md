# Personal Agent Skills

This repository is the user's skill collection at `~/.agents/skills`. Maintain practical instructions that reflect the user's preferences and help agents make better decisions.

## Ownership and scope

- Any skill whose folder, name, or display name starts with `KL` (case-insensitive) is hand-crafted or closely curated by the user. Read and use it when relevant, but do not edit, rename, move, delete, regenerate, or overwrite it or its supporting files unless the user explicitly requests that change. A general cleanup or update request is not permission.
- Treat user-curated preferences as intentional. Do not replace them with another author's tastes or generic best practices.
- Keep changes limited to the requested skills. Preserve unrelated work and third-party attribution and licenses.
- `.skill-lock.json` tracks installed skills; let the installer manage it when installing or updating them.

## Authoring skills

Follow [Agent Skills authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices).

- Put each skill in `skills/<skill-folder>/SKILL.md`, with YAML frontmatter containing `name` and a precise `description` of when to use it. Use kebab-case for new names.
- Keep `SKILL.md` concise and actionable. Include decision rules, non-obvious constraints, and practical patterns; omit introductions, installation guides, and knowledge the agent already has.
- Explain when and why a pattern applies. Include working code examples when they clarify a coding task.
- Add supporting files only when needed. Link references directly from `SKILL.md` and explain when to read them; avoid copying whole manuals or duplicating instructions.
- For documentation-derived skills, synthesize authoritative sources and record source links and the version or revision used. Do not invent source metadata or attribute the user's preferences to Anthony Fu.
- Read the existing skill and relevant sources before changing it. Preserve its scope and invocation behavior unless the user asks to change them.

## Review

- Check frontmatter, local links, and whether the instructions handle realistic requests without adding unrelated work or permissions.
- Run any available skill validator and test executable examples or scripts that changed. For prose-only edits, review the content and diff; do not add a test harness.
- Report what changed, what was checked, and any remaining limitation briefly.
