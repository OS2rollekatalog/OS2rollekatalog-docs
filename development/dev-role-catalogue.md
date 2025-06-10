---
title: OS2rollekatalog - application
layout: default
nav_order: 1
parent: Development
has_children: false
---

# Development - OS2rollekatalog
OS2rollekatalog is a Java application that requires a minimum of JDK 21 to compile. Apache Maven is used as the tool to compile and run the application.  
The codebase is structured with 2 Maven modules, including a parent POM file located at the root of the codebase, and a POM file in each module.  

```
.
├── pom.xml
├── ui
│    └── pom.xml
└── webjar
     └── pom.xml
```

## Prerequisites
Before starting OS2rollekatalog, you need to have a local SQL database running. The application uses the following configuration file when running locally in your development environment.  
Here, you can modify the connection parameters to the SQL database you wish to use.  

```
ui/config/application.properties
```
Here are the two sections that can be customized and commented in/out. One section is for MariaDB and the other for MySQL. Use the section that corresponds to the SQL database you wish to use.  
```
# MariaDB
spring.datasource.driver-class-name=org.mariadb.jdbc.Driver
spring.datasource.url=jdbc:mariadb://localhost/rc?serverTimezone=Europe/Copenhagen
spring.datasource.username=root
spring.datasource.password=Test1234

# MYSQL
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost/rc?serverTimezone=Europe/Copenhagen
spring.datasource.username=root
spring.datasource.password=Test1234
```
  
OS2rollekatalog assumes that the database (i.e., the root schema) has already been created. However, OS2rollekatalog will automatically create the necessary tables, which is handled by the Flyway framework.  
  
Additionally, OS2rollekatalog assumes that you have a Microsoft AD FS server to handle login to the application.  
Setting up an AD FS server is beyond the scope of this guide, but the underlying AD must have a user with the username "rolunittest01," as this is the default administrator in OS2rollekatalog when running in developer mode. With this user, you can log in as an administrator in a newly set up test role catalog.  

You need to point to the AD FS server you are setting up via the ```application.properties``` file, where you should adjust this line:

```
saml.idp.metadatafile=url:https://demo-adfs.digital-identity.dk/FederationMetadata/2007-06/FederationMetadata.xml
```


## Compilation and Execution
You can compile the application using Maven - the following command is used for this purpose.  
**Note** that test cases are not executed here, as they have a number of other prerequisites. Refer to the documentation for running tests to see how to execute all test cases.
```
./mvnw clean install -Dmaven.test.skip=true
```
The above is executed at the root of the codebase, so both the webjar and UI modules are compiled. After compiling both modules, you can simply compile from within the UI module going forward, as the "install" part installs the webjar module into your local Maven repository.  

The UI module can be recompiled and run using the following Maven commands:  
```
./mvnw clean package -Dmaven.test.skip=true
./mvnw spring-boot:run
```

When running the final command, the compiled application starts up, and the role catalog is now running locally on your development machine.  
The application is now available on port 8090 under HTTPS, meaning you can access it at:  

```
https://localhost:8090/
```

If you have the following setting in the ```application.properties``` file, basic master data (users, units, etc.) will be automatically created, allowing you to perform a login.  
However, it is important that the AD FS server you have set up can authenticate one of the users that exists in the application (see above).  
```
environment.dev=true
```
