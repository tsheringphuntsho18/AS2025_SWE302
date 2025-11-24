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

## Exercises Covered

### Exercise 1-2: Basic CRUD (user_repository_test.go)
- `TestGetByID` - Retrieve user by ID
- `TestGetByEmail` - Retrieve user by email
- `TestCreate` - Create new user
- `TestUpdate` - Update existing user
- `TestDelete` - Delete user
- `TestList` - List all users

### Exercise 3: Advanced Queries (user_repository.go)
- `FindByNamePattern` - Pattern matching with ILIKE
- `CountUsers` - Count total users
- `GetRecentUsers` - Filter by date range

### Exercise 4: Transactions (user_repository.go)
- `BatchCreate` - Atomic batch operations
- `TransferUserData` - Complex transactions
- `TestTransactionRollback` - Verify rollback behavior
- `TestConcurrentWrites` - Concurrent access

### Exercise 5: Multi-Container (cached_user_repository_test.go)
- PostgreSQL + Redis setup
- Cache hit/miss testing
- Cache invalidation
- TTL verification

## Running Tests

### All Tests
```bash
go test ./... -v
```

### With Coverage
```bash
go test -cover ./repository
go test -coverprofile=coverage.out ./repository
go tool cover -html=coverage.out
```

### Race Detection
```bash
go test -race ./repository
```

### Specific Test
```bash
go test ./repository -run TestGetByID -v
```

### Skip Slow Tests
```bash
go test ./repository -short
```

## Understanding the Tests

### TestMain Setup
Each test file has a `TestMain` function that:
1. Starts Docker container(s)
2. Waits for services to be ready
3. Initializes database schema
4. Runs all tests
5. Cleans up containers

### Test Isolation
Tests maintain isolation through:
- Deferred cleanup (`defer repo.Delete(user.ID)`)
- Transaction rollback for some tests
- Shared container with careful data management

### Container Lifecycle
```
Test Start → Container Starts → Database Initializes → Tests Run → Container Stops
```


## Key Learnings

1. **Real Database Testing**: Tests run against actual PostgreSQL, not mocks
2. **Container Lifecycle**: Automatic setup and teardown
3. **Test Isolation**: Each test can have clean state
4. **CI/CD Ready**: Works in any environment with Docker
5. **Production-Like**: Same database as production

## Next Steps

- Apply TestContainers to your own projects
- Try other databases (MySQL, MongoDB)
- Experiment with message queues (Kafka, RabbitMQ)
- Add API layer and test end-to-end
- Explore performance optimization

## Resources

- [TestContainers Go Docs](https://golang.testcontainers.org/)
- [PostgreSQL Module](https://golang.testcontainers.org/modules/postgres/)
- [Redis Module](https://golang.testcontainers.org/modules/redis/)
- Main Tutorial: `../practical5.md`
