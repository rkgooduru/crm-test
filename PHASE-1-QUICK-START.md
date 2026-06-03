# 🚀 Phase 1 Quick Start - Execute Now!

**START DATE**: June 3, 2026  
**DURATION**: 1-2 Days  
**STATUS**: 🔴 NOT STARTED → ⏳ IN PROGRESS → 🟢 COMPLETE

---

## ⚡ Quick Summary - What You're Building

You're creating a **Dataverse database** with 6 interconnected tables:

```
📊 ACCOUNTS (customers with credit limits)
    ↓
📋 CONTACTS (people at each account)
📄 TRANSACTIONS (invoices, credits, debits)
💰 PAYMENTS (received payments)
📦 INVENTORY (products/services)
👥 USERS (system users with roles)
```

---

## 📝 Phase 1 Execution Steps (Copy-Paste Ready)

### ✅ STEP 1: Create Power Platform Environment (15 min)

**Go to**: https://admin.powerplatform.com

1. Click **"Environments"** (left sidebar)
2. Click **"+ New environment"** (top right)
3. **Fill this form**:
   - **Name**: `CRM-Development`
   - **Region**: [Your region]
   - **Type**: `Production`
   - **Enable Dataverse**: ✓ Yes
   - **Language**: `English`
   - **Currency**: `USD`
4. Click **"Next"** → **"Save"**
5. ⏳ Wait 5-10 minutes

**✅ Verification**: Environment appears in list and is accessible

---

### ✅ STEP 2: Create 6 Tables (2-3 hours)

**Go to**: https://make.powerapps.com

Select your environment from dropdown → Click **"Tables"**

#### TABLE 1: ACCOUNTS ✏️

Click **"Create a table"**:
- **Display name**: `Accounts`
- **Plural name**: `Accounts`
- **Click Create** (then add columns)

**ADD THESE 23 COLUMNS** (copy-paste the name):

```
1. Account Name (Text, REQUIRED, searchable)
2. Industry (Choice)
3. Annual Revenue (Currency)
4. Address Street (Text)
5. City (Text)
6. State Province (Text)
7. ZIP Postal Code (Text)
8. Country Region (Text)
9. Phone (Phone)
10. Email (Email)
11. Website (URL)
12. Number of Employees (Whole Number)
13. Account Status (Choice, REQUIRED)
14. Credit Limit (Currency) ⭐ FINANCIAL
15. Payment Terms (Choice) ⭐ FINANCIAL
16. Credit Status (Choice) ⭐ FINANCIAL
17. Last Payment Date (Date) ⭐ FINANCIAL
18. Last Transaction Date (Date) ⭐ FINANCIAL
19. Default Contact (Lookup to Contacts)
```

**CHOICE OPTIONS**:
- **Account Status**: Active, Inactive, Lead, Prospect, Customer, Archived
- **Credit Status**: Good Standing, At Risk, Suspended, Collections
- **Payment Terms**: Net 30, Net 60, Net 90, COD, Prepaid, Custom
- **Industry**: Technology, Finance, Healthcare, Retail, Manufacturing, Services, Other

**Save** ✓

---

#### TABLE 2: CONTACTS ✏️

**Click "Create a table"**:
- **Display name**: `Contacts`
- **Create** → Add columns:

```
1. First Name (Text, REQUIRED)
2. Last Name (Text, REQUIRED)
3. Email (Email, REQUIRED)
4. Phone (Phone)
5. Mobile Phone (Phone)
6. Job Title (Text)
7. Department (Text)
8. Account (Lookup to Accounts, REQUIRED)
9. Contact Status (Choice, REQUIRED)
10. Preferred Contact Method (Choice)
11. Is Billing Contact (Yes/No)
12. Is Primary Contact (Yes/No)
```

**CHOICE OPTIONS**:
- **Contact Status**: Active, Inactive
- **Preferred Contact Method**: Email, Phone, In-Person

**Save** ✓

---

#### TABLE 3: TRANSACTIONS ✏️

**Click "Create a table"**:
- **Display name**: `Transactions`
- **Create** → Add columns:

```
1. Transaction ID (Text, REQUIRED, Auto: TXN-{000001})
2. Account (Lookup to Accounts, REQUIRED)
3. Transaction Type (Choice, REQUIRED)
4. Transaction Date (Date, REQUIRED)
5. Due Date (Date)
6. Amount (Currency, REQUIRED)
7. Description (Text Area)
8. Reference Number (Text)
9. Payment Method (Choice)
10. Status (Choice, REQUIRED)
11. Related Invoice (Lookup to Transactions)
12. Notes (Text Area)
13. Created By (User, auto)
```

