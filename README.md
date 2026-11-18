# Event-Driven Order Processing for FinTech Ecosystems

## Client Story

**Client:** NovaPay SG — a Singapore-based FinTech startup processing digital payments and loan disbursements for SMEs.

## The Problem

NovaPay SG was processing 200+ financial transactions a day — payment confirmations, loan approvals, ledger updates. But their backend was fragile.

Every time a customer triggered a payment, the system worked like a chain reaction — one Lambda directly called the next, which called the next. At low volume it worked. When traffic spiked, one slow service caused the entire chain to fail. Transactions were falling through the cracks. No confirmation. No audit trail.

Their CTO came in with one requirement: **"We cannot afford to lose a single transaction. Fix it."**

## The Solution

Replaced the tightly coupled Lambda chain with an event-driven architecture using AWS EventBridge as the central nervous system.

When a transaction triggers, it publishes an event to a custom EventBridge bus. Independent microservices — ledger updates, customer alerts, audit logging — subscribe to that bus and process in parallel. Each service is fully decoupled. If one fails, SQS retries automatically. If it still fails, the message moves to a Dead Letter Queue — nothing is lost, ever.

AWS X-Ray provides full visibility into every transaction's journey across services.

## Architecture

```
API Gateway → Lambda (Producer) → EventBridge Bus
                                        ↓
                    ┌───────────────────┼───────────────────┐
                    ↓                   ↓                   ↓
             Ledger Lambda       Alerts Lambda        Audit Lambda
             (Supabase)          (SNS → Email)        (CloudWatch)
                    ↓
                   SQS → DLQ (zero message loss)
                    ↓
                 X-Ray (distributed tracing)
```

## Tech Stack

- AWS EventBridge
- AWS Lambda (Python + Boto3)
- Amazon SQS + Dead Letter Queue
- AWS X-Ray
- Amazon SNS
- CloudWatch

## Project Phases

| Phase | Task | Time |
|-------|------|------|
| 1 | Event Bus Architecture | ~4h |
| 2 | Refactor Lambda Producers | ~6h |
| 3 | Subscribed Microservices | ~8h |
| 4 | Distributed Tracing with X-Ray | ~5h |
| 5 | Dead Letter Queue Integration | ~4h |

**Total: ~27 hours**

## Status
🚧 In Progress
