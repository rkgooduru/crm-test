# PowerApps Configuration Guide - Financial Module

## Canvas App Architecture - Enhanced with Financial Features

### App Name: CRM Management System (Enhanced)
**Target Device:** Tablet (responsive for mobile and web)

---

## New Financial Screens

### 1. Account Financial Summary Panel
```
Screen: AccountFinancialSummary
├── Credit Information Card
│   ├── Credit Limit (large display)
│   ├── Current Balance (with color coding)
│   ├── Available Credit (highlighted)
│   └── Credit Status Badge (Good/At Risk/Suspended)
├── Payment Information
│   ├── Payment Terms
│   ├── Last Payment Date
│   ├── Days Since Last Payment
│   └── Next Due Date (if applicable)
├── Aging Summary
│   ├── Current (0-30 days): Amount & %
│   ├── 31-60 days: Amount & %
│   ├── 61-90 days: Amount & %
│   └── 90+ days: Amount & % (RED alert)
├── Quick Stats
│   ├── Total Transactions YTD
│   ├── Total Payments YTD
│   ├── Average Transaction Size
│   └── Total Outstanding
└── Action Buttons
    ├── Record Payment
    ├── Create Invoice
    ├── View Transactions
    └── Print Aging Report
```

**Conditional Formatting:**
- Available Credit < 20% of limit: YELLOW warning
- Available Credit < 10% of limit: RED alert
- Days Past Due > 0: RED highlight with count
- Credit Status = Suspended: DARK RED background

---

### 2. Transactions List Screen
```
Screen: TransactionsListScreen
├── Search/Filter Bar
│   ├── Search by reference number
│   ├── Date range picker
│   ├── Filter by Transaction Type
│   ├── Filter by Status
│   └── Filter by Amount range
├── Gallery Control
│   ├── Transaction ID (Reference)
│   ├── Date
│   ├── Type (Invoice/Payment/Credit)
│   ├── Amount
│   ├── Status badge (color-coded)
│   ├── Remaining Balance
│   └── Days Overdue (if applicable)
├── Summary Cards (above gallery)
│   ├── Total Open: [Amount]
│   ├── Total Overdue: [Amount]
│   ├── Overdue Count: [Number]
│   └── Total Paid This Month: [Amount]
└── Actions
    ├── New Transaction button
    ├── New Payment button
    └── Item → Transaction Detail
```

**Color Coding:**
- Open: BLUE
- Partial: YELLOW
- Paid: GREEN
- Overdue: RED
- Cancelled: GRAY

---

### 3. Transaction Detail Screen
```
Screen: TransactionDetailScreen
├── Header
│   ├── Transaction ID / Reference
│   ├── Type badge
│   ├── Status badge
│   └── Edit/Delete buttons (if permitted)
├── Transaction Information
│   ├── Date
│   ├── Type
│   ├── Amount
│   ├── Description
│   ├── Reference Number (Invoice/PO)
│   └── Due Date (if invoice)
├── Payment Information
│   ├── Status
│   ├── Remaining Balance
│   ├── Total Payments Applied
│   ├── Last Payment Date
│   └── Payment Method (for payments)
├── Related Payments (if invoice)
│   └── Sub-gallery of payments applied
├── Related Invoice (if payment)
│   └── Link to invoice this payment covers
└── Actions
    ├── Record Payment (for invoices)
    ├── Apply Payment (for payments)
    ├── Edit
    └── Delete (Admin only)
```

---

### 4. Record Payment Screen
```
Screen: RecordPaymentScreen
├── Payment Header
│   ├── Account Name
│   ├── Payment Date (default: today)
│   └── Total Amount Outstanding (reference)
├── Payment Form
│   ├── Account (required, lookup)
│   ├── Payment Date (required)
│   ├── Amount (required, currency)
│   ├── Payment Method (dropdown)
│   │   ├── Check
│   │   ├── Credit Card
│   │   ├── ACH Transfer
│   │   ├── Wire Transfer
│   │   ├── Cash
│   │   └── Other
│   ├── Reference Number (check/conf number)
│   └── Notes (optional)
├── Auto-Payment Application
│   ├── "Apply to:" section
│   ├── Checkbox to auto-apply to oldest open invoices
│   ├── OR manual selection of invoices
│   └── Preview of application
├── Validation
│   ├── Amount cannot exceed outstanding
│   ├── Required fields enforced
│   └── Duplicate payment detection
└── Buttons
    ├── Record Payment
    ├── Record & Create Another
    ├── Cancel
    └── Save as Draft
```

