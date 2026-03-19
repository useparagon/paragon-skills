# Headless Connect Portal

## Overview
The Headless Connect Portal is an implementation using the Paragon SDK to authenticate, connect, and install 
integrations with the user's own UI components and theming (not Paragon's pre-built Connect Portal).

## How to implement a Headless Connect Portal
### Step 0: Enable Headless Mode in the SDK
This only needs to be done once when initializing the Paragon SDK.

```typescript
import { paragon } from "@useparagon/connect";

paragon.setHeadless(true);
```

### Step 1: Building an integration catalog

#### Step 1a: Getting Integration Metadata
Use the Paragon SDK's `Get Integration Metadata` method to retrieve data on all of the developer's enabled integrations 

```typescript
import { paragon } from '@useparagon/connect';

paragon.getIntegrationMetadata();
```

This method will return data with this schema:

```json
[
  {
    type: 'salesforce',
    name: 'Salesforce',
    brandColor: '#057ACF',
    icon: 'https://cdn.useparagon.com/2.35.0/dashboard/public/integrations/salesforce.svg'
  },
  {
    type: 'hubspot',
    name: 'Hubspot',
    brandColor: '#F67600',
    icon: 'https://cdn.useparagon.com/2.35.0/dashboard/public/integrations/hubspot.svg'
  },
]
```

In typescript:

```typescript
import { paragon, IntegrationMetadata } from "@useparagon/connect";

const integrationMetadata: IntegrationMetadata[] = await paragon.getIntegrationMetadata();
```

The developer can use the integration metadata to build catalogs in their frontend if desired:

```tsx
<div className="flex flex-wrap">
  {integrationMetadata.map((integration: IntegrationMetadata) => (
    <div key={integration.type} className="flex gap-4">
      <div>{integration.name}</div>
      <button onClick={() => paragon.connect(integration.type)} />
    </div>
  ))}
</div>
```

#### Step 1b: Displaying descriptions, user settings, and workflows
If the user wants to display: 
* descriptions of the integration
* user settings associated with an integration
* available integration workflows

use the `paragon.getIntegrationConfig("INTEGRATION_NAME")` method.

```tsx
import { paragon } from "@useparagon/connect";

paragon.getIntegrationConfig("slack");
```

This will return a payload like:

```json
{
  "shortDescription": "Send notifications to Slack",
  "longDescription": "Connect your Slack workspace to receive notifications and alerts...",
  "availableUserSettings": [
    {
      "id": "2d5662c9-6750-46c2-8588-2ac904532efb",
      "type": "DYNAMIC_ENUM",
      "title": "Channel",
      "required": false,
      "sourceType": "channels"
    }
  ],
  "availableWorkflows": [
    {
      "id": "2248335c-671c-47e4-b9a0-3641a9f2d301",
      "inputs": [],
      "infoText": "Send a Slack notification when a Task is created",
      "defaultEnabled": false,
      "description": "Send Slack Notification"
    }
  ],
  "hiddenWorkflows": []
}
```

#### Step 1c: Displaying account state
If the user wants the status of an integration (if the end-user has connected to an integration or not), they can use `paragon.getUser()`

This will return a payload like:

```json
{
  "authenticated": true,
  "userId": "user-id",
  "integrations": {
    "salesforce": {
      "enabled": false,
      "configuredWorkflows": {}
    },
    "slack": {
      "enabled": true,
      "configuredWorkflows": {},
      "credentialStatus": "VALID",
      "credentialId": "81af6717-9476-458d-8c29-f0aee7ce6d12",
      "providerId": "TM7FL705V",
      "providerData": {}
    },
    "hubspot": {
      "enabled": false,
      "configuredWorkflows": {}
    }
  },
  "meta": {}
}
```

Generally, the user will also want to subscribe to install/connect changes. The user 
can do that on their frontend by using `paragon.subscribe("EVENT_NAME", callback())`.

```tsx
import { paragon, AuthenticatedConnectUser } from "@useparagon/connect"
function MyComponent() {
  const [user, setUser] = useState<AuthenticatedConnectUser | undefined>();

  // Listen for account state changes
  useEffect(() => {
    const listener = () => {
      if (paragon) {
        const authedUser = paragon.getUser();
        if (authedUser.authenticated) {
          setUser({ ...authedUser });
        }
      }
    };
    listener();
    paragon?.subscribe("onIntegrationInstall", listener);
    paragon?.subscribe("onIntegrationUninstall", listener);
    return () => {
      paragon?.unsubscribe("onIntegrationInstall", listener);
      paragon?.unsubscribe("onIntegrationUninstall", listener);
    };
  }, [paragon]);
}
```

### Step 2: Guiding end-users through the install flow
The install flow for any integration can have these 4 types of stages (**not all stages will be relevant for a given integration**):

1. `AccountTypeState`: i.e. Salesforce Production vs Salesforce Sandbox
2. `PreOptionsState`: Pre-OAuth/API Key inputs
3. `PostOptionsStage`: i.e. Atlassian site, SharePoint site selection
4. `InstructionStage`: Instructional content (i.e. Salesforce package installation)

These stages are not part of the OAuth flow (hence the naming `PreOptionsState` and `PostOptionsState`.
Any redirection or popup window that is required for the **OAuth flow** will automatically occur between stages. The SDK will internally manage all state for the popup window and OAuth prompt.

Begin an Install Flow by calling paragon.installFlow.start:

```tsx
paragon.installFlow
  .start(props.integration, {
    onNext: (next) => {
      // Handle next install stage by presenting or updating UI for user input
    },
    onComplete: () => {
      // Installation has completed successfully
    },
    onError: (error, errorContext) => {
      // Handle error messaging
    }
  });
```

After calling .start, you may receive required install stages in the onNext callback (depending on the specific install flow required by the integration). To handle each of the install stages, you will need to:
1. Receive the stage details in the onNext callback.
2. Render the appropriate UI for your user to provide the necessary details in that stage.
    * For the PostOptionsStage, refer to the [User Settings](user-settings.md) for help rendering integration-specific dropdown inputs.
3. Update SDK state with the user’s inputs by calling one of:
    1. `installFlow.setAccountType`
    2. `installFlow.setPreOptions`
    3. `installFlow.setPostOptions`
