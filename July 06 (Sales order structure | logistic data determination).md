# 📘 Day 6 – Sales Order Structure | Schedule Line | Logistics Determination | Delivery Scheduling

---

# 🏗️ Structure of Sales Document

Every Sales Document (VA01) consists of **3 levels of data**.

```text
Sales Order
     │
     ├── Header Data
     │
     ├── Item Data
     │
     └── Schedule Line Data
```

Each level has different responsibilities and is controlled by different configurations.

---

# 1️⃣ Header Data

### Definition

Header Data contains information applicable to the **entire Sales Document**.

If the value changes at the header level, it affects all items (unless overridden at item level).

### Controlled By

**Sales Document Type (VOV8)**

### Examples

* Sales Document Number
* Sales Document Type (OR, IN, QT)
* Sales Organization
* Distribution Channel
* Division
* Sold-to Party
* Document Date
* Overall Delivery Status
* Overall Billing Status
* Overall Pricing Procedure
* Currency
* Payment Terms (default from Customer Master)

### 🧠 Remember

> **Header Data = Common information for the whole order**

---

# 2️⃣ Item Data

### Definition

Item Data contains information specific to **each material (line item)**.

Every material in the Sales Order has its own Item Data.

### Controlled By

**Item Category (VOV7)**

### Examples

* Material Number
* Order Quantity
* Plant
* Storage Location
* Shipping Point
* Route
* Net Price
* Item Category (TAN, REN, AFN...)
* Delivery Priority
* Material Description

### 🧠 Remember

> **Item Data = Information related to one particular material**

---

# 3️⃣ Schedule Line Data

### Definition

Schedule Line Data contains **delivery scheduling and stock-related information** for each item.

It tells SAP **when and how the material will be delivered**.

### Controlled By

**Schedule Line Category (VOV6)**

### Examples

* Schedule Line Category
* Requested Delivery Date
* Confirmed Quantity
* Confirmed Delivery Date
* Material Availability Date (MAD)
* Movement Type
* Delivery Block
* Availability Check

### 🧠 Remember

> **Schedule Line = Quantity + Date + Stock + Delivery**

---

# Relationship

```text
Sales Document Type
        │
        ▼
Controls Header

Item Category
        │
        ▼
Controls Item

Schedule Line Category
        │
        ▼
Controls Delivery Scheduling
```

---

# Schedule Line Category

### Definition

A Schedule Line Category controls how SAP handles:

* Stock
* Delivery
* Availability Check
* Movement Type
* Delivery Block

Configuration

```
VOV6
```

---

## Important Rule ⭐

Schedule Line is created **only if the Item Category allows Schedule Lines.**

If Item Category has

```
Schedule Line Allowed = No
```

↓

No Schedule Line

↓

No Delivery

---

# Standard Schedule Line Categories

| Sales Process  | Schedule Line |
| -------------- | ------------- |
| Inquiry        | AT            |
| Quotation      | BN            |
| Standard Order | CP / CN       |
| Return Order   | DN            |
| Rush Order     | CP / CN       |
| Cash Sales     | CP / CN       |

---

# What does Schedule Line Category control?

## 1️⃣ Delivery Block

Suppose

Schedule Line Category CP

↓

Delivery Block = 01

Whenever Sales Order is created

↓

Delivery becomes blocked.

Sales Order

↓

Schedule Line

↓

Delivery Block

↓

VL01N

↓

❌ Delivery Not Allowed

---

### Removing Block

Inside Sales Order

Schedule Line Tab

↓

Delivery Block

↓

Blank

This removes block **only for current Sales Order.**

Configuration remains unchanged.

---

## 2️⃣ Item Relevant for Delivery

Inside VOV6

```
Item Relevant for Delivery
```

If unchecked

↓

Sales Order created successfully

↓

VL01N

↓

"No Item Available for Delivery"

---

### Example

CP

✔ Delivery Relevant

AT

❌ Delivery Relevant

Reason

Inquiry is only asking.

No goods should move.

---

## 3️⃣ Availability Check

