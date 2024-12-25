---
title: Testcontainers Docker Compose module
date: 2024-12-15
categories: [testing]
---

Using **Docker Compose module** with Testcontainers you can simplify testing for applications that rely on multiple services like:

- Databases
- APIs
- Message queues

This approach ensures that all dependencies start and work together as expected in a controlled, reproducible environment during your Java tests.

<!--more-->

## Key features of the Docker Compose module

- It allows you to define complex service dependencies in a single docker-compose.yml file.
- During tests, Testcontainers can automatically start all services defined in the Compose file.
- It ensures your test environment is isolated and reproducible.
- The module also provides Java APIs to control the containers and check their status during tests.

## How to use the Docker Compose module

### Add Testcontainers Dependency

Include the Testcontainers library in your pom.xml (for Maven) or build.gradle (for Gradle):

```xml
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>testcontainers</artifactId>
    <version>YOUR_VERSION</version>
    <scope>test</scope>
</dependency>
```

### Create a Docker Compose File
  
Define the services your application needs in a docker-compose.yml file:

```yaml
version: '3.8'
services:
  db:
    image: postgres:15
    environment:
      POSTGRES_USER: test
      POSTGRES_PASSWORD: test
      POSTGRES_DB: testdb
    ports:
      - "5432:5432"

  redis:
    image: redis:latest
    ports:
      - "6379:6379"
```

### Write the Test Class
  
Use the DockerComposeContainer class in your test to manage services:

```java
import org.junit.jupiter.api.Test;
import org.testcontainers.containers.DockerComposeContainer;
import org.testcontainers.junit.jupiter.Container;
import org.testcontainers.junit.jupiter.Testcontainers;

import java.io.File;

@Testcontainers
public class MyAppIntegrationTest {

    @Container
    public static DockerComposeContainer<?> environment =
            new DockerComposeContainer<>(new File("src/test/resources/docker-compose.yml"))
                .withExposedService("db", 5432)  // Expose PostgreSQL
                .withExposedService("redis", 6379); // Expose Redis

    @Test
    public void testAppServices() {
        String dbHost = environment.getServiceHost("db", 5432);
        Integer dbPort = environment.getServicePort("db", 5432);

        String redisHost = environment.getServiceHost("redis", 6379);
        Integer redisPort = environment.getServicePort("redis", 6379);

        // Use these values to connect to the services and run assertions
        System.out.println("DB is running at " + dbHost + ":" + dbPort);
        System.out.println("Redis is running at " + redisHost + ":" + redisPort);

        // Add assertions or actual app testing here
    }
}
```

### Run the Test

The test automatically starts the services defined in docker-compose.yml before execution.
Testcontainers ensures that the services shut down after the tests complete.

## Example Application for Testing

Imagine a shopping cart microservice that depends on:

- A PostgreSQL database for storing user and cart data.
- A Redis cache for storing temporary session data.

With the Docker Compose module, you can:

- Simulate the interaction between these services.
- Write tests to validate database connectivity, query correctness, and cache behavior under test scenarios.

## Conclusion

This approach simplifies the setup for complex applications by managing dependencies, eliminating manual configuration, and ensuring consistency across test runs.

Read more about Docker Compose module:
[https://java.testcontainers.org/modules/docker_compose/](https://java.testcontainers.org/modules/docker_compose/)
