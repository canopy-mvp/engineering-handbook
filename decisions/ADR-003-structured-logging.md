# ADR-003: Structured Logging Standard

## Status
Accepted

## Date
2024-03-10

## Context
Inconsistent logging across services made debugging production issues
difficult. Some services used console.log, others used Winston with
different formats. PII was occasionally logged, creating compliance risk.

## Decision
Standardize on Pino with JSON output:
- All services use the shared `@novapay/logger` package
- Structured JSON format with: timestamp, level, service, requestId, message
- Automatic PII redaction for known patterns (email, phone, SSN, card numbers)
- Log levels: fatal, error, warn, info, debug, trace
- Production: info level. Development: debug level.

## Rules
- NEVER use console.log in production code
- NEVER log: customer name, email, phone, SSN, card numbers, account numbers
- SAFE to log: internal IDs (user_id, txn_id), amounts (non-PII), error codes
- Always include requestId for correlation

## Consequences
- Consistent log format enables centralized log analysis (CloudWatch Insights)
- PII redaction reduces compliance risk
- Request correlation enables distributed tracing
