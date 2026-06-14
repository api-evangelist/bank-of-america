# Bank of America GraphQL Schema

## Overview

This conceptual GraphQL schema models the Bank of America CashPro API platform and broader retail banking services. It covers the full surface area of consumer and corporate banking operations: accounts, balances, transactions, payments, cards, rewards, customers, credit, locations, loan applications, statements, tax documents, open banking consent, and API authentication.

The schema is designed to complement the existing CashPro REST API (baseURL: `https://api.bankofamerica.com/cashpro/v1`) and maps directly to the capabilities described in the CashPro Developer Studio at `https://developer.bankofamerica.com/`.

## Provider

- **Name:** Bank of America
- **Developer Portal:** https://developer.bankofamerica.com/
- **GitHub:** https://github.com/bankofamerica
- **Base URL:** https://api.bankofamerica.com/cashpro/v1

## Schema File

`bank-of-america-schema.graphql`

## Type Summary

| Domain | Types |
|---|---|
| Account interfaces and concrete types | Account (interface), CheckingAccount, SavingsAccount, MoneyMarket, CD, CreditCard, MortgageAccount, AutoLoan, StudentLoan, LOC |
| Balance | AccountBalance, AvailableBalance, PendingBalance |
| Transaction | Transaction, TransactionHistory, PaymentRecord |
| Payments | BillPayment, Transfer, WireTransfer, ACHPayment, Zelle, CheckDeposit, ATMTransaction, Pament |
| Cards | Card, CardTransaction, PurchaseDetail, CashAdvance |
| Rewards | Reward, RewardPoint, Reimbursement, Offer |
| Customer | Customer, CustomerProfile, Address, Contact, Organization, Employer, Joint, Authorized |
| Location | Branch, ATM, FinancialCenter, ServiceCenter, Appointment, HoursOfOperation |
| Mortgage and Loans | MortgageDetail, LoanApplication |
| Credit | CreditScore, FICOScore, CreditScoreHistory, CreditReport |
| Spending and Summaries | MonthlySpending, SpendingCategory, AnnualSummary |
| Documents | Statement, StatementPeriod, TaxDocument, IRS |
| Auth and API Management | APIKey, Token, AccessToken |
| Webhooks | Webhook, Subscription |
| Open Banking | TrueLayer, OpenBanking, OpenBankingAccess |
| Enums | AccountType, AccountStatus, TransactionStatus, PaymentStatus, PaymentType, CardStatus, LoanApplicationStatus, AppointmentType, AppointmentStatus, WebhookEvent |

**Total named types: 68**

## Key Design Decisions

### Account Interface

All account products implement the `Account` interface, ensuring a consistent contract for `accountNumber`, `routingNumber`, `balance`, `transactions`, and `statements`. Concrete types (CheckingAccount, SavingsAccount, CD, etc.) extend the interface with product-specific fields.

### Balance Decomposition

Balance is split into three types — `AccountBalance` (top-level ledger), `AvailableBalance`, and `PendingBalance` — matching the three balance tiers exposed by the CashPro balance reporting API.

### Payment Coverage

The schema models all payment rails supported by CashPro: `ACHPayment` (with SEC code), `WireTransfer` (SWIFT and ABA routing), `BillPayment`, `Transfer` (internal book transfers), `Zelle` (consumer P2P), and `CheckDeposit` (mobile capture). A `Pament` type is included as an alias for polymorphic payment references.

### Credit Intelligence

`CreditScore` wraps a `FICOScore` subtype to distinguish the numeric score from its interpretation (range, category, percentile). `CreditReport` provides bureau-level detail for underwriting workflows.

### Open Banking and TrueLayer

`OpenBanking` models TPP consent grants with data access logging. `TrueLayer` represents an individual third-party connection, supporting open banking integration patterns.

### Webhook Subscriptions

`Webhook` and `Subscription` types support event-driven architectures. `WebhookEvent` enum covers the key real-time triggers: transaction posting, payment status changes, balance thresholds, statement availability, and card transactions.

## GraphQL Operations

### Queries (selected)

```graphql
# Get all accounts for a customer
query CustomerAccounts($customerId: ID!) {
  accounts(customerId: $customerId) {
    id
    accountNumber
    accountType
    status
    balance {
      ledgerBalance
      availableBalance { amount }
      pendingBalance { amount }
    }
  }
}

# Paginated transaction history
query AccountTransactions($accountId: ID!, $start: Date!, $end: Date!) {
  transactionHistory(accountId: $accountId, startDate: $start, endDate: $end, limit: 50, offset: 0) {
    transactions {
      id
      transactionDate
      amount
      description
      status
      isDebit
    }
    totalCount
    openingBalance
    closingBalance
  }
}

# Customer credit profile
query CreditProfile($customerId: ID!) {
  creditScore(customerId: $customerId) {
    score
    bureau
    asOf
    factors
    scoreType { category range }
  }
  creditReport(customerId: $customerId) {
    totalDebt
    creditUtilization
    paymentHistory
  }
}

# Branch and ATM locator
query NearbyLocations($lat: Decimal!, $lon: Decimal!) {
  branches(latitude: $lat, longitude: $lon, radiusMiles: 5) {
    id name phone hasATM
    address { street1 city state postalCode }
  }
  atms(latitude: $lat, longitude: $lon, radiusMiles: 2) {
    id hasDeposit isSurchargeFreeBankOfAmerica
    address { street1 city state }
  }
}
```

### Mutations (selected)

```graphql
# Initiate a wire transfer
mutation SendWire($input: WireTransferInput!) {
  initiateWireTransfer(input: $input) {
    id
    status
    confirmationNumber
    feeAmount
    valueDate
  }
}

# Schedule a bill payment
mutation PayBill($input: BillPaymentInput!) {
  scheduleBillPayment(input: $input) {
    id
    payee
    amount
    scheduledDate
    confirmationNumber
    status
  }
}

# Lock a card
mutation LockCard($cardId: ID!) {
  lockCard(cardId: $cardId) {
    id
    status
    cardholderName
  }
}

# Create a webhook
mutation RegisterWebhook($input: WebhookInput!) {
  createWebhook(input: $input) {
    id
    url
    events
    isActive
  }
}
```

## Authentication

The CashPro API uses OAuth 2.0 Client Credentials flow. The schema models this via the `generateAccessToken` mutation and the `AccessToken`, `Token`, and `APIKey` types. All queries and mutations require a valid Bearer token in the Authorization header.

## Supported Payment Types

The `PaymentType` enum covers the full range of CashPro payment rails:

- `ACH` — Automated Clearing House (credit and debit)
- `WIRE` — Domestic wire transfer
- `SWIFT` — International wire via SWIFT network
- `RTP` — Real-Time Payments (The Clearing House)
- `BOOK` — Internal book transfer
- `CHECK` — Paper check
- `ZELLE` — Zelle consumer P2P
- `BILL_PAY` — Bill payment to registered payees

## Related Resources

- OpenAPI specification: `openapi/bank-of-america-cashpro-api-openapi.yml`
- Spectral rules: `rules/bank-of-america-spectral-rules.yml`
- Vocabulary: `vocabulary/bank-of-america-vocabulary.yaml`
- JSON-LD context: `json-ld/bank-of-america-context.jsonld`
