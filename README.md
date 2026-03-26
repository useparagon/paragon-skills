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
Agents can **setup your integrations** in just minutes with Paragon Skills. The Paragon Skill provides your agent context with everything it needs to:

1. Setup the Paragon SDK 

2. Add the Connect Portal to authenticate your users 

3. Get started with [ActionKit](https://useparagon.com/product/actionkit) and/or [Managed Sync](https://useparagon.com/product/managed-sync) to build any integration use case.

If you're setting up Paragon for the first time, use this Skill to get up and running with your integrations.

![Paragon Skill](/assets/opencode-skill.png)

## Getting Started

### Installing via the Skills CLI (Recommended method)
The `npx skills` CLI by vercel automatically adds the Paragon Skill for the most popular agentic IDEs.
See the manual installation section if needed.
```
npx skills add useparagon/paragon-skills
```

Update the Paragon Skill with the following command

```
npx skills update
```


### Manual installation (Skip this if installing via Skills CLI)
1. Navigate to the **skills** directory of your agentic IDE

```javascript
//for example
~/.cursor/skills/
~/.claude/skills/
~/.agents/skills/
~/.config/opencode/skills/
```

2. Clone this repo

## Start Prompting With The Skill
You agent should naturally detect when the Paragon Skill is needed.

In some cases, it may be useful to reference the skill directly by name (i.e. "Using the Paragon Setup Skill...").

### Starter Prompts

```markdown
Set up the Paragon SDK in my project
```

```markdown
Build an integration card for Google Drive
```

```markdown
Set up auth for my Paragon integrations
```

