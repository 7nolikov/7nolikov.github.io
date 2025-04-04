---
title: Standalone Spring tests
date: 2024-12-04
categories: [spring, testing]
---

I was surprised that Spring Test library doesn't support standalone acceptance tests format out of the box.
Here's how I tackled this issue and why standalone tests are useful for modular Spring applications.

<!--more-->

## Motivation

My project separates core logic from cloud-specific modules. Usual Spring Test doesn't work for this. I needed standalone tests to validate HTTP status codes and response content as black-box tests.

Previous tests used plain Java. I wanted to use Spring features like dependency injection and properties management. Standalone tests also serve as acceptance tests, running in a separate CI/CD pipeline stage.

## How to implement

Use `@SpringBootTest(webEnvironment = WebEnvironment.NONE)` with a lightweight configuration.

Import only the necessary beans, such as `TestRestTemplate` and test-specific properties.

```java
@SpringBootTest(webEnvironment = WebEnvironment.NONE)
@Import(IntegrationTestConfig.class)
@TestPropertySource(locations = "classpath:test-application.properties")
public abstract class BaseIntegrationTest {

  @Autowired
  protected TestRestTemplate restTemplate;
  @Autowired
  protected TestProperties properties;
  @Autowired
  protected TestFixtures testFixtures;

  /**
   * This method is used to set up the test environment, access tokens and environment-specific properties.
   */
  public void setup() {}

  /** This optional method is used to clean up test data. */
  public void tearDown() {}
}

```

Configure test context with `@SpringBootConfiguration`. Use `@ComponentScan` to load only core beans.

Add `@EnableConfigurationProperties` for test settings. Include useful beans like `TestRestTemplate` and `ObjectMapper`.

```java
@SpringBootConfiguration
@ComponentScan({"org.application"})
@EnableConfigurationProperties(TestProperties.class)
public class IntegrationTestConfig {

  @Bean
  public TestRestTemplate testRestTemplate(TestProperties properties) {
    return new TestRestTemplate(new RestTemplateBuilder().rootUri(properties.getBaseUrl()));
  }

  @Bean
  public static ObjectMapper initMapper() {
    // custom ObjectMapper configuration
    ObjectMapper objectMapper = new ObjectMapper();
    objectMapper.registerModule(new JavaTimeModule());
    objectMapper.setSerializationInclusion(Include.NON_NULL);
    objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
    objectMapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
    objectMapper.setTimeZone(TimeZone.getDefault());
    return objectMapper;
  }
}

```

## Integration

Standalone tests run independently in a CI/CD pipeline. These tests validate application behavior post-deployment, acting as acceptance tests for backend APIs.

## Conclusion

Compared to plain Java tests, standalone Spring tests leverage features like **dependency injection** and **property management**. They allow a shared test base while supporting different cloud providers via injected beans.

## Alternatives

Cucumber, RestAssured, or Robot Framework are possible alternatives. But standalone Spring tests strike a balance between simplicity, modularity, and the power of Spring features.

What are your thoughts on this approach? Have you used standalone Spring tests in your projects? Share your experience!
