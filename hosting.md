---
title: Hosting
layout: home
nav_order: 3
---
# Hosting  

## Requirements
*  A server capable of running Linux Docker containers.  
* At least 1500MB of available memory.  
* **SAML Identity Provider Metadata**. OS2rollekatalog assumes that you have a SAML Identity Provider to handle the login to the role catalog’s user interface.  
* **SAML Certificate**. In the above context, OS2rollekatalog acts as a SAML Service Provider and needs to be equipped with a certificate keystore, which must be available as a PKCS#12 file (p12/pfx).  
* **SQL Database**. OS2rollekatalog relies on an SQL database to store its data. MariaDB 10.11+ is recommended, but MySQL 5.7+ is supported as well.  

Regarding the database, it may be necessary to change the ```group_concat_max_len``` value in the database configuration. MySQL typically has a max length of 1024 bytes when using the ```GROUP_CONCAT``` SQL function.  
Since this is used by the role catalog to generate lists of KLE numbers, there may be users who have so many KLEs assigned that they exceed the 1024-byte limit.  
You can change this value in MySQL's configuration file by setting the following value. Similarly, it may be necessary to increase the stack size for recursive SQL calls if you have a very deep hierarchy. The following settings are used in production today without issues with actual production data:

    group_concat_max_len = 10240
    thread_stack = 512kb

## Optional requirements

* **KOMBIT certificate**. OS2rollekatalog can integrate with KOMBIT’s administration module. If you wish to use this integration, you must register a certificate in KOMBIT’s Administration Module, and the corresponding certificate keystore must be available as a PKCS#12 file (p12/pfx).
Please see [KOMBIT integration](integrations/kombit) for instructions on how to get started.  
* **CICS certificate**. OS2rollekatalog can integrate with KSP/CICS. If you wish to use this integration, you must register a certificate with KMD, and the corresponding certificate keystore must be available as a PKCS#12 file (p12/pfx).  
Please see [KSP/CICS integration](integrations/kspcics) for instructions on how to get started.  

## Running OS2rollekatalog using docker-compose
Docker images are released on [Docker Hub](https://hub.docker.com/r/rollekatalog/linux) with each release.  
  
All new images (since 2021) use the same configuration, and below is an example of the configuration using Docker Compose.  
The primary configuration consists of a set of environment variables that can be set up similarly in other orchestration tools.  
In addition to the environment variables, access to the relevant certificate files is required, for example, via a mounted volume. The Docker Compose configuration below shows how this can be set up.  

### Docker Compose configuration
Below is a very simple docker-compose.yml file, which shows the configuration of OS2rollekatalog.
<details>
<summary><b>docker-compose.yml</b></summary>
<code><pre>
    version: "2.0"
    services:
      rollekatalog:
        image: rollekatalog/linux:2025-05-26
        ports:
          # the internal port is 8090 - map to the external port here. If no loadbalancer/ssl-offloader is used,
          # you'll probably want to expose it on port 443 instead. Check the documentation for SSL configuration
          # if OS2rollekatalog needs to handle HTTPS/SSL internally - by default it is exposed as HTTP
          - 8090:8090
        environment:
          # basic customer information - the ApiKey is the secret key used to access the API's, and should
          # be a strong password (e.g. a random UUID).
          rc.customer.cvr: "12345678"
          rc.customer.apikey: "00000000-0000-4000-0000-000000000000"

          # database must be running somewhere - MariaDB and MySQL is supported. The example shows MariaDB
          # if MySQL is used, change the url and driver to reflect this
          spring.datasource.username: "rollekatalog"
          spring.datasource.password: "00000000-0000-4000-0000-000000000000"
          spring.datasource.url: "jdbc:mariadb://mariadb.domain.com/rollekatalog"
          spring.datasource.driver-class-name: "org.mariadb.jdbc.Driver"
  
          # SAML setup for OS2rollekatalog (url endpoints and entityId). It is recommended to use the same
          # values for all three fields (the servername section only contains the FQDN without the protocol though),
          # but this is not a requirement
          saml.baseUrl: "https://kunde.rollekatalog.dk"
          saml.entityId: "https://kunde.rollekatalog.dk"
          saml.proxy.servername: "kunde.rollekatalog.dk"
  
          # SAML setup for the Identity Provider used for login - this points to the SAML metadata file exposed
          # by the Identity Provider
          saml.idp.metadatafile: "url:https://kunde.dk/adfs/sso/FederationMetadata.xml"

          # SAML setup for the keystore used by OS2rollekatalog - points to a PKCS#12 file and its password
          saml.keystore.location: "file:///home/cert/saml.pfx"
          saml.keystore.password: "password"

          # If running a multi-container setup, only enable scheduling on ONE of the containers. It should
          # be enabled on a container to ensure that batch/scheduled jobs are executed
          rc.scheduled.enabled: "true"
          
          # If you want to use the "title" feature in OS2rollekatalog, enable it with this flag. This allows
          # assigning roles to titles (i.e. all users with a given title within a given OrgUnit)
          rc.titles.enabled: "true"
          
          # If you want to integrate with KSP/CICS, you need to add the following section.
          # Reading from CICS is enabled with the first flag, and sending data (assignments) back to CICS is
          # enabled with the next flag. The losid field should contain Kaldenavn Kort for the top-level OrgUnit
          # inside LOS, and finally the path and password to the certificate keystore used by the integration
          rc.integrations.kspcics.enabled: "false"
          rc.integrations.kspcics.enabledOutgoing: "false"
          rc.integrations.kspcics.losid: "KOMMUNE"
          rc.integrations.kspcics.keystoreLocation: "/home/cert/cics.pfx"
          rc.integrations.kspcics.keystorePassword: "password"
          
          # If you want to integrate with KOMBIT Administrationsmodul, you need to add the following section
          # the domain is filled with the "Jobfunktionsrolle domæne" used in KOMBIT, and the keystore section
          # should point to the certificate keystore used for the integration
          rc.integrations.kombit.enabled: "false"
          rc.integrations.kombit.domain: "kommune.dk"
          rc.integrations.kombit.keystoreLocation: "/home/cert/kombit.pfx"
          rc.integrations.kombit.keystorePassword: "password"
          
          # If you want OS2rollekatalog to be able to send emails, fill out this section with the credentials
          # to access the email server
          rc.integrations.email.enabled: "true"
          rc.integrations.email.from: "rollekatalog@kommune.dk"
          rc.integrations.email.username: "rollekatalog"
          rc.integrations.email.password: "password"
          rc.integrations.email.host: "smtp.kommune.dk"
        volumes:
          # map a local folder (here the "cert" folder) to an internal folder. The filepaths used above
          # points to the internal folder name.
          - ./cert:/home/cert
</pre></code>
</details>
  
  
### Starting and Stopping the OS2rollekatalog with Docker Compose
#### Starting
* Make sure you're in the directory where your docker-compose.yml file is located.  
* Run the following command to start the container(s):

```
$ docker-compose up
```

If you want to run it in the background (as a daemon), add -d:  
```
$ docker-compose up -d
```
This will download the necessary images and start the container(s) based on the configuration in the docker-compose.yml file.  

#### Stopping
* To stop the container(s), run:
```
$ docker-compose down
```
This will stop all running containers and remove them.  
  
## Monitoring
The solution exposes a health monitoring endpoint at /manage/health under the context root, for example:  
```
https://ROLE-CATALOGUE-URL/manage/health
```  

As long as this endpoint returns an HTTP 200 status, the solution is healthy. If it returns anything other than HTTP 200, something is wrong, which will typically be indicated in the response from the endpoint.  
