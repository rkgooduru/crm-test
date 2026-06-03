# PowerApps Configuration Guide

## Canvas App Architecture

### App Name: CRM Management System
**Target Device:** Tablet (responsive for mobile and web)

---

## Screen Structure

### 1. Splash/Login Screen
```
Screen: SplashScreen
├── Background image/color
├── App logo
├── Welcome message
├── Login status indicator
```

---

### 2. Main Dashboard
```
Screen: DashboardScreen
├── Header
│   ├── Welcome [UserName]
│   └── Role badge (Admin/User)
├── Quick Actions (4 tiles)
│   ├── New Account
│   ├── New Contact
│   ├── View Inventory
│   └── My Records
├── Recent Records (3 sections)
│   ├── Recent Accounts
│   ├── Recent Contacts
│   └── Pending Tasks
└── Navigation Menu (role-based)
    ├── Accounts
    ├── Contacts
    ├── Inventory
    ├── User Admin (Admin only)
    ├── Reports (Admin only)
    └── Settings (Admin only)
```

---

### 3. Accounts Module

#### Screen: Accounts List
```
Screen: AccountsListScreen
├── Search/Filter Bar
│   ├── Search box (by name)
│   ├── Industry filter
│   └── Status filter
├── Gallery Control
│   ├── Account Name (Title)
│   ├── Industry (Subtitle)
│   ├── Status badge
│   └── Phone number
├── Actions
│   ├── New Account button
│   ├── Refresh button
│   └── Item selection → Detail
└── Pagination (if >100 records)
```

**Formula for gallery:**
```powerapps
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
```

#### Screen: Account Detail
```
Screen: AccountDetailScreen
├── Header
│   ├── Account Name
│   ├── Status badge
│   └── Edit/Delete buttons (if permitted)
├── Account Information
│   ├── Industry
│   ├── Annual Revenue
│   ├── Phone
│   ├── Email
│   ├── Website
│   ├── Address (Street, City, State, ZIP)
│   └── Number of Employees
├── Related Section
│   ├── Associated Contacts (sub-gallery)
│   └── Related Interactions
├── Actions
│   ├── Edit Account
│   ├── New Contact (add related)
│   └── Delete Account (Admin only)
└── Back button
```

#### Screen: Account Form (Create/Edit)
```
Screen: AccountFormScreen
├── Form title (New Account / Edit Account)
├── Form Controls
│   ├── Account Name (required, text)
│   ├── Industry (choice dropdown)
│   ├── Annual Revenue (currency)
│   ├── Phone (phone format)
│   ├── Email (email validation)
│   ├── Website (URL format)
│   ├── Status (choice)
│   ├── Address Section (expandable)
│   │   ├── Street
│   │   ├── City
│   │   ├── State/Province
│   │   ├── ZIP/Postal Code
│   │   └── Country
│   └── Number of Employees (whole number)
├── Validation Messages
│   ├── Required field warnings
│   └── Format error messages
└── Buttons
    ├── Save
    ├── Cancel
    └── Reset (for new)
```

**Validation Rules:**
- Account Name: Required, max 100 chars
- Email: Valid email format
- Phone: Numeric with hyphen separators
- Annual Revenue: Positive currency value

---

### 4. Contacts Module

#### Screen: Contacts List
```
Screen: ContactsListScreen
├── Search/Filter Bar
│   ├── Search by name or email
│   ├── Filter by Account
│   ├── Filter by Status
│   └── Filter by Job Title
├── Gallery Control
│   ├── Full Name
│   ├── Company/Account
│   ├── Job Title
│   ├── Email
│   └── Phone
├── Actions
│   ├── New Contact button
│   └── Item → Detail view
└── Sorting options
```

#### Screen: Contact Detail
```
Screen: ContactDetailScreen
├── Header
│   ├── Full Name
│   ├── Title badge
│   └── Edit/Delete buttons
├── Contact Information
│   ├── Email (clickable)
│   ├── Phone (clickable for dial)
│   ├── Mobile Phone
│   ├── Job Title
│   ├── Department
│   ├── Account (linked)
│   └── Contact Status
├── Account Information
│   └── Link to parent account
├── Communication History
│   └── Recent interactions
└── Actions
    ├── Call contact
    ├── Send email
    ├── Edit contact
    └── Delete contact (if permitted)
```

#### Screen: Contact Form
```
Screen: ContactFormScreen
├── Form title
├── Contact Name Section
│   ├── First Name (required)
│   └── Last Name (required)
├── Contact Information
│   ├── Email (required, validated)
│   ├── Phone
│   ├── Mobile Phone
│   ├── Job Title
│   └── Department
├── Account Lookup
│   ├── Account selection (required)
│   └── Linked account confirmation
├── Status
│   └── Active/Inactive
└── Buttons
    ├── Save
    ├── Cancel
    └── Save & New (for bulk entry)
```

