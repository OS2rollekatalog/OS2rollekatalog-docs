---
title: OS2kitos
layout: default
parent: Integrations
has_children: false
---

# OS2kitos integration

The OS2kitos integration fetches IT systems from OS2kitos and maintains **System Owner** and **System Responsible** assignments on those systems in OS2rollekatalog. The integration uses the municipality's CVR number to identify the correct organization in Kitos and synchronizes data via the Kitos REST API.

## How it works

Synchronization runs as three independent scheduled jobs:

| Job | Default schedule | What it does |
|---|---|---|
| **Main sync** | Every 6 hours from 03:00 (03:00, 09:00, 15:00, 21:00) | Fetches changed IT systems and IT system usages, updates System Owner and System Responsible |
| **Deletion sync** | Daily at 02:10 | Fetches and processes IT systems that have been deleted in Kitos |
| **User sync** | Every 6 hours from 00:00 (00:00, 06:00, 12:00, 18:00) | Re-matches Kitos users to OS2rollekatalog users based on email or name |

Synchronization is **delta-based**: only changes since the last run are fetched. The timestamp of the last successful run is persisted in the database.

### User matching

System Owners and System Responsibles are matched to OS2rollekatalog users using the following logic:

1. Match by **email address** (case-insensitive)
2. If no email match is found, fall back to matching by **full name** (case-insensitive)
3. If no match is found, the field is left empty and a warning icon is shown in the UI

### Data freshness

| Data type | Maximum age |
|---|---|
| Changed IT systems | ~6 hours |
| User / ownership assignments | ~6 hours |
| **Deleted IT systems** | **~24 hours** |
| UI dropdown list (cached) | Up to 4 additional hours on top of the above |

> ⚠️ The dropdown list used when linking an IT system to a Kitos IT system is cached in memory for up to 4 hours. This means a deleted IT system may remain visible in the dropdown for up to ~28 hours after it has been removed in Kitos.

## Enabling the integration

> ℹ️ This section is only relevant for municipalities that self-host OS2rollekatalog. If your instance is hosted by Digital Identity, contact them to have the integration enabled.

Update your `docker-compose.yml` with the following configuration:

```
rc.integrations.kitos.enabled: "true"
rc.integrations.kitos.cvr: "<municipal CVR number>"
rc.integrations.kitos.systemOwnerRoleUUID: "<UUID of System Owner role in Kitos>"
rc.integrations.kitos.systemResponsibleRoleUUID: "<UUID of System Responsible role in Kitos>"
di.kitosClient.email: "<service account email for Kitos API>"
di.kitosClient.basePath: "https://kitos.dk"
```

The `systemOwnerRoleUUID` and `systemResponsibleRoleUUID` values must match the role UUIDs configured in your Kitos instance.

### Optional: Adjusting sync frequency

The sync schedules can be overridden using standard cron expressions:

```
rc.integrations.kitos.cron: "0 0 3/6 ? * *"          # Main sync
rc.integrations.kitos.deletion.cron: "0 10 2 * * ?"   # Deletion sync
rc.integrations.kitos.users.cron: "0 0 0/6 ? * *"     # User sync
```

To reduce the delay for deletions, consider running the deletion sync more frequently, e.g. every hour:

```
rc.integrations.kitos.deletion.cron: "0 10 * * * ?"
```

## Using the integration

Once enabled, a dropdown appears on the IT system edit page allowing administrators to link an OS2rollekatalog IT system to a Kitos IT system. System Owner and System Responsible are then automatically populated from Kitos on the next sync.

A report listing all IT systems with their linked Kitos IT system and System Owner is available at:

```
/ui/report/custom?reportType=ITSYSTEM_KITOS
```
