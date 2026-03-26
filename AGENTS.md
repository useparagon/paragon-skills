# AGENTS.md - Paragon Skills Repository

This is a documentation repository for the Paragon Skill used by agentic coding agents.

## Repository Overview

This repository contains skill files that help agentic IDEs (Cursor, Claude, opencode, etc.) set up Paragon integrations for users. Skills are markdown-based documentation stored in the `skills/` directory.

## Build/Lint/Test Commands

**This repository has no build system, tests, or linting.** It contains only markdown documentation files.

- No `package.json`, `Cargo.toml`, or other build configuration
- No automated testing framework
- No linting tools configured

## Content Guidelines

### Skill Structure

Each skill must follow this directory structure:

```
skills/
  └── {skill-name}/
      ├── SKILL.md           # Main skill entry point (required)
      └── references/          # Supporting documentation
          ├── {topic}.md
          └── ...
```

### SKILL.md Format

SKILL.md files must include YAML frontmatter:

```yaml
---
name: {skill-name}
description: Brief description of what this skill does
---
```

### Writing Style

- Use clear, concise language
- Include step-by-step checklists with `- [ ]` format
- Provide code examples in TypeScript/JavaScript
- Use inline comments to explain code snippets
- Keep paragraphs short and scannable

### Code Examples

- Use TypeScript for type safety demonstrations
- Include both inline examples and full implementation examples
- Always specify language for code blocks (```typescript, ```bash, etc.)
- Use realistic example data (UUIDs, actual integration names)

### References

- Store detailed documentation in `references/` subdirectory
- Use relative links between files: `[Link text](references/file.md)`
- Cross-reference related skills when applicable

### Formatting Standards

- Use ATX-style headers (`# Header` not `Header\n===`)
- Max 2 consecutive blank lines
- No trailing whitespace
- Use `-` for unordered lists
- Number ordered lists manually for readability

## Common Tasks

### Adding a New Reference Document

1. Create file in `skills/{skill-name}/references/`
2. Link from SKILL.md with relative path
3. Follow existing document structure

### Updating Skill Content

1. Edit SKILL.md for high-level changes
2. Edit reference files for detailed changes
3. Ensure all code examples remain valid
4. Update README.md if installation steps change

## Important Notes

- This is a PUBLIC repository - never include sensitive data
- Skills are consumed by multiple agentic IDEs - keep content IDE-agnostic
- The `assets/` directory contains images referenced by README.md
- Follow the existing Paragon Skill as a template for new skills
