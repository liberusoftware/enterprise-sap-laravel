# Enterprise Architecture

## Overview

The enterprise domain models integrated business processes inspired by ERP systems. It connects organisational structure, financial management, supply chain, human resources, and asset management into a unified platform with full audit trails and compliance controls.

## Organisational Structure

- `Organisation` is the top-level entity representing a company, department, or business unit.
- Teams form hierarchical structures under organisations with defined reporting lines.
- `Role` records define functional responsibilities (manager, accountant, warehouse operator) linked to permission sets.
- Position assignments bind employees to roles within teams with effective date ranges for historical tracking.
- Organisation hierarchies support matrix reporting with primary and secondary reporting lines.

## Business Process Management

- Workflows are defined as ordered steps with conditional branching, parallel paths, and escalation rules.
- Each process instance tracks current step, assigned actor, due date, and completion history.
- Approval chains support single approver, majority vote, and consensus models.
- Audit trails record every state transition with actor, timestamp, before/after values, and optional comments.

## Financial Management

- `Ledger` records group `Account` entries following standard double-entry bookkeeping.
- `Transaction` records link debits and credits with balanced entries validated at commit time.
- Period closing locks ledgers for a fiscal period, preventing further modifications.
- Financial reports (balance sheet, P&L, cash flow) are generated from rolled-up account balances.
- Multi-currency support tracks exchange rates per transaction date with revaluation workflows.

## Supply Chain

- Procurement workflows move requests through approval, PO generation, receiving, and invoicing.
- Inventory tracks stock levels by location with reservations for pending orders.
- Logistics manages shipping methods, tracking numbers, and delivery confirmations.
- Order management handles customer orders from quote through fulfillment and returns.
- Reorder points trigger automatic procurement suggestions when stock falls below threshold.

## Human Resources

- `Employee` records track personal details, employment history, and compensation.
- Position management links employees to organisational roles with salary bands.
- Time tracking captures attendance, leave requests, and overtime with approval workflows.
- Payroll runs process timesheets, apply deductions and benefits, and generate pay slips.
- Performance reviews support goal setting, self-assessment, and manager evaluation cycles.

## Asset Management

- The asset register tracks depreciable and non-depreciable assets with acquisition cost and date.
- Depreciation runs calculate book value using straight-line, declining balance, or units-of-production methods.
- Maintenance schedules trigger preventive work orders based on time or usage intervals.
- Disposal workflows manage sale, transfer, or write-off with approval chains and documentation.

## Integration Patterns

- REST APIs expose resources with OpenAPI documentation and OAuth2 authentication.
- EDI support handles X12 and EDIFACT message formats for partner data exchange.
- Webhooks deliver event notifications to registered endpoints with retry and signing.
- Message queues (Redis, RabbitMQ, SQS) handle asynchronous processing and inter-service communication.
- Import/export jobs support CSV, XML, and JSON formats with mapping validation.

## Compliance and Governance

- Audit logs are immutable and centrally retained with tamper-evident hashing.
- Data retention policies define how long different record types are kept before archival or deletion.
- Policy enforcement checks run on write operations to ensure compliance with configured rules.
- Role-based access control with segregation-of-duties checks prevents conflicting permissions.
- GDPR and privacy tools support data export and right-to-erasure workflows.

## Module Boundaries and Extension Points

| Module | Responsibility | Extension Points |
|--------|---------------|------------------|
| Organisation | Structure, roles, positions | Custom hierarchy types |
| BPM | Workflows, approvals, audit | Custom step types, transition rules |
| Finance | Ledgers, transactions, reports | Custom report generators, tax rules |
| Supply Chain | Procurement, inventory, logistics | Custom shipping providers |
| HR | Employees, payroll, time | Custom payroll calculations |
| Assets | Register, depreciation, disposal | Custom depreciation methods |
| Integration | API, EDI, webhooks, queues | Custom message formats, endpoints |

### Extension Points

- Register custom workflow steps via the `WorkflowStepRegistry`.
- Implement `ReportGeneratorInterface` to add financial report types.
- Add EDI format parsers through the `EdiParserInterface`.
- Hook into audit events via the `AuditTrailRecorder`.
- Define custom depreciation strategies implementing `DepreciationMethodInterface`.
- Register webhook event subscriptions through the event dispatcher.
