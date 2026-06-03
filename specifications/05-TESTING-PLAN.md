# Testing Plan & Test Cases

## Testing Strategy

### Test Levels
1. **Unit Testing**: Individual controls and formulas
2. **Integration Testing**: Module interactions
3. **System Testing**: End-to-end workflows
4. **UAT**: User acceptance testing
5. **Security Testing**: Role-based access verification

---

## Test Cases

### Accounts Module

#### TC-ACC-001: Create New Account
**Prerequisites:** User logged in as Admin or User role

**Steps:**
1. Navigate to Accounts
2. Click "New Account" button
3. Enter account name, industry, phone, email
4. Click Save

**Expected Result:** Account created successfully, appears in list

---

#### TC-ACC-002: User Can Only Edit Own Accounts
**Prerequisites:** Two user accounts created

**Steps:**
1. User A creates Account X
2. User B logs in
3. Attempt to edit Account X

**Expected Result:** User B cannot edit Account X (read-only or hidden)

---

#### TC-ACC-003: Search and Filter Accounts
**Prerequisites:** Multiple accounts exist

**Steps:**
1. Navigate to Accounts
2. Enter search term in search box
3. Select filter criteria (Industry, Status)
4. Click Apply

**Expected Result:** Results filtered correctly, showing only matching records

---

### Contacts Module

#### TC-CON-001: Create Contact with Account Link
**Prerequisites:** Accounts exist in system

**Steps:**
1. Navigate to Contacts
2. Click "New Contact"
3. Enter first name, last name, email
4. Select Account from lookup
5. Click Save

**Expected Result:** Contact created and linked to account

---

#### TC-CON-002: Duplicate Detection Warning
**Prerequisites:** Contact exists with same email

**Steps:**
1. Create new contact
2. Enter email that already exists in system
3. Attempt to save

**Expected Result:** Warning displayed for potential duplicate

---

#### TC-CON-003: View Related Contacts on Account
**Prerequisites:** Account has multiple contacts

**Steps:**
1. Open Account detail
2. Scroll to Related Contacts section
3. Verify all contacts displayed

**Expected Result:** All related contacts displayed correctly

---

### Inventory Module

#### TC-INV-001: User Can View Inventory (Read-Only)
**Prerequisites:** User logged in (non-admin)

**Steps:**
1. Navigate to Inventory
2. Attempt to edit inventory item
3. Attempt to create new item

**Expected Result:** Create/Edit buttons disabled or hidden for non-admin users

---

#### TC-INV-002: Low Stock Alert Triggered
**Prerequisites:** Inventory item with reorder level set

**Steps:**
1. Reduce item quantity below reorder level
2. Save changes
3. Check for alert notification

**Expected Result:** Alert appears for low stock item

---

#### TC-INV-003: Admin Can Manage Inventory
**Prerequisites:** Admin logged in

**Steps:**
1. Create new inventory item
2. Edit existing item
3. Delete (if permitted)

**Expected Result:** All operations successful

---

### User Admin Module

#### TC-USR-001: Admin Can Access User Admin Module
**Prerequisites:** Admin logged in

**Steps:**
1. Check navigation menu
2. Click User Admin
3. View user directory

**Expected Result:** User Admin visible in menu, users listed

---

#### TC-USR-002: Non-Admin Cannot Access User Admin Module
**Prerequisites:** Non-admin user logged in

**Steps:**
1. Check navigation menu
2. Look for User Admin option
3. Attempt direct URL navigation

**Expected Result:** User Admin not in menu, URL access denied

---

#### TC-USR-003: Create New User and Assign Role
**Prerequisites:** Admin logged in

**Steps:**
1. Go to User Admin
2. Click "New User"
3. Enter user details
4. Select role (Admin or User)
5. Save

**Expected Result:** User created with correct role assigned

---

### Role-Based Access Control

#### TC-SEC-001: Menu Items Based on Role
**Prerequisites:** Admin and User accounts created

**Steps:**
1. Log in as Admin - verify full menu
2. Log out
3. Log in as User - verify limited menu
4. Verify Inventory is read-only or hidden

**Expected Result:** Menu items appropriate for each role

---

#### TC-SEC-002: Field-Level Security
**Prerequisites:** Users logged in

**Steps:**
1. Admin views Account - sees Annual Revenue
2. User views Account - verify sensitive fields hidden
3. Non-Admin views Inventory - Cost hidden

**Expected Result:** Sensitive fields hidden from non-admin users

---

#### TC-SEC-003: Data Ownership Verification
**Prerequisites:** Multiple users

**Steps:**
1. User A creates Account
2. User B attempts to delete User A's Account
3. Admin attempts to delete any Account

**Expected Result:** User B denied, Admin allowed

---

## Test Data Requirements

### Test Accounts
- 5 test accounts across different industries
- Various statuses (Active, Prospect, Inactive)
- Different revenue ranges

### Test Contacts
- 15 test contacts distributed across accounts
- Mix of valid and duplicate email addresses
- Various job titles and departments

### Test Inventory
- 20 test inventory items
- Various stock levels
- Some items below reorder level
- Different suppliers and categories

### Test Users
- 3 Admin users
- 5 Non-Admin users
- Users in different departments

---

## UAT Checklist

- [ ] All screens load without errors
- [ ] Create operations work correctly
- [ ] Edit operations save changes
- [ ] Delete operations (where permitted) work
- [ ] Search and filter return correct results
- [ ] Role-based access enforced
- [ ] Navigation is intuitive
- [ ] Performance is acceptable (<2s load time)
- [ ] No data corruption observed
- [ ] Workflows execute automatically
- [ ] Error messages are clear
- [ ] Help text is available

---

## Defect Severity Levels

| Severity | Description | Example |
|----------|-------------|---------|
| Critical | App crashes or data loss | Unsaved data lost on save |
| High | Major feature broken | Cannot create accounts |
| Medium | Workaround exists | Minor field validation issue |
| Low | Minor cosmetic issues | Alignment issue in UI |

---

## Sign-Off

- [ ] QA Manager: _______________ Date: _______
- [ ] Business Owner: _______________ Date: _______
- [ ] IT Manager: _______________ Date: _______

