---
title: ADFS Attribute Store
layout: default
nav_order: 5
parent: Development
has_children: false
---

# ADFS Attribute Store
This attribute store is a .NET 4.6 application and can be opened in Visual Studio 2019 and compiled there. Fundamentally, the application is "just" an implementation of a single interface exposed by Microsoft. This interface is used by an AD FS server to retrieve information about a user at login time (e.g., roles ;)).  
  
When compiling the application from Visual Studio, you must ensure that a reference is added to the correct version of the ClaimsPolicy.dll file. There is a folder in the codebase with three versions of this DLL, one for each version of Windows Server (2012R2, 2016, and 2019). It is important to link to the version that matches the Windows Server on which the attribute store will be installed.  
  
Once the application is compiled, a single DLL (RoleCatalogueAttributeStore.dll) is generated, which must be manually copied to the Windows Server where the AD FS server is running. It should be copied to the folder:  

```
c:\windows\adfs
```
  
For instructions on how to use the ADFS Attribute store [Attribute store - usage guide](https://github.com/OS2rollekatalog/OS2rollekatalog/raw/refs/heads/master/doc/Rollekataloget%20-%20Anvendelse%20af%20Attribute%20Store.docx)