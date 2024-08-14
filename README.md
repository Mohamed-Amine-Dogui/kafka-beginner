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
2. [Starting Kafka and Creating a Topic](#2-starting-kafka-and-creating-a-topic)
    1. [Start Zookeeper](#21-start-zookeeper)
    2. [Start Kafka](#22-start-kafka)
    3. [Create the Kafka Topic](#23-create-the-kafka-topic)
3. [Creating Your First Kafka Producer](#3-creating-your-first-kafka-producer)
    1. [Setup Producer Properties](#31-setup-producer-properties)
    2. [Create and Configure the Producer](#32-create-and-configure-the-producer)
    3. [Send Data to Kafka](#33-send-data-to-kafka)
4. [Verifying Data Reception](#4-verifying-data-reception)
5. [Running the Producer](#5-running-the-producer)
6. [Code Overview](#6-code-overview)

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

1. Name the project (e.g., `kafka-beginner`).
2. Specify a location on your computer for the project files.
3. Set the **GroupId** to `org.example`.
4. Keep the **ArtifactId** as `kafka-beginner`.
5. Set the version as `1.0-SNAPSHOT`.
6. Click **Finish** to create the project.

## 1.3. Adding Dependencies

### 1.3.1. Configure Subprojects

1. After creating the project, Gradle will synchronize the project structure.
2. **Delete** the default `src/main` and `src/test` directories. These will be replaced with subprojects.
3. Right-click on the project root (`kafka-beginner`), then select **New > Module**.
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

---

## 2. Starting Kafka and Creating a Topic

Before running the Kafka producer, you need to start Zookeeper and Kafka, and then create the topic you'll be sending messages to.

### 2.1. Start Zookeeper

Start Zookeeper by running the following command:

```bash
zookeeper-server-start.sh ~/kafka_2.13-3.8.0/config/zookeeper.properties
```

### 2.2. Start Kafka

Once Zookeeper is running, start the Kafka broker:

```bash
kafka-server-start.sh ~/kafka_2.13-3.8.0/config/server.properties
```

### 2.3. Create the Kafka Topic

Create the topic `demo_java` where the producer will send messages:

```bash
kafka-topics.sh --bootstrap-server localhost:9092 --topic demo_java --create --partitions 3 --replication-factor 1
```

This command creates a topic named `demo_java` with 3 partitions and a replication factor of 1.

---

## 3. Creating Your First Kafka Producer

### 3.1. Setup Producer Properties

In your `ProducerDemo` class, you will first define the properties required to configure your Kafka producer. These properties include the bootstrap server and serializers for keys and values.

```java
// create Producer Properties
Properties properties = new Properties();

//connect to localhost
properties.setProperty("bootstrap.server", "127.0.0.1:9092");

// Set Producer properties
properties.setProperty("key.serializer", StringSerializer.class.getName());
properties.setProperty("value.serializer", StringSerializer.class.getName());
```

### 3.2. Create and Configure the Producer

With the properties set, you can now create and configure the Kafka producer. The producer will be responsible for sending messages to a specific Kafka topic.

```java
// create the Producer
KafkaProducer<String, String> producer = new KafkaProducer<>(properties);
```

### 3.3. Send Data to Kafka

Create a producer record that contains the topic name and the message you want to send. Then, use the producer to send this record to the Kafka broker.

```java
// create a Producer Record
ProducerRecord<String, String> producerRecord =
        new ProducerRecord<>("demo_java", "hello from demo_java topic");

// send data
producer.send(producerRecord);

//  flush: tell the producer to send all data and block until done --synchronous
producer.flush();

// close the producer
producer.close();
```

---

## 4. Verifying Data Reception

To confirm that the data was successfully sent to Kafka, you can use a Kafka Console Consumer. Open a terminal and run the following command:

```bash
kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic demo_java --from-beginning
```

This command will display all messages sent to the `demo_java` topic, including the `"hello from demo_java topic"` message you sent using your producer.

---

## 5. Running the Producer

1. Ensure that your Kafka broker is running on `localhost:9092`.
2. Run the `ProducerDemo` class from IntelliJ IDEA.
3. Check the terminal where your Kafka Console Consumer is

running to verify that the message has been received.

---

## 6. Code Overview

Here is the complete code for your `ProducerDemo` class:

```java
package org.example.kafka;

import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerRecord;
import org.apache.kafka.common.serialization.StringSerializer;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.util.Properties;

public class ProducerDemo {

   private static final Logger log = LoggerFactory.getLogger(ProducerDemo.class.getSimpleName());
   public static void main(String[] args) {
      log.info("Hello world");

      // create Producer Properties
      Properties properties = new Properties();

      //connect to localhost
      properties.setProperty("bootstrap.servers", "127.0.0.1:9092");

      //connect to Conduktor playground
//        properties.setProperty("bootstrap.server", "cluster.playground.cdkt.io:9092");
//        properties.setProperty("security.protocol", "SASL_SSL");
//        properties.setProperty("sasl.jaas.config", "org.apache.kafka.common.security.plain.PlainLoginModule required  username=\"alice\" password=\"alice-secret\";");
//        properties.setProperty("sasl.mechanism", "PLAIN");


      // set Producer properties
      properties.setProperty("key.serializer", StringSerializer.class.getName());
      properties.setProperty("value.serializer", StringSerializer.class.getName());

      // create the Producer
      KafkaProducer<String, String> producer = new KafkaProducer<>(properties);

      // create a Producer Record
      ProducerRecord<String, String> producerRecord =
              new ProducerRecord<>("demo_java", "hello from demo_java topic");

      // send data
      producer.send(producerRecord);



      //  flush: tell the producer to send all data and block until done --synchronous
      producer.flush();

      // close the producer
      producer.close();
   }
}
```
