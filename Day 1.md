# Day 1 Notes - SAP & ERP Structure (SAP SD)

# 1. What is SAP?

SAP is an **ERP (Enterprise Resource Planning)** software used to manage all business processes from a single system.

Instead of separate systems or Excel files for different departments, SAP integrates everything into one database.

**Departments using SAP:**

* Sales (SD)
* Purchasing (MM)
* Finance (FI)
* Controlling (CO)
* Production (PP)
* Warehouse (WM/EWM)
* Human Resources (HCM)

**Benefit:**

* Single source of data
* Real-time information
* No duplicate records
* Better coordination between departments

---

# 2. What is ERP?

**ERP (Enterprise Resource Planning)** is software that integrates and manages all business functions of an organization in one system.

**Examples of ERP Software**

* SAP
* Oracle ERP
* Microsoft Dynamics
* Infor

SAP is one of the most widely used ERP systems in the world.

---

# 3. SAP SD (Sales & Distribution)

SAP SD manages the complete sales process.

### Order-to-Cash (O2C) Process

```
Customer
    ↓
Sales Order
    ↓
Delivery
    ↓
Post Goods Issue (PGI)
    ↓
Billing (Invoice)
    ↓
Payment
```

SAP SD handles:

* Customer Management
* Sales Orders
* Delivery
* Shipping
* Billing
* Sales Reporting

---

# 4. SAP GUI

SAP runs on a central server.

Users access SAP through **SAP GUI (Graphical User Interface)**.

```
User
   ↓
SAP GUI
   ↓
SAP Server
```

SAP GUI is simply the interface used to work with SAP.

---

# 5. Enterprise Structure

Enterprise Structure defines the organizational hierarchy of a company in SAP.

It tells SAP:

* Who is selling?
* How products are sold?
* What products are sold?
* From where products are shipped?

Enterprise Structure must be created before creating customers or sales orders.

---

# 6. Organizational Units

## Company Code

Represents a **legal entity** for which financial statements are maintained.

**Purpose**

* Financial Accounting
* Balance Sheet
* Profit & Loss Statement

**Remember**

> Company Code = Legal Company

---

## Sales Organization

Responsible for all sales activities.

Handles:

* Sales Orders
* Pricing
* Customer Sales
* Revenue

**Remember**

> Sales Organization = Who sells?

---

## Distribution Channel

Defines how products reach customers.

Examples:

* Retail
* Wholesale
* Online
* Dealer Network

**Remember**

> Distribution Channel = How products are sold?

---

## Division

Represents a product line.

Examples:

* Electronics
* Furniture
* Mobile
* Clothing

**Remember**

> Division = What is sold?

---

# 7. Sales Area (Very Important)

Sales Area is a combination of:

```
Sales Organization
        +
Distribution Channel
        +
Division
        =
Sales Area
```

Example:

```
Sales Organization : Y452
Distribution Channel : Y1
Division : Y1

Sales Area = Y452 / Y1 / Y1
```

Sales Area is mandatory while creating:

* Customer Master
* Sales Order
* Delivery
* Billing

---

# 8. Sales Office

Represents a regional sales office.

Examples:

* Pune
* Mumbai
* Delhi

**Remember**

> Sales Office = Where is the sales team located?

---

# 9. Sales Group

Represents a sales team within a Sales Office.

Example:

```
Sales Office
     ↓
Sales Group A
Sales Group B
Sales Group C
```

**Remember**

> Sales Group = Which sales team sells?

---

# 10. Shipping Point

Location from where goods are dispatched to customers.

Example:

* Warehouse
* Loading Dock
* Dispatch Center

**Remember**

> Shipping Point = From where goods are shipped?

---

# 11. Enterprise Structure Hierarchy

```
Company Code
      │
      ▼
Sales Organization
      │
      ├──────────────┐
      ▼              ▼
Distribution      Division
Channel
      │              │
      └──────┬───────┘
             ▼
        Sales Area
             │
             ▼
       Sales Office
             │
             ▼
       Sales Group
             │
             ▼
      Shipping Point
```

---

# 12. Why Enterprise Structure Comes First?

Enterprise Structure is the foundation of SAP.

Without it, SAP cannot determine:

* Which company is selling?
* Which sales organization is responsible?
* Which distribution channel?
* Which division?

Therefore, before creating:

* Customer Master
* Material Master
* Sales Orders

Enterprise Structure must already exist.

---

# 13. Memory Tricks

| Question                       | SAP Object           |
| ------------------------------ | -------------------- |
| Who legally owns the business? | Company Code         |
| Who sells?                     | Sales Organization   |
| How are products sold?         | Distribution Channel |
| What products are sold?        | Division             |
| Where is the sales office?     | Sales Office         |
| Which sales team sells?        | Sales Group          |
| From where are goods shipped?  | Shipping Point       |

---

# 14. Exam Questions

### What is ERP?

ERP is software that integrates all business processes such as Sales, Finance, Purchasing, Production, and HR into a single system.

---

### What is SAP?

SAP is one of the world's leading ERP software solutions used to manage an organization's end-to-end business processes.

---

### What is a Sales Area?

A Sales Area is a combination of:

* Sales Organization
* Distribution Channel
* Division

It uniquely identifies the sales unit responsible for processing sales transactions such as customer creation, sales orders, deliveries, and billing.

---

# 15. Quick Revision

```
SAP
│
├── ERP Software
│
├── Modules
│     ├── SD
│     ├── MM
│     ├── FI
│     ├── CO
│     ├── PP
│     ├── WM/EWM
│     └── HCM
│
└── Enterprise Structure
      │
      ├── Company Code
      ├── Sales Organization
      ├── Distribution Channel
      ├── Division
      ├── Sales Area
      ├── Sales Office
      ├── Sales Group
      └── Shipping Point
```

---

## ⭐ Exam Takeaways

* SAP is an ERP software.
* SAP SD manages the complete **Order-to-Cash (O2C)** process.
* Enterprise Structure must be defined before creating master data or transactions.
* **Sales Area = Sales Organization + Distribution Channel + Division** (one of the most frequently asked interview questions).
* Company Code is the legal and financial organizational unit.
* Shipping Point is the location from which goods are dispatched to customers.
