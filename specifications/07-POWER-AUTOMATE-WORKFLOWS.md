# Power Automate Workflows

## Workflow 1: Low Inventory Alert

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

## Workflow 2: New Contact Notification

**Trigger:** When a new contact is created

**Flow Name:** `NewContactNotification`

### Steps

1. **Trigger**: When a row is added (Contacts table)

2. **Get Account Details**: Retrieve parent account info
   - Look up Account by ID

3. **Get Account Owner**: Find user who owns the account
   - Query Users table

4. **Send Email**: Notify account owner
   - **To**: Account Owner email
   - **Subject**: "New Contact Added: [Contact Name]"
   - **Body**:
     ```
     A new contact has been added to your account.
     
     Contact: [Full Name]
     Title: [Job Title]
     Email: [Email]
     Phone: [Phone]
     Department: [Department]
     Account: [Account Name]
     
     Review and update contact details as needed.
     ```

---

## Workflow 3: User Login Tracking

**Trigger:** User logs into the application

**Flow Name:** `UserLoginTracker`

### Steps

1. **Trigger**: Manual trigger or App launch event

2. **Get Current User**: Retrieve logged-in user details
   - User email from context
   - Look up user record

3. **Update Last Login**: Update Users table
   - Field: Last Login = Current Date/Time
   - Check for failed attempts and reset if successful

4. **Log Activity**: Create audit log entry
   - User: [User Email]
   - Action: Login
   - Timestamp: Current time
   - IP Address: (if available)

5. **Failed Login Alert**: (Optional)
   - If login attempts > 5, send alert to admin

---

## Workflow 4: Automated Reorder Suggestion

**Trigger:** Scheduled daily at 8 AM

**Flow Name:** `DailyReorderSuggestion`

### Steps

1. **Trigger**: Scheduled (Daily, 8 AM)

2. **Query Low Stock Items**:
   ```
   Filter Inventory where:
   Quantity On Hand < Reorder Level
   Status = Active
   ```

3. **Generate Report**: Create summary
   - Total items needing reorder
   - Total suggested order quantity
   - Total estimated cost

4. **Send Daily Summary Email**:
   - **To**: Inventory Manager
   - **Subject**: "Daily Reorder Summary - [Date]"
   - **Body**: Formatted table of items needing reorder
   - **Attachment**: CSV export (if applicable)

5. **Create Task** (if applicable):
   - For each low-stock item
   - Assign to inventory manager
   - Due date: Today

---

## Workflow 5: Account Status Change Notification

**Trigger:** Account status is changed

**Flow Name:** `AccountStatusChangeAlert`

### Steps

1. **Trigger**: When row is modified (Accounts table)
   - Check: Account Status has changed

2. **Condition**: Determine status change
   - If Status = "Inactive"

3. **Notify Related Contacts**:
   - Find all contacts for this account
   - Get contact emails

4. **Send Notification Email**:
   - **To**: Related contacts
   - **Subject**: "Account Status Changed: [Account Name]"
   - **Body**:
     ```
     Please note: [Account Name] status has changed to [New Status]
     
     This may affect service delivery or support availability.
     Contact your manager for more information.
     ```

5. **Log Change**: Create audit entry
   - Record old status and new status
   - Timestamp
   - User who made change

---

## Workflow 6: New User Welcome Email

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

## Workflow 7: Data Export Request (Admin)

**Trigger:** Admin requests data export

**Flow Name:** `DataExportRequest`

### Steps

1. **Trigger**: Manual button click (Admin only)

2. **Select Data**: Choose entities to export
   - Accounts
   - Contacts
   - Inventory

3. **Generate CSV Files**:
   - Export to CSV format
   - Apply filters (date range, status, etc)

4. **Create Folder**: In OneDrive/SharePoint
   - Folder name: "CRM_Export_[Date]"

5. **Upload Files**: Store exported data
   - Accounts_[Date].csv
   - Contacts_[Date].csv
   - Inventory_[Date].csv

6. **Send Download Link**:
   - **To**: Requesting admin
   - **Subject**: "Your CRM Data Export is Ready"
   - **Body**: Links to download files
   - **Expiration**: 7 days

7. **Log Request**: Audit trail
   - Who requested
   - What data
   - When
   - Completion status

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

