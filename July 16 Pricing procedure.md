# 💰 SAP SD Pricing Configuration & Condition Technique (Complete Notes)

> **Topic:** Pricing Configuration in SAP SD
> **End User T-Code:** `VK11` (Maintain Condition Records)

---

# 💵 VK11 - Maintain Material Prices

`VK11` is used to maintain the **price/condition records** of a material.

✅ This is generally an **End User activity**.

However, before an end user can maintain prices in VK11, the **SAP Consultant must complete the pricing configuration**.

---

# 📌 Different Types of Prices

Businesses maintain many kinds of prices.

Examples:

* 💰 Gross Price
* 💵 Net Price
* 🏷 Selling Price
* 📈 MRP
* 💲 Cost Price
* 📊 Standard Price
* 🎁 Discount
* ⭐ Special Price
* 🌍 Market Price
* 🧾 Billing Price
* 🏛 Tax
* 👤 Customer Discount
* 📦 Material Discount
* 🎉 Festival Discount
* etc.

---

# 📌 SAP Groups All Pricing Elements into 4 Categories

SAP divides all pricing elements into **4 basic categories**:

### 1️⃣ Base Price

Example:

* PR00

---

### 2️⃣ Discounts / Surcharges

Examples:

* Customer Discount
* Material Discount
* Festival Discount
* Additional Charges

---

### 3️⃣ Freight

Transportation Charges

Example:

* KF00

---

### 4️⃣ Taxes

Example:

* MWST

---

# 🧮 How does SAP calculate Net Value?

Net Value = Final selling price of the material.

Formula:

```
Net Value =
Base Price
- Discounts
+ Freight
+ Taxes
```

Example:

```
Base Price      = 40,000
Discount        = 3,500
Freight         = 100
Tax             = 2,000

Net Value = 38,600
```

---

# ⚙️ CONDITION TECHNIQUE

For SAP to determine all pricing elements automatically during Sales Order processing, SAP uses a mechanism called

# 👉 Condition Technique

This is a **Backend Configuration** done by the SAP SD Consultant according to business requirements.

---

## Condition Technique consists of 5 Elements

1️⃣ Condition Tables

2️⃣ Access Sequence

3️⃣ Condition Types

4️⃣ Pricing Procedure

5️⃣ Pricing Procedure Determination

---

# 1️⃣ CONDITION TABLES

### T-Codes

| Purpose | T-Code |
| ------- | ------ |
| Create  | V/03   |
| Change  | V/04   |
| Display | V/05   |

---

## What is a Condition Table?

A Condition Table contains the **Key Fields (Key Combination)** on which prices are maintained.

In simple words,

It answers:

> **"On what combination should SAP maintain the price?"**

---

## Example

Price can be maintained based on:

* Sales Org + Distribution Channel + Division
* Sales Org + Distribution Channel
* Sales Org + Plant
* Sales Org
* Plant
* Customer + Material
* Material
* etc.

Each combination can become a **different Condition Table**.

---

## What happens in VK11?

Suppose:

```
VK11

Condition Type = PR00
```

After pressing Enter,

SAP shows **multiple options**.

Those options are actually **different Condition Tables**.

Each Condition Table represents a different way of maintaining prices.

---

## Creating a Condition Table (Educational Purpose)

### T-Code

```
V/03
```

Change:

```
V/04
```

Display:

```
V/05
```

---

### Notes

SAP Standard Tables

```
Below 500
```

Custom Tables

```
Above 500
```

Example:

```
573
```

---

Inside V/04 you'll see:

### Left Side

✅ Selected Fields

### Right Side

✅ Field Catalog

Move required fields from **Field Catalog** to **Selected Fields**.

Example:

* Sales Org
* Customer
* Material
* Plant

Then click

```
Generate
```

SAP creates the Condition Table using all selected fields.

---

# 2️⃣ ACCESS SEQUENCE

### T-Code

```
V/07
```

---

## Definition

Access Sequence is a **Search Strategy** used by SAP to determine the correct Condition Record.

---

## Why do we need it?

Suppose same material has multiple prices.

Example:

### First Record

```
Customer + Material

Price = 40,000
```

---

### Second Record

```
Material

Price = 35,000
```

Now create Sales Order.

Scenario 1

Customer ✔

Material ✔

Result

```
Price = 40,000
```

---

Scenario 2

Customer ❌

Material ✔

Result

```
Price = 35,000
```

---

How does SAP know which one to check first?

👉 Through **Access Sequence**

---

## What does Access Sequence contain?

Access Sequence contains **multiple Condition Tables arranged in priority order.**

Example:

```
Customer + Material

↓

Material

↓

Sales Org

↓

Plant
```

SAP checks one by one until it finds a valid record.

