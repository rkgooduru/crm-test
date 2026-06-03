# Getting Started - Development Guide

## 🚀 Quick Start for Developers

This guide will help you build the PowerApps CRM system from the specifications.

---

## **Phase 1: Dataverse Setup (Week 1-2)**

### Step 1: Create the Environment
1. Go to [Power Platform Admin Center](https://admin.powerplatform.com)
2. Click **"Create environment"**
3. Name: `CRM-DEV` or `CRM-PROD`
4. Type: Select **Production** or **Sandbox**
5. Region: Choose closest to your location
6. Create database: **Yes**
7. Click **Create**

### Step 2: Create Tables in Dataverse
Use the **specifications/01-DATAVERSE-SCHEMA.md** as your guide.

#### Create Accounts Table
1. Go to [Power Apps](https://make.powerapps.com)
2. Click **Tables** → **New table**
3. Name: `Accounts` (System name: `accounts`)
4. Add columns:
   ```
   - Account Name (Primary name, Text, Required)
   - Industry (Choice)
   - Annual Revenue (Currency)
   - Phone (Phone)
   - Email (Email)
   - Website (URL)
   - Address Street, City, State, ZIP (Text)
   - Number of Employees (Whole Number)
   - Account Status (Choice: Active/Inactive/Lead)
   - Credit Limit (Currency)
   - Current Balance (Currency) - Calculated
   - Available Credit (Currency) - Calculated
   - Payment Terms (Choice)
   - Credit Status (Choice)
   - Last Payment Date (Date)
   - Last Transaction Date (Date)
   - Total Transactions YTD (Currency) - Calculated
   - Overdue Amount (Currency) - Calculated
   - Days Past Due (Whole Number) - Calculated
   - Default Contact (Lookup to Contacts)
   ```

#### Create Contacts Table
```
- First Name (Text, Required)
- Last Name (Text, Required)
- Full Name (Text) - Calculated: FirstName + " " + LastName
- Email (Email, Required)
- Phone (Phone)
- Mobile Phone (Phone)
- Job Title (Text)
- Department (Text)
- Account (Lookup to Accounts, Required)
- Contact Status (Choice: Active/Inactive)
- Preferred Contact Method (Choice: Email/Phone/In-Person)
- Is Billing Contact (Yes/No)
- Is Primary Contact (Yes/No)
```

#### Create Transactions Table
```
- Transaction ID (Text, Required) - Auto-generate: TXN-###
- Account (Lookup to Accounts, Required)
- Transaction Type (Choice: Invoice/Payment/Credit Memo/Debit Memo)
- Transaction Date (Date, Required)
- Due Date (Date)
- Amount (Currency, Required)
- Description (Text Area)
- Reference Number (Text)
- Payment Method (Choice: Check/CC/ACH/Wire/Cash)
- Status (Choice: Open/Paid/Partial/Overdue/Cancelled)
- Remaining Balance (Currency) - Calculated
- Applied Payments (Currency) - Calculated
- Related Invoice (Lookup to Transactions)
- Notes (Text Area)
- Created By (User, auto-populated)
```

#### Create Payments Table
```
- Payment ID (Text, Required) - Auto-generate: PAY-###
- Account (Lookup to Accounts, Required)
- Payment Date (Date, Required)
- Amount (Currency, Required)
- Payment Method (Choice: Check/CC/ACH/Wire/Cash)
- Reference Number (Text)
- Applied To (Lookup to Transactions)
- Unapplied Amount (Currency)
- Notes (Text Area)
- Received By (User, auto-populated)
```

#### Create Inventory Table
```
- Product Name (Text, Required)
- Product Code (Text, Required)
- Description (Text Area)
- Unit Price (Currency, Required)
- Quantity On Hand (Whole Number, Required)
- Reorder Level (Whole Number)
- Reorder Quantity (Whole Number)
- Supplier (Text)
- Supplier Contact (Text)
- Category (Choice)
- Unit of Measure (Choice: Pieces/Boxes/etc)
- Last Restocked (Date)
- Status (Choice: Active/Discontinued)
```

#### Create Users Table
```
- User Name (Text, Required)
- Email (Email, Required)
- Full Name (Text, Required)
- Department (Text)
- Role (Choice: Admin/User, Required)
- Manager (Lookup to Users)
- User Status (Choice: Active/Inactive/On Leave, Required)
- Last Login (Date and Time)
- Login Attempts (Whole Number)
```

### Step 3: Configure Relationships
1. **Accounts → Contacts** (1:N)
   - Parent: Accounts
   - Child: Contacts
   - Relationship name: `accounts_contacts`

2. **Accounts → Transactions** (1:N)
   - Parent: Accounts
   - Child: Transactions

3. **Accounts → Payments** (1:N)
   - Parent: Accounts
   - Child: Payments

4. **Users → Users** (1:N) for Manager field

### Step 4: Add Calculated Columns

**For Accounts table:**

1. **Current Balance**
   ```
   Type: Calculated (Sum)
   Entity: Transactions
   Filter: Account = This Account AND Status <> "Paid" AND Status <> "Cancelled"
   Function: SUM(Amount)
   ```

2. **Available Credit**
   ```
   Type: Formula
   Formula: [Credit Limit] - [Current Balance]
   ```

3. **Overdue Amount**
   ```
   Type: Calculated (Sum)
   Entity: Transactions
   Filter: Account = This Account AND Status = "Overdue"
   Function: SUM(Amount)
   ```

4. **Days Past Due**
   ```
   Type: Formula
   Formula: IF([Last Payment Date] < TODAY(), DAYS([Last Payment Date], TODAY()), 0)
   ```

---

## **Phase 2: Create PowerApps Canvas App (Week 3-4)**

### Step 1: Create New Canvas App
1. Go to [Power Apps](https://make.powerapps.com)
2. Click **Create** → **Canvas app**
3. Name: `CRM Management System`
4. Format: **Tablet** (wider screens)
5. Click **Create**

### Step 2: Set Up App Theme
1. **File** → **Settings**
2. Set primary color to your brand color
3. Set text colors for contrast
4. Save

### Step 3: Create Navigation Menu
Create a **Screens** list:
```
Screen 1: DashboardScreen
Screen 2: AccountsListScreen
Screen 3: AccountDetailScreen
Screen 4: AccountFormScreen
Screen 5: ContactsListScreen
Screen 6: ContactDetailScreen
Screen 7: ContactFormScreen
Screen 8: TransactionsListScreen
Screen 9: TransactionDetailScreen
Screen 10: InvoiceFormScreen
Screen 11: RecordPaymentScreen
Screen 12: AgingReportScreen
Screen 13: FinancialDashboardScreen (Admin)
Screen 14: InventoryListScreen
Screen 15: InventoryDetailScreen
Screen 16: UserDirectoryScreen (Admin)
Screen 17: UserFormScreen (Admin)
```

### Step 4: Create Dashboard Screen

**Add Controls:**

```powerapps
// Header
Label "Welcome [User]" 
Label with role badge

// Quick Action Tiles
Button "New Account"
Button "New Contact" 
Button "Record Payment"
Button "View Inventory"

// Recent Records Section
Gallery "Recent Accounts"
Gallery "Recent Contacts"
Gallery "Recent Transactions"

// Navigation Menu (role-based)
IF(User().Email in AdminEmails,
  Container with: Accounts, Contacts, Transactions, Payments, Inventory, User Admin, Financial Dashboard,
  Container with: Accounts, Contacts, Transactions, Payments, Inventory
)
```

### Step 5: Create Accounts List Screen

```powerapps
// Add Gallery Control
Gallery1.Items = 
  Search(
    Filter(
      Accounts,
      Status = LookUp(sldStatus.Value),
      If(IsBlank(txtSearchAccounts.Value), true, 
        SearchBox in AccountName)
    ),
    txtSearchAccounts.Value,
    "AccountName"
  )

// Gallery Fields
- Account Name (Title)
- Industry (Subtitle)
- Status (Badge - color coded)
- Phone (Description)
- Available Credit (Right side - RED if low)

// On Select → Go to Account Detail
Select(Gallery1) → Navigate(AccountDetailScreen, ScreenTransition.Cover)
```

### Step 6: Create Account Detail Screen

```powerapps
// Show selected account details
Label: Accounts.AccountName (from Gallery selection)

// Display Fields
Account Name
Industry  
Phone
Email
Website
Account Status

// Financial Summary Section (NEW)
Subheading "Financial Information"
Card "Credit Limit": [Credit Limit]
Card "Current Balance": [Current Balance] (RED if high)
Card "Available Credit": [Available Credit] (GREEN/YELLOW/RED)
Card "Credit Status": [Credit Status] (Badges)

// Payment Information
Card "Last Payment": [Last Payment Date]
Card "Payment Terms": [Payment Terms]
Card "Days Overdue": [Days Past Due] (RED if > 0)

// Related Records
Subheading "Contacts"
Gallery of related Contacts

Subheading "Recent Transactions"
Gallery of recent Transactions

// Action Buttons
Button "Edit" → Navigate(AccountFormScreen)
Button "New Contact"
Button "View Transactions"
```

### Step 7: Create Account Form Screen

```powerapps
// Form for creating/editing accounts
EditForm1.Item = If(
  IsBlank(varAccountID),
  Blank(),
  LookUp(Accounts, ID = varAccountID)
)

// Form Fields
DataCardValue "Account Name" - TextInput (Required)
DataCardValue "Industry" - Dropdown
DataCardValue "Phone" - TextInput
DataCardValue "Email" - TextInput
DataCardValue "Credit Limit" - NumberInput
DataCardValue "Payment Terms" - Dropdown
DataCardValue "Account Status" - Dropdown

// Save Button
Button "Save"
OnSelect: SubmitForm(EditForm1)

// Cancel Button  
Button "Cancel"
OnSelect: ResetForm(EditForm1); Navigate(AccountDetailScreen)
```

### Step 8: Create Transactions List Screen

```powerapps
// Gallery of transactions for selected account
Gallery2.Items = 
  Filter(
    Transactions,
    Account.ID = varSelectedAccountID,
    If(IsBlank(sldTransactionType.Value), true, 
      TransactionType = sldTransactionType.Value),
    If(IsBlank(sldTransactionStatus.Value), true,
      Status = sldTransactionStatus.Value)
  )

// Gallery Fields
- Transaction ID (Reference) - Title
- Date - Subtitle
- Type (Invoice/Payment/Credit) - Badge
- Amount - Large number (right side)
- Status - Badge (color: Green/Yellow/Red)
- Remaining Balance - Right side

// Color Coding
Status = "Open" → BLUE
Status = "Paid" → GREEN
Status = "Partial" → YELLOW
Status = "Overdue" → RED (with days past due)

// Summary Cards (above gallery)
Card "Total Open": [Sum of Open transaction amounts]
Card "Total Overdue": [Sum of Overdue amounts] (RED)
Card "Total This Month": [Sum of this month transactions]
```

### Step 9: Create Record Payment Screen

```powerapps
// Form for recording payments
Container "Payment Header"
  Label: "Record Payment for [AccountName]"
  Label: "Outstanding: [CurrentBalance]"

EditForm2.Item = Blank()

// Form Fields
Dropdown "Account" (Lookup, Required)
DatePicker "Payment Date" (Default: Today())
NumberInput "Amount" (Required, Currency)
Dropdown "Payment Method" (Required)
TextInput "Reference Number" (Optional)
TextArea "Notes" (Optional)

// Auto-Application Logic
Checkbox "Auto-apply to oldest invoices"
IF(Checked,
  Show gallery of open invoices auto-selected,
  Show gallery for manual selection
)

// Validation
IF(Amount > OutstandingBalance,
  Error: "Amount exceeds outstanding balance",
  OK
)

// Buttons
Button "Record Payment"
OnSelect: 
  Set(varPaymentAmount, Value(Amount));
  Set(varSelectedAccount, Account.Value);
  Set(varPaymentMethod, PaymentMethod.Value);
  Patch(Payments, 
    Defaults(Payments),
    {
      Account: varSelectedAccount,
      PaymentDate: PaymentDate.Value,
      Amount: varPaymentAmount,
      PaymentMethod: varPaymentMethod,
      ReferenceNumber: ReferenceNumber.Value,
      Notes: Notes.Value
    }
  );
  Notify("Payment recorded successfully");
  Navigate(TransactionsListScreen)

Button "Cancel"
OnSelect: ResetForm(EditForm2); Navigate(AccountDetailScreen)
```

### Step 10: Create Aging Report Screen

```powerapps
// Report view
Label "Accounts Receivable Aging Report"
Label "As of: " & Today()

// Summary Cards
Card "Total A/R": [Sum all open transactions]
Card "Current (0-30)": [Amount] [%]
Card "31-60 Days": [Amount] [%]
Card "61-90 Days": [Amount] [%]
Card "90+ Days": [Amount] [%] (RED)

// Detailed Table
Table with columns:
  - Account Name
  - Invoice #
  - Invoice Date
  - Due Date
  - Amount
  - Outstanding
  - Days Overdue (color: GREEN < 30, YELLOW 30-60, RED > 60)

Table.Items = 
  Sort(
    Filter(
      Transactions,
      Account.ID = varSelectedAccountID,
      Status <> "Paid"
    ),
    'Due Date',
    Ascending
  )

// Action Buttons
Button "Print" → Print()
Button "Export to Excel" → ExportAsExcel()
Button "Email Report" → SendEmail()
```

### Step 11: Create Financial Dashboard (Admin Only)

```powerapps
// Check if admin
IF(
  LookUp(Users, Email = User().Email, Role) = "Admin",
  ShowControls,
  Label "Access Denied"
)

// KPI Cards (top row)
Card "Total Receivables": [SUM all open transactions]
Card "Overdue Amount": [SUM overdue transactions] (RED)
Card "Utilization %": [Average credit usage %]
Card "Collections Rate": [Payments / Invoiced * 100] %

// Charts
PieChart "Aging Distribution"
  Series: Current (0-30) / 31-60 / 61-90 / 90+

BarChart "Top 10 Overdue Accounts"
  X-axis: Account Name
  Y-axis: Overdue Amount

LineChart "6-Month Collections Trend"
  X-axis: Month
  Y-axis: Payments Received

// Top Overdue Accounts Table
Gallery "Overdue Accounts"
  Items: Sort(
    Filter(Accounts, 'Days Past Due' > 0),
    'Days Past Due',
    Descending
  )

// Export Button
Button "Export Dashboard" → ExportAsExcel()
```

---

## **Phase 3: Power Automate Workflows (Week 5)**

### Create Flow 1: Invoice Due Soon Alert

1. Go to [Power Automate](https://make.powerautomate.com)
2. Click **Create** → **Scheduled cloud flow**
3. Name: `InvoiceDueSoonAlert`
4. Schedule: **Daily** at **8 AM**

**Actions:**
```
1. Trigger: Recurrence (Daily, 8 AM)

2. List rows from Dataverse
   Table: Transactions
   Filter: 
   Status = "Open" OR Status = "Partial"
   AND Type = "Invoice"
   AND DueDate = Tomorrow + 2 days
   AND Account.CreditStatus <> "Suspended"

3. For each (transaction):
   a. Get the related Account
   b. Get the related Billing Contact
   c. Send email:
      To: [BillingContact.Email]
      Subject: Invoice Payment Due in 3 Days - [Amount]
      Body: [Template from specifications]
   d. CC: [AccountManager.Email]
```

### Create Flow 2: Overdue Payment Reminder

1. Name: `OverduePaymentReminder`
2. Schedule: **Daily** at **10 AM**

**Actions:**
```
1. Trigger: Recurrence (Daily, 10 AM)

2. List rows (Transactions)
   Filter: DueDate < TODAY() AND Status = "Open"

3. For each (transaction):
   a. Calculate DaysOverdue
   b. If DaysOverdue = 1-7:
      - Send "courtesy reminder" email
   c. If DaysOverdue = 8-14:
      - Send "urgent reminder" email
   d. If DaysOverdue >= 15:
      - Update Account: CreditStatus = "At Risk"
      - Send "collections notice" email
      - Create task for collections team
   e. If DaysOverdue >= 30:
      - Update Account: CreditStatus = "Suspended"
      - Create "High Priority" task
```

### Create Flow 3: Payment Received Notification

1. Name: `PaymentReceivedNotification`
2. Trigger: **When a row is added** (Payments table)

**Actions:**
```
1. Get the Account details

2. Send email to Account contact:
   Subject: Payment Received - [Amount] - [Account]
   Body: [Template from specifications]

3. Send email to Accounting team:
   Subject: Payment Recorded - [Amount]
   Body: [Payment details for reconciliation]

4. Update Account record:
   Set LastPaymentDate = TODAY()
   If CurrentBalance <= 0:
     Set CreditStatus = "Good Standing"

5. Create audit log entry
```

---

## **Phase 4: Testing (Week 6)**

### Test Accounts Functionality
```
[ ] Create account with credit limit
[ ] Edit account information
[ ] Search accounts by name
[ ] Filter by industry/status
[ ] View related contacts
[ ] Check financial summary displays correctly
```

### Test Financial Features
```
[ ] Record invoice (amount deducted from available credit)
[ ] Record payment (applied to invoice)
[ ] Verify balance calculations
[ ] Test credit status changes
[ ] Verify aging bucket calculations
[ ] Test color coding (green/yellow/red)
```

### Test Workflows
```
[ ] Invoice due soon alert triggers
[ ] Overdue payment reminder triggers
[ ] Credit status change notifications sent
[ ] Payment received confirmation emails sent
```

### Test Security
```
[ ] Admin can access all screens
[ ] User cannot access admin screens
[ ] User can only edit own records
[ ] Financial dashboard only visible to admin
```

---

## **Phase 5: Deployment (Week 7)**

### Pre-Deployment Checklist
```
[ ] All screens functional
[ ] All workflows tested
[ ] Database backups configured
[ ] Email notifications working
[ ] Security roles assigned
[ ] Users trained
[ ] Documentation complete
```

### Deploy to Production
1. Export app as managed solution
2. Import to production environment
3. Verify all components
4. Monitor workflow runs
5. Collect user feedback

---

## **Common PowerApps Formulas Reference**

### Search and Filter
```powerapps
// Search accounts
Search(Accounts, txtSearch.Value, "AccountName", "Email")

// Filter by status
Filter(Accounts, Status = "Active")

// Combine search and filter
Filter(
  Search(Accounts, txtSearch.Value, "AccountName"),
  Status = ddStatus.Value
)
```

### Data Validation
```powerapps
// Check if required field empty
If(IsBlank(txtAccountName.Value),
  Notify("Account name is required", NotificationType.Error),
  Patch(Accounts, ...)
)

// Validate amount
If(Value(txtAmount.Value) > varOutstandingBalance,
  Notify("Amount exceeds outstanding balance"),
  Patch(Payments, ...)
)
```

### Calculated Fields
```powerapps
// Available Credit
Credit_Limit - Current_Balance

// Days Overdue
Days(Last_Payment_Date, Today())

// Utilization %
Current_Balance / Credit_Limit * 100
```

### Color Coding
```powerapps
// Credit status color
Switch(CreditStatus,
  "Good Standing", Green,
  "At Risk", Yellow,
  "Suspended", Red,
  Gray
)

// Days overdue color
If(DaysPastDue > 60, Red, If(DaysPastDue > 30, Yellow, Green))
```

---

## **Need Help?**

- **Power Apps Documentation**: https://docs.microsoft.com/power-apps/
- **Power Automate Documentation**: https://docs.microsoft.com/power-automate/
- **Dataverse Documentation**: https://docs.microsoft.com/powerapps/maker/data-platform/

---

**Start with Phase 1 - Create your Dataverse tables first, then build the app!** 🚀

