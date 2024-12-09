---
title: Standalone Spring tests
date: 2024-12-03
categories: [Spring, Testing]
---

I was surprized that Spring Test library doesn't support standalone acceptance tests format out of the box.
Here's how I tackled this issue and why standalone tests are useful for modular Spring applications.

## Motivation

My project separates core logic from cloud-specific modules. Usual Spring Test doesn't work for this. I needed standalone tests to validate HTTP status codes and response content as black-box tests.

Previous tests used plain Java. I wanted to use Spring features like dependency injection and properties management. Standalone tests also serve as acceptance tests, running in a separate CI/CD pipeline stage.

## How to implement

Use `@SpringBootTest(webEnvironment = WebEnvironment.NONE)` with a lightweight configuration. Import only the necessary beans, such as `TestRestTemplate` and test-specific properties.

## Example

Use `@Import(IntegrationTestConfig.class)` and `@TestPropertySource(locations = "classpath:test-application.properties")`. Inject beans like `TokenProvider` or `TestFixtures` for test-specific needs.

Configure test context with `@SpringBootConfiguration`. Use `@ComponentScan` to load only core beans. Add `@EnableConfigurationProperties` for test settings. Include useful beans like `TestRestTemplate` and `ObjectMapper`.

## Integration

Standalone tests run independently in a CI/CD pipeline. These tests validate application behavior post-deployment, acting as acceptance tests for backend APIs.

## Conclusion

Compared to plain Java tests, standalone Spring tests leverage features like **dependency injection** and **property management**. They allow a shared test base while supporting different cloud providers via injected beans.

## Alternatives

Cucumber, RestAssured, or Robot Framework are possible alternatives. But standalone Spring tests strike a balance between simplicity, modularity, and the power of Spring features.

What are your thoughts on this approach? Have you used standalone Spring tests in your projects? Share your experience!
