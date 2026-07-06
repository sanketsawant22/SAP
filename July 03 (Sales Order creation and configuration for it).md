# 📘 Day 5 – Sales Order Configuration & Sales Order Creation (VA01)

---

# 🎯 Objective

Before creating a Sales Order, SAP must know:

* Which Sales Organization is selling?
* Which Distribution Channel?
* Which Division?
* Which Sales Document Type is allowed for that Sales Area?
* How pricing should work?
* How Shipping Point should be determined?

All these settings are called **Sales Order Configuration**.

---

# Prerequisites for Creating a Sales Order

Before VA01 works successfully, the following must already exist:

✅ Enterprise Structure

* Company Code
* Sales Organization
* Distribution Channel
* Division
* Plant
* Storage Location
* Shipping Point

✅ Master Data

* Customer Master
* Material Master

✅ Configuration

* Sales Area Assignment
* Sales Document Type Assignment
* Pricing Procedure
* Shipping Point Determination

---

# O2C Process (Revision)

```text
Inquiry (IN)
      │
Quotation (QT)
      │
Sales Order (OR)
      │
Delivery (LF)
      │
Post Goods Issue (PGI)
      │
Billing (F2)
      │
Payment
```

---

# Important Transaction Codes

| Process     | Document | T-Code                |
| ----------- | -------- | --------------------- |
| Inquiry     | IN       | VA11 / VA12 / VA13    |
| Quotation   | QT       | VA21 / VA22 / VA23    |
| Sales Order | OR       | VA01 / VA02 / VA03    |
| Delivery    | LF       | VL01N / VL02N / VL03N |
| Billing     | F2       | VF01                  |

---

# Sales Order (VA01)

### Definition

A **Sales Order** is a legal document created when the customer confirms the purchase of goods or services.

It contains:

* Customer
* Material
* Quantity
* Price
* Delivery Details
* Shipping Details

---

# Sales Document Type

When creating a Sales Order,

SAP first asks

```text
Order Type
```

Example

```text
OR
```

OR = Standard Order

This tells SAP

> "Create a Standard Sales Order."

---

# Configuration Required Before VA01

---

# 1️⃣ Assign Sales Document Type to Sales Area

Path

```text
SPRO

Sales & Distribution

Sales

Sales Documents

Sales Document Header

Assign Sales Area to Sales Document Types
```

T-Code

```text
OVAZ
```

---

### Why is this required?

SAP checks

> Is this Sales Document Type allowed for this Sales Area?

Example

```text
Sales Area

Y452/Y1/Y1

↓

Allowed

OR
```

If assignment is missing

❌ Sales Order cannot be created.

---

# Steps (Configuration)

### 1. Combine Sales Organization

Reference Sales Organization

↓

Our Sales Organization

---

### 2. Combine Distribution Channel

Reference Distribution Channel

↓

Our Distribution Channel

---

### 3. Combine Division

Reference Division

↓

Our Division

---

### 4. Assign Sales Document Type

Assign

```text
Sales Organization

Distribution Channel

Division

↓

Sales Document Type (OR)
```

---

# ⭐ Viva Question

**Why do we assign Sales Document Type to Sales Area?**

Because SAP must know which document types are permitted for a particular Sales Area.

---

# Pricing Procedure Determination

Pricing is **not entered manually**.

SAP determines it automatically.

Configuration

```text
OVKK
```

---

## Pricing depends on

```text
Sales Area
        +
Document Pricing Procedure
        +
Customer Pricing Procedure
        ↓
Pricing Procedure
```

Example

```text
RVAA01
```

---

# ⭐ Where do these fields come from?

| Field                      | Source                                          |
| -------------------------- | ----------------------------------------------- |
| Sales Organization         | Sales Area                                      |
| Distribution Channel       | Sales Area                                      |
| Division                   | Sales Area                                      |
| Document Pricing Procedure | Sales Document Type (VOV8)                      |
| Customer Pricing Procedure | Customer Master → Sales Area Data → Billing Tab |
| Pricing Procedure          | Determined through OVKK                         |

---

## Viva Question

**How is the Pricing Procedure determined?**

Answer:

It is determined using:

* Sales Area
* Document Pricing Procedure
* Customer Pricing Procedure

through configuration **OVKK**.

---

# Sales Order Creation (VA01)

T-Code

```text
VA01
```

Order Type

```text
OR
```

---

## Important Fields

### Sold-to Party

Customer Number

📍 Comes From

> Customer Master

---

### Material

Material Number

📍 Comes From

> Material Master

---

### Quantity

Entered manually

---

# Shipping Tab (Very Important)

One of the favorite SAP interview topics.

Here SAP automatically determines

```text
Shipping Point
```

---

# Shipping Point Determination

Configuration

```text
OVL2
```

SAP determines Shipping Point using

```text
Shipping Condition
        +
Loading Group
        +
Plant
        ↓
Shipping Point
```

---

# ⭐ Where do these values come from?