If enabled

SAP checks

```text
Required Quantity

≤

Available Stock ?
```

If stock exists

↓

Confirmed Quantity

Otherwise

↓

Partial confirmation / Backorder

---

## 4️⃣ Movement Type

### Definition

Movement Type represents the **physical or logical movement of stock**.

Whenever stock increases or decreases,

SAP uses a Movement Type.

---

### Standard Movement Types

| Process      | Movement Type |
| ------------ | ------------- |
| PGI          | 601           |
| Return Order | 651           |

Movement Type is maintained in

```
VOV6
```

---

## Schedule Line Determination ⭐⭐⭐

Interview favorite.

SAP does NOT ask

"What Schedule Line Category?"

It determines automatically.

Logic

```text
Item Category
        +
MRP Type
        ↓
Schedule Line Category
```

Configuration

```
VOV5
```

---

### Where do these values come from?

| Field                  | Source                      |
| ---------------------- | --------------------------- |
| Item Category          | Item Category Determination |
| MRP Type               | Material Master → MRP1 View |
| Schedule Line Category | Determined using VOV5       |

---

Example

```
Item Category

TAN

+

MRP Type

ND

↓

CP
```

---

# Logistics Data Determination

Logistics means

Managing movement of goods.

Important Logistics Data

* Plant
* Shipping Point
* Route

---

# Shipping Point Determination ⭐⭐⭐

Configuration

```
OVL2
```

SAP determines Shipping Point automatically.

Logic

```text
Shipping Condition
        +
Loading Group
        +
Delivering Plant
        ↓
Shipping Point
```

---

### Where do these values come from?

| Field              | Source                                      |
| ------------------ | ------------------------------------------- |
| Shipping Condition | Customer Master → Shipping Tab              |
| Loading Group      | Material Master → Sales: General/Plant Data |
| Delivering Plant   | Plant Determination                         |
| Shipping Point     | OVL2                                        |

---

# Route Determination ⭐⭐⭐

### Definition

Route represents the transportation path used to deliver goods.

Examples

* Pune → Mumbai
* Air Cargo
* Road Transport

---

## Route in Sales Order

Determined using

* Departure Country
* Departure Transportation Zone
* Destination Country
* Destination Transportation Zone
* Shipping Condition
* Transportation Group

---

### Source

| Field                           | Source          |
| ------------------------------- | --------------- |
| Departure Country               | Shipping Point  |
| Departure Transportation Zone   | Shipping Point  |
| Destination Country             | Customer Master |
| Destination Transportation Zone | Customer Master |
| Shipping Condition              | Customer Master |
| Transportation Group            | Material Master |

---

## Route in Delivery

Delivery uses one extra factor.

Previous six

*

Weight Group

Weight Group comes from

Material Master

---

## Viva Question

Difference between Route Determination in Sales Order and Delivery?

**Answer**

Delivery additionally considers **Weight Group**.

---

# CMIR (Customer Material Info Record)

Transaction Code

```
VD51
```

---

## Definition

CMIR maps

Customer Material Number

↓

Our Material Number

Purpose

Customer may identify the same material using their own internal code.

SAP translates it automatically.

---

Example

```
Customer

Material

ABC123

↓

Our Material

10000045
```

Now user can enter

```
ABC123
```

SAP automatically selects

```
10000045
```

---

# Plant Determination ⭐⭐⭐

Interview favorite.

SAP determines Plant using

```text
CMIR
      ↓
Customer Master
      ↓
Material Master
      ↓
Manual Entry
```

Priority

1. CMIR
2. Customer Master (Shipping View)
3. Material Master (Sales Org 1)
4. Manual Entry

---

### Where does Plant come from?

| Priority | Source          |
| -------- | --------------- |
| 1        | CMIR            |
| 2        | Customer Master |
| 3        | Material Master |
| 4        | Manual          |

---

# Delivery Scheduling

### Definition

Delivery Scheduling is the process by which SAP automatically calculates all important delivery dates during Sales Order creation.

These dates appear in the **Schedule Line Tab**.

