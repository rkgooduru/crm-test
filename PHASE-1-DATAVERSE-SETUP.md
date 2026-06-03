# Phase 1: Dataverse Setup - Step-by-Step Implementation

**Duration**: Week 1-2  
**Status**: 🟡 IN PROGRESS  
**Last Updated**: 2026-06-03

---

## 📋 Phase 1 Checklist

- [ ] Environment created
- [ ] Accounts table created
- [ ] Contacts table created
- [ ] Transactions table created
- [ ] Payments table created
- [ ] Inventory table created
- [ ] Users table created
- [ ] All relationships configured
- [ ] Calculated columns setup
- [ ] Choice columns configured
- [ ] Validation rules applied
- [ ] Phase 1 Complete ✅

---

## Step 1: Create Power Platform Environment

### 1.1 Access Power Platform Admin Center

1. Go to **https://admin.powerplatform.com**
2. Sign in with your Microsoft 365 account
3. Click **"Environments"** in the left sidebar

### 1.2 Create New Environment

1. Click **"+ New environment"** button (top right)
2. Fill in the form:

| Field | Value |
|-------|-------|
| **Name** | CRM-Development or CRM-Production |
| **Region** | Select closest to you (US/EU/APAC) |
| **Type** | Production (or Sandbox for testing) |
| **Enable Dataverse** | ✓ Yes |
| **Language** | English |
| **Currency** | USD (or your currency) |

3. Click **"Save"**
4. Wait 5-10 minutes for environment to provision

### 1.3 Verify Environment Created

1. Refresh the page
2. Your new environment should appear in the list
3. Click on it to open

---

## Step 2: Access Dataverse and Create Tables

### 2.1 Open Power Apps Maker Portal

1. Go to **https://make.powerapps.com**
2. Select your new environment from dropdown (top right)
3. Click **"Tables"** in left sidebar
4. You should see default Dataverse tables

### 2.2 Create Accounts Table

**Click "Create a table" or "+ New table"**

#### Basic Setup
- **Display name**: `Accounts`
- **Plural name**: `Accounts` (auto-filled)
- **Description**: "Customer and organizational accounts with financial tracking"

#### Add Columns

Use this exact structure. Copy each column specification:

| Column Name | Data Type | Required | Additional Settings |
|-------------|-----------|----------|---------------------|
| Account Name | Text | Yes | Primary column, searchable |
| Industry | Choice | No | Options: Technology, Finance, Healthcare, Retail, Manufacturing, Services, Other |
| Annual Revenue | Currency | No | Precision: 2 decimals |
| Address Street | Text | No | Max length: 255 |
| City | Text | No | Max length: 100 |
| State Province | Text | No | Max length: 50 |
| ZIP Postal Code | Text | No | Max length: 20 |
| Country Region | Text | No | Max length: 100 |
| Phone | Phone | No | Format: Phone |
| Email | Email | No | Format: Email |
| Website | URL | No | Format: URL |
| Number of Employees | Whole Number | No | Min: 0 |
| Account Status | Choice | Yes | Options: Active, Inactive, Lead, Prospect, Customer, Archived |
| **Credit Limit** | Currency | No | Precision: 2 decimals |
| **Payment Terms** | Choice | No | Options: Net 30, Net 60, Net 90, COD, Prepaid, Custom |
| **Credit Status** | Choice | No | Options: Good Standing, At Risk, Suspended, Collections |
| **Last Payment Date** | Date | No | Date only |
| **Last Transaction Date** | Date | No | Date only |
| **Default Contact** | Lookup | No | Table: Contacts |

**After adding all columns, click "Save"**

---

### 2.3 Create Contacts Table

**Click "+ New table"**

#### Basic Setup
- **Display name**: `Contacts`
- **Plural name**: `Contacts`
- **Description**: "Individual contacts linked to accounts"

#### Add Columns