---

### 5. Apply Payments Screen
```
Screen: ApplyPaymentsScreen
├── Payment Information (read-only)
│   ├── Payment ID
│   ├── Date
│   ├── Amount
│   ├── Unapplied Balance
│   └── Payment Method
├── Open Invoices Gallery
│   ├── Invoice ID
│   ├── Date
│   ├── Amount Due
│   ├── Days Overdue
│   └── Apply checkbox
├── Application Details
│   ├── Checkbox next to each invoice
│   ├── Amount to apply input field (per invoice)
│   ├── Running balance display
│   └── Confirm application amount = payment amount
├── Summary
│   ├── Total to Apply: [Amount]
│   ├── Remaining Unapplied: [Amount]
│   └── Invoices to Apply To: [Count]
└── Buttons
    ├── Apply Payment
    ├── Hold for Manual Review
    └── Cancel
```

---

### 6. Invoice Form (New)
```
Screen: InvoiceFormScreen
├── Invoice Header
│   ├── Account (required, lookup)
│   ├── Invoice Type (choice)
│   │   ├── Invoice
│   │   ├── Credit Memo
│   │   └── Debit Memo
│   └── Form title updates based on type
├── Invoice Details
│   ├── Invoice Date (required, default today)
│   ├── Due Date (required, calculated based on terms)
│   ├── Reference Number (PO/Invoice#)
│   ├── Description (required)
│   └── Amount (required, currency)
├── Payment Terms
│   ├── Payment Terms (lookup to account default)
│   ├── Calculated Due Date display
│   └── Edit if different from account default
├── Line Items (expandable section)
│   ├── Product lookup
│   ├── Quantity
│   ├── Unit Price (auto-populated)
│   ├── Line Total (calculated)
│   └── Add/Remove Line buttons
├── Summary Section
│   ├── Subtotal (calculated)
│   ├── Tax (if applicable)
│   ├── Total Amount (calculated)
│   └── Currency conversion (if needed)
├── Status
│   ├── Status dropdown (Open/Sent/Paid/Partial)
│   └── Status-specific fields
└── Buttons
    ├── Save
    ├── Save & Close
    ├── Save & Send (email integration)
    ├── Cancel
    └── Delete (if permitted)
```

---

### 7. Financial Dashboard Screen (Admin)
```
Screen: FinancialDashboardScreen
├── KPI Cards (top row)
│   ├── Total Receivables (large)
│   ├── Overdue Amount (RED)
│   ├── Credit Limit Utilization %
│   └── Collections Rate %
├── Charts Section
│   ├── Aging Breakdown (pie chart)
│   │   ├── Current (0-30): %
│   │   ├── 31-60: %
│   │   ├── 61-90: %
│   │   └── 90+: %
│   ├── Top 10 Overdue Accounts (bar chart)
│   ├── Collections Trend (line chart - last 6 months)
│   └── Payment Methods Breakdown (pie)
├── Top Overdue Accounts Table
│   ├── Account Name
│   ├── Amount Overdue
│   ├── Days Overdue
│   ├── Credit Status
│   └── Click to detail
├── Credit Utilization Table
│   ├── Top accounts by credit usage %
│   ├── Account Name
│   ├── Credit Limit
│   ├── Current Balance %
│   └── Available Credit %
├── Quick Actions
│   ├── Record Payment
│   ├── Create Invoice
│   ├── Export Aging Report
│   └── Send Collection Letters
└── Navigation
    ├── Filter by date range
    ├── Filter by sales territory/rep (if multi-user)
    └── Export buttons
```

---

