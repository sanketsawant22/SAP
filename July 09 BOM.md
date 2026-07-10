# 📦 SAP SD – Bills of Material (BOM) & Invoice Correction Request

---

# 📚 Previous Lecture Recap

The following billing types were covered previously:

* 💳 Credit Memo
* 💸 Debit Memo
* 📄 Pro Forma Invoice
* ❌ Cancellation Invoice

---

# 🏗️ Bills of Material (BOM)

## What is BOM?

**BOM (Bill of Material)** is a master data object that defines the relationship between a **finished (main) material** and the **components** required to make or sell it.

In SAP SD, BOM is mainly used when a product is sold as a **combination (kit/package)** of multiple components.

---

## Example

🖥️ Computer

Main Material

```
Computer
```

Components

* Keyboard
* Mouse
* CPU
* Monitor
* Power Cable

Instead of manually entering every component in the Sales Order,

the user enters only:

```
Computer
```

SAP automatically explodes the BOM and adds all component materials into the Sales Order.

This process is called **BOM Explosion**.

---

# 📌 BOM Transaction Code

| T-Code   | Purpose     |
| -------- | ----------- |
| **CS01** | Create BOM  |
| **CS02** | Change BOM  |
| **CS03** | Display BOM |

---

# 📌 BOM Usage

While creating BOM,

for **Sales & Distribution** the BOM Usage should always be:

```
5 → Sales & Distribution (SD)
```

> **Important:** BOM Usage tells SAP in which application the BOM will be used.
> Usage **5** is specifically for SD processing.

---

# 🎯 Why do we use BOM?

Instead of selling each component separately,

SAP allows selling them as one product.

Benefits:

* Automatic component determination
* Easier Sales Order creation
* Correct pricing
* Easier inventory management
* Supports Kit Sales

---

# 📦 Two BOM Scenarios

SAP SD supports two standard BOM billing scenarios.

---

# 🟢 Scenario 1 – Main Item is Billed

### Billing Logic

✅ Main Material is billed.

❌ Component Materials are NOT billed.

Customer sees only one product on the invoice.

---

### Example

Customer orders

```
Computer
```

Sales Order contains

```
Computer
Keyboard
Mouse
CPU
Monitor
```

Invoice

```
Computer
₹50,000
```

Components are supplied,

but only the main item appears on the invoice.

---

## Item Category Group

Material Master

```
Sales Org 2

↓

Item Category Group

↓

ERLA
```

ERLA = **Sales Kit (Main Item Pricing)**

---

# Item Category Determination

### Main Material

| Field               | Value   |
| ------------------- | ------- |
| Sales Doc Type      | OR      |
| Item Category Group | ERLA    |
| Higher Level Item   | Blank   |
| Item Category       | **TAQ** |

---

### Component Materials

| Field               | Value   |
| ------------------- | ------- |
| Sales Doc Type      | OR      |
| Item Category Group | NORM    |
| Higher Level Item   | TAQ     |
| Item Category       | **TAE** |

---

# 🔵 Scenario 2 – Components are Billed

### Billing Logic

❌ Main Item is NOT billed.

✅ Components are billed individually.

---

### Example

Sales Order

```
Computer
Keyboard
Mouse
CPU
Monitor
```

Invoice

```
Keyboard

Mouse

CPU

Monitor
```

Main Material appears only for structuring purposes.

Billing happens at component level.

---

## Item Category Group

Material Master

```
Sales Org 2

↓

Item Category Group

↓

LUMF
```

LUMF = **Sales Kit (Component Pricing)**

---

# Item Category Determination

### Main Material

| Field               | Value   |
| ------------------- | ------- |
| Sales Doc Type      | OR      |
| Item Category Group | LUMF    |
| Higher Level Item   | Blank   |
| Item Category       | **TAP** |

---

### Component Material

| Field               | Value   |
| ------------------- | ------- |
| Sales Doc Type      | OR      |
| Item Category Group | NORM    |
| Higher Level Item   | TAP     |
| Item Category       | **TAN** |

---

