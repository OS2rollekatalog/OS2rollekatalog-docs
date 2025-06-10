---
title: Azure EntraID
layout: default
parent: Integrations
has_children: false
---

# Azure EntraID integration
This integration allows OS2rollekatalog to manage group memberships in Azure EntraID.  
First, please read this guide: [Opsætning af Microsoft Entra ID integration](https://github.com/OS2rollekatalog/OS2rollekatalog/raw/refs/heads/master/doc/Rollekataloget%20-%20Ops%C3%A6tning%20af%20EntraID%20(Azure).docx)

# Limitiations
It is not possible to control mail-enabled groups (managed by exchange online), we have tried different solutions, but it is simply not possible.  
  
# Requirements
ClientId, clientSecret, and tenantId are required. Please refer to the guide linked above.


## Configuration
Configure the following environment variables and restart the OS2rollekatalog container, and everything should be working.  

```
  rc.integrations.entraID.backSyncEnabled: "true"
  rc.integrations.entraID.membershipSyncEnabled: "true"
  rc.integrations.entraID.clientId: "XXXXX"
  rc.integrations.entraID.clientSecret: "XXXXXXX"
  rc.integrations.entraID.tenantId: "XXXXXX"
```

```backSyncEnabled``` this is the feature that imports the groups from EntraId into OS2rollekatalog and assigns the roles to the users that have it in EntraID, it is only run when a group is initially imported - should be set to true.  
```membershipSyncEnabled``` this is the normal membership sync feature, it will add and remove group member, where the corresponding role has been added or removed in the role-catalogue - should be set to true.  

### Job's done
