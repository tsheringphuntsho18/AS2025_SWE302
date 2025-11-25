# Practical_05 Report: Integration Testing with TestContainers for Database Testing


## Overview

This project demonstrates a complete and practical approach to backend testing and data handling. It brings together integration testing using TestContainers and a fully isolated PostgreSQL environment to verify how the application behaves with real databases instead of mocks. The implementation covers full CRUD functionality along with more advanced querying features such as pattern matching, record counting, and filtering by dates. It also includes tests focused on transactional behavior and isolation levels to ensure data consistency. In addition, the setup demonstrates how multiple containers specifically PostgreSQL and Redis can work together, with Redis used to test caching mechanisms in a realistic multi-service environment.

## Prerequisites

- Go 1.21 or higher
- Docker Desktop running
- 500MB disk space for Docker images

## Project Structure

```
practical_05/
├── models/
│   └── user.go                          
├── repository/
│   ├── user_repository.go               
│   ├── user_repository_test.go          
│   ├── cached_user_repository.go        
│   └── cached_user_repository_test.go   
├── migrations/
│   └── init.sql                         
├── go.mod 
├── go.sum                              
└── README.md                            
```
## TestContainers Integration

TestContainers enables running real PostgreSQL and Redis instances in Docker containers for integration testing. Each test suite starts containers, initializes schemas, and ensures clean state and isolation. Containers are automatically cleaned up after tests, making the setup CI/CD friendly and reproducible.

**Key Benefits:**
- Real database testing (no mocks)
- Automatic setup/teardown
- Test isolation
- Production-like environment

## Testing Approach

- **TestMain**: Sets up containers and database connections before tests, and tears them down after.
- **CRUD Tests**: Cover all basic operations (Create, Read, Update, Delete).
- **Advanced Queries**: Pattern matching, counting, and date filtering.
- **Transactions**: Test atomicity, rollback, and concurrent access.
- **Multi-Container**: PostgreSQL + Redis for cache testing.

**Isolation Strategies:**
- Cleanup after each test (`defer repo.Delete(id)`)
- Transaction rollback for some tests
- Optionally, fresh containers per test for full isolation

## Key Exercises & Coverage

### 1. Basic CRUD Operations
- `TestGetByID`, `TestGetByEmail`, `TestCreate`, `TestUpdate`, `TestDelete`, `TestList`

### 2. Advanced Queries
- `FindByNamePattern`
- `CountUsers`
- `GetRecentUsers`

### 3. Transactional Behavior
- `BatchCreate`
- `TransferUserData`
- `TestTransactionRollback`
- `TestConcurrentWrites`

### 4. Multi-Container Testing
- Redis caching: cache hit/miss, invalidation, TTL

## How to Run the Tests

**All Tests:**
```bash
go test ./... -v
```

**With Coverage:**
```bash
go test -cover ./repository
go test -coverprofile=coverage.out ./repository
go tool cover -html=coverage.out
```

**Race Detection:**
```bash
go test -race ./repository
```

**Specific Test:**
```bash
go test ./repository -run TestGetByID -v
```

**Skip Slow Tests:**
```bash
go test ./repository -short
```

## Challenges & Solutions

| Challenge                                  | Solution/Approach                                      |
|---------------------------------------------|--------------------------------------------------------|
| Docker container startup delays             | Used Alpine images, increased wait timeouts            |
| Test data isolation                         | Cleanup in tests, transaction rollbacks                |
| Port conflicts                             | Used dynamic port mapping via TestContainers           |
| CI/CD environment differences               | Ensured Docker is available, used portable configs     |
| Slow image pulls in CI                      | Pre-pulled images, cached Docker layers                |
| Data persistence between tests              | Truncated tables or used fresh containers as needed    |




## Conclusion
In summary, this setup shows how effective integration testing can be when TestContainers are used to recreate a production like environment. Each test remains isolated, preventing interference and ensuring reliable, repeatable results. Running PostgreSQL alongside Redis also adds realism by allowing caching and data consistency to be tested across multiple services. The environment works smoothly both locally and in CI/CD pipelines, making it practical for real development workflows. Overall, by using TestMain for structured setup and cleanup, clearing temporary data, and relying on transactions for isolation, the project maintains a clean and dependable testing process from start to finish.