### 8. Aging Report Screen
```
Screen: AgingReportScreen
├── Report Header
│   ├── "Accounts Receivable Aging Report"
│   ├── As of Date: [Date]
│   └── Generated: [Timestamp]
├── Summary Section
│   ├── Total A/R: [Amount]
│   ├── Total Current: [Amount] [%]
│   ├── Total 31-60: [Amount] [%]
│   ├── Total 61-90: [Amount] [%]
│   ├── Total 90+: [Amount] [%] ← highlighted in RED
│   └── Days Sales Outstanding (DSO)
├── Detailed Aging Table
│   └── Columns:
│       ├── Account Name
│       ├── Invoice #
│       ├── Invoice Date
│       ├── Due Date
│       ├── Original Amount
│       ├── Paid Amount
│       ├── Outstanding
│       ├── Days Overdue
│       ├── Aging Bucket
│       └── Action (Record Payment link)
├── Sorting & Filtering
│   ├── Sort by: Days Overdue/Amount/Account
│   ├── Filter by: Aging Bucket/Credit Status
│   ├── Date range selector
│   └── Search by account name
└── Export Options
    ├── Export to Excel
    ├── Export to PDF
    ├── Email Report
    └── Print
```

---

## PowerApps Formulas for Financial Features

### Calculate Available Credit
```powerapps
Credit_Limit - Current_Balance
```

### Auto-Update Credit Status
```powerapps
If(
  Days_Past_Due >= 30,
  "Suspended",
  If(
    Days_Past_Due >= 15,
    "At Risk",
    "Good Standing"
  )
)
```

### Calculate Days Past Due
```powerapps
If(
  Current_Balance > 0 And Last_Payment_Date < Today(),
  Days(Last_Payment_Date, Today()),
  0
)
```

### Calculate Current Balance
```powerapps
SUM(
  Filter(
    Transactions,
    Account = CurrentAccount.ID And Status <> "Paid" And Status <> "Cancelled"
  ),
  Amount
) - SUM(
  Filter(
    Payments,
    Account = CurrentAccount.ID
  ),
  Amount
)
```

### Validate Payment Amount
```powerapps
If(
  PaymentAmount <= SUM(
    Filter(
      Transactions,
      Account = SelectedAccount.ID And Status = "Open"
    ),
    Amount
  ),
  true,
  false
)
```

---

## Updated Navigation Flow

```
Login/Splash
    ↓
Dashboard (Main)
    ├→ Accounts
    │  ├→ Accounts List
    │  ├→ Account Detail
    │  ├→ Account Financial Summary ← NEW
    │  ├→ Account Form
    │  └→ Transactions List ← NEW
    │
    ├→ Transactions ← NEW
    │  ├→ Transactions List
    │  ├→ Transaction Detail
    │  ├→ Invoice Form ← NEW
    │  ├→ Aging Report ← NEW
    │  └→ Record Payment ← NEW
    │
    ├→ Payments ← NEW
    │  ├→ Payments List
    │  ├→ Payment Detail
    │  ├→ Record Payment Form
    │  └→ Apply Payments ← NEW
    │
    ├→ Contacts
    │  ├→ Contacts List
    │  ├→ Contact Detail
    │  └→ Contact Form
    │
    ├→ Inventory
    │  ├→ Inventory Dashboard
    │  ├→ Inventory List
    │  └→ Item Detail
    │
    ├→ Financial Dashboard ← NEW (Admin only)
    │  ├→ Dashboard View
    │  ├→ Aging Analysis
    │  ├→ Credit Utilization
    │  └→ Collection Report
    │
    ├→ User Admin (Admin only)
    │  ├→ User Directory
    │  ├→ User Detail
    │  ├→ User Form
    │  └→ Activity Log
    │
    └→ Settings (Admin only)
```

---

## Additional Features

### Automated Workflows (Power Automate)
- **Invoice Due Soon Alert**: Email 3 days before due date
- **Overdue Payment Alert**: Email 1 day after due date, then weekly
- **High Credit Usage Alert**: Alert when account reaches 80% of credit limit
- **Payment Received Notification**: Email confirmation to account contact
- **Credit Status Change Alert**: Notify sales/collections when status changes

