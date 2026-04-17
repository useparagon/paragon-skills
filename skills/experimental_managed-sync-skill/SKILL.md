---
name: paragon-managed-sync-skill
description: Skill for using Paragon's Managed Sync for ingesting and syncing 3rd-party data like Google Drive files or CRM records
---
## When to use this skill
Use this Skill to help the user implement Managed Sync with the Sync API and the Permissions API, using the references in the Table of Contents.

## Managed Sync Overview
Managed Sync is a set of APIs for developers to spin up data pipelines that pull their 
users' 3rd-party integration data (like Salesforce CRM data, Google Drive file data, and ServiceNow tickets)
and keep that data up-to-date. These data pipelines live in Paragon's infrastructure 
and are completely managed by Paragon. Developers can easily start syncs, pause syncs, 
and pull synced data.

When integration data is synced, that data is also normalized across that integration's category. For example, the data schema from Google Drive, Dropbox, Box, and Sharepoint are all normalized so that code/logic can be used across that category.

For file storage integration data, the Permissions API is an API to check permissions 
of any synced file. Paragon manages a graph database behind the scenes, so developers 
can get and check up-to-date permissions without managing that data.

## Prerequisites
Managed Sync is built on top of Paragon's managed authentication and monitoring. When using any Sync APIs or Permissions APIs, 
a JWT with a unique end-user ID is required so that Paragon knows which end-users data to sync.

Before helping the user implement Managed Sync, it's best to have the Paragon SDK and Connect Portal setup.

- [ ] Check the user's project for "@useparagon/connect" in their `package.json` or references of `paragon.authenticate` in their frontend codebase

- [ ] If a user does not have the Paragon SDK and Connect Portal for authorization set up: Direct them to use the `paragon-setup-skill`

- [ ] If the user does have the Paragon SDK and authorization set up: Proceed with using the resources in the "Table of Contents" to help implement Managed Sync


## Table of Contents

### Using the Sync API - Basic Usage
- Overview on how the Sync API works to start syncing files, CRM records, tickets, etc.: [Sync API Overview](https://docs.useparagon.com/managed-sync/sync-api.md)
- Enable a sync: [Enable Sync](https://docs.useparagon.com/managed-sync/api/enable-a-sync.md)
- Check sync status: [Sync Status](https://docs.useparagon.com/managed-sync/api/get-sync-status.md)
- Pull records from a sync: [Pull Synced Records](https://docs.useparagon.com/managed-sync/api/pull-synced-records.md)
- Get metadata for a specific record: [Get Synced Record](https://docs.useparagon.com/managed-sync/api/get-synced-record.md)
- Download a file associated with a synced record: [Download Synced File](https://docs.useparagon.com/managed-sync/api/download-content.md) 

### Sync Webhooks
- Receiving webhooks for when records are created, updated, or deleted from a Sync: [Sync Webhooks](https://docs.useparagon.com/managed-sync/webhooks.md)

### Sync APIs for Managing Active Syncs 
- List all syncs: [List Syncs](https://docs.useparagon.com/managed-sync/api/list-syncs.md)
- Disable a sync: [Disable Sync](https://docs.useparagon.com/managed-sync/api/disable-sync.md)
- Re-enable a sync: [Re-enable Sync](https://docs.useparagon.com/managed-sync/api/reenable-sync.md)
- Update Credential on a sync: [Update Sync](https://docs.useparagon.com/managed-sync/api/update-sync.md)
- Delete a sync: [Delete Sync](https://docs.useparagon.com/managed-sync/api/delete-sync.md)

### Permissions API (file storage integration specific)
- Overview on how the Permissions API works to check user access to synced files: [Permissions API Overview](https://docs.useparagon.com/managed-sync/permissions-api.md)
- Check if user has access to a synced object: [Check Access](https://docs.useparagon.com/managed-sync/api/check-access.md)
- Batch check multiple user object relationships: [Batch Check Access](https://docs.useparagon.com/managed-sync/api/batch-check-access.md)
- List users that have access to a synced object: [List Users](https://docs.useparagon.com/managed-sync/api/list-users.md)
- List synced objects that a user has access to: [List Objects](https://docs.useparagon.com/managed-sync/api/list-objects.md)
- Get all users and group relationships associated with an object by role: [Expand Relationship](https://docs.useparagon.com/managed-sync/api/expand)