| Field              | Source                                                                                     |
| ------------------ | ------------------------------------------------------------------------------------------ |
| Shipping Condition | Customer Master (Sales Area → Shipping Tab)                                                |
| Loading Group      | Material Master (Sales: General/Plant Data)                                                |
| Plant              | Material Master / Customer-Material Determination / Sales Order (depends on configuration) |
| Shipping Point     | Automatically determined through OVL2                                                      |

---

## Viva Question

**How is Shipping Point determined?**

Answer

Shipping Point is determined using:

* Shipping Condition
* Loading Group
* Plant

through configuration **OVL2**.

---

## If Shipping Point is not determined

Check

✅ Shipping Condition

✅ Loading Group

✅ Plant

✅ OVL2 configuration

---

# Stock Check

Before Delivery,

SAP checks stock.

T-Code

```text
MMBE
```

---

## Stock Types

| Stock Type         | Meaning             |
| ------------------ | ------------------- |
| Unrestricted       | Available for sale  |
| Quality Inspection | Under quality check |
| Blocked            | Cannot be sold      |

---

# If Stock is Zero

MM users perform Goods Receipt.

T-Code

```text
MIGO
```

Movement

```text
A01

Goods Receipt

Other

501
```

Enter

* Material
* Quantity
* Plant
* Storage Location

Save

Stock becomes available.

---

# ⭐ Viva Question

**Which stock is used for Sales Orders?**

Answer

Only **Unrestricted Stock**.

---

# Saving Sales Order

After entering all details

SAP checks

```text
Incompletion Log
```

If mandatory fields are missing

❌ Order cannot proceed.

If no errors

Save

SAP generates

```text
Sales Order Number
```

---

# Display / Change Order

| Activity | T-Code |
| -------- | ------ |
| Change   | VA02   |
| Display  | VA03   |

---

# Delivery Creation

T-Code

```text
VL01N
```

Enter

* Shipping Point
* Sales Order Number

SAP creates

```text
Delivery Document
```

---

# Picking

Enter

```text
Picked Quantity

=

Delivery Quantity
```

Storage Location

Same as material stock location.

---

# PGI (Post Goods Issue)

Click

```text
Post Goods Issue
```

---

## Effects of PGI

* Stock decreases
* Inventory updates
* Material Document created
* Accounting Document generated (integration with FI)
* Delivery status updated

---

# Display Delivery

```text
VL03N
```

---

# Billing

T-Code

```text
VF01
```

Input

```text
Delivery Number
```

Execute

SAP creates Invoice.

Billing Type

```text
F2
```

(Standard Invoice)

---

# Common Error

### Billing Error

Missing

```text
HSN Code
```

Reason

Material Master is incomplete.

HSN information is maintained in the Material Master (or relevant tax/classification configuration depending on system setup).

Without it

Billing cannot be completed.

---

# Complete Flow

```text
Customer Master
        │
        ▼
Sold-to Party
        │
Material Master
        │
        ▼
Material
        │
        ▼
Sales Order (VA01)
        │
        ▼
Shipping Point Determination
(Shipping Condition + Loading Group + Plant)
        │
        ▼
Delivery (VL01N)
        │
        ▼
Picking
        │
        ▼
PGI
        │
        ▼
Billing (VF01)
```

---

# ⭐ Exam & Viva Questions

### 1. What are the prerequisites for creating a Sales Order?

* Enterprise Structure
* Customer Master
* Material Master
* Sales Area Assignment
* Sales Document Type Assignment
* Pricing Procedure Configuration
* Shipping Point Determination

---

### 2. Where does the Sold-to Party come from?

👉 Customer Master.

---

### 3. Where does the Material come from?

👉 Material Master.

---

### 4. How is the Pricing Procedure determined?

👉 **Sales Area + Document Pricing Procedure + Customer Pricing Procedure** (configured in **OVKK**).

---

### 5. How is the Shipping Point determined?

👉 **Shipping Condition (Customer Master) + Loading Group (Material Master) + Plant** (configured in **OVL2**).

---

### 6. Which stock is considered during delivery?

👉 **Unrestricted Stock**.

---

### 7. What is the purpose of PGI?

👉 PGI confirms that goods have physically left the warehouse. It reduces stock, updates inventory, creates the material document (and accounting entries through integration), and allows the process to continue toward billing.

---

## 🧠 Memory Map

```text
Customer Master
   ├── Sold-to Party
   ├── Shipping Condition
   └── Customer Pricing Procedure

Material Master
   ├── Material Number
   ├── Loading Group
   ├── Item Category Group
   ├── Plant
   └── HSN / Tax Data

Sales Document Type (VOV8)
   └── Document Pricing Procedure

OVKK
   └── Determines Pricing Procedure

OVL2
   └── Determines Shipping Point

VA01
   └── Creates Sales Order

VL01N
   └── Creates Delivery

PGI
   └── Dispatches Goods

VF01
   └── Creates Billing
```
