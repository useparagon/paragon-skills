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

Permission Syncs currently run automatically for supported file storage integrations:

- Box
- Confluence
- Dropbox
- Google Drive
- OneDrive
- SharePoint

## Prerequisites
Managed Sync is built on top of Paragon's managed authentication and monitoring. When using any Sync APIs or Permissions APIs, 
a JWT with a unique end-user ID is required so that Paragon knows which end-users data to sync.

Before helping the user implement Managed Sync, it's best to have the Paragon SDK and Connect Portal setup.

- [ ] Check the user's project for "@useparagon/connect" in their `package.json` or references of `paragon.authenticate` in their frontend codebase

- [ ] If a user does not have the Paragon SDK and Connect Portal for authorization set up: Direct them to use the `paragon-setup-skill`

- [ ] If the user does have the Paragon SDK and authorization set up: Proceed with using the resources in the "Table of Contents" to help implement Managed Sync

## Permissions API implementation patterns

If the user is building search, knowledge, or RAG experiences on top of synced files, guide them to the Permissions API pattern that matches their corpus size:

- [ ] Pre-search filtering for small corpora (about 1000 total documents or fewer): use `List Objects` to collect permitted file IDs before querying the search index
- [ ] Post-search filtering for larger corpora: over-fetch search results, then use `Batch Check Access` to remove documents the current user cannot read
- [ ] Hybrid filtering for large corpora: apply coarse metadata filters first (team, folder, drive, space, etc.), then use `Batch Check Access` on the final ranked results

When recommending post-search or hybrid filtering, remind the user to over-fetch their search results by at least 2x so permission filtering does not remove too many relevant results.

If the user needs to reason about sync architecture, remind them that the Permissions API uses the same base URL and authorization model as the Sync API.


## Table of Contents

### Using the Sync API - Basic Usage
- Overview on how the Sync API works to start syncing files, CRM records, tickets, etc.: [Sync API Overview](https://docs.useparagon.com/managed-sync/sync-api)
- Enable a sync: [Enable Sync](https://docs.useparagon.com/managed-sync/api/enable-a-sync)
- Check sync status: [Sync Status](https://docs.useparagon.com/managed-sync/api/get-sync-status)
- Pull records from a sync: [Pull Synced Records](https://docs.useparagon.com/managed-sync/api/pull-synced-records)
- Get metadata for a specific record: [Get Synced Record](https://docs.useparagon.com/managed-sync/api/get-synced-record)
- Download a file associated with a synced record: [Download Synced File](https://docs.useparagon.com/managed-sync/api/download-content)

### Sync Webhooks
- Receiving webhooks for when records are created, updated, or deleted from a Sync: [Sync Webhooks](https://docs.useparagon.com/managed-sync/webhooks)

### Sync APIs for Managing Active Syncs 
- List all syncs: [List Syncs](https://docs.useparagon.com/managed-sync/api/list-syncs)
- Disable a sync: [Disable Sync](https://docs.useparagon.com/managed-sync/api/disable-sync)
- Re-enable a sync: [Re-enable Sync](https://docs.useparagon.com/managed-sync/api/reenable-sync)
- Update Credential on a sync: [Update Sync](https://docs.useparagon.com/managed-sync/api/update-sync)
- Delete a sync: [Delete Sync](https://docs.useparagon.com/managed-sync/api/delete-sync)

### Permissions API (file storage integration specific)
- Overview on how the Permissions API works to check user access to synced files: [Permissions API Overview](https://docs.useparagon.com/managed-sync/permissions-api)
- Check if user has access to a synced object: [Check Access](https://docs.useparagon.com/managed-sync/api/check-access)
- Batch check multiple user object relationships: [Batch Check Access](https://docs.useparagon.com/managed-sync/api/batch-check-access)
- List users that have access to a synced object: [List Users](https://docs.useparagon.com/managed-sync/api/list-users)
- List synced objects that a user has access to: [List Objects](https://docs.useparagon.com/managed-sync/api/list-objects)
- Get all users and group relationships associated with an object by role: [Expand Relationship](https://docs.useparagon.com/managed-sync/api/expand)
