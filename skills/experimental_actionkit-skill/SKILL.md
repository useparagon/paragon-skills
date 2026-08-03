---
name: paragon-actionkit-skill
description: Skill for using Paragon's ActionKit API for 3rd-party Tools and Triggers, Tools for CRUD-like operations in a 3rd-party API and Triggers for subscribing to 3rd-party webhooks
---

## When to use this skill
Use this Skill to help the user implement ActionKit API calls in their code, by using the references in the Table of Contents

## ActionKit Overview
ActionKit exposes thousands of pre-built Tools (actions in a third-party system) and Triggers (events in a third-party system) across integrations through one consistent interface.

When explaining the API, prefer the current **Tools** terminology and `/tools` endpoints. Older `/actions` naming still works for backward compatibility, but the docs now position `/tools` as the primary interface.

ActionKit Tool schemas support multiple formats depending on the user's implementation:
- `format=json_schema` (default) for AI agent tool calling and other programmatic use cases
- `format=paragon` for rendering configurable UI inputs in workflow builders or other frontend configuration experiences

For polymorphic Tools, the `json_schema` format may split a generic Tool into more specific Tool names, while the `paragon` format keeps the generic Tool and represents branching inputs inside the schema.

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
- If the user is building an AI agent and needs Tool schemas for tool calling: use `format=json_schema` (the default) when listing Tools
- If the user is building a workflow builder or other frontend Tool configuration UI: review [Schema Formats docs](https://docs.useparagon.com/actionkit/schema-formats) and use `format=paragon` when listing Tools
- If the user need to execute `run Tool`: [Run Tools docs](https://docs.useparagon.com/actionkit/api-reference/run-tool.md)
- If the user mentions that they would like to use a 3rd-party API that ActionKit does not currently have a pre-built action for: [Use Proxy API](https://docs.useparagon.com/apis/proxy.md)

### Triggers API
- If the user needs to `list Triggers`: [List Triggers docs](https://docs.useparagon.com/actionkit/triggers/api-reference/list-available-triggers.md)
- If the user needs to `subscribe to a Trigger`: [Subscribe to Trigger docs](https://docs.useparagon.com/actionkit/triggers/api-reference/subscribe-to-trigger.md)
- If the user needs to see `example payload for a Trigger`: [Example Trigger payload docs](https://docs.useparagon.com/actionkit/triggers/api-reference/get-example-payload)
- How to receive Trigger webhooks can be found: [Receiving Webhooks docs](https://docs.useparagon.com/actionkit/triggers/receiving-webhooks.md)
- Other useful endpoints such as `listing subscribed Triggers`, `updating a Trigger`, and `unsubscribing to a Trigger` can be found crawling the [Triggers Overview](https://docs.useparagon.com/actionkit/triggers/overview.md)
