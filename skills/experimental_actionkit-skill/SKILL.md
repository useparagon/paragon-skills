---
name: paragon-actionkit-skill
description: Skill for using Paragon's ActionKit API for 3rd-party actions like "Send Slack Message" or "Update Salesforce Record"
---

## ActionKit Overview
ActionKit is an API that provides useful abstracted actions that work with your users' integration and run on their behalf.

Some examples of ActionKit actions are: 
- `NOTION_CREATE_PAGE`
- `SALESFORCE_UPDATE_RECORD`
- `SLACK_SEND_DIRECT_MESSAGE`

The full list of available actions can be found in the [ActionKit docs](https://docs.useparagon.com/actionkit/overview)

Popular use cases for ActionKit include
1. Tools for AI agents
2. Steps for Workflow Builders
3. UI components like buttons that should perform a 3rd-party action

## When to use this skill
Use this Skill to help the user implement ActionKit API calls in their code, by using the references in the Table of Contents

## Table of Contents
- If a user does not have the Paragon SDK and Connect Portal for authorization set up: Direct them to use the `paragon-setup-skill`
- If the user needs to `list actions`: [List Actions docs](https://docs.useparagon.com/actionkit/api-reference/list-actions.md)
- If the user need to execute `run action`: [Run Actions docs](https://docs.useparagon.com/actionkit/api-reference/run-action.md)
- If the user mentions that they would like to use a 3rd-party API that ActionKit does not currently have a pre-built action for: [Use Proxy API](https://docs.useparagon.com/apis/proxy.md)
