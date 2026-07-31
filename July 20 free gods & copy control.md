
# 🎁 FREE GOODS

Free Goods allow customers to receive products free of charge based on predefined business rules.

There are **2 types** of Free Goods:

* ✅ Inclusive Free Goods
* ✅ Exclusive Free Goods

---

# ✅ 1. Inclusive Free Goods

The free quantity is **included within the ordered quantity**.

### Example

Offer:

> Buy **10** units, Pay for **8** units.

### SAP Behavior

* ✅ No extra item is generated.
* ✅ Billing is done only for the chargeable quantity.
* ✅ Free quantity is adjusted within the ordered quantity.

---

# ✅ 2. Exclusive Free Goods

The free quantity is **given in addition** to the ordered quantity.

### Example

Offer:

> Buy **10** units, Get **2** units free.

### SAP Behavior

* ✅ SAP creates a separate free goods item.
* ✅ Free item has **₹0 value**.
* ✅ Customer receives extra quantity.

---

# ⚙️ Configuration of Free Goods

Free Goods also uses the **Condition Technique**.

---

## 1️⃣ Condition Tables

Defines **which fields SAP uses** to search for Free Goods records.

### Examples

* Sales Organization
* Distribution Channel
* Customer
* Material

### Purpose

🎯 Decide the key combination.

---

## 2️⃣ Access Sequence

Collection of condition tables searched in sequence.

SAP checks one table after another until it finds a valid record.

### Purpose

🎯 Tell SAP where to search first, second, third, etc.

---

## 3️⃣ Condition Type (Free Goods Type)

Defines the Free Goods condition.

It controls:

* Free Goods determination
* Links Access Sequence
* Used inside Free Goods Procedure

### Purpose

🎯 Identify the Free Goods condition.

---

## 4️⃣ Free Goods Procedure

Collection of one or more Free Goods condition types.

Controls:

* Inclusive Free Goods
* Exclusive Free Goods

### Purpose

🎯 Defines overall Free Goods rules.

---

## 5️⃣ Free Goods Procedure Determination

Determines which Free Goods Procedure should be used.

Uses combinations such as:

* Sales Area
* Customer Pricing Procedure
* Document Pricing Procedure

(Works similar to Pricing Procedure Determination.)

### Purpose

🎯 Automatically assigns the correct Free Goods Procedure during Sales Order creation.

---

# 📝 Free Goods Condition Records (VBN1)

**T-Code:** `VBN1`

Used to maintain Free Goods master data after configuration.

Here we maintain:

* Material
* Minimum Quantity
* Free Quantity
* Calculation Rule
* Free Goods Category

---

# 📦 Item Category Determination (Free Goods)

| Field                      | Value |
| -------------------------- | ----- |
| Sales Document             | OR    |
| Item Category Group        | NORM  |
| Usage                      | FREE  |
| Higher Level Item Category | TAN   |
| Item Category              | TANN  |

---

# 🔍 TANN Configuration (VOV7)

For Item Category **TANN**

### Pricing

```
B
(Pricing for Free Goods - 100% Discount)
```

### Billing Relevance

```
Blank
```

(No Billing)

---

# 💰 Pricing Procedure Configuration

To achieve **100% discount** for Free Goods:

Maintain **R100** in Pricing Procedure (`V/08`)

| Field                       | Value |
| --------------------------- | ----- |
| Condition Type              | R100  |
| Requirement                 | 55    |
| Alt. Calculation Base Value | 28    |

---

# 📐 Free Goods Calculation Rules

Example Offer:

> Buy **100**, Get **20** Free

---

## ✅ 1. Pro Rate

Free quantity is calculated proportionally.

| Ordered | Chargeable | Free |
| ------- | ---------- | ---- |
| 100     | 80         | 20   |
| 162     | 130        | 32   |
| 200     | 160        | 40   |

✔ Free quantity is proportional.

---

## ✅ 2. Unit Reference

Free goods apply only when each milestone is reached.

| Ordered | Chargeable | Free |
| ------- | ---------- | ---- |
| 100     | 80         | 20   |
| 162     | 142        | 20   |
| 200     | 160        | 40   |

✔ Only completed blocks of 100 qualify.

---

## ✅ 3. Whole Units

Order must exactly satisfy the condition.

| Ordered | Chargeable | Free |
| ------- | ---------- | ---- |
| 100     | 80         | 20   |
| 162     | 162        | 0    |
| 200     | 160        | 40   |

✔ If the complete quantity is not fulfilled, no Free Goods are given.

---

# 🛠️ Configuration Exercise

---

## 📌 Step 1 – Create Material

Create Material Master as usual.

---

## 📌 Step 2 – Create Access Sequence

```
SPRO
→ Sales and Distribution
→ Basic Functions
→ Free Goods
→ Condition Technique
→ Maintain Access Sequences
```

### Steps

* New Entries
* Create Access Sequence
* Enter ID & Description
* Select Access Sequence
* Double-click **Accesses**
* Add Condition Tables

Example

```
No. 10
Table 010
```

* Select the checkbox
* Select Fields
* Enter → Enter → Enter
* Save

---

## 📌 Step 3 – Create Condition Type

```
Maintain Condition Types
```

* New Entry
* Enter ID
* Description
* Assign Access Sequence
* Save

---

## 📌 Step 4 – Create Free Goods Procedure

```
Maintain Free Goods Procedure
```

* New Entry
* Name
* Description

Select Procedure

Control Data

New Entry

```
Step : 10
Condition Type : (Created Condition Type)
```

Save

---

## 📌 Step 5 – Activate Free Goods Determination

Maintain:

* Sales Org
* Distribution Channel
* Division
* Document Pricing Procedure = A
* Customer Pricing Procedure = 1

Assign the Free Goods Procedure.

Save.

---

## 📌 Step 6 – Item Category Determination

```
VOV4
```

No configuration required.

Just verify how:

```
OR
+
NORM
+
FREE
+
TAN
↓

TANN
```

is determined.

---

## 📌 Step 7 – Pricing Procedure

```
V/08
```

Open your Pricing Procedure.

Control Data

Create new entry:

| Field                 | Value |
| --------------------- | ----- |
| Step                  | 399   |
| Condition Type        | R100  |
| Requirement           | 55    |
| Alt. Cond. Base Value | 28    |
| Account Key           | ERS   |

Save.

---

## 📌 Step 8 – Create Free Goods Condition Record

```
VBN1
```

Enter:

* Condition Type (Example: ZZ21)
* Material
* Minimum Quantity
* Free Quantity

Example

```
Buy : 100

Get : 20 Free
```

Maintain

```
Calculation Rule : 1

Free Goods Category : 1
```

Save.

---

## 📌 Step 9 – Create Sales Order

```
VA01

Order Type : OR
```

Create Sales Order normally.

SAP automatically determines Free Goods.

---

# 🚫 Free Goods Without Item Generation (Inclusive)

Example

```
Buy 10

Get 1 Free
```

### Normal Behavior

SAP creates:

```
Main Item : 9

Sub Item : 1 (Free)
```

Customer pays for **9 items**.

---

## Business Requirement

Business does **not** want separate Free Goods item.

Sales Order should display only:

```
10 Qty
```

instead of

```
9 + 1
```

---

### SAP Solution

Instead of maintaining:

```
R100
```

Maintain:

```
NRAB
```

inside Pricing Procedure (`V/08`).

Then in **VBN1**

```
Free Goods Category = 3
```

---

### Result

No extra Free Goods item is generated.

Instead,

SAP adjusts the unit price.

### Example

Original Price

```
₹10 × 10 = ₹100
```

Offer

```
Buy 10
Pay for 9
```

SAP changes unit price to:

```
₹9 × 10 = ₹90
```

Only **one item** appears in the Sales Order.

---

---

# 📄 COPYING CONTROL

Copy Control defines **how data is copied from one document to another** in SAP SD.

---

## Example Flow

### Inquiry

```
VA11

Type : IN
```

Enter

* Customer
* Validity
* Material

System determines

```
Item Category : AFN

Schedule Line : AT
```

Save.

---

### Create Quotation

```
VA21

Type : QT
```

Choose

```
Create With Reference
```

Enter Inquiry Number.

SAP copies:

* Customer
* Material
* Quantities
* Other relevant data

Save.

---

### Create Sales Order

```
VA01

Create With Reference
```

Reference can be:

* Inquiry
* Quotation

Again SAP copies all relevant information.

---

# 💡 What Makes This Possible?

✔ **Copy Control**

It controls:

* Which data is copied
* Which data is not copied
* What validations should happen

---

# 📂 Types of Copy Control

## 1️⃣ Sales → Delivery

Examples

```
OR → LF

KB → LF

RO → LF
```

---

## 2️⃣ Delivery → Billing

Examples

```
LF → F2

LO → F2

LF → IV
```

---

## 3️⃣ Sales → Billing

Examples

```
OR → F5

CR → G2

DR → L2

BV → BV

RE → RE

KR → RE
```

---

## 4️⃣ Sales → Sales

Examples

```
IN → QT

IN → OR

QT → OR

QC → OR

OR → RE

RE → SFD
```

---

## 5️⃣ Billing → Sales

Examples

```
F2 → RE

F2 → RK
```

---

## 6️⃣ Billing → Billing

Examples

```
F2 → S1

G2 → S2
```

---

# 🧭 Copy Control T-Codes

The first two letters are always:

```
VT
```

The **3rd letter = Target Document**

The **4th letter = Source Document**

| Document | Letter |
| -------- | ------ |
| Sales    | A      |
| Delivery | L      |
| Billing  | F      |

---

## 📌 Sales → Delivery

```
VTLA
```

Target = Delivery (L)

Source = Sales (A)

---

## 📌 Delivery → Billing

```
VTFL
```

Target = Billing (F)

Source = Delivery (L)

---

## 📌 Sales → Billing

```
VTFA
```

Target = Billing (F)

Source = Sales (A)

---

## 📌 Sales → Sales

```
VTAA
```

Target = Sales (A)

Source = Sales (A)

---

## 📌 Billing → Sales

```
VTAF
```

Target = Sales (A)

Source = Billing (F)

---

## 📌 Billing → Billing

```
VTFF
```

Target = Billing (F)

Source = Billing (F)

---

# 🔎 Example

Open

```
VTAA
```

Target

```
OR
```

Source

```
QT
```

You can see how SAP copies data from Quotation to Sales Order.

---

# ⭐ Important Features of Copy Control

## ✅ 1. Copying Requirements

Determines **whether copying should happen**.

SAP checks predefined routines before allowing the copy process.

---

## ✅ 2. Data Transfer Routines

Determines **which data is copied and how it is copied** from the source document to the target document.

---

## ✅ 3. Item/Data Mapping

Controls how header, item, partner, schedule line, pricing, and other document data are mapped from the source to the target document.