---

### 5. Inventory Module

#### Screen: Inventory Dashboard (Admin Only)
```
Screen: InventoryDashboardScreen
├── KPI Cards
│   ├── Total Items
│   ├── Low Stock Count
│   ├── Total Value
│   └── Items Needing Reorder
├── Low Stock Alert List
│   └── Items below reorder level
├── Top Products by Value
│   └── Chart display
└── Navigation to Details
```

#### Screen: Inventory List
```
Screen: InventoryListScreen
├── Search/Filter Bar
│   ├── Search by product name or code
│   ├── Filter by category
│   ├── Filter by status
│   └── Sort options
├── Gallery Control (Read-only for Users)
│   ├── Product Name
│   ├── Product Code
│   ├── Category
│   ├── Quantity On Hand
│   ├── Status indicator
│   └── Low stock warning (if applicable)
├── Edit Button (Admin only)
└── Item → Detail view
```

**Conditional Formatting:**
- Red background: Quantity < Reorder Level
- Yellow background: Quantity < Reorder Level + 50%

#### Screen: Inventory Detail
```
Screen: InventoryDetailScreen
├── Product Information
│   ├── Product Name
│   ├── Product Code
│   ├── Category
│   ├── Description
│   └── Status
├── Stock Information
│   ├── Quantity On Hand
│   ├── Reorder Level
│   ├── Unit of Measure
│   ├── Unit Price
│   └── Total Value (calculated)
├── Supplier Information
│   ├── Supplier Name
│   ├── Supplier Contact
│   └── Last Restocked date
├── Stock Alert (if applicable)
│   └── "LOW STOCK - Reorder Recommended"
└── Actions
    ├── Edit (Admin only)
    ├── Reorder (Admin only)
    └── Back
```

---

### 6. User Admin Module (Admin Only)

#### Screen: User Directory
```
Screen: UserDirectoryScreen
├── Search Bar
│   └── Search by name, email, department
├── Filter Options
│   ├── Filter by Role
│   ├── Filter by Status
│   └── Filter by Department
├── User Gallery
│   ├── User Name
│   ├── Email
│   ├── Role (Admin/User)
│   ├── Department
│   ├── Status
│   └── Last Login
├── New User button
└── Item → User Detail
```

#### Screen: User Detail
```
Screen: UserDetailScreen
├── User Information
│   ├── User Name
│   ├── Email
│   ├── Full Name
│   ├── Department
│   ├── Manager
│   └── Status
├── Role Information
│   ├── Current Role (badge)
│   ├── Permissions Summary
│   └── Change Role button (Admin only)
├── Activity Information
│   ├── Last Login
│   ├── Login Attempts
│   └── Activity Log link
├── Actions
│   ├── Edit User
│   ├── Change Role
│   ├── Deactivate User
│   └── Reset Password (future)
└── Back button
```

#### Screen: User Form
```
Screen: UserFormScreen
├── User Details Section
│   ├── User Name (required)
│   ├── Email (required, unique validation)
│   ├── Full Name (required)
│   ├── Department (optional)
│   └── Manager lookup (optional)
├── Role Assignment
│   ├── Role selector (Admin/User)
│   ├── Role description
│   └── Permissions preview
├── Account Status
│   └── Active/Inactive toggle
└── Buttons
    ├── Save
    ├── Cancel
    └── Reset Password (edit mode)
```

---

## Common Controls & Patterns

### Search Implementation
```powerapps
Search(
  datasource,
  SearchTerm,
  "Column1", "Column2", "Column3"
)
```

### Filter Implementation
```powerapps
Filter(
  datasource,
  Status = SelectedStatus,
  If(IsBlank(SelectedCategory), true, Category = SelectedCategory)
)
```

### Row-Level Security Check
```powerapps
If(
  User().Email = RecordOwner.Email || 
  LookUp(Users, Email = User().Email, Role) = "Admin",
  true,
  false
)
```

### Edit Permission Check
```powerapps
If(
  LookUp(Users, Email = User().Email, Role) = "Admin" ||
  CurrentRecord.Owner.Email = User().Email,
  false,  // Enable edit
  true    // Disable edit
)
```

---

## Navigation Flow

```
Login/Splash
    ↓
Dashboard (Main)
    ├→ Accounts List → Account Detail → Account Form
    ├→ Contacts List → Contact Detail → Contact Form
    ├→ Inventory List → Inventory Detail (→ Inventory Form - Admin)
    ├→ User Admin (Admin only)
    │   ├→ User Directory → User Detail → User Form
    │   └→ Activity Logs
    └→ Settings (Admin only)
```

