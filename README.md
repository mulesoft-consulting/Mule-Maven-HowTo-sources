# Standardizing Maven files

There are two files that are standardized to create a reusable Maven framework for Mule projects. These two files (settings.xml and pom.xml) are starting points for creating your own standard.

## About the Maven settings.xml file
There are environment specific properties that define where a project is to be deployed, the default deployment properties, and the location of binary artifacts. These properties are specified in a settings.xml file. 

Maven is installed and configured to use a special directory in the user’s home directory. Locate the user’s home directory and find the Maven folder (or directory) named .m2 (note the ‘dot’ in front of the m2). We’ll call this the .m2 directory.

The settings.xml file is located in the .m2 directory. We use the settings.xml file to define the Mule runtime environments available in the Anypoint Runtime Manager (ARM), as well as defining where various compilation and packaging dependencies can be found.  

### Review settings.xml contents

* The global-settings properties contain the Anypoint Platform attributes, SCM definitions and deployment attributes used for all deployment environments.

* The “deployment configuration” profiles (such as dev, test and prd) define the specific deployment attributes used for each environment.

* The standard-repositories profile contains the list of artifact repositories that will be used for locating binary dependencies used by Maven to execute its functions and package its binary artifacts.

* The servers section contains the credentials for the repositories in the standard-repositories profile.

* The proxies section defines the proxy servers to use when accessing the repositories..none of the proxy definitions are active in this setup.


## About the Mule project pom.xml file
The second standardized file is the pom.xml which is in each mule project. The pom.xml provides the project specific information needed to assemble the Mule project into a binary artifact. 

The pom.xml is used by the mvn command to execute the functions such as packaging the code into executable form, publishing the artifact to a binary repository, deploying the binary artifact to an ARM environment, etc. 

### Review pom.xml contents

Quickly reviewing what is in the pom.xml

* The top includes the name of the artifactId which will be used in creating the name of the deployed application. Also included is the version of the artifact.

* The groupId of the application will be the Anypoint Org id where the application will be deployed. This is so that we can publish our deployable artifacts in Anypoint Exchange.

* The app.runtime contains the Mule runtime version will use when deploying to CloudHub and Runtime Fabrics and when running in Anypoint Studio. Note that Anypoint Studio will often change this value to a patched runtime version name. Deployments to CloudHub and RTF will fail if this patched version number is not replaced with a version number the Runtime Manager recognizes, such as 4.3.0 instead of 4.3.0-02042021 that Studio might use.

* The maven-deploy-plugin.verson, mule.maven.plugin.version and maven-release-plugin.version are set to versions that have been tested together and are known to work properly. In particular, the maven-release-plugin does not work with some combinations of the other two plugins. Make sure to test the functionality you need before moving to a different version of these three plugins.

* Any files in the src/main/resources project folder will be packaged into the deployable artifact un-changed.

* Any files in the src/main/resources-filtered will be scanned by Maven during its packaging and any property placeholder values found from the pom.xml file will replace the placeholder name (for example ${project.artifactId} will be replaced in the src/main/resources-filtered files. This feature is generally not used since the pom.xml properties can be directly injected into the runtime using deployment properties.

* Two profiles are used to support the dlb and public-lb profile options. But note that the project file must use the ${api.http.port} placeholder for the HTTP Listener port and ${api.https.port} for the HTTPS Listener port for this to work.

* There are three profiles that determine which deployment model will be used. All three use the mule-maven-plugin to execute the deployment through the Anypoint Runtime Manager (ARM) interface.
	* -Pcloudhub to deploy to CloudHub

	* -Prtf to deploy to a runtime fabric

	* -Phybrid to deploy to a Mule standalone
	
* The -Pexchange profile is used to publish deployable binaries to Exchange.

* The -Partifact-repo is used to publish deployable binaries to an artifact repository (although this must be configured in order to be used).

* The scm section is used to configure the source code management system that will be used by some of the Maven functions. This will be discussed later in this series. The standard Maven command described below do not use SCM. The format of the SCM elements may be different from what is provided in this setup. This setup is using GitHub accessed with SSH. 

