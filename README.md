# book-api

A minimal Spring Boot REST API sample: CRUD for a `Book` resource, backed by an
in-memory H2 database with Flyway migrations. Meant as a starting point for a
real service, not a toy — proper layering (controller / service / repository),
DTOs at the boundary, bean validation, a global exception handler, and one
MockMvc integration test class.

## Stack

- Java 21
- Spring Boot 3.3 (Web, Data JPA, Validation)
- H2 (in-memory) + Flyway migrations
- JUnit 5 / MockMvc

## Run it

```bash
mvn spring-boot:run
```

API is on `http://localhost:8080`. H2 console (for poking at the DB) is at
`http://localhost:8080/h2-console` — JDBC URL `jdbc:h2:mem:bookdb`, user `sa`,
empty password.

## Test it

```bash
mvn test
```

## Endpoints

| Method | Path              | Body                              | Description        |
|--------|-------------------|------------------------------------|---------------------|
| GET    | /api/books        | —                                  | list all books      |
| GET    | /api/books/{id}   | —                                  | get one book        |
| POST   | /api/books        | `{"title","author","isbn"}`        | create a book       |
| PUT    | /api/books/{id}   | `{"title","author","isbn"}`        | replace a book      |
| DELETE | /api/books/{id}   | —                                  | delete a book       |

```bash
curl -X POST localhost:8080/api/books \
  -H 'Content-Type: application/json' \
  -d '{"title":"Effective Java","author":"Joshua Bloch","isbn":"9780134685991"}'

curl localhost:8080/api/books
```

## Layout

```
src/main/java/com/example/bookapi/
├── BookApiApplication.java
├── book/               # everything about the Book feature lives together
│   ├── Book.java               (entity)
│   ├── BookRequest.java        (inbound DTO)
│   ├── BookResponse.java       (outbound DTO)
│   ├── BookRepository.java
│   ├── BookService.java
│   ├── BookController.java
│   └── BookNotFoundException.java
└── common/
    └── GlobalExceptionHandler.java
```

Packaged by feature, not by layer — adding a second resource (e.g. `Author`)
means a new sibling package, not scattering files across `controllers/`,
`services/`, `repositories/`.

## Extending this

- Swap H2 for Postgres: change `spring.datasource.url` and add
  `org.postgresql:postgresql`; Flyway migrations already live in
  `src/main/resources/db/migration`.
- Add OpenAPI docs: add `springdoc-openapi-starter-webmvc-ui`.
- Add Testcontainers for a real-Postgres integration test instead of H2.
