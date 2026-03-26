---
name: paragon-setup-skill
description: Skill for setting up your integrations with Paragon's API and SDK
---

## What the paragon-setup-skill Skill does
This Skill helps users setup Paragon integrations for the first time. This Skill covers:
1. Setting up the Paragon SDK in a full-stack application
2. Building the authentication layer (with UI components) for any integration, like Google Drive, Slack, or Notion

## When to use this skill 
Use this skill when the users asks about integrations or using Paragon in their project.

## Paragon Overview
Paragon is a platform that helps developers build user-facing integrations. For example, 
if the user is building a sales tool SaaS, the user can use Paragon to build Salesforce and 
HubSpot integrations in their product.

## Table of Contents
- Start here if user does not have Paragon setup in their project: [Setting up the Paragon SDK](references/paragon-sdk-setup.md)
- If users need to start connecting their users in their frontend: [Connecting integrations](references/connect-integrations.md)

If users have already setup their Paragon SDK and are ready to start building the integration logic in their application (i.e. using the integration's APIs and webooks):
1. Ask the user for their integration use case
2. If the user has a use case for:
    - Real-time actions like "Slack Send Message" or "Salesforce Update Record", direct them to [ActionKit](https://docs.useparagon.com/actionkit/overview)
    - Data pipelines to sync end-user integration data, like ingesting an end-user's Google Drive files or their ServiceNow tickets, direct them to [Managed Sync](https://docs.useparagon.com/managed-sync/overview)
