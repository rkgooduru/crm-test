# Power Automate Workflows - Complete Guide

## Financial Management Workflows

### Workflow 1: Invoice Due Soon Alert

**Trigger:** Scheduled daily at 8 AM

**Flow Name:** `InvoiceDueSoonAlert`

### Steps

1. **Trigger**: Scheduled (Daily, 8 AM)

2. **Query Invoices Due in 3 Days**:
   ```
   Filter Transactions where:
   Type = "Invoice"
   Status = "Open" or "Partial"
   Due Date = Tomorrow + 2 days
   Account.Credit Status <> "Suspended"
   ```

3. **For Each Invoice**:
   - Get Account details
   - Get Billing Contact email
   - Calculate amount due

4. **Send Email Alert**:
   - **To**: Account Billing Contact
   - **Subject**: "Invoice Payment Due in 3 Days - [Amount]"
   - **Body**:
     ```
     Invoice #: [Invoice ID]
     Amount Due: [Amount]
     Due Date: [Due Date]
     Current Balance: [Current Balance]
     
     Payment Terms: [Payment Terms]
     
     Please arrange payment by the due date to avoid late payment fees.
     
     [Payment Instructions]
     ```

5. **CC**: Sales Rep / Account Manager (if assigned)

---

### Workflow 2: Overdue Payment Reminder

**Trigger:** Scheduled daily at 10 AM

**Flow Name:** `OverduePaymentReminder`

### Steps

1. **Trigger**: Scheduled (Daily, 10 AM)

2. **Query Overdue Invoices**:
   ```
   Filter Transactions where:
   Type = "Invoice"
   Status = "Open" or "Partial"
   Due Date < Today
   Overdue Days >= 1
   Account.Status = "Active"
   ```

3. **Group by Days Overdue**:
   - 1-7 days overdue: Courtesy reminder
   - 8-14 days overdue: Urgent reminder
   - 15+ days overdue: Collections escalation

4. **For Each Overdue Invoice**:
   - Get Account contacts
   - Get Account credit status
   - Calculate late fees (if applicable)

5. **Send Appropriate Email**:
   - **Template varies by overdue days**
   - Include amount, invoice #, and overdue days
   - Include payment options/instructions
   - For 15+ days: Include collections contact

6. **Update Account Credit Status** (if 15+ days overdue):
   - Set to "At Risk"
   - Trigger credit review

7. **Create Task** (if 30+ days overdue):
   - Assign to Collections team
   - Flag for manual follow-up

---

### Workflow 3: High Credit Usage Alert

**Trigger:** When a Transaction is added or modified (Amount > threshold)

**Flow Name:** `HighCreditUsageAlert`

### Steps

1. **Trigger**: When row is added/modified (Transactions table)
   - Table: Transactions
   - Scope: Organization

2. **Calculate Credit Utilization**:
   ```
   Credit Utilization % = Current Balance / Credit Limit
   ```

3. **Condition**: Check if utilization >= 80%
   ```
   If Current Balance >= (Credit Limit × 0.80)
   ```

4. **If Condition Met**:

   a. **Send Alert to Account Manager**:
   - **To**: Sales Rep/Account Manager
   - **Subject**: "ALERT: [Account Name] - Credit Limit 80% Utilized"
   - **Body**:
     ```
     Account: [Account Name]
     Credit Limit: [Credit Limit]
     Current Balance: [Current Balance]
     Available Credit: [Available Credit]
     Utilization: [Percentage]%
     
     Recommended Actions:
     - Contact account to discuss payment schedule
     - Consider credit limit increase if warranted
     - Monitor for payment delays
     ```

   b. **Send Alert to Finance/Credit Manager**:
   - Similar email with additional credit risk assessment

   c. **Log Alert**: Create audit entry

5. **If Utilization >= 95%**:
   - Send URGENT email to Finance Manager
   - Flag account for credit review
   - Reduce order limits if automated ordering in place

6. **If Utilization >= 100%**:
   - Send CRITICAL alert
   - Freeze new transactions (if system allows)
   - Alert Collections team

---

### Workflow 4: Payment Received Notification

**Trigger:** When a new Payment is recorded

**Flow Name:** `PaymentReceivedNotification`

### Steps

1. **Trigger**: When row is added (Payments table)

2. **Get Account Details**:
   - Account information
   - Primary and billing contacts
   - Account manager

3. **Get Applied Transactions** (Invoices):
   - Identify which invoices this payment covers
   - Calculate remaining balance on each

4. **Send Email to Account Contact**:
   - **To**: Account Primary/Billing Contact
   - **Subject**: "Payment Received - [Amount] - [Account Name]"
   - **Body**:
     ```
     Payment Confirmation
     
     Payment Date: [Date]
     Amount Received: [Amount]
     Payment Method: [Method]
     Reference Number: [Ref #]
     
     Applied To:
     [Invoice # - Amount] [New Balance]
     [Invoice # - Amount] [New Balance]
     
     Current Outstanding Balance: [Total Balance]
     Available Credit: [Available Credit]
     
     Thank you for your payment!
     ```

