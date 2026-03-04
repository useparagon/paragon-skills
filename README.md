<p align="center">
  <a href="https://www.useparagon.com/" target="blank"><img src="https://raw.githubusercontent.com/useparagon/aws-on-prem/master/assets/paragon-logo-dark.png" width="150" alt="Paragon Logo" /></a>
</p>

<p align="center">
  <b>
    The embedded integration platform for developers.
  </b>
</p>

---

# Paragon Skills
Agents can build integrations in just minutes with Paragon Skills. The main Paragon Skill
provides your agent context with everything it needs to setup Paragon, add the Connect 
Portal to authenticate your users, and get started with ActionKit and/or Managed Sync 
to build any integration use case.

![Paragon Skill](/assets/opencode-skill.png)

## Getting Started

### Installing via the Skills CLI
The `npx skills` CLI by vercel automatically adds the Paragon Skill for the most popular agentis IDEs.
See the manual installation section if needed.
```
npx skills add useparagon/paragon-skills
```


### Manual installation
1. Clone this repo

2. Copy the `skills/paragon-skill` directory

3. Navigate to the **skills** directory of your agentic IDE

**For Cursor**
```
~/.cursor/skills/
```

**For Claude Code**
```
~/.claude/skills
```

**For Opencode**
```
~/.config/opencode/skills
```

4. Paste the `paragon-skill` directory into your IDE's `skills` directory

## Prompt the Skill
You agent should naturally detect when the Paragon Skill is needed.

In some cases, it may be useful to reference the skill directly by name (i.e. "Using the Paragon Skill...").

![Paragon Skill Trigger](/assets/opencode-skill-trigger.png)