**CHOICE OPTIONS**:
- **Transaction Type**: Invoice, Payment, Credit Memo, Debit Memo
- **Status**: Open, Paid, Partial, Overdue, Cancelled
- **Payment Method**: Check, Credit Card, ACH, Wire, Cash, Other

**Save** ✓

---

#### TABLE 4: PAYMENTS ✏️

**Click "Create a table"**:
- **Display name**: `Payments`
- **Create** → Add columns:

```
1. Payment ID (Text, REQUIRED, Auto: PAY-{000001})
2. Account (Lookup to Accounts, REQUIRED)
3. Payment Date (Date, REQUIRED)
4. Amount (Currency, REQUIRED)
5. Payment Method (Choice, REQUIRED)
6. Reference Number (Text)
7. Applied To (Lookup to Transactions)
8. Unapplied Amount (Currency)
9. Notes (Text Area)
10. Received By (User, auto)
```

**CHOICE OPTIONS**:
- **Payment Method**: Check, Credit Card, ACH, Wire, Cash, Other

**Save** ✓

---

#### TABLE 5: INVENTORY ✏️

**Click "Create a table"**:
- **Display name**: `Inventory`
- **Create** → Add columns:

```
1. Product Name (Text, REQUIRED, searchable)
2. Product Code (Text, REQUIRED, unique)
3. Description (Text Area)
4. Unit Price (Currency, REQUIRED)
5. Quantity On Hand (Whole Number, REQUIRED)
6. Reorder Level (Whole Number)
7. Reorder Quantity (Whole Number)
8. Supplier (Text)
9. Supplier Contact (Text)
10. Category (Choice)
11. Unit of Measure (Choice, REQUIRED)
12. Last Restocked (Date)
13. Status (Choice, REQUIRED)
```

**CHOICE OPTIONS**:
- **Category**: Hardware, Software, Service, Other
- **Unit of Measure**: Pieces, Boxes, Cases, Units, Hours, Days
- **Status**: Active, Discontinued

**Save** ✓

---

#### TABLE 6: USERS ✏️

**Click "Create a table"**:
- **Display name**: `Users`
- **Create** → Add columns:

```
1. User Name (Text, REQUIRED, unique)
2. Email (Email, REQUIRED, unique)
3. Full Name (Text, REQUIRED)
4. Department (Text)
5. Role (Choice, REQUIRED)
6. Manager (Lookup to Users)
7. User Status (Choice, REQUIRED)
8. Last Login (Date and Time)
9. Login Attempts (Whole Number)
```

**CHOICE OPTIONS**:
- **Role**: Admin, User (Non-Admin)
- **User Status**: Active, Inactive, On Leave

**Save** ✓

---

### ✅ STEP 3: Configure Relationships (30 min)

Each table is now independent. You need to connect them.

**For each relationship below**:
1. Open the **PARENT** table (e.g., Accounts)
2. Click **"Relationships"** tab
3. Click **"+ New relationship"** → **"One-to-many"**
4. Set "Related table" to **CHILD** table
5. Set "Lookup column name"
6. Click **Save**

#### REL 1: Accounts → Contacts
```
Parent: Accounts
Child: Contacts
Lookup name: Account
Result: One account has many contacts ✓
```

#### REL 2: Accounts → Transactions
```
Parent: Accounts
Child: Transactions
Lookup name: Account
Result: One account has many transactions ✓
```

#### REL 3: Accounts → Payments
```
Parent: Accounts
Child: Payments
Lookup name: Account
Result: One account has many payments ✓
```

#### REL 4: Transactions → Payments
```
Parent: Transactions
Child: Payments
Lookup name: Applied To
Result: One invoice can have many payments ✓
```

#### REL 5: Users → Users
```
Parent: Users
Child: Users
Lookup name: Manager
Result: Manager hierarchy setup ✓
```

---

### ✅ STEP 4: Add Calculated Columns (30 min)

These columns automatically calculate values based on related data.

**For each calculated column**:
1. Open the table
2. Click **"+ Add column"** → **"Calculated or rollup"**
3. Configure per instructions below
4. Click **Save**

#### CALC 1: Current Balance (on Accounts)
```
Name: Current Balance
Type: Rollup
Related table: Transactions
Filter: Status ≠ "Paid" AND Status ≠ "Cancelled"
Aggregation: SUM
Field: Amount
Data type: Currency
Result: Shows total open invoices ✓
```

