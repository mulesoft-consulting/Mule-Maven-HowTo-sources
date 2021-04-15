# initial-project
This project is an Experience API...

## Workstation properties for Studio

The file **example-studio-run-arguments.txt** shows the properties set in the Mule runtime environment. When running the application in Studio, include these values in the VM Arguments tab. 

## Configuration Properties

Properties that do not change for any of the deployment environments can be stored in the src/main/resources/common.properties file. Example of what is the common.property file is:

* api.base is the base uri for the API, the default is the project name taken from the pom file. This is used to form the API's base uri.
* api.semantic.version is the API syntax version, used to form the API's base uri.

The deployment environment sensitive property files are used to store the application's placeholder name and properties. These files are named {env}-config.properties (for instance DEV-config.properties). The ApiConfigTool uses this same naming convention to interact with API Manager to store the API management values for each environment.


## Runtime properties

There are many properties associated with the API as part of its Runtime configuration. These are set in deployment properties for CloudHub, the wrapper.conf file for standalone Mule Runtimes and in the Studio Run configuration arguments.

An example of the runtime properties needed for Studio are in the file "example-studio-run-arguments.txt".

The pom.xml file contains the Runtime properties for CloudHub deployment.

## Maven Settings

The Mule deployment assumes that certain deployment properties will come from profiles specified when the mvn command is executed. An example-settings.xml is provided as a reference for creating your own settings.xml file. The settings.xml file is a standard part of Maven and is described in its online documentation. 

Once the settings.xml file has been created, export a u and a p shell environment variables containing your Anypoint user name and password. 

## Specifying User Credentials on Command Line

Most of the following commands require your Anypoint user name and password. If you have set up the environment variables as described above, then you can specify the credentials as described below. Note that deployment commands will require Anypoint native users, single signon users will not be able to deploy to Anypoint Runtime environments.

For Windows systems, use this format for specifying your user credentials:

```
-Du=%u% -Dp=%p%
```

For Mac/OSX systems, use this format for specifying your user credentials:

```
-Du=${u} -Dp=${p}
```

### Build on local workstation

```
mvn clean install -Du= -Dp=
```

### Build and Deploy to Cloudhub Runtime:

```
mvn clean install deploy -DmuleDeploy -Denv=xxx -Pcloudhub -Ppublic-lb -Du= -Dp= -s ~/.m2/settings-blog.xml
```

Replacing the **xxx** with the appropriate lowercase environment name (for instance dev, test, or prd)

### Build and Deploy to Runtime Fabric (RTF):

This takes two commands,
  1) Build and publish the deployable to Exchange:
  
```
mvn clean install deploy -Pexchange -Du= -Dp= -s ~/.m2/settings-blog.xml
```

  2) Deploy from Exchange to RTF:

```
mvn mule:deploy -Dmule.artifact=dummy -Denv=xxx -Prtf -Du= -Dp= -s ~/.m2/settings-blog.xml
```

### Build and Deploy to Hybrid Mule Runtime:

```
mvn clean install deploy -DmuleDeploy -Denv=xxx -Phybrid -Du= -Dp= -s ~/.m2/settings-blog.xml
```

Replacing the xxx's with the appropriate environment name.

### Additional Deployment Command Options

The deployment command can also include the option **-Pdlb** can be used to force deployment to CloudHub using the DLB ports (8091 and 8092)

Or the option **-Ppublic-lb** can be used to force deployment to CloudHub using the public ports (8081 and 8082)

By default, -Ppublic-lb is used.
