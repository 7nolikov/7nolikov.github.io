---
title: Maven archetypes
date: 2024-12-14
categories: [java]
---

Maven archetypes are templates that help developers quickly set up a new project with a predefined structure, files, and code for a specific type of application.
It's extremely useful for livecoding interviews when you need to quickly create a project skeleton.

<!--more-->

## How Maven Archetypes Work

- Archetypes act as "project blueprints," saving time and ensuring consistency across projects.
- When creating a new project, you select an archetype, and Maven generates a ready-to-use project structure.
- You can also customize the generated project by providing specific details, like the group ID, artifact ID, and version.

## Two Popular Archetype Examples

### maven-archetype-quickstart

- Ideal for setting up a basic Java project.
- Includes a standard directory structure (src/main/java, src/test/java) and a simple App.java file with a main method.
- Perfect for small projects where no specific framework is required.

### maven-archetype-webapp

- Used to create a basic Java web application.
- Includes a default structure for web development (e.g., src/main/webapp for web files like HTML, CSS, and JSP).
- Sets up essential files like web.xml for deploying the application on a servlet container like Tomcat.

## How to Create a Spring Application Using Maven Archetypes

1. Generate the Project with Maven Quickstart Archetype Run the following command in your terminal:

    ```bash
    mvn archetype:generate 
    -DgroupId=com.example 
    -DartifactId=spring-app 
    -DarchetypeArtifactId=maven-archetype-quickstart 
    -DinteractiveMode=false
    ```

2. Open the pom.xml file in the generated project and add the required dependencies for Spring. For example:

    ```xml
    <dependencies>
        <!-- Spring Core -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.30</version>
        </dependency>
        <!-- Spring Boot Starter for simplicity -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
            <version>3.1.5</version>
        </dependency>
    </dependencies>
    ```

3. Create a Basic Spring Configuration Create a configuration class for the Spring application. For instance:

    ```java
    package com.example;

    import org.springframework.context.ApplicationContext;
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;

    @Configuration
    public class AppConfig {

        @Bean
        public HelloWorld helloWorld() {
            return new HelloWorld();
        }

        public static void main(String[] args) {
            ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
            HelloWorld helloWorld = context.getBean(HelloWorld.class);
            helloWorld.sayHello();
        }
    }

    class HelloWorld {
        public void sayHello() {
            System.out.println("Hello, Spring!");
        }
    }
    ```

4. Build and Run the Project Use the following commands to build and run the project:

    ```bash
    mvn clean package
    java -cp target/spring-app-1.0-SNAPSHOT.jar com.example.AppConfig
    ```

5. Example Spring Application Output
    If the project is configured correctly, running the application will output:

    ```plaintext
    Hello, Spring!
    ```

## Why Use Archetypes

**It is especially useful on technical livecoding interviews!**

Maven archetypes simplify the process of starting new projects by reducing repetitive setup tasks and ensuring that best practices are followed from the start. This makes them helpful for both beginners and experienced developers.

You can learn more about how to use archetypes and their configurations at [Maven Archetype Introduction](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html).

## Using Spring Initializr Instead

If you're open to using a modern tool, **Spring Initializr** is the recommended way to generate Spring projects from scratch. It provides a flexible and easy-to-use interface for generating Spring Boot projects with pre-configured dependencies.

### Steps

1. Go to Spring Initializr.
2. Configure your project:
    - Project: Maven
    - Language: Java
    - Spring Boot Version: Latest stable version
    - Add dependencies like Spring Web, Spring Data JPA, Thymeleaf, etc.
3. Click Generate to download the pre-configured project.
4. Extract and open the project in your favorite IDE.

## Using Spring CLI

If you prefer the speed of command-line tools, you can also use the Spring CLI to create a new Spring project. The following command generates a basic Spring Boot project with common dependencies:

```bash
spring init --dependencies=web,data-jpa,actuator,devtools,h2,lombok 
-t=maven-project demo-project
```

I've just added this command to my hotkey list, so I can start a new Spring project in seconds!
