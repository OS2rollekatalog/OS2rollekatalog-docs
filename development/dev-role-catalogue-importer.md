---
title: RoleCatalogueImporter application
layout: default
nav_order: 5
parent: Development
has_children: false
---

# RoleCatalogueImporter application
This application is a .NET 4.6 application and can be opened in Visual Studio 2019. When you compile the application in Visual Studio, it creates a complete application that can be installed as a Windows Service.

To simplify the installation, an InnoSetup script has been created, located in the Installer folder. When run using InnoSetup, it builds an EXE installer with the latest compiled version of the application.

This EXE installer can be executed on a Windows Service, where it will install the application and set it up as a Windows Service.