# ⭐ Important Concept

Changing the **Item Category Group** of the main material changes the **Item Category Determination**.

This is because Item Category Determination (configured in **VOV4**) depends on:

* Sales Document Type
* Item Category Group
* Usage
* Higher Level Item Category

Therefore:

* **ERLA → TAQ**
* **LUMF → TAP**

---

# 📝 Exercise – BOM Creation

## Step 1 – Create Main Material

Transaction

```
MM01
```

Create a new material.

Maintain:

```
Sales Org 2

↓

Item Category Group

↓

ERLA
```

Save the material.

---

## Step 2 – Create Component Materials

Transaction

```
MM01
```

Create multiple component materials.

You can use:

```
Copy From
```

Maintain:

```
Item Category Group = NORM
```

Save all component materials.

---

## Step 3 – (Optional) Check Item Category Determination

Transaction

```
VOV4
```

Verify:

ERLA → TAQ

LUMF → TAP

Higher Level Item logic.

> This step is for understanding the configuration and is not mandatory for the exercise.

---

## Step 4 – Maintain Pricing

Transaction

```
VK11
```

Condition Type

```
PR00
```

Maintain prices for:

* Main Material
* Component Materials

### Important Note

For **ERLA**, keep the main material price **greater than or equal to** the total price of the components.

---

## Step 5 – Create BOM

Transaction

```
CS01
```

Enter:

* Main Material
* Plant
* BOM Usage = **5**

Continue.

Maintain:

```
Item Category = L (Stock Item)
```

Add all component materials.

Maintain component quantities.

Example

| Component | Quantity |
| --------- | -------- |
| Keyboard  | 1        |
| Mouse     | 1        |
| CPU       | 1        |
| Monitor   | 1        |

Save.

The BOM is now created.

---

## Step 6 – Maintain Stock

Transaction

```
MIGO
```

Increase stock for:

* Main Material (if required)
* All Component Materials

Without stock,

Delivery cannot be created.

---

## Step 7 – Create Sales Order

```
VA01
```

Enter only the:

Main Material

↓

Press Enter.

SAP automatically performs **BOM Explosion**.

All component materials are inserted automatically into the Sales Order.

Save.

---

## Step 8 – Create Delivery

```
VL01N
```

Create Delivery.

Maintain Storage Location for all components.

If Warehouse Management is active,

Create Transfer Order.

Perform:

* Picking
* PGI

---

## Step 9 – Billing

```
VF01
```

### ERLA Scenario

Only the **Main Material** is billed.

Invoice contains:

```
Computer
₹50,000
```

---

### LUMF Scenario

Only the **Component Materials** are billed.

Invoice contains:

```
Keyboard

Mouse

CPU

Monitor
```

---

# ⭐ Interview Points (BOM)

* **ERLA** → Main Item Pricing.
* **LUMF** → Component Pricing.
* **Usage = 5** is mandatory for SD BOM.
* **CS01** creates BOM.
* **BOM Explosion** happens automatically during Sales Order creation.
* Components are added automatically in the Sales Order.
* The **Higher Level Item Category** plays an important role in determining the component item category.

---

# 🧾 Invoice Correction Request

## Definition

An **Invoice Correction Request** is used when an invoice has already been created, but the customer requests a correction instead of issuing a separate Credit Memo or Debit Memo.

Typical reasons:

* Incorrect Price
* Incorrect Quantity
* Overcharging
* Undercharging
* Wrong Discount

It allows SAP to calculate the exact difference between the original invoice and the corrected values.

---

# 📌 Sales Document Type

```
RK
```

Invoice Correction Request is a **Sales Document**, not a Billing Document.

---

# 📌 Key Features

✔ Created **with reference to an existing Billing Document**.

✔ Two line items are automatically generated for each original item:

1. **Credit Line Item** (not editable)
2. **Debit Line Item** (editable)

✔ Initial Net Value = **0**

✔ User edits only the **Debit Line Item**.

✔ SAP calculates the difference automatically.

---

# Why are there two line items?

Suppose Original Invoice

