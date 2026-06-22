# Integration-Specific Sync Configuration

## When to use this reference
- [ ] The user needs a Managed Sync option for a specific integration or pipeline
- [ ] The user needs to confirm integration-specific OAuth scopes before enabling a sync
- [ ] The user needs to pass provider-specific fields in the sync `configuration` object

## Workflow
1. Identify the integration and pipeline the user wants to sync.
2. Open the matching Managed Sync integration docs page before suggesting any provider-specific option.
3. Confirm the required OAuth scopes for that integration.
4. Add any documented provider-specific fields to the sync `configuration` object.
5. If an option is not documented for that integration, do not invent it.

## Zoho CRM
Use the Zoho CRM Managed Sync docs when the user is syncing `contacts`, `companies`, `deals`, or `custom_objects`.

- [ ] Confirm the OAuth app includes the standard Zoho CRM scopes for the pipeline being synced
- [ ] Add `ZohoCRM.org.READ` if the user wants to use `backoffAtCreditPercentage`
- [ ] Set `configuration.backoffAtCreditPercentage` to a number between `0.5` and `1.0`
- [ ] Explain that the sync will pause and retry hourly once the account reaches the configured API credit usage threshold

Example payload shape for a Zoho CRM contacts sync:

```typescript
const syncConfiguration = {
  integration: 'zohocrm',
  pipeline: 'contacts',
  configuration: {
    backoffAtCreditPercentage: 0.8,
  },
};
```

For custom object syncs, continue to provide the Zoho module API name alongside the backoff option:

```typescript
const customObjectSyncConfiguration = {
  integration: 'zohocrm',
  pipeline: 'custom_objects',
  configuration: {
    customObjectName: 'Subscriptions',
    backoffAtCreditPercentage: 0.8,
  },
};
```

Reference docs:
- [Zoho CRM integration docs](https://docs.useparagon.com/managed-sync/integrations/zohocrm)
- [Enable Sync](https://docs.useparagon.com/managed-sync/api/enable-a-sync.md)
