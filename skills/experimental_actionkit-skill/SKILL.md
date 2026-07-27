---
name: paragon-actionkit-skill
description: Skill for using Paragon's ActionKit API for 3rd-party Tools and Triggers, Tools for CRUD-like operations in a 3rd-party API and Triggers for subscribing to 3rd-party webhooks
---

## When to use this skill
Use this Skill to help the user implement ActionKit API calls in their code, by using the references in the Table of Contents

## ActionKit Overview
ActionKit exposes thousands of pre-built Tools (actions in a third-party system) and Triggers (events in a third-party system) across integrations through one consistent interface.

Some examples of ActionKit Tools are: 
- `NOTION_CREATE_PAGE`
- `SALESFORCE_UPDATE_RECORD`
- `SLACK_SEND_DIRECT_MESSAGE`

Some examples of Action Triggers are: 
- `NOTION_PAGE_CREATED`
- `SALESFORCE_RECORD_CREATED`
- `SLACK_MESSAGE_RECEIVED`

The full list of available Tools and Triggers can be found in the [ActionKit docs](https://docs.useparagon.com/actionkit/overview.md)

Popular use cases for ActionKit include
1. AI agent tool calling: Expose all actions as function tools to your AI agent over the API or MCP server, with agent-optimized descriptions for tool calling accuracy.

2. Front-end functions: For use cases where your users interact with integration data in your app, like displaying a table of Salesforce opportunities or a button to send a Slack notification.

3. Integrations for your workflow product: Expose all actions as integration nodes in your workflow automation product/feature by wrapping ActionKit’s input schemas.

## Implementation notes
- [ ] For AI agent tool calling, prefer `format=json_schema` from the `List Tools` endpoint so the returned schema can be passed directly to the model.
- [ ] For end-user configuration UIs such as workflow builders, prefer `format=paragon` and render the returned Paragon input types instead of assuming raw JSON Schema widgets.
- [ ] Expect polymorphic tools in `json_schema` format to split generic actions into integration-specific variants (for example, `SALESFORCE_CREATE_RECORD` becomes tools like `SALESFORCE_CREATE_OPPORTUNITY` and `SALESFORCE_CREATE_ANY`).
- [ ] When running a polymorphic tool, either the generic tool name or the split-out tool name can be used as long as the required parameters are supplied.

## Prerequisites
ActionKit is built on top of Paragon's managed authentication and monitoring. When using ActionKit, 
a JWT with a unique end-user ID is required so that Paragon knows who the action should be run on behalf of.

Before helping the user implement ActionKit, it's best to have the Paragon SDK and Connect Portal setup.

- [ ] Check the user's project for "@useparagon/connect" in their `package.json` or references of `paragon.authenticate` in their frontend codebase

- [ ] If a user does not have the Paragon SDK and Connect Portal for authorization set up: Direct them to use the `paragon-setup-skill`

- [ ] If the user does have the Paragon SDK and authorization set up: Proceed with using the resources in the "Table of Contents" to help implement ActionKit

## Table of Contents
### Tools API
- If the user needs to `list Tools`: [List Tools docs](https://docs.useparagon.com/actionkit/api-reference/list-tools.md)
- If the user need to execute `run Tool`: [Run Tools docs](https://docs.useparagon.com/actionkit/api-reference/run-tool.md)
- If the user mentions that they would like to use a 3rd-party API that ActionKit does not currently have a pre-built action for: [Use Proxy API](https://docs.useparagon.com/apis/proxy.md)

### Triggers API
- If the user needs to `list Triggers`: [List Triggers docs](https://docs.useparagon.com/actionkit/triggers/api-reference/list-available-triggers.md)
- If the user needs to `subscribe to a Trigger`: [Subscribe to Trigger docs](https://docs.useparagon.com/actionkit/triggers/api-reference/subscribe-to-trigger.md)
- If the user needs to see `example payload for a Trigger`: [Example Trigger payload docs](https://docs.useparagon.com/actionkit/triggers/api-reference/get-example-payload)
- How to receive Trigger webhooks can be found: [Receiving Webhooks docs](https://docs.useparagon.com/actionkit/triggers/receiving-webhooks.md)
- Other useful endpoints such as `listing subscribed Triggers`, `updating a Trigger`, and `unsubscribing to a Trigger` can be found crawling the [Triggers Overview](https://docs.useparagon.com/actionkit/triggers/overview.md)