```
Quantity = 10
Price = ₹100
```

Invoice Value

```
₹1000
```

Customer says:

Actual Quantity should have been:

```
8
```

SAP creates:

Credit Line

```
10 × ₹100
```

Debit Line

```
8 × ₹100
```

Difference

```
₹200
```

Net Value becomes:

```
₹200
```

SAP automatically knows that ₹200 should be refunded.

---

# 📌 Reference Document Requirement

When creating RK,

SAP immediately asks for a Billing Document Number.

Why?

Because in **VOV8**, the RK Sales Document Type is configured with:

```
Reference Mandatory = M
```

This means an Invoice Correction Request **must always be created with reference to a Billing Document**.

---

# 📝 Exercise – Invoice Correction Request

## Step 1 – Create Invoice Correction Request

Transaction

```
VA01
```

Sales Document Type

```
RK
```

SAP asks for:

Billing Document Number

Enter the Billing Document.

Continue.

---

## Step 2 – Observe Line Items

SAP automatically creates:

* Credit Line (Not Editable)
* Debit Line (Editable)

Both initially have the same value.

Therefore:

```
Net Value = 0
```

---

## Step 3 – Make Required Corrections

### Quantity Error

Update the **Debit Line** quantity.

SAP recalculates the difference automatically.

---

### Price Error

Go to:

```
Conditions Tab
```

Update the pricing condition.

SAP recalculates the Net Value automatically.

---

## Step 4 – Remove Billing Block

Go to:

```
Item Overview

↓

Billing Block

↓

Blank
```

Save.

---

## Step 5 – Complete Incompletion Log

If SAP shows:

```
Incomplete Data
```

Go to:

```
Edit

↓

Complete Data

↓

Save
```

---

## Step 6 – Create Billing

Transaction

```
VF01
```

Enter:

Invoice Correction Request Number.

Save.

SAP generates the appropriate billing document based on the value difference.

---

# ⭐ Important Note

Depending on the correction:

* Positive Difference → Customer pays more (Debit)
* Negative Difference → Customer receives money back (Credit)

The financial impact is determined automatically by SAP based on the calculated difference.

> **Correction to your note:** Standard SAP Invoice Correction Requests can result in either a credit or debit effect depending on the configuration and the calculated difference. It's better to remember that SAP determines the financial outcome from the difference rather than thinking it "always generates a Credit Memo with positive/negative values."

---

# 📚 Important Transaction Codes

| T-Code   | Purpose                           |
| -------- | --------------------------------- |
| **CS01** | Create BOM                        |
| **CS02** | Change BOM                        |
| **CS03** | Display BOM                       |
| **MM01** | Create Material                   |
| **MIGO** | Goods Movement / Maintain Stock   |
| **VK11** | Maintain Pricing (PR00)           |
| **VA01** | Create Sales Order                |
| **VF01** | Create Billing Document           |
| **VOV4** | Item Category Determination       |
| **VOV8** | Sales Document Type Configuration |

---

# 🎯 Important Points

⭐ BOM stands for **Bill of Materials** and defines the relationship between a finished product and its component materials.

⭐ **BOM Usage = 5** is used for Sales & Distribution.

⭐ **ERLA** = Main item pricing (invoice contains only the main material).

⭐ **LUMF** = Component pricing (invoice contains only the components).

⭐ BOM automatically **explodes** during Sales Order creation, adding component materials to the order.

⭐ **CS01** is used to create a BOM.

⭐ Invoice Correction Request uses **Sales Document Type RK**.

⭐ RK **must always be created with reference to a Billing Document** because its reference is mandatory in **VOV8**.

⭐ Invoice Correction Request automatically creates **two line items**:

* Credit Line (non-editable)
* Debit Line (editable)

⭐ The **initial Net Value is always zero**, and SAP calculates the financial difference after the Debit Line is edited.

⭐ Always review the **Document Flow (VA03)** after completing BOM-based sales or Invoice Correction Requests to understand how Sales Orders, Deliveries, Billing Documents, and Corrections are linked.
