# ADR-001: Microservices Architecture

## Status
Accepted

## Date
2024-01-15

## Context
NovaPay started as a monolithic Node.js application. As the team grew to 40+
engineers and transaction volume exceeded 10M/month, we hit scaling and
deployment bottlenecks. Multiple teams were blocked by merge conflicts and
coupled deployments.

## Decision
Decompose the monolith into domain-bounded microservices:
- **api-gateway**: Public API, routing, rate limiting
- **payments-core**: Transaction processing, ledger
- **users-service**: Authentication, profiles, sessions
- **credit-service**: Credit scoring, risk assessment
- **notification-hub**: Email, SMS, push notifications
- **etl-pipeline**: Data processing, reporting
- **webhook-gateway**: Inbound webhook processing

## Consequences
- Teams can deploy independently
- Services communicate via async events (SQS) where possible, sync REST only when necessary
- Each service owns its database (no shared databases)
- Increased operational complexity (monitoring, tracing, debugging)
- Need for service mesh / API gateway for routing