| Column Name | Data Type | Required | Settings |
|-------------|-----------|----------|----------|
| First Name | Text | Yes | Max: 100 |
| Last Name | Text | Yes | Max: 100 |
| Email | Email | Yes | Unique: No |
| Phone | Phone | No | Format: Phone |
| Mobile Phone | Phone | No | Format: Phone |
| Job Title | Text | No | Max: 100 |
| Department | Text | No | Max: 100 |
| **Account** | Lookup | Yes | Table: Accounts |
| Contact Status | Choice | Yes | Options: Active, Inactive |
| Preferred Contact Method | Choice | No | Options: Email, Phone, In-Person |
| Is Billing Contact | Yes/No | No | Default: No |
| Is Primary Contact | Yes/No | No | Default: No |

**Save the table**

---

### 2.4 Create Transactions Table

**Click "+ New table"**

#### Basic Setup
- **Display name**: `Transactions`
- **Plural name**: `Transactions`
- **Description**: "Financial transactions (invoices, payments, credits, debits)"

#### Add Columns

| Column Name | Data Type | Required | Settings |
|-------------|-----------|----------|----------|
| Transaction ID | Text | Yes | Auto number format: TXN-{000001} |
| **Account** | Lookup | Yes | Table: Accounts |
| Transaction Type | Choice | Yes | Options: Invoice, Payment, Credit Memo, Debit Memo |
| Transaction Date | Date | Yes | Default: Today() |
| Due Date | Date | No | For invoices |
| Amount | Currency | Yes | Precision: 2 |
| Description | Text Area | No | Max: 2000 |
| Reference Number | Text | No | Invoice/PO/Check number |
| Payment Method | Choice | No | Options: Check, Credit Card, ACH, Wire, Cash, Other |
| Status | Choice | Yes | Options: Open, Paid, Partial, Overdue, Cancelled |
| **Related Invoice** | Lookup | No | Table: Transactions (for payments) |
| Notes | Text Area | No | Max: 2000 |
| Created By | User | No | Auto-populated |

**Save the table**

---

### 2.5 Create Payments Table

**Click "+ New table"**

#### Basic Setup
- **Display name**: `Payments`
- **Plural name**: `Payments`
- **Description**: "Payment records received from customers"

#### Add Columns

| Column Name | Data Type | Required | Settings |
|-------------|-----------|----------|----------|
| Payment ID | Text | Yes | Auto number: PAY-{000001} |
| **Account** | Lookup | Yes | Table: Accounts |
| Payment Date | Date | Yes | Default: Today() |
| Amount | Currency | Yes | Precision: 2 |
| Payment Method | Choice | Yes | Options: Check, Credit Card, ACH, Wire, Cash, Other |
| Reference Number | Text | No | Check/Confirmation # |
| **Applied To** | Lookup | No | Table: Transactions |
| Unapplied Amount | Currency | No | Amount not yet assigned |
| Notes | Text Area | No | Max: 2000 |
| Received By | User | No | Auto-populated |

**Save the table**

---

### 2.6 Create Inventory Table

**Click "+ New table"**

#### Basic Setup
- **Display name**: `Inventory`
- **Plural name**: `Inventory Items`
- **Description**: "Product and service inventory"

#### Add Columns

| Column Name | Data Type | Required | Settings |
|-------------|-----------|----------|----------|
| Product Name | Text | Yes | Max: 255, searchable |
| Product Code | Text | Yes | Max: 50, unique |
| Description | Text Area | No | Max: 2000 |
| Unit Price | Currency | Yes | Precision: 2 |
| Quantity On Hand | Whole Number | Yes | Min: 0 |
| Reorder Level | Whole Number | No | Min: 0 |
| Reorder Quantity | Whole Number | No | Qty to order |
| Supplier | Text | No | Max: 255 |
| Supplier Contact | Text | No | Max: 255 |
| Category | Choice | No | Options: Hardware, Software, Service, Other |
| Unit of Measure | Choice | Yes | Options: Pieces, Boxes, Cases, Units, Hours, Days |
| Last Restocked | Date | No | Auto-updated |
| Status | Choice | Yes | Options: Active, Discontinued |

