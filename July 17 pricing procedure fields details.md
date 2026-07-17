# 💰 SAP SD Pricing Procedure

---

# 🎯 Pricing Procedure

A **Pricing Procedure** defines **how the SAP system calculates the final price** of a sales order.

It consists of **18 fields (columns)**, where each row represents a **Condition Type**.

---

# 📋 18 Fields of Pricing Procedure

---

## 1️⃣ Step

* Represents the **sequence number** of the condition type.
* SAP processes conditions from **top to bottom** according to the step number.

**Example**

| Step | Condition Type |
| ---- | -------------- |
| 10   | PR00           |
| 20   | K004           |
| 30   | KF00           |

---

## 2️⃣ Counter

Used when **multiple condition types** exist in the same step.

Example

| Step | Counter | Condition Type |
| ---- | ------- | -------------- |
| 20   | 1       | K004           |
| 20   | 2       | K005           |

Without Counter, SAP cannot differentiate multiple conditions in one step.

---

## 3️⃣ Condition Type

Specifies **which pricing element** is being used.

Examples

* PR00 → Base Price
* K004 → Material Discount
* K007 → Customer Discount
* KF00 → Freight
* MWST → Tax

---

## 4️⃣ Description

Displays the description of the condition type.

Example

| Condition Type | Description       |
| -------------- | ----------------- |
| PR00           | Base Price        |
| K004           | Material Discount |

---

## 5️⃣ From & To ⭐ (Very Important)

These fields specify **which step(s)** should be used as the **base for calculation**.

---

### Case 1️⃣ No From/To maintained

SAP applies the percentage on the **current calculated value**.

Example

```
PR00 = 1000

K004 = 10%

Discount = 100
Net = 900
```

---

### Case 2️⃣ Only "From" maintained ⭐

Suppose

```
PR00 → Step 11
K007 → Step 30
From = 11
```

Now SAP calculates K007 **only on Step 11's value**, regardless of later calculations.

This is mostly used for **percentage-based conditions**.

> ✅ **Important:** Usually only the **From** field is maintained for percentage calculations.

---

## 6️⃣ Manual

If checked,

SAP **does not determine this condition automatically**.

The user must enter it manually in the Sales Order.

Example

```
KF00 (Freight)

If Manual ✔

→ Freight will not appear automatically.
→ User must enter it manually.
```

---

## 7️⃣ Required

If checked,

This condition becomes **mandatory**.

If SAP cannot find its condition record,

➡️ Sales Order cannot be saved.

Example

```
Condition Type : K005

No Condition Record found

Error

Mandatory condition K005 missing.
```

---

## 8️⃣ Statistical ⭐

If checked,

SAP **calculates** this condition,

✅ Displays it in pricing,

❌ Does NOT include it in Net Value.

Used only for reporting or analysis.

### Example

```
Base Price = 1000

Profit Margin = 150 (Statistical)

Net Price remains = 1000
```

Common examples

* EK01
* VPRS

---

## 9️⃣ Relevant for Account Determination

Determines whether the condition participates in **account determination**.

> 📌 Not commonly asked in SD configuration. Basic understanding is sufficient.

---

## 🔟 Print

Determines whether this condition should be printed on the invoice.

Example

✔ Print

Invoice shows

```
Base Price
Discount
Freight
Tax
```

❌ Not Printed

Customer never sees that condition.

---

## 1️⃣1️⃣ Subtotal

Stores intermediate values during pricing.

These subtotal values can later be used by other condition types.

Example

```
Subtotal A

↓

Freight calculated

↓

Tax calculated
```

---

## 1️⃣2️⃣ Requirement ⭐

Acts like a **checkpoint**.

Condition type is executed **only if the requirement is satisfied**.

Example

Requirement = **2 (Pricing)**

Now,

Suppose in **VOV7**

```
Pricing = None
```

Then SAP **does not execute pricing**.

No pricing condition types appear in the Sales Order.

---

## 1️⃣3️⃣ Alternative Calculation Type

Used when standard SAP calculation is insufficient.

SAP uses a custom formula (VOFM routine) to calculate the value.

Example

```
Instead of

10% Discount

Business wants

Fixed Formula

↓

Alternative Calculation Type
```

---

## 1️⃣4️⃣ Alternative Condition Base Value

Changes the **base amount** on which a condition is calculated.

Instead of calculating on Net Value,

Business may calculate on

* Gross Value
* Quantity
* Weight
* Volume

etc.

---

## 1️⃣5️⃣ Account Key ⭐⭐⭐ (Very Important)

Used during **Revenue Account Determination**.

It links **SD with FI**.

The Account Key tells SAP **which G/L Account** should receive the posting.

Common Account Keys

| Account Key | Purpose        |
| ----------- | -------------- |
| ERL         | Sales Revenue  |
| ERS         | Sales Discount |
| ERF         | Freight        |
| MWS         | Taxes          |

---

## Remaining fields

