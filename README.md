# Bank of America (bank-of-america)
Bank of America is a multinational investment bank and financial services holding company providing consumer banking, wealth management, corporate banking, and investment banking services worldwide. The CashPro Developer Studio provides REST APIs for corporate treasury clients to automate account management, payments, balance reporting, and statement access.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/bank-of-america/refs/heads/main/apis.yml)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags:

 - Banking, Corporate Banking, Finance, Payments, Treasury, CashPro

## Timestamps

- **Created:** 2024-01-01
- **Modified:** 2026-04-19

## APIs

### Bank of America CashPro API
The Bank of America CashPro API enables corporate treasury clients to programmatically access banking services including payments, account information, balance reporting, and transaction history. Supports over 350 payment types.

**Human URL:** [https://developer.bankofamerica.com/](https://developer.bankofamerica.com/)

#### Tags:

 - Accounts, Balances, Banking, Corporate Banking, Payments, Statements, Treasury

#### Properties

- [Documentation](https://developer.bankofamerica.com/)
- [OpenAPI](openapi/bank-of-america-cashpro-api-openapi.yml)

## Common Properties

- [Website](https://www.bankofamerica.com/)
- [CashPro Developer Studio](https://developer.bankofamerica.com/)
- [Newsroom](https://newsroom.bankofamerica.com/)
- [TermsOfService](https://www.bankofamerica.com/online-banking/digital-banking-agreement.go)
- [PrivacyPolicy](https://www.bankofamerica.com/security-center/overview.go)

## Features

| Name | Description |
|------|-------------|
| Account Management | Programmatic access to CashPro account details and metadata. |
| Balance Reporting | Real-time ledger, available, and collected balance queries. |
| Transaction History | Paginated transaction history with date range filtering. |
| Payment Initiation | Initiate payments across 350+ payment types including ACH, wire, SWIFT, and RTP. |
| Payment Status Tracking | Real-time payment status monitoring and cancellation support. |
| Statement Access | Programmatic retrieval of monthly account statements. |
| OAuth2 Security | Client credentials OAuth2 flow for secure API access. |
| TMS/ERP Integration | Pre-built connectors for 28+ Treasury Management and ERP platforms. |
| Sandbox Environment | Developer sandbox for testing and accelerated onboarding. |

## Use Cases

| Name | Description |
|------|-------------|
| Treasury Automation | Automate daily cash positioning, balance reporting, and payment workflows. |
| ERP Integration | Connect SAP, Oracle, or other ERP systems to Bank of America CashPro. |
| Payments Hub | Centralize payment initiation across ACH, wire, SWIFT, and real-time payment rails. |
| Liquidity Management | Real-time visibility into global account balances for liquidity decisions. |
| Reconciliation | Automated transaction download for account reconciliation workflows. |
| Cash Concentration | Sweep and concentration account management via API. |

## Artifacts

Machine-readable API specifications organized by format.

### OpenAPI

- [Bank of America CashPro API](openapi/bank-of-america-cashpro-api-openapi.yml)

### JSON Schema

- [account-schema.json](json-schema/account-schema.json)
- [balance-schema.json](json-schema/balance-schema.json)
- [transaction-schema.json](json-schema/transaction-schema.json)
- [paymentrequest-schema.json](json-schema/paymentrequest-schema.json)
- [paymentresponse-schema.json](json-schema/paymentresponse-schema.json)

### JSON-LD

- [Bank of America JSON-LD Context](json-ld/bank-of-america-context.jsonld)

## Capabilities

Naftiko capabilities organized as shared per-API definitions composed into customer-facing workflows.

### Shared Per-API Definitions

- [CashPro API](capabilities/shared/cashpro-api.yaml) — 10 operations for account management, payments, and statements

### Workflow Capabilities

| Workflow | APIs Combined | Tools | Persona |
|----------|--------------|-------|---------|
| [Treasury Banking](capabilities/treasury-banking.yaml) | CashPro | 6 | Corporate Treasurer, Treasury Analyst, ERP Integration |

## Vocabulary

- [Bank of America Vocabulary](vocabulary/bank-of-america-vocabulary.yaml) — Unified taxonomy mapping 3 resources, 6 actions, 1 workflow, and 3 personas

## Rules

- [Bank of America Spectral Rules](rules/bank-of-america-spectral-rules.yml) — 16 rules enforcing CashPro API conventions

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
