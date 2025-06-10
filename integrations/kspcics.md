---
title: KSP/CICS
layout: default
parent: Integrations
has_children: false
---

# KSP/CICS integration
OS2rollekatalog can be configured to integrate with KSP/CICS, allowing user profile assignments to be made via the OS2rollekatalog user interface.  
Once the integration is enabled, new KSP/CICS usernames and user profiles are automatically retrieved from KSP/CICS each night.  
Within OS2rollekatalog, it is then possible to assign these user profiles just like any other job function roles (user profiles appear as job function roles alongside other job function roles).  
  
## Limitations

* It is not possible to delete, create, or edit user profiles within OS2rollekatalog, including changing the name or description of the user profile. These actions must be performed via KSP/CICS.  
* Only the assignment of user profiles can be managed. Rights at a lower level (e.g., role profiles) cannot be managed via OS2rollekatalog.  
* It is not possible to assign rights to KSP/CICS users who do not already have an AD account imported into OS2rollekatalog.  
* KSP/CICS user accounts that cannot be matched with an existing employee’s AD account must still be managed via KSP/CICS (e.g., robots, system accounts, etc.).  

The reason for this is that the employee’s AD account and KSP/CICS user account are paired via the CPR number. In OS2rollekatalog, it is the AD account that is visually used to assign the KSP/CICS user profiles, but through this pairing with the KSP/CICS user account, the correct KSP/CICS user account is assigned the rights in KSP/CICS.

## Requirements
The integration with KSP/CICS is managed using an FOCES certificate. This certificate must be ordered and registered on a KSP/CICS user account via KMD.  
This user account must also be assigned the following rights, and it is important to ensure that the password for the account does not expire, as the account will be locked and the integration will stop functioning if it does:
  
* The user must be created as a security administrator.  
* The user must be created without an expiration date for the password.  
* The user must be assigned to the highest level in the LOS hierarchy.  
* The user must be authorized for the KSP/CICS interface user administration, and therefore assigned the KSP-OPR-AA and KSP-SOG-AA role profiles.  
  

When registering the certificates in KSP/CICS, it is the Subject UUID that is registered, see the image below.

![KSP/CICS screenshot](/assets/kspcics-cert-reg.png)


Once the KSP/CICS user account is created, the certificate must be set up in OS2rollekatalog by the operations administrator, who can then enable the integration.  
As part of this setup, the alias Short for the municipality’s top-level LOS unit must be configured in OS2rollekatalog, as it is used for the calls to KSP/CICS.  

## Configuration
When the every thing is in place on the KSP/CICS side, it needs to be configured in the OS2rollekatalog.  
This is done using the following environments variables, in the docker-compose file:  

```
rc.integrations.kspcics.enabled: "true"
rc.integrations.kspcics.enabledOutgoing: "false"
rc.integrations.kspcics.losid: "KALDEKORT"
rc.integrations.kspcics.keystoreLocation: "/home/cert/cics.pfx"
rc.integrations.kspcics.keystorePassword: "XXXXX"
```
When OS2rollekatalog starts up with these settings, it will import user-profiles and assignments, the initial assignments import happens only once! so do not stop the process. After the initial import check that the assignments matches as expected, and set change the ```...enabledOutgoing``` to "true"  
  
Please take note of the ```...enabledOutgoing``` when this is set to "true" the OS2rollekatalog will start managing the assignments in KSP/CICS.  

### Job's done

## FAQ
- What users to will the OS2rollekatalog manage.  
Only the ones which have CPR and a matching AD user in role-catalogue, all other users will be untouched.  