---

# Important Dates

## Requested Delivery Date (RDD)

Customer wants goods on this date.

Entered during Sales Order creation.

---

## Sales Order Preparation Date

Date by which Sales Order processing should begin.

Calculated by SAP.

---

## PGI Date

Goods leave warehouse.

Effects

* Stock reduced
* Accounting updated
* Ownership transferred

---

## Transit Time

Time required for goods to reach customer.

---

## Packing Time

Time required for packing goods.

---

## Picking Time

Time required to pick goods from storage.

---

## Loading Time

Time required to load goods onto vehicle.

---

## Transportation Lead Time

Time required to arrange transport.

Example

* Truck booking
* Transporter assignment

---

## Material Availability Date (MAD)

Date by which material must be available in stock.

ATP (Availability Check) happens on this date.

---

## Replenishment Lead Time

Time required to procure or manufacture material if stock is unavailable.

---

# Backward Scheduling ⭐⭐⭐

Used when SAP tries to meet the customer's Requested Delivery Date.

SAP starts from RDD and moves backward.

```text
Requested Delivery Date
        ↑
Transit Time
        ↑
PGI Date
        ↑
Loading Time
        ↑
Packing Time
        ↑
Picking Time
        ↑
Transportation Lead Time
        ↑
Material Availability Date
```

If MAD is **today or later**, SAP can meet the requested date.

---

# Forward Scheduling ⭐⭐⭐

Used when Backward Scheduling fails.

If calculated MAD falls **before today's date**, SAP knows it is impossible to meet the requested delivery date.

SAP starts from **Today's Date**.

```text
Today's Date
      ↓
Replenishment Lead Time
      ↓
Material Availability Date
      ↓
Transportation Lead Time
      ↓
Picking Time
      ↓
Packing Time
      ↓
Loading Time
      ↓
PGI Date
      ↓
Transit Time
      ↓
Confirmed Delivery Date
```

Confirmed Delivery Date is usually **later than Requested Delivery Date**.

---

# Forward vs Backward Scheduling

| Backward Scheduling                 | Forward Scheduling                       |
| ----------------------------------- | ---------------------------------------- |
| Starts from Requested Delivery Date | Starts from Today's Date                 |
| Tries to meet customer request      | Calculates earliest possible delivery    |
| Uses reverse calculation            | Uses forward calculation                 |
| Preferred method                    | Used only when backward scheduling fails |

---

# ⭐ Complete Determination Flow (Very Important)

```text
Customer Master
   ├── Shipping Condition
   ├── Customer Pricing Procedure
   ├── Destination Country
   └── Transportation Zone

Material Master
   ├── Item Category Group
   ├── Loading Group
   ├── Transportation Group
   ├── MRP Type
   ├── Weight Group
   └── Plant (fallback)

VOV8
   └── Sales Document Type

VOV7
   └── Item Category

VOV5
   └── Schedule Line Determination

VOV6
   └── Schedule Line Category

OVKK
   └── Pricing Procedure

OVL2
   └── Shipping Point

VA01
   └── Sales Order
```

# 🎯 Viva & Interview Questions

### 1. What are the three levels of a Sales Document?

* Header Data
* Item Data
* Schedule Line Data

### 2. Which configuration controls each level?

* Header → **Sales Document Type (VOV8)**
* Item → **Item Category (VOV7)**
* Schedule Line → **Schedule Line Category (VOV6)**

### 3. How is the Schedule Line Category determined?

**Item Category + MRP Type → Schedule Line Category (VOV5)**

### 4. How is the Shipping Point determined?

**Shipping Condition + Loading Group + Delivering Plant → Shipping Point (OVL2)**

### 5. How is the Plant determined?

**CMIR → Customer Master → Material Master → Manual Entry**

### 6. Difference between Backward and Forward Scheduling?

* **Backward Scheduling** tries to meet the customer's requested delivery date by calculating dates backward.
* **Forward Scheduling** starts from today's date and calculates the earliest possible confirmed delivery date when the requested date cannot be met.