---

## Inside V/07

Open

```
PR02
```

Go to

```
Accesses
```

You can see multiple Condition Tables.

The sequence decides **priority**.

---

## Exclusive Indicator

If **Exclusive** is checked,

SAP immediately stops searching after finding a valid record in that table.

No further Condition Tables are checked.

---

## Important Rule ⭐

Arrange Condition Tables from

**Most Specific ➜ Most Generic**

Example:

```
Customer + Material

↓

Customer

↓

Material

↓

Sales Org

↓

Plant
```

---

## Important Note

One Access Sequence can contain

✅ Multiple Condition Tables

SAP derives the price according to this search sequence.

---

## Search Logic

If **Exclusive is NOT marked**

SAP checks all tables and picks the **last valid condition record found**.

If **Exclusive IS marked**

SAP immediately stops searching at that table and uses that price.

---

# 3️⃣ CONDITION TYPE

### T-Code

```
V/06
```

---

## Definition

Each Pricing Element is represented by a **Condition Type**.

---

Examples

| Condition Type | Purpose     |
| -------------- | ----------- |
| PR00           | Price       |
| ZGPR           | Gross Price |
| ZNPR           | Net Price   |

---

## Standard Condition Types

| Condition Type | Meaning                      |
| -------------- | ---------------------------- |
| PR00           | Standard Price               |
| K004           | Material Discount            |
| K005           | Customer + Material Discount |
| K007           | Customer Discount            |
| KF00           | Freight                      |
| MWST           | Tax                          |
| PI01           | Intercompany                 |

---

## Important Fields in Condition Type

### Condition Class

Defines the category.

Examples:

* Price
* Discount
* Tax
* Freight

---

### Calculation Type

Defines how SAP calculates the value.

Examples:

* Quantity
* Percentage
* Weight
* Fixed Amount

---

### Access Sequence

Inside the Condition Type,

assign the required Access Sequence.

Example

```
PR02
```

This links the Condition Type to the correct Condition Tables.

---

# 4️⃣ PRICING PROCEDURE

### T-Code

```
V/08
```

---

## Definition

Pricing Procedure is a **Sequential List of Condition Types** used by SAP to calculate the Net Value.

---

When Sales Order is created,

SAP processes all Condition Types one after another according to the Pricing Procedure.

---

Example

Pricing Procedure

```
ZRVA01
```

Condition Records maintained in VK11

```
PR00 = 40,000

K004 = -2,000

K005 = -1,000

K007 = -500

KF00 = +100
```

---

Net Value

```
40,000

-2,000

-1,000

-500

+100

----------------

36,600
```

---

## Standard Pricing Procedure

```
RVAA01
```

---

# Pricing Procedure Columns (Very Important ⭐)

