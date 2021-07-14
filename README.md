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

# Configuring the SonarScanner
We’re going to start by including the sonar scanner in our build.gradle file in the plugins object:
```
plugins {
    id 'org.sonarqube' version '3.0'
}
```