#### CALC 2: Available Credit (on Accounts)
```
Name: Available Credit
Type: Calculated
Expression: credit_limit - current_balance
Data type: Currency
Result: Credit Limit minus Current Balance ✓
```

#### CALC 3: Overdue Amount (on Accounts)
```
Name: Overdue Amount
Type: Rollup
Related table: Transactions
Filter: Status = "Overdue"
Aggregation: SUM
Field: Amount
Data type: Currency
Result: Shows total overdue invoices ✓
```

#### CALC 4: Days Past Due (on Accounts)
```
Name: Days Past Due
Type: Calculated
Expression: IF(last_payment_date < TODAY(), DAYS(last_payment_date, TODAY()), 0)
Data type: Whole Number
Result: How many days overdue ✓
```

---

### ✅ STEP 5: Load Test Data (1-2 hours)

Create sample records so you have something to work with.

**Go to Tables → [Table Name] → Data tab**

#### Add 5 TEST ACCOUNTS:

```
1️⃣  Tech Solutions Inc
   Industry: Technology
   Credit Limit: $50,000
   Status: Active
   Payment Terms: Net 30

2️⃣  Global Finance Corp
   Industry: Finance
   Credit Limit: $100,000
   Status: Active
   Payment Terms: Net 60

3️⃣  Healthcare Plus
   Industry: Healthcare
   Credit Limit: $75,000
   Status: Active
   Payment Terms: Net 30

4️⃣  Retail Stores LLC
   Industry: Retail
   Credit Limit: $40,000
   Status: Prospect
   Payment Terms: COD

5️⃣  Manufacturing Co
   Industry: Manufacturing
   Credit Limit: $150,000
   Status: Active
   Payment Terms: Net 90
```

#### Add CONTACTS (2 per account):

```
FOR: Tech Solutions Inc
  ✓ John Smith, CFO, john@tech.com, Is Billing: Yes
  ✓ Sarah Jones, Procurement, sarah@tech.com, Is Primary: Yes

FOR: Global Finance Corp
  ✓ Mike Davis, Controller, mike@finance.com, Is Billing: Yes
  ✓ Lisa Chen, AP Manager, lisa@finance.com

FOR: Healthcare Plus
  ✓ Jennifer Brown, Finance Dir, jennifer@health.com, Is Billing: Yes
  ✓ Robert Wilson, Purchasing, robert@health.com

FOR: Retail Stores LLC
  ✓ Patricia Garcia, Manager, patricia@retail.com, Is Billing: Yes

FOR: Manufacturing Co
  ✓ Thomas Anderson, CFO, thomas@mfg.com, Is Billing: Yes
  ✓ Maria Lopez, AP Clerk, maria@mfg.com
```

#### Add TRANSACTIONS (Invoices):

```
FOR: Tech Solutions Inc
  ✓ Invoice, $15,000, Date: 5/1/26, Due: 5/31/26, Status: Paid
  ✓ Invoice, $25,000, Date: 6/1/26, Due: 6/30/26, Status: Partial
  ✓ Invoice, $10,000, Date: 6/3/26, Due: 7/3/26, Status: Open

FOR: Global Finance Corp
  ✓ Invoice, $50,000, Date: 5/15/26, Due: 7/15/26, Status: Open
  ✓ Invoice, $30,000, Date: 4/1/26, Due: 6/1/26, Status: Overdue

FOR: Healthcare Plus
  ✓ Invoice, $20,000, Date: 5/20/26, Due: 6/20/26, Status: Partial
  ✓ Invoice, $15,000, Date: 6/1/26, Due: 7/1/26, Status: Open
```

#### Add PAYMENTS:

```
FOR: Tech Solutions Inc
  ✓ Payment, $15,000, Date: 5/28/26, Method: ACH, Applied To: Invoice from 5/1
  ✓ Payment, $10,000, Date: 6/2/26, Method: Check, Applied To: Invoice from 6/1

FOR: Global Finance Corp
  ✓ Payment, $25,000, Date: 6/1/26, Method: Wire, Applied To: Invoice from 5/15

FOR: Healthcare Plus
  ✓ Payment, $10,000, Date: 5/25/26, Method: ACH, Applied To: Invoice from 5/20
```

#### Add INVENTORY (5 items):

```
1. Software License Pro, Code: SLP-001, Price: $5,000, Qty: 50
2. Support Services, Code: SUP-001, Price: $2,000, Qty: 100
3. Implementation, Code: IMP-001, Price: $10,000, Qty: 10
4. Training Days, Code: TRN-001, Price: $1,500, Qty: 200
5. Hardware Device, Code: HW-001, Price: $3,500, Qty: 25
```

