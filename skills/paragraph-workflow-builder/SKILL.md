---
name: paragraph-workflow-builder
description: >-
  Builds and edits Paragon Paragraph workflows as TypeScript (triggers, core
  steps, integration actions, orchestration with nextStep, Operators,
  register/context/connectUser). Use when authoring workflows under
  src/integrations/*/workflows/, para new workflow, RequestStep,
  IntegrationRequestStep, EndpointStep, or when the user mentions Paragraph
  workflow code or @useparagon/core workflow APIs.
---

# Paragraph workflows (code)

## When to use

Use for **workflow definitions** in a Paragraph repo: files under `src/integrations/<integration>/workflows/`, triggers, steps, branching, and wiring to the Paragon dashboard.

If the request also touches **integration config** files like `config.ts` or `inputs.ts`, keep that work in the same repo and anchor it to the live Paragraph docs rather than looking for a separate skill. Recent Paragraph updates make three config rules easy to miss:

- Keep custom integration `slug` values stable. They are now immutable identifiers and should not be recomputed from a renamed display name.
- Put provider-specific OAuth query params like `access_type=offline` under `requestOptions.authorizationCodeOptions.configuration.queryParams`.
- Preserve `showWatermark` when it is already present in a custom integration config.

**Official references**

- [Defining workflows](https://docs.useparagon.com/paragraph/defining-workflows) — structure, orchestration, conditions, fan-out, reusing steps.
- [@useparagon/core glossary](https://docs.useparagon.com/paragraph/reference/useparagon-core) — triggers and step constructors, inputs/outputs.
- [Custom Integrations](https://docs.useparagon.com/resources/custom-integrations.md) — custom provider setup, request auth, and SDK usage.
- [User-Configured OAuth](https://docs.useparagon.com/resources/user-configured-oauth.md) — account type handling and user-supplied OAuth credentials.
- [Paragraph release notes](https://docs.useparagon.com/changelog/paragraph) — current CLI and `config.ts` behavior changes.
- Doc index for deeper pages: [llms.txt](https://docs.useparagon.com/llms.txt).

---

## Create and place files

1. From the Paragraph project root: `para new workflow --integration <integration>` (e.g. `hubspot`, `linear`, or `custom.myapp`).
2. New workflow files live in **`src/integrations/<integration>/workflows/`**.
3. Keep **`readonly id`** as generated; Paragon maintains it.
4. List workflow classes in that integration’s **`config.ts`** in **`workflowDisplayOrder`** so they appear in the Connect Portal and dashboard.

---

## Workflow class shape (typical native integration)

Match the generated pattern in this repo:

- Extend **`Workflow<IIntegration, IPersona<typeof personaMeta>, InputResultMap>`** from `@useparagon/core`.
- Import **`IContext`**, **`IConnectUser`**, **`IPermissionContext`**, persona types, and **`InputResultMap`** / **`I…Integration`** from `@useparagon/integrations/<name>`.
- Import **`personaMeta`** from `../../../persona.meta` (adjust depth if the workflow folder depth differs).

**`define(integration, context, connectUser)`**

- Declare steps (any order).
- Wire control flow with **`.nextStep(...)`** (this repo’s `@useparagon/core` surface).
- Return **`this.register({ … })`** with an object whose **keys are stable** (they map to the workflow graph); every step used in the graph should appear here.

**Class fields (common)**

| Member | Role |
|--------|------|
| `name`, `description` | Dashboard and Connect Portal copy |
| `inputs` | Workflow-level user settings via `createInputs({ … })` from the integration package |
| `defaultEnabled`, `hidden` | Connect Portal behavior (see Paragon connect-portal docs) |
| `definePermissions` | Optional `ConditionalInput` for workflow visibility |
| `id` | Do not edit |

---

## Where steps come from

| Kind | Source |
|------|--------|
| Triggers: `CronStep`, `EndpointStep`, `EventStep`, `IntegrationEnabledStep` | `@useparagon/core` |
| Core steps: `RequestStep`, `ResponseStep`, `ConditionalStep`, `FunctionStep`, `FanOutStep`, `DelayStep`, `IntegrationRequestStep`, … | `@useparagon/core` |
| Integration-specific **actions** and **triggers** | `integration` argument in `define`, e.g. `integration.triggers.recordCreated({ … })`, `integration.actions.searchRecords({ … }, { … })` |

Optional step options (second arg on many integration actions, or step config): `autoRetry`, `continueWorkflowOnError`, `description`, etc., per [defining workflows](https://docs.useparagon.com/paragraph/defining-workflows#defining-steps).

---

## Referencing data

- **Prior step output**: `someStep.output…` (template strings can interpolate like the Workflow Editor’s `{{1.output…}}`).
- **Environment secrets**: `context.getEnvironmentSecret("KEY")`.
- **User settings**: `context.getInput(…)` — integration-level inputs from that integration’s `inputs.ts` / exports; workflow-level from **`this.inputs`** (see [defining workflows](https://docs.useparagon.com/paragraph/defining-workflows#referencing-user-settings-and-environment-secrets)).
- **Connected user**: `connectUser.userId`, `connectUser.meta…` (metadata typed from `persona.meta.ts`).

---

## Orchestration (control flow)

- **Linear chain**: `triggerStep.nextStep(stepA).nextStep(stepB)`.
- **Conditional**: `conditionalStep.whenTrue(stepIfTrue).whenFalse(stepIfFalse)` then continue with **`.nextStep(...)`** on the conditional as needed (see [conditional branches](https://docs.useparagon.com/paragraph/defining-workflows#conditional-branches)).
- **Fan out**: `fanOutStep.branch(innerStep.nextStep(…))` — **`.branch` is not chainable**; then connect the fan-out’s continuation per docs ([fan out branches](https://docs.useparagon.com/paragraph/defining-workflows#fan-out-branches)).

---

## Conditional logic

1. Import operators: `import * as Operators from '@useparagon/core/operator';`
2. Pass **`ConditionalInput`** to `ConditionalStep` (and similar APIs such as pagination **stopCondition**).
3. Paragon expects conditions in **disjunctive normal form**: **OR of ANDs** ([conditional logic](https://docs.useparagon.com/paragraph/defining-workflows#conditional-logic)).

---

## Triggers (quick reference)

| Trigger | Use case |
|---------|-----------|
| `EndpointStep` | HTTP request trigger; set `allowArbitraryPayload` or validations; output: **`output.request`** (`headers`, `body`, `params`, `file`). |
| `CronStep` | Scheduler; init params include **`cron`** and optional **`timezone`** (IANA string; types live in `@useparagon/core`). Some older docs may show different casing—prefer the installed package. |
| `EventStep` | App Event from **`src/events`**; import the event module into the workflow file. |
| `IntegrationEnabledStep` | Runs when user enables the integration. |

Details and examples: [@useparagon/core glossary — Triggers](https://docs.useparagon.com/paragraph/reference/useparagon-core).

---

## Requests

- **`RequestStep`**: full URL; you supply auth (e.g. Bearer) via `authorization` or headers; output **`output.response`** (`statusCode`, `body`, `headers`).
- **`IntegrationRequestStep`**: path relative to integration **apiBaseUrl** (or full URL); user’s integration auth applied automatically; same **`output.response`** shape.

Pagination: optional `pagination` callback returning `outputPath`, `pageToken`, `stopCondition` (Operators). See [request pagination](https://docs.useparagon.com/workflows/requests/request-pagination) and the glossary example.

---

## `ResponseStep`

Only for workflows driven by an HTTP request trigger: set `responseType`, `statusCode`, and `body` (JSON or file). See glossary **ResponseStep**.

---

## `FunctionStep`

Runs sandboxed code: must be **self-contained**; pass data via **`parameters`**. Signatures and allowed **`libraries`** are in Paragon’s [JavaScript libraries](https://docs.useparagon.com/resources/javascript-libraries) doc. Output: **`functionStep.output.result`**. Follow the pattern already used in the repo (string module-style `code` vs inline `code`) for consistency with neighboring workflows.

---

## Reusing steps across workflows

Put shared helpers under a top-level **`src/<something>/`** folder **not** named `integrations` (e.g. `src/common/`), export step factories, import from workflows. See [Reusing steps](https://docs.useparagon.com/paragraph/defining-workflows#reusing-steps). For secrets inside shared code, patterns using `Execution` appear in the same doc page.

---

## Checklist before finishing a workflow change

1. **Trigger** defined and is the start of the **`.nextStep`** chain (or branches/fan-out as designed).
2. Every step in the graph is passed to **`this.register({ … })`** with **unchanged keys** if the workflow already exists in Paragon.
3. **Types**: integration imports align with the folder’s integration (`@useparagon/integrations/...`).
4. **Conditions** use Operators in DNF where required.
5. **Integration** `config.ts` **`workflowDisplayOrder`** includes this workflow class if it should show in the portal.

For step parameter details not repeated here, prefer the live glossary: [@useparagon/core](https://docs.useparagon.com/paragraph/reference/useparagon-core).
