---
title: Dansk Miljøportal
layout: default
parent: Integrations
has_children: false
---

# Dansk Miljøportal integration
OS2rollekatalog has an integration with Dansk Miljøportal's [Provision API](https://github.com/danmarksmiljoeportal/brugerstyring/wiki/Provisioning-API).  
The integration allows you to manage role assignments to Dansk Miljøportal, with users being automatically created on the portal when they are assigned one or more miljøportal rights.  
To get started with the integration, there is a process to follow.   
Please refer to [OS2rollekatalog - DMP opsætning](https://github.com/OS2rollekatalog/OS2rollekatalog/raw/refs/heads/master/doc/Rollekataloget%20-%20DMP%20ops%C3%A6tning.docx) for details.  
There is no initial migration to Dansk Miljøportal, so if you have any existing assignments that need to be imported, this needs to be done manually or via a script.  
  
## Limitations  
In the Dansk Miljøportal, it is allowed to assign roles to users from other municipalities on behalf of the current municipality. This works fine; however, it may cause issues if the other municipality is also using OS2rollekatalog, as their role catalog will remove user roles it has not assigned.  
  
## Requirements  
Please refer to [OS2rollekatalog - DMP opsætning](https://github.com/OS2rollekatalog/OS2rollekatalog/raw/refs/heads/master/doc/Rollekataloget%20-%20DMP%20ops%C3%A6tning.docx)

## Configuration
Configure the following environment variables:
```
  rc.integrations.dmp.enabled: "true"
  # This is the clientId received from DMP
  rc.integrations.dmp.clientId: "dmp-bs2-api-XXXX-kommune"
  # This is the clientSecret received from DMP
  rc.integrations.dmp.clientSecret: "XXXXX"
```
**Note:** If the client secret constains ```$``` sign(s) it needs to be escaped with antoher ```$``` ... in short change ```$``` to ```$$```

