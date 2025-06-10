---
title: ItSystem Master - application
layout: default
nav_order: 3
parent: Development
has_children: false
---

# ItSystem Master application
The OS2rollekatalog codebase also includes a "master" application. This is a small web application where administrators (created within the application) can log in and maintain master data for IT systems. Individual OS2rollekatalog installations can choose to subscribe to IT systems maintained in the master application.  
This way, many IT systems can be easily created and maintained, making them available to everyone using OS2rollekatalog.  
  
The application is a Java 21 application, and Apache Maven is used as the tool to compile and run the codebase.  
The application is located in the ItSystemMaster folder and depends on the webjar module (so it needs to be compiled first and possibly installed into the local Maven repository via the install command).  
The application also requires a local SQL database. Therefore, if you wish to run this locally, you need to have a MySQL or MariaDB database running.  
The connection details for the database are set in the config/application.properties file, just like for OS2rollekatalog. Again, the Flyway framework handles the creation of tables, etc., but the database schema must be created in advance.  
The compilation and execution of the application is identical to OS2rollekatalog, meaning you run these commands:  
```
$ mvn clean package -Dmaven.test.skip=true
$ mvn spring-boot:run
```
  
The application is then available on port 8091 under HTTPS, meaning you can access it at:  
```
https://localhost:8091/
```
