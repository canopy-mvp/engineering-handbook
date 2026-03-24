# ADR-002: Repository Pattern for Database Access

## Status
Accepted

## Date
2024-02-01

## Context
Multiple patterns were being used for database access across services:
direct Prisma calls in route handlers, raw SQL in some places, and
inconsistent error handling. This made it hard to enforce access controls,
add caching, or switch ORMs.

## Decision
All database access goes through a Repository layer:
- One repository per aggregate root (UserRepository, TransactionRepository, etc.)
- Repositories are the ONLY layer that imports Prisma
- Route handlers and services call repository methods
- Repositories handle connection pooling, retries, and error translation

## Consequences
- Consistent data access patterns across all services
- Easy to add caching layer (Redis) between service and repository
- Easy to mock for unit tests
- Slightly more boilerplate per entity
- Enforces separation of concerns