Pricing Procedure contains **18 columns** (your trainer may refer to 16 in older systems, but in S/4HANA you'll commonly see 18).

Important columns:

1. Step
2. Counter
3. Condition Type
4. Description
5. From
6. To
7. Manual
8. Statistical
9. Required
10. Relevant for Accounting *(S/4HANA)*
11. Print Type
12. Subtotal
13. Requirement
14. Alternative Calculation Type
15. Alternative Condition Base Value
16. Account Key
17. Accrual Key
18. Access Level *(S/4HANA)*

---

To view:

```
V/08

↓

RVAA01

↓

Pricing Procedure
```

You will see all Condition Types in sequence with these columns.

---

# 5️⃣ PRICING PROCEDURE DETERMINATION

### T-Code

```
OVKK
```

---

## Definition

Pricing Procedure Determination tells SAP

> **Which Pricing Procedure should be used?**

---

SAP determines Pricing Procedure using **3 Factors**

```
Sales Area

+

Customer Pricing Procedure

+

Document Pricing Procedure

↓

Pricing Procedure
```

---

### Customer Pricing Procedure

Found in

```
Customer Master

↓

CM

↓

Orders Tab
```

---

### Document Pricing Procedure

Found in

```
VOV8
```

---

Pricing Procedure determined here can also be seen in

```
Sales Order

↓

Header Data

↓

Sales Tab
```

---

# 📝 After Configuration

Once all 5 configuration steps are complete,

End User creates **Condition Records** in

```
VK11
```

---

# 🔄 Complete Pricing Flow During Sales Order

Create Sales Order

```
VA01

↓

Document Type = OR
```

---

From Document Type (OR)

↓

System gets

```
Document Pricing Procedure
```

---

Enter Sales Area

↓

System gets

```
Sales Area
```

---

Enter Customer

↓

System gets

```
Customer Pricing Procedure
```

---

Now SAP has all three values

```
Sales Area

+

Customer Pricing Procedure

+

Document Pricing Procedure
```

↓

Using

```
OVKK
```

SAP determines

```
Pricing Procedure

Example

RVAA01
```

---

Next,

Enter Material

↓

SAP checks the Condition Type

Example

```
PR00
```

↓

Inside PR00

SAP finds

```
Access Sequence

PR02
```

↓

Access Sequence searches

```
Condition Tables
```

↓

Condition Tables locate

```
Condition Record
```

↓

SAP gets the Price

↓

All Condition Types are processed according to the Pricing Procedure

↓

Final Net Value is calculated and shown in the Sales Order.

---

# 🧪 PRACTICAL EXERCISE

# 1️⃣ Create Condition Table

```
V/03
```

Use Table Number

```
Above 500
```

> *(In training, existing tables were used because records were already available.)*

---

# 2️⃣ Create Access Sequence

```
V/07
```

### Steps

* New Entries
* Enter any 4-digit Access Sequence ID (note it down)
* Give Description
* Select it
* Open **Accesses**
* Add Condition Tables in priority sequence

Example:

```
10 → 305

20 → 307

30 → 304
```

Use the same standard tables.

Select each Condition Table.

Click **Fields**.

SAP will show warnings.

👉 Keep pressing **Enter** until the fields are locked for that Access Sequence.

Repeat for every Condition Table.

Save.

Again, bypass all warnings by pressing **Enter**.

---

### Transport Request

Since this is the first custom configuration,

Transport Request will be blank.

Click

```
Create Request
```

Enter Description.

Save.

SAP generates a new **Transport Request Number**.

Save again.

---

# 3️⃣ Create Condition Type

```
V/06
```

Search

```
PR00
```

Double-click it.

Click

```
Copy
```

(next to New Entries)

Enter your own custom Condition Type code (trainer mentioned **not "IT"**, use your own naming convention).

Save.

Now search for your newly created Condition Type.

Open it.

Assign your own **Access Sequence** (created in V/07) in the **Access Sequence** field.

Save.

---

# 4️⃣ Create Pricing Procedure

```
V/08
```

Use the standard Pricing Procedure as a reference.

Reference Steps:

```
Step 11  → PR00

Step 100

Step 103 → K005

Step 104 → K007

Step 105 → K004

Step 300

Step 815 → KF00
```

Create a new Pricing Procedure.

Add all reference entries.

Only replace **PR00** with your **custom Condition Type** created earlier.

Save.

---

# 5️⃣ Pricing Procedure Determination

```
OVKK
```

Search using:

* Sales Area
* Customer Pricing Procedure
* Document Pricing Procedure

You will find the standard Pricing Procedure.

Replace it with your newly created Pricing Procedure.

Keep **Condition Type** field blank.

Save.

---

# 6️⃣ Maintain Condition Records (VK11)

Maintain records for your custom Condition Type.

### First Condition Type (Your Custom PR00)

* Sales Area
* Customer
* Material
* Amount
* Validity (till end of year)

Save.

---

### K005

Maintain

* Discount Amount
* Validity

Save.

---

### K007

Maintain

* Discount Percentage
* Validity

Save.

---

### K004

Maintain Material Discount.

Save.

---

### KF00

Select the **Second Condition Table**.

Maintain:

* Incoterms = **FOB**
* Freight Amount

Save.

---

# 🧪 Testing

Create Sales Order

```
VA01

↓

OR
```

Enter

* Sales Area
* Customer

SAP automatically determines the Pricing Procedure you created.

Enter

* Material
* Quantity

Go to

```
Item Data

↓

Conditions Tab
```

You will see all your Condition Types with the calculated values.

Click

```
Analysis
```

to view the **complete pricing procedure**, including:

* Determined Pricing Procedure
* Access Sequence used
* Condition Tables searched
* Condition Records found
* Final Net Value calculation

---

# 🎯 Quick Revision Flow (One-Line Summary)

```
V/03  → Create Condition Table
        ↓
V/07  → Create Access Sequence
        ↓
V/06  → Create Condition Type (Assign Access Sequence)
        ↓
V/08  → Create Pricing Procedure (Add Condition Types)
        ↓
OVKK  → Pricing Procedure Determination
        ↓
VK11  → Maintain Condition Records
        ↓
VA01  → Create Sales Order
        ↓
Conditions Tab → Analysis → Final Net Value
```

# 🧠 Easy Memory Trick

> **"Table → Sequence → Type → Procedure → Determination → Record → Sales Order"**

Or simply:

**📋 Table → 🔍 Sequence → 🏷 Type → 🧮 Procedure → 🎯 Determination → 💰 VK11 → 🛒 VA01**

This is the complete pricing flow from backend configuration to price determination in a Sales Order, without dropping any of the topics from your notes.
