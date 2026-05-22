---
name: paragon-headless-connect-skill
description: Skill for implementing a headless implementation of Paragon using Paragon's SDK to authenticate and connect users to integrations
---

## When to use this skill
Use this Skill when the user asks for a **headless implementation of Paragon Connect**. Paragon's Headless Connect uses Paragon's SDK to connect and configure the end-users' integrations, but without using Paragon's pre-built Connect Portal UI. That way, the user can build their own UI components while still leveraging Paragon to manage 3rd-party authentication and credential storage.

If the user wants Paragon's pre-built Connect Portal UI, use the `paragon-setup-skill`

## Prerequisites
Headless Connect is implemented with the Paragon SDK. Before implementing, the user should have:

- [ ] `Paragon_project_id` and `Signing_key` available in their project (The `.env` file is a good place for storing these variables).

- [ ] A server-side method for creating a JWT signed with your Paragon `Signing_key`

If these pre-requisites are not fufilled, use the `paragon-setup-skill` to guide the user through getting their `Paragon_project_id` and `Signing_key`

## How to implement Headless Connect
- Read these docs for an overview: [Headless Connect Overview](https://docs.useparagon.com/connect-portal/headless-connect-portal.md)
- If the user is implementing dropdowns, text inputs, toggles, etc, read the [Headless Connect Input Reference](https://docs.useparagon.com/connect-portal/input-types-reference.md)
- Explore this codebase for a full end-to-end example of Headless Connect: [Example repo](https://github.com/useparagon/connect-headless-example/tree/main)
