# 1. Kafka Java Project Setup Guide

This guide will walk you through setting up a Kafka project using Java, Gradle, and IntelliJ IDEA, including all necessary dependencies and initial configurations.

## Table of Contents

1. [Kafka Java Project Setup Guide](#1-kafka-java-project-setup-guide)
    1. [Prerequisites](#11-prerequisites)
    2. [Setting Up the Project](#12-setting-up-the-project)
        1. [Create a New Project in IntelliJ IDEA](#121-create-a-new-project-in-intellij-idea)
        2. [Configure Project Settings](#122-configure-project-settings)
    3. [Adding Dependencies](#13-adding-dependencies)
    4. [Final Project Setup Steps](#14-final-project-setup-steps)
    5. [Verifying the Setup](#15-verifying-the-setup)
2. [Next Steps](#2-next-steps)

---

## 1.1. Prerequisites

Before starting, make sure you have the following tools installed:

- **IntelliJ IDEA Community Edition**: The development environment used in this guide.
- **Java 11 JDK**: Recommended is Amazon Corretto 11, but any Java 11 distribution should work.
- **Gradle**: Build automation tool used for managing dependencies and building the project.

## 1.2. Setting Up the Project

### 1.2.1. Create a New Project in IntelliJ IDEA

1. Open **IntelliJ IDEA**.
2. Click on **New Project**.
3. On the left-hand side, select **Gradle** as the project type.
4. Choose **Java** as the language for your Gradle project.
5. For the **Project SDK**, select the JDK you installed (e.g., **Corretto 11**).
6. Click **Next**.

### 1.2.2. Configure Project Settings

1. Name the project (e.g., `kafka-beginners-course`).
2. Specify a location on your computer for the project files.
3. Set the **GroupId** to `org.example`.
4. Keep the **ArtifactId** as `kafka-beginners-course`.
5. Set the version as `1.0-SNAPSHOT`.
6. Click **Finish** to create the project.

## 1.3. Adding Dependencies

### 1.3.1. Configure Subprojects

1. After creating the project, Gradle will synchronize the project structure.
2. **Delete** the default `src/main` and `src/test` directories. These will be replaced with subprojects.
3. Right-click on the project root (`kafka-beginners-course`), then select **New > Module**.
4. Select **Gradle** as the module type, choose **Java** as the language, and click **Next**.
5. Name this module `kafka-basics`, and keep the **GroupId** as `org.example`.
6. Finish setting up the module.

### 1.3.2. Add Kafka Dependencies

1. Go to the `build.gradle` file inside the `kafka-basics` module.
2. Add the following dependencies:

```gradle
dependencies {
    // https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients
    implementation 'org.apache.kafka:kafka-clients:3.8.0'

    // https://mvnrepository.com/artifact/org.slf4j/slf4j-api
    implementation 'org.slf4j:slf4j-api:2.0.16'


    // https://mvnrepository.com/artifact/org.slf4j/slf4j-simple
    implementation 'org.slf4j:slf4j-simple:2.0.16'
}
```

3. **Note**: Ensure all `testImplementation` entries are changed to `implementation` if not needed.

4. Reload the Gradle project by clicking on the **Load Gradle Changes** icon or by using the **Refresh** button under the Gradle tool window.

## 1.4. Final Project Setup Steps

1. **Delete** the `src/main` and `src/test` folders in the root project if IntelliJ automatically recreated them.
2. Use only the directories within the `kafka-basics` module for your development work.
3. Ensure that all Gradle dependencies have been loaded correctly by checking the **External Libraries** section.

## 1.5. Verifying the Setup

### 1.5.1. Create a Java Class

1. Inside the `kafka-basics/src/main/java` directory, create a new package `org.example.kafka`.
2. Inside this package, create a new Java class named `ProducerDemo`.

### 1.5.2. Write and Run a Basic Java Program

1. In `ProducerDemo`, add the following code:

```java
package org.example.kafka;

public class ProducerDemo {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```

2. **Run** the `ProducerDemo` class to ensure the project is correctly set up. You should see `"Hello World"` in the output window.

### 1.5.3. Configure IntelliJ IDEA for Building

1. Go to **IntelliJ IDEA > Preferences > Build, Execution, Deployment > Build Tools > Gradle**.
2. Under **Build and run using**, select **IntelliJ IDEA**.
3. Apply the changes and run the `ProducerDemo` class again to confirm everything works correctly.