**Save the table**

---

### 2.7 Create Users Table

**Click "+ New table"**

#### Basic Setup
- **Display name**: `Users`
- **Plural name**: `System Users`
- **Description**: "CRM system users with roles and permissions"

#### Add Columns

| Column Name | Data Type | Required | Settings |
|-------------|-----------|----------|----------|
| User Name | Text | Yes | Max: 100, unique |
| Email | Email | Yes | Unique, searchable |
| Full Name | Text | Yes | Max: 255 |
| Department | Text | No | Max: 100 |
| Role | Choice | Yes | Options: Admin, User (Non-Admin) |
| **Manager** | Lookup | No | Table: Users (for hierarchy) |
| User Status | Choice | Yes | Options: Active, Inactive, On Leave |
| Last Login | Date and Time | No | Auto-updated |
| Login Attempts | Whole Number | No | Failed attempts |

**Save the table**

---

## Step 3: Configure Table Relationships

### 3.1 Create Accounts → Contacts Relationship

1. Open **Accounts** table
2. Click **"Relationships"** tab
3. Click **"+ New relationship"** → **"One-to-many"**
4. Configure:

| Setting | Value |
|---------|-------|
| **Related table** | Contacts |
| **Relationship name** | accounts_contacts |
| **Lookup column name** | Account |
| **Cascade delete** | No |

5. Click **Save**

### 3.2 Create Accounts → Transactions Relationship

1. Open **Accounts** table
2. Click **"Relationships"** → **"+ New relationship"** → **"One-to-many"**
3. Configure:

| Setting | Value |
|---------|-------|
| **Related table** | Transactions |
| **Relationship name** | accounts_transactions |
| **Lookup column name** | Account |
| **Cascade delete** | No |

### 3.3 Create Accounts → Payments Relationship

1. Open **Accounts** table
2. Click **"Relationships"** → **"+ New relationship"** → **"One-to-many"**
3. Configure:

| Setting | Value |
|---------|-------|
| **Related table** | Payments |
| **Relationship name** | accounts_payments |
| **Lookup column name** | Account |
| **Cascade delete** | No |

### 3.4 Create Transactions → Payments Relationship (Optional)

1. Open **Transactions** table
2. Click **"Relationships"** → **"+ New relationship"** → **"One-to-many"**
3. Configure:

| Setting | Value |
|---------|-------|
| **Related table** | Payments |
| **Lookup column name** | Applied To |
| **Cascade delete** | No |

### 3.5 Create Users → Users Relationship (Self-referencing for Manager)

1. Open **Users** table
2. Click **"Relationships"** → **"+ New relationship"** → **"One-to-many"**
3. Configure:

| Setting | Value |
|---------|-------|
| **Related table** | Users |
| **Lookup column name** | Manager |
| **Cascade delete** | No |

---

## Step 4: Create Calculated Columns

### 4.1 Add Current Balance to Accounts

1. Open **Accounts** table
2. Click **"+ Add column"** → **"Calculated or rollup"**
3. Configure:

| Setting | Value |
|---------|-------|
| **Column name** | Current Balance |
| **Type** | Rollup |
| **Related table** | Transactions |
| **Related column** | Amount |
| **Filter** | Status ≠ "Paid" AND Status ≠ "Cancelled" AND Account = Current Account |
| **Aggregation** | SUM |
| **Data type** | Currency |

4. Click **Save**

### 4.2 Add Available Credit to Accounts

1. Open **Accounts** table
2. Click **"+ Add column"** → **"Calculated or rollup"**
3. Configure:

| Setting | Value |
|---------|-------|
| **Column name** | Available Credit |
| **Type** | Calculated |
| **Expression** | credit_limit - current_balance |
| **Data type** | Currency |

4. Click **Save**

### 4.3 Add Overdue Amount to Accounts

1. Open **Accounts** table
2. Click **"+ Add column"** → **"Calculated or rollup"**
3. Configure:

| Setting | Value |
|---------|-------|
| **Column name** | Overdue Amount |
| **Type** | Rollup |
| **Related table** | Transactions |
| **Filter** | Status = "Overdue" |
| **Aggregation** | SUM |
| **Data type** | Currency |

### 4.4 Add Days Past Due to Accounts

1. Open **Accounts** table
2. Click **"+ Add column"** → **"Calculated or rollup"**
3. Configure:

| Setting | Value |
|---------|-------|
| **Column name** | Days Past Due |
| **Type** | Calculated |
| **Expression** | IF(last_payment_date < TODAY(), DAYS(last_payment_date, TODAY()), 0) |
| **Data type** | Whole Number |

### 4.5 Add Remaining Balance to Transactions

1. Open **Transactions** table
2. Click **"+ Add column"** → **"Calculated or rollup"**
3. Configure:

| Setting | Value |
|---------|-------|
| **Column name** | Remaining Balance |
| **Type** | Calculated |
| **Expression** | amount - applied_payments |
| **Data type** | Currency |

---

## Step 5: Configure Choice Columns

### Verify All Choice Columns Are Created

Go through each table and verify the Choice columns have been created with these options:

#### Accounts - Account Status
- Active
- Inactive
- Lead
- Prospect
- Customer
- Archived

#### Accounts - Credit Status
- Good Standing
- At Risk
- Suspended
- Collections

#### Accounts - Payment Terms
- Net 30
- Net 60
- Net 90
- COD
- Prepaid
- Custom

#### Transactions - Transaction Type
- Invoice
- Payment
- Credit Memo
- Debit Memo

#### Transactions - Status
- Open
- Paid
- Partial
- Overdue
- Cancelled

#### Transactions - Payment Method
- Check
- Credit Card
- ACH
- Wire
- Cash
- Other

#### Inventory - Category
- Hardware
- Software
- Service
- Other

#### Inventory - Unit of Measure
- Pieces
- Boxes
- Cases
- Units
- Hours
- Days

#### Users - Role
- Admin
- User (Non-Admin)

#### Users - User Status
- Active
- Inactive
- On Leave

---

## Step 6: Add Test Data

### 6.1 Create Test Accounts

1. Go to Power Apps
2. Open **Apps** → Select your environment
3. Go to **Tables** → **Accounts** → **Data**
4. Click **"+ Add record"** or **"+ New row"**

Create 5 test accounts:

```
1. Account Name: "Tech Solutions Inc"
   Industry: Technology
   Credit Limit: 50,000
   Account Status: Active
   Payment Terms: Net 30

2. Account Name: "Global Finance Corp"
   Industry: Finance
   Credit Limit: 100,000
   Account Status: Active
   Payment Terms: Net 60

3. Account Name: "Healthcare Plus"
   Industry: Healthcare
   Credit Limit: 75,000
   Account Status: Active
   Payment Terms: Net 30

4. Account Name: "Retail Stores LLC"
   Industry: Retail
   Credit Limit: 40,000
   Account Status: Prospect
   Payment Terms: COD

5. Account Name: "Manufacturing Co"
   Industry: Manufacturing
   Credit Limit: 150,000
   Account Status: Active
   Payment Terms: Net 90
```

### 6.2 Create Test Contacts

For each account, create 2-3 contacts:

```
Tech Solutions Inc:
  - Contact: John Smith, Title: CFO, Email: john@tech.com, Is Billing: Yes
  - Contact: Sarah Jones, Title: Procurement, Email: sarah@tech.com, Is Primary: Yes

Global Finance Corp:
  - Contact: Mike Davis, Title: Controller, Email: mike@finance.com, Is Billing: Yes
  - Contact: Lisa Chen, Title: AP Manager, Email: lisa@finance.com

(And so on...)
```

### 6.3 Create Test Transactions (Invoices)

