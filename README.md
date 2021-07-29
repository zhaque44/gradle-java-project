# gradle-java-project

I’m working on a Java Spring Boot service and the time has come to upgrade from Apache Maven to Gradle. At last, 
I won’t need that pom.xml file anymore and I’ll have something much more readable. I’m going to be providing steps on how 
to get everyone up and running as well configuring a SonarScanner to do periodic build checks.

# Installation

```
$ brew install gradle
```
If you want to make sure the install was successful, check the version of Gradle you have installed by running this command:
```
$ gradle -v
```
the terminal output should list out the Gradle version & Groovy version on your machine. You can also run:
```
$ which gradle
```
you’ll see Gradle installed in your `/usr/local/bin` directory, which is a common place where third party software is installed on your box.

# Setting up your project
Once you have Gradle installed, you can now run the initialization script in a new or existing project:
```
$ gradle init
```
The init built-in task initializes Gradle in your repository. If you run ls in your terminal you’ll see that a wrapper folder was auto-generated in the gradle directory:
```
├── gradle 
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
```
A settings.gradle file was also generated, which defines the project being built.
```
rootProject.name = 'my-service'
```
The `rootProject.name` is where you can assign the name of the service, the assignment will occur during run time of your build script. You should also see these files listed below at the root level of your project:
```
├── gradlew 
├── gradlew.bat
```
these are wrapper files that are generated when a Gradle project is instantiated.
# build.gradle
When executing tasks via the command line, gradle looks for the build.gradle file which is a build execution script. This script allows you to run tasks that will make your day to day work life much easier and streamline efforts such as compiling your changes, running tests, etc., etc. To start off we’re going to want to add plugins to our build script:
```
plugins {
    id 'org.springframework.boot' version 'VERSION.RELEASE'
    id 'io.spring.dependency-management' version 'VERSION.RELEASE'
    id 'java'
}
```
Add the standard plugins for your Spring Boot project: Spring Dependency Management and Spring Boot Framework. You can also add plugins like SonarQube which is a tool used to continuously inspect your code changes for security vulnerabilities and whole lot more. After that we’re going to set the version of our Java project:
```
version = "0.0.${System.getenv('SEM_VER')}"
```
Gradle allows you to declare the version as a String this isn’t mandatory, so if you don’t set the version it will just default to unspecified. A common practice in a microservices architecture is to share a common set of dependencies across all services. A custom configuration will make it easier to incorporate all of these dependencies, which can live in a specific location. You can use the built in apply function in Gradle to call your parent gradle file which probably lives in JFrog Artifactory or Nexus:
```
apply from: "https://pathtoparentfile.gradle"
```
Now add the necessary dependencies needed for your project, by declaring a dependencies object in our `build.gradle` file:
```
dependencies {
    implementation 'org.postgresql:postgresql:42.2.8.jre7'
    testImplementation 'junit:junit:4.13'
}
```
There are two types of dependencies being add to the dependencies object above:
- implementation: these are required dependencies needed for your project to compile, above it shows us adding PostGreSQL database to our project as an example.
- testImplementation: these dependencies are used for compiling and running tests, but not necessarily required for running the project’s runtime code. Above we are adding the JUnit dependency needed to test the various endpoint in our project.
# Configuring the SonarScanner
We’re going to start by including the sonar scanner in our build.gradle file in the plugins object:
```
plugins {
    id 'org.sonarqube' version '3.0'
}
```