The remaining technical fields are rarely used in SD interviews or certification and are generally handled by SAP standard configuration.

---

# 📚 Important Condition Types

---

## 📌 Statistical Condition Types

These are used only for reporting.

### EK01

Estimated Cost

Used for internal cost analysis.

---

### VPRS ⭐

Very important.

Automatically picks the **Standard Cost** from

```
Material Master

↓

Accounting View
```

Used for

* Profit Margin
* Cost Analysis

Not included in customer pricing.

---

### SKTO

Cash Discount

Applied after tax calculation.

---

# 🚗 Variant Price Discounts

---

### VA00

Variant Price

Used for configurable materials.

Example

Car

Variants

* Petrol
* Diesel
* Automatic
* Manual
* Red
* Blue
* Different Engine

Each variant may have different pricing.

---

# 💲 AMIW / AMIZ (Minimum Order Value)

Business Rule

Minimum Order Value = **$200**

Customer Order

```
Product Value = $200
```

Discount

```
K004 = 10%

↓

Discount = $20

↓

Net = $180
```

Business policy says

Customer must pay at least **$200**

Difference

```
200 - 180

=

20
```

SAP automatically adds

```
AMIZ = $20
```

Final Net Value

```
$200
```

---

# 💳 AZWR

Used for

* Down Payments
* Settlement

---

# 🏢 Intercompany Pricing

Requirement Routine

```
22
```

Condition Types

* PI01
* PI02

---

# 🎁 Free Goods Pricing

Requirement Routine

```
55
```

---

# 📦 KP00

General-purpose condition type (usage depends on business scenario and configuration).

---

# 🎁 Condition Supplement

## Definition

A **Condition Supplement** automatically adds one condition type whenever another condition type is maintained.

---

### Example

```
PR00

↓

Always charge

PK00 (Packing Charges)
```

Instead of entering both every time,

SAP automatically proposes PK00.

---

## Configuration

### Step 1

```
V/06

↓

Open PR00

↓

Maintain Condition Supplement

↓

Add PK00
```

---

### Step 2

```
VK11

↓

Maintain PR00

↓

Additional column appears

↓

PK00
```

Maintain both values together.

---

### Result

Whenever PR00 is determined,

➡️ PK00 is automatically added.

---

# 🚫 Condition Exclusion ⭐⭐⭐

## Why?

Sometimes multiple discounts are applicable,

but business wants **only one discount**.

---

### Example

Available Discounts

```
K005 = ₹1000

K004 = ₹2000

K007 = ₹500
```

Business wants

Only the **lowest** discount

OR

Only the **highest** discount

NOT all discounts together.

---

## Solution

Use **Condition Exclusion**.

---

## Configuration Steps

### Step 1

Define Condition Exclusion Groups

```
SPRO

↓

Sales and Distribution

↓

Basic Functions

↓

Pricing

↓

Condition Exclusion

↓

Define Condition Exclusion Groups
```

Create

```
Group 1

Group 2
```

---

### Step 2

Assign Condition Types

Example

```
Group 1

K005

K007

Total = 1500
```

```
Group 2

K004

Total = 2000
```

---

### Step 3

Assign Groups to Pricing Procedure

```
Open Pricing Procedure

↓

Exclusions

↓

New Entries
```

Maintain

* Sequence Number
* Exclusion Indicator (select the appropriate option based on business rule)
* Group 1
* Group 2

Save.

---

## Result

SAP compares both groups.

Depending on the exclusion indicator,

it can apply:

* ✅ Highest Discount
* ✅ Lowest Discount
* ✅ Best Price
* ✅ Only one valid condition

instead of applying every discount.

---

# ⭐ Most Important Points for Exams

| Topic                                     | Importance |
| ----------------------------------------- | ---------- |
| From & To                                 | ⭐⭐⭐⭐⭐      |
| Manual                                    | ⭐⭐⭐        |
| Required                                  | ⭐⭐⭐⭐       |
| Statistical                               | ⭐⭐⭐⭐⭐      |
| Requirement                               | ⭐⭐⭐⭐⭐      |
| Account Key                               | ⭐⭐⭐⭐⭐      |
| VPRS                                      | ⭐⭐⭐⭐⭐      |
| Condition Supplement                      | ⭐⭐⭐⭐       |
| Condition Exclusion                       | ⭐⭐⭐⭐⭐      |
| AMIZ (Minimum Order Value)                | ⭐⭐⭐⭐       |
| VA00                                      | ⭐⭐⭐        |
| Intercompany Pricing (PI01/PI02, Req. 22) | ⭐⭐⭐        |
| Free Goods Requirement (55)               | ⭐⭐⭐        |

> 📝 **Note:** A few items in your original notes (such as **Relevant for Accounting**, **Subtotal**, **Alternative Condition Base Value**, and **KP00**) were incomplete or context-dependent. I've corrected and expanded them with standard SAP SD behavior while preserving your trainer's explanations where applicable.