```
Tech Solutions Inc:
  - Amount: 15,000, Date: 2026-05-01, Due: 2026-05-31, Status: Paid
  - Amount: 25,000, Date: 2026-06-01, Due: 2026-06-30, Status: Partial
  - Amount: 10,000, Date: 2026-06-03, Due: 2026-07-03, Status: Open

Global Finance Corp:
  - Amount: 50,000, Date: 2026-05-15, Due: 2026-07-15, Status: Open
  - Amount: 30,000, Date: 2026-04-01, Due: 2026-06-01, Status: Overdue
```

### 6.4 Create Test Payments

```
Tech Solutions Inc:
  - Amount: 15,000, Date: 2026-05-28, Method: ACH, Status: Applied
  - Amount: 10,000, Date: 2026-06-02, Method: Check, Status: Applied

Global Finance Corp:
  - Amount: 25,000, Date: 2026-06-01, Method: Wire, Status: Applied
```

### 6.5 Create Test Inventory

```
1. Product: "Software License Pro", Code: "SLP-001", Price: 5,000, Qty: 50
2. Product: "Support Services", Code: "SUP-001", Price: 2,000, Qty: 100
3. Product: "Implementation", Code: "IMP-001", Price: 10,000, Qty: 10
4. Product: "Training Days", Code: "TRN-001", Price: 1,500, Qty: 200
5. Product: "Hardware Device", Code: "HW-001", Price: 3,500, Qty: 25
```

### 6.6 Create Test Users

```
Admin Users:
  1. User: "admin.user@company.com", Full Name: "Admin User", Role: Admin, Status: Active
  2. User: "finance.manager@company.com", Full Name: "Finance Manager", Role: Admin, Status: Active

Regular Users:
  1. User: "sales.rep1@company.com", Full Name: "Sales Rep 1", Role: User, Status: Active
  2. User: "sales.rep2@company.com", Full Name: "Sales Rep 2", Role: User, Status: Active
  3. User: "customer.service@company.com", Full Name: "Customer Service", Role: User, Status: Active
```

---

## Step 7: Verify Phase 1 Completion

### Checklist

- [ ] Environment created and accessible
- [ ] All 6 tables created (Accounts, Contacts, Transactions, Payments, Inventory, Users)
- [ ] All columns added with correct data types
- [ ] All relationships configured
- [ ] Calculated columns working
- [ ] Test data loaded successfully
- [ ] Can navigate between related records
- [ ] Balances calculating correctly

### Verification Steps

1. **Open Accounts table**
   - Verify Current Balance shows sum of open transactions
   - Verify Available Credit = Credit Limit - Current Balance
   - Verify Days Past Due calculated correctly

2. **Open Contacts table**
   - Verify can see related contacts under each account
   - Verify lookup to Account working

3. **Open Transactions table**
   - Verify linked to correct account
   - Verify status values showing correctly
   - Verify calculations working

4. **Check data integrity**
   - All required fields populated
   - No errors in calculations
   - Relationships working both ways

---

## 📊 Phase 1 Summary

**Total Time Estimate**: 4-6 hours

**Completed Items**:
- ✅ Power Platform environment
- ✅ 6 Dataverse tables
- ✅ 50+ columns with proper data types
- ✅ 5 table relationships
- ✅ 5 calculated columns
- ✅ Test data loaded
- ✅ Basic validation working

**Next Steps** (Phase 2):
- Create PowerApps canvas app
- Build 17 screens
- Connect to Dataverse
- Implement financial features
- Build navigation and menus

---

## ❓ Troubleshooting

### "Column already exists" error
- Click **"Columns"** to see existing columns
- Rename your new column with different name
- Or delete the existing column first

### "Relationship already exists" error
- The relationship is already configured
- Check **"Relationships"** tab to verify

### Calculated columns not showing values
- Wait 5 minutes for calculation to process
- Refresh the page
- Check that the filter/formula is correct

### Data not appearing
- Verify you're in the correct environment
- Check table permissions
- Refresh the browser

---

## ✅ Phase 1 Complete!

**Ready to move to Phase 2: PowerApps Development**

See `DEVELOPMENT-GUIDE.md` - Phase 2 section to start building your app screens.