5. **Send Confirmation to Finance**:
   - **To**: Accounting/Finance team
   - Include payment details and application
   - Reference for reconciliation

6. **Update Last Payment Date**:
   - Account.Last_Payment_Date = Today()
   - Account.Credit_Status = "Good Standing" (if applicable)

7. **If Payment is Partial**:
   - Note that invoice is partially paid
   - Calculate new due date for remainder

---

### Workflow 5: Credit Status Change Notification

**Trigger:** Account Credit Status is changed

**Flow Name:** `CreditStatusChangeAlert`

### Steps

1. **Trigger**: When row is modified (Accounts table)
   - Check: Credit_Status has changed

2. **Determine Old and New Status**:
   - Get previous credit status from audit
   - Compare to new status

3. **If Status Changed TO "At Risk"**:
   - **Notify**: Sales Rep, Account Manager, Finance Manager
   - **Subject**: "Credit Alert: [Account Name] - Status Changed to At Risk"
   - **Body**: Include overdue details and recommended actions

4. **If Status Changed TO "Suspended"**:
   - **Notify**: Sales Rep, Account Manager, Finance Manager, Collections
   - **Subject**: "CRITICAL: [Account Name] - Credit Suspended"
   - **Body**: Include overdue details and payment instructions
   - **Action**: Log case for collections follow-up
   - **Action**: Potentially restrict new orders

5. **If Status Changed Back TO "Good Standing"**:
   - **Notify**: Sales Rep (positive news)
   - **Subject**: "Good News: [Account Name] - Credit Status Restored"
   - **Action**: Restore order limits if restricted

6. **Create Audit Log Entry**:
   - Record status change
   - Previous status
   - New status
   - Trigger reason
   - Timestamp

---

### Workflow 6: New User Welcome Email

**Trigger:** New user created in system

**Flow Name:** `NewUserWelcome`

### Steps

1. **Trigger**: When row is added (Users table)

2. **Get User Details**: Retrieve new user info
   - Name, email, role, department

3. **Get Manager Info**: Retrieve manager details (if assigned)

4. **Send Welcome Email**:
   - **To**: New user email
   - **Subject**: "Welcome to CRM System - [Company Name]"
   - **Body**:
     ```
     Welcome to the CRM System!
     
     Your Account Details:
     Username: [User Name]
     Role: [Admin/User]
     Department: [Department]
     
     Your Manager: [Manager Name] - [Manager Email]
     
     Getting Started:
     1. Log in with your corporate credentials
     2. Complete your profile
     3. Review the training materials
     4. Contact IT if you need assistance
     
     Support: IT Help Desk (IT@company.com)
     ```

5. **Send Admin Notification**:
   - **To**: System admins
   - **Subject**: "New User Created: [User Name]"
   - Alert admin to verify user permissions

---

### Workflow 7: Low Inventory Alert

**Trigger:** When an inventory item quantity falls below reorder level

**Flow Name:** `LowStockAlert`

### Steps

1. **Trigger**: When a row is modified (Inventory table)
   - Table: Inventory
   - Scope: Organization

2. **Condition**: Check if quantity < reorder level
   ```
   Quantity On Hand < Reorder Level
   ```

3. **If Yes**: Send Email Notification
   - **To**: Inventory Manager (admin users)
   - **Subject**: "LOW STOCK ALERT: [Product Name] needs reordering"
   - **Body**:
     ```
     Product: [Product Name]
     Current Stock: [Quantity On Hand] [Unit of Measure]
     Reorder Level: [Reorder Level]
     Suggested Order Qty: [Reorder Quantity]
     Supplier: [Supplier Name]
     Supplier Contact: [Supplier Contact]
     
     Action Required: Place reorder immediately
     ```

4. **Log**: Add entry to activity log

---

## Workflow Error Handling

### General Error Handling Strategy

All workflows include:

1. **Try-Catch**: Wrap critical steps
   - If error occurs: Send notification to admin
   - Log error details: Timestamp, error message, affected record

2. **Retry Logic**:
   - Retry up to 3 times on transient failures
   - Wait 5 seconds between retries

3. **Admin Notification**:
   - Send email to admins if workflow fails
   - Include error details and affected records

4. **Logging**:
   - All workflow actions logged
   - Failures tracked in audit table
   - Success metrics tracked

### Workflow Monitoring

- Monitor workflow runs in Power Automate cloud
- Set up alerts for repeated failures
- Weekly review of workflow success rates
- Target: 99% success rate

---

## Deployment Checklist

- [ ] All workflows created in Power Automate
- [ ] Triggers configured correctly
- [ ] Email templates created
- [ ] Error handling implemented
- [ ] Admin notifications set up
- [ ] Workflows tested with test data
- [ ] Monitoring/alerts configured
- [ ] Documentation updated
- [ ] Training provided to admins
- [ ] Financial workflows scheduled
- [ ] Credit limit thresholds configured
- [ ] Payment reminder schedules set

