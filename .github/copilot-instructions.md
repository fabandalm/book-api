# Copilot Instructions

## Stack
Java 21, Spring Boot 3.3, Maven, H2 (dev) + Flyway, JUnit 5 / MockMvc.

## Build & validate
- Build: `mvn clean install`
- Test: `mvn test`

## Conventions
- Package by feature (`book/`), not by layer.
- Controllers stay thin; put logic in `*Service` classes.
- DTOs (records) at the API boundary — never return a JPA entity from a
  controller.
- Schema changes are new Flyway migrations under
  `src/main/resources/db/migration`, never edits to existing ones.
- Every new endpoint needs a MockMvc test covering the happy path and one
  failure case (validation or not-found).

## Do not
- Add dependencies without asking.
- Change `spring.jpa.hibernate.ddl-auto` away from `validate`.
