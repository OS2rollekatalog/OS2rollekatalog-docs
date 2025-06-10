---
title: KOMBIT
layout: default
parent: Integrations
has_children: false
---

# KOMBIT integration
The KOMBIT integration, will fetch new it-systems, system-roles and job function roles from KOMBIT and create them in OS2rollekatalog. The municipal can then create job function roles, and OS2rollekatalog will automatically create them in fk-adgangsstyring.  
After that, the roles can be assigned to users, and if the municipality's AD is set up correctly, users will be able to log in using fk-adgangsstyring with the correct roles.  
  
*Notice* the role syncronization is by default only happening every 4 hours during the day.  
*Disclaimer* The links in this guide point to the GitHub master branch, meaning they will always reference the latest versions. If you are using an older version, please obtain the documents/installers directly from your OS2rollekatalog instance. They can be found on ```https://*your-role-catalogue-url*/info```


## Important!!
The first time you activate the KOMBIT integration, it will import all the current job function roles from KOMBIT. This process only happens once. Any changes made directly to the job function roles in fk-adgangsstyring after the OS2rollekatalog integration has been activated will be overwritten by OS2rollekatalog.  
If some of the job function roles from fk-adgangsstyring have roles assigned from multiple IT systems, they will not be imported, as the OS2rollekatalog data model does not support this. Please check the OS2rollekatalog log for warnings and create corresponding job function roles and role groups manually.  
Unless you want to manually assign roles, if you're migrating from another solution (such as ADFS or another system), I recommend creating a script or program to help migrate the users' role assignments from the old solution to OS2rollekatalog.  
This is beyond the scope of this guide, but please refer to the API documentation in your OS2rollekatalog installation if you're implementing such a script.  

```
https://YOU-ROLE-CATALOGUE-URL/download/api.html
https://YOU-ROLE-CATALOGUE-URL/swagger-ui/index.html
```  


## Procedure for enabling KOMBIT integration. 

You need a certificate that is registered in the KOMBIT Administration Module. (The certificate must be an OCES 3 certificate of the System certificate type.)  
The certificate is placed somewhere that is accessible from the OSrollekatalog docker container eg. in cert/ if using the example docker-compose file from [hosting](../hosting)    
Now update the docker-compose.yml file with the municipal domain and certificate information:  

``` 
rc.integrations.kombit.enabled: "true"
rc.integrations.kombit.domain: "kommune.dk"
rc.integrations.kombit.keystoreLocation: "/home/cert/kombit.pfx"
rc.integrations.kombit.keystorePassword: "password"
``` 
  
### Attribute store
Install the OS2rollekatalog attribute store in AD if it is not already installed. It should be installed on all AD instances if there are multiple.  
You can find the installation guide here [Installation guide](https://github.com/OS2rollekatalog/OS2rollekatalog/raw/refs/heads/master/doc/Rollekataloget%20-%20Anvendelse%20af%20Attribute%20Store.docx)  

#### For ADFS 2016:
[ADFS Plugin ADFS40.zip](https://github.com/OS2rollekatalog/OS2rollekatalog/raw/refs/heads/master/ui/src/main/resources/static/download/ADFS-Plugin-ADFS40.zip)  

#### For ADFS 2019:
[ADFS Plugin ADFS50.zip](https://github.com/OS2rollekatalog/OS2rollekatalog/raw/refs/heads/master/ui/src/main/resources/static/download/ADFS-Plugin-ADFS50.zip)
  
After installation, check the event log to ensure the attribute store has started correctly.  
Take a backup of your existing claim rules.  

Import new claim rules on the ADFS server, remember to replace the *ROLE-CATALOGUE-URL-GOES-HERE* and *CVR-GOES-HERE* placeholders first.  

<details>
<summary><b>Claim rules</b></summary>
<code><pre>
@RuleName = "SpecVer"
 => issue(Type = "dk:gov:saml:attribute:SpecVer", Value = "DK-SAML-2.0", Properties["http://schemas.xmlsoap.org/ws/2005/05/identity/claimproperties/attributename"] = "urn:oasis:names:tc:SAML:2.0:attrname-format:basic");

@RuleName = "AssuranceLevel"
 => issue(Type = "dk:gov:saml:attribute:AssuranceLevel", Value = "3", Properties["http://schemas.xmlsoap.org/ws/2005/05/identity/claimproperties/attributename"] = "urn:oasis:names:tc:SAML:2.0:attrname-format:basic");

@RuleName = "KOMBIT SpecVer"
 => issue(Type = "dk:gov:saml:attribute:KombitSpecVer", Value = "1.0", Properties["http://schemas.xmlsoap.org/ws/2005/05/identity/claimproperties/attributename"] = "urn:oasis:names:tc:SAML:2.0:attrname-format:basic");

@RuleName = "Fetch NameID"
c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
 => add(store = "RoleCatalogueAttributeStore", types = ("http://ROLE-CATALOGUE-URL-GOES-HERE/nameid"), query = "getNameID", param = c1.Value);

@RuleName = "Issue NameID"
c:[Type == "http://ROLE-CATALOGUE-URL-GOES-HERE/nameid"]
 => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, Value = c.Value, Properties["http://schemas.xmlsoap.org/ws/2005/05/identity/claimproperties/format"] = "urn:oasis:names:tc:SAML:1.1:nameid-format:X509SubjectName");

@RuleName = "Fetch Roles"
c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
 => add(store = "RoleCatalogueAttributeStore", types = ("http://ROLE-CATALOGUE-URL-GOES-HERE/oio-bpp"), query = "getBasicPriviligeProfile", param = c1.Value, param = "KOMBIT");

@RuleName = "Issue Roles"
c:[Type == "http://ROLE-CATALOGUE-URL-GOES-HERE/oio-bpp"]
 => issue(Type = "dk:gov:saml:attribute:Privileges_intermediate", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, Value = c.Value, Properties["http://schemas.xmlsoap.org/ws/2005/05/identity/claimproperties/attributename"] = "urn:oasis:names:tc:SAML:2.0:attrname-format:basic", Properties["http://schemas.xmlsoap.org/ws/2005/05/identity/claimproperties/spnamequalifier"] = "Privileges");

@RuleName = "CVR"
 => issue(Type = "dk:gov:saml:attribute:CvrNumberIdentifier", Value = "CVR-GOES-HERE", Properties["http://schemas.xmlsoap.org/ws/2005/05/identity/claimproperties/attributename"] = "urn:oasis:names:tc:SAML:2.0:attrname-format:basic");

</pre></code>
</details>


Test by assigning a KOMBIT role and logging in, e.g., BBR.  

### Job's done