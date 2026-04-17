---
name: paragon-actionkit-skill
description: Skill for using Paragon's ActionKit API for 3rd-party actions like "Send Slack Message" or "Update Salesforce Record"
---

## When to use this skill
Use this Skill to help the user implement ActionKit API calls in their code, by using the references in the Table of Contents

## ActionKit Overview
ActionKit is an API that provides useful abstracted actions that work with your users' integration and run on their behalf.

Some examples of ActionKit actions are: 
- `NOTION_CREATE_PAGE`
- `SALESFORCE_UPDATE_RECORD`
- `SLACK_SEND_DIRECT_MESSAGE`

The full list of available actions can be found in the [ActionKit docs](https://docs.useparagon.com/actionkit/overview.md)

Popular use cases for ActionKit include
1. AI agent tool calling: Expose all actions as function tools to your AI agent over the API or MCP server, with agent-optimized descriptions for tool calling accuracy.

2. Front-end functions: For use cases where your users interact with integration data in your app, like displaying a table of Salesforce opportunities or a button to send a Slack notification.

3. Integrations for your workflow product: Expose all actions as integration nodes in your workflow automation product/feature by wrapping ActionKit’s input schemas.

## Prerequisites
ActionKit is built on top of Paragon's managed authentication and monitoring. When using ActionKit, 
a JWT with a unique end-user ID is required so that Paragon knows who the action should be run on behalf of.

Before helping the user implement ActionKit, it's best to have the Paragon SDK and Connect Portal setup.

- [ ] Check the user's project for "@useparagon/connect" in their `package.json` or references of `paragon.authenticate` in their frontend codebase

- [ ] If a user does not have the Paragon SDK and Connect Portal for authorization set up: Direct them to use the `paragon-setup-skill`

- [ ] If the user does have the Paragon SDK and authorization set up: Proceed with using the resources in the "Table of Contents" to help implement ActionKit

## Table of Contents
- If the user needs to `list actions`: [List Actions docs](https://docs.useparagon.com/actionkit/api-reference/list-actions.md)
- If the user need to execute `run action`: [Run Actions docs](https://docs.useparagon.com/actionkit/api-reference/run-action.md)
- If the user mentions that they would like to use a 3rd-party API that ActionKit does not currently have a pre-built action for: [Use Proxy API](https://docs.useparagon.com/apis/proxy.md)