#### Add USERS (5 users):

```
ADMINS:
  ✓ admin.user@company.com, Full Name: Admin User, Role: Admin, Status: Active
  ✓ finance.manager@company.com, Full Name: Finance Manager, Role: Admin, Status: Active

REGULAR USERS:
  ✓ sales.rep1@company.com, Full Name: Sales Rep 1, Role: User, Status: Active
  ✓ sales.rep2@company.com, Full Name: Sales Rep 2, Role: User, Status: Active
  ✓ customer.service@company.com, Full Name: Customer Service, Role: User, Status: Active
```

---

### ✅ STEP 6: Verify Everything Works (30 min)

**Test each feature**:

#### Check Accounts:
- [ ] Open "Tech Solutions Inc"
- [ ] Can you see Current Balance? (should be $25,000 from open invoices)
- [ ] Can you see Available Credit? (should be $25,000)
- [ ] Can you see related Contacts? (should see John & Sarah)
- [ ] Can you see related Transactions? (should see 3 invoices)

#### Check Calculations:
- [ ] Refresh page
- [ ] Do numbers still show? ✓
- [ ] Is math correct?
  - Current Balance = Open + Partial invoices = $25,000 + $10,000 = $35,000
  - Available Credit = $50,000 - $35,000 = $15,000

#### Check Relationships:
- [ ] Open Contacts
- [ ] Can you see Account lookup working?
- [ ] Can you click Account to go back to account?

#### Check Data Integrity:
- [ ] All required fields filled?
- [ ] No errors on page?
- [ ] Can navigate between tables?

---

## 📊 Phase 1 Status Checklist

### BEFORE YOU START:
- [ ] You have Microsoft 365 account
- [ ] You have Power Platform license (included with most Microsoft 365)
- [ ] You have at least 2 hours available
- [ ] You've read PHASE-1-DATAVERSE-SETUP.md

### DURING EXECUTION:
- [ ] Step 1: Environment created (15 min)
- [ ] Step 2: All 6 tables created (2-3 hours)
- [ ] Step 3: All 5 relationships configured (30 min)
- [ ] Step 4: All calculated columns added (30 min)
- [ ] Step 5: Test data loaded (1-2 hours)
- [ ] Step 6: Verification tests passing (30 min)

### AFTER COMPLETION:
- [ ] All tables have data
- [ ] Calculations working
- [ ] Relationships showing
- [ ] Ready for Phase 2
- [ ] Document any issues

---

## ⏱️ Timeline

```
START: Now!

Step 1 (15 min)  → Environment created
Step 2 (2-3 hrs) → 6 tables with 90+ columns
Step 3 (30 min)  → 5 relationships connected
Step 4 (30 min)  → Calculated columns working
Step 5 (1-2 hrs) → Test data loaded
Step 6 (30 min)  → Verification complete

TOTAL: ~5-6 hours → Phase 1 Complete ✅
```

---

## 🆘 Quick Troubleshooting

| Issue | Fix |
|-------|-----|
| Environment won't create | Check you have admin rights, try again |
| Can't see tables | Refresh page, select environment from dropdown |
| Column creation fails | Check column name not already used, try refresh |
| Relationships won't save | Ensure both tables exist first |
| Calculations show no value | Wait 5 minutes, refresh page, check formula |
| No data appearing | Go to table → Data tab (not Columns tab) |

---

## ✅ Phase 1 SUCCESS = You Have:

✓ Power Platform environment  
✓ 6 Dataverse tables  
✓ 90+ configured columns  
✓ 5 working relationships  
✓ 5 calculated fields  
✓ 20+ test records  
✓ Ready for PowerApps development  

---

## 🚀 Next Steps (After Phase 1 Complete):

1. **Phase 2**: Build PowerApps (17 screens)
2. **Phase 3**: Create Power Automate workflows
3. **Phase 4**: Testing
4. **Phase 5**: Deployment

---

## 📖 Reference Links

- **Power Platform Admin**: https://admin.powerplatform.com
- **Power Apps Maker**: https://make.powerapps.com
- **Help**: Open PHASE-1-DATAVERSE-SETUP.md for detailed instructions
- **Questions?**: Check Troubleshooting section above

---

## 🎯 Your Mission

**BUILD PHASE 1 TODAY!**

Start with Step 1 in the next 15 minutes. By end of day, you'll have a functioning Dataverse with:
- ✅ Database ready
- ✅ Tables connected
- ✅ Calculations working
- ✅ Test data loaded

**LET'S GO! 🚀**

