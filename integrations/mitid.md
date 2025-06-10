---
title: MitID erhverv (NemLogin)
layout: default
parent: Integrations
has_children: false
---

# MitID erhverv integration
OS2rollekatalog can be configured to integrate with MitID Erhverv, allowing role assignments to be made via the OS2rollekatalog user interface.  
When the integration is activated, OS2rollekatalog will retrieve system roles from MitID Erhverv and assign them to the ```NemLog-in``` IT-system. These roles will then be updated three times daily. Upon the first activation, all existing assignments from MitID Erhverv will be imported, job function roles will be created, and they will be assigned to users, ensuring a seamless transition without any data loss.
  
## Limitations
* Users in OS2rollekatalog must have a NemLogin UUID on their user object; otherwise, OS2rollekatalog cannot associate MitID Erhverv users with its own users. This UUID must come from the data source, such as AD, an IDM system, or similar.
* Only regular role assignments are managed from OS2rollekatalog. Roles that are selected via checkboxes on the user's profile cannot be managed from OS2rollekatalog.


## Requirements
The integration with MitId Erhverv is managed using an FOCES certificate. This certificate must be ordered and registered on a MitId erhverv.  
The certificate must have "Adgang til IdM services i MitId Erhverv" enabled, se image below:  
  
![MitId certificate order](/assets/mit-id-certificate.png)
  
## Configuration
Once the certificate is ready, place it in a location that the container can access, for example, in the cert/ folder if you’ve followed the hosting guide.
Then configure the following environment variables:  
```
  rc.integrations.nemLogin.enabled: "true"
  rc.integrations.nemLogin.keystoreLocation: "/home/cert/Filnavn.p12"
  rc.integrations.nemLogin.keystorePassword: "password"
  rc.integrations.nemLogin.userDryRunOnly: "true"
  rc.integrations.nemLogin.baseUrl: "https://services.nemlog-in.dk"
```

**About rc.integrations.nemLogin.userDryRunOnly**  
This setting ensures that OS2rollekatalog only reads from MitID Erhverv and does not write back. It is useful the first time the integration is enabled, allowing you to verify that the user assignments match those in MitID Erhverv.
It is recommended to always start with this setting set to "true" to check that everything has been imported correctly.  
  
**Note:** The import into OS2rollekatalog is one-time, so any changes made in MitID Erhverv afterward will be overwritten when "userDryRunOnly" is turned off. It is a good idea not to run with "userDryRunOnly" set to true for an extended period.  
  
**Note:** Roles may have been deleted in NemLogin, even though they still appear as assigned. This will trigger a warning in the log.  


### Job's done