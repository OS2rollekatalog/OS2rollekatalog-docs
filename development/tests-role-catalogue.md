---
title: OS2rollekatalog - tests
layout: default
nav_order: 2
parent: Development
has_children: false
---

# Running OS2rollekatalog test cases
The OS2rollekatalog codebase includes a number of test cases. Some of these are used to generate documentation for the OS2rollekatalog API (and, as a side effect, to test that they work ;)), while others are test cases executed against the user interface.  
  
When running tests on OS2rollekatalog, all test cases are executed by default.  
  
The easiest way to run all tests is to simply execute the following command:  
```
./mvnw compile test
```
This will likely fail if all dependencies have not been set up yet. It is recommended to first establish a development environment where you can compile and run OS2rollekatalog. A guide for this can be found [here](dev-role-catalogue).

# AD FS user account
All user interface tests are executed by logging into a running AD FS server.

In the configuration file:
```
ui/src/test/resources/test.properties
```
You need to specify the username and password for the user used in the test cases. It is recommended that the username be "rolunittest01," as the test data generated during the execution of the test cases assumes that the administrator in the solution has this username.  


# Running documentation tests
Please don't create any new documentation tests, as the documentation are being moved to swagger-ui.  
All tests that validate (and document) the APIs are located in the folder:  
```
ui/src/test/java/dk/digitalidentity/rc/test/documentation/
```

These tests can be executed without many prerequisites and generate metadata about the tested services as a side effect of the test. They can be run individually from an IDE like Eclipse or IntelliJ, but if you want to generate API documentation, you must run all tests through Maven.  
  
Upon successful execution of all test cases, an API documentation file will be generated in this folder:  
```
ui/target/generated-docs/main.html
```

# Running User Interface Tests
All user interface tests are executed using Selenium. If an error occurs in the tests, a video will be saved in the target/ folder, showing the progress of the test on the user interface.
The tests can be found in this directory:   
```
ui/src/test/java/dk/digitalidentity/rc/test/ui/
```
