# 💰 SAP SD – Billing Process

---

# 📌 Billing Process

## What is Billing?

Billing is the **final step of the Order-to-Cash (O2C) process** in SAP SD.

It converts completed **deliveries or services into invoices**, which are sent to the customer for payment.

Billing also integrates the **Sales & Distribution (SD)** module with the **Financial Accounting (FI)** module.

---

## Objectives of Billing

✔ Generate customer invoice

✔ Transfer accounting entries to FI

✔ Trigger customer payment process

✔ Update document flow

✔ Complete the Sales Cycle

---

# 📚 Billing Transaction Codes

| T-Code   | Purpose                  |
| -------- | ------------------------ |
| **VF01** | Create Billing Document  |
| **VF02** | Change Billing Document  |
| **VF03** | Display Billing Document |
| **VF11** | Cancel Billing Document  |

---

# 📄 Billing Document

A Billing Document contains:

* Customer Billing Information
* Pricing
* Taxes
* Discounts
* Payment Terms
* Billing Date
* Company Code
* Currency

It serves as the official invoice sent to the customer.

---

# 🏗️ Billing Document Structure

## Header Data

Contains overall billing information such as:

* Billing Document Type
* Payer
* Company Code
* Billing Date
* Currency
* Net Value
* Payment Terms
* Pricing Procedure
* Tax Information

---

## Item Data

Contains material-level information:

* Material Number
* Description
* Quantity
* Unit Price
* Net Price
* Taxes
* Discounts
* Item Category

---

# 📑 Billing Document Types

**Configuration T-Code:** `VOFA`

| Billing Type | Description                        |
| ------------ | ---------------------------------- |
| **F2**       | Standard Invoice                   |
| **F5**       | Order-related Pro Forma Invoice    |
| **F8**       | Delivery-related Pro Forma Invoice |
| **G2**       | Credit Memo                        |
| **L2**       | Debit Memo                         |
| **RE**       | Returns Invoice                    |
| **IV**       | Intercompany Invoice               |
| **S1**       | Cancellation Invoice               |
| **BV**       | Cash Sales Invoice                 |

> **Correction:**
> RE = Returns Invoice (not simply "Return")

---

# ⚙️ How does SAP determine the Billing Type?

Suppose:

Sales Order Type = **OR**

When creating the Billing Document, we only enter the **Delivery Number**.

So how does SAP decide the Billing Type?

### Process

Sales Order (OR)

⬇

SAP checks **Sales Document Type configuration (VOV8)**

⬇

Billing Type maintained = **F2**

⬇

SAP automatically creates Billing Document as **F2**

---

## Important Note ⭐

Normally, SAP determines the Billing Type automatically.

However, during **VF01**, you can manually select another Billing Type (if allowed by configuration).

If no Billing Type is entered manually, SAP uses the one determined through configuration.

---

# ⚙️ Billing Type Controls (VOFA)

Billing Type configuration controls:

* Number Range
* Output Determination
* Partner Determination
* Account Determination
* Posting Block
* Cancellation Billing Type
* Billing Category
* Document Flow
* Pricing Type

These settings determine how the Billing Document behaves.

---

# 📦 Billing Relevance

Billing can be created:

* 📦 With reference to **Delivery**
* 📄 With reference to **Sales Order**

This depends on the **Billing Relevance** maintained in the **Item Category Configuration**.

---

## Example

Sales Order contains:

* M1 → Product
* S1 → Installation Service

Billing can be:

### Product

Delivery-related Billing

↓

Invoice after PGI

---

### Service

Order-related Billing

↓

Invoice without Delivery

---

Even if both are products, separate invoices can still be generated if configuration allows.

---

# 💳 Special Billing Types

SAP supports several special billing documents:

* Credit Memo
* Debit Memo
* Cancellation Invoice
* Pro Forma Invoice
* Invoice Correction Request

---

# 💵 Credit Memo

## Definition

A Credit Memo is issued when the **company has to return money to the customer**.

### Examples

* Customer returned goods.
* Customer was overcharged.
* Customer paid extra by mistake.
* Pricing error occurred.

**Result:**

Company pays money back to customer.

---

## Standard Sales Flow

```
VA01 (OR)
      ↓
VL01N (LF)
      ↓
PGI
      ↓
VF01 (F2)
```

Now suppose the customer returns the goods.

Company must issue a Credit Memo.

---

# 📌 Credit Memo Process

### Step 1 – Create Credit Memo Request

```
VA01

Document Type = CR
```

---

### Step 2 – If CR is not assigned

Transaction:

`OVAZ`

Assign Sales Area to:

* CR
* DR
* RK

Save.

---

### Step 3 – Create with Reference

Create **with reference to Billing Document**.

Enter Billing Document Number.

Click **Copy**.

SAP copies:

* Customer
* Material
* Quantity
* Pricing

---

### Step 4 – Modify Data

If required:

* Change Quantity
* Change Price

---

### Step 5 – Item Category

Item Category becomes:

**G2N**

⭐ Important Interview Point

This comes from **Item Category Determination (VOV4)**.

---

### Step 6 – Complete Incompletion Log

If SAP shows:

Incomplete Data

Go to:

Edit

↓

Complete Data

↓

Maintain Order Reason

↓

Save

---

### Step 7 – Billing

Transaction:

```
VF01
```

Enter Credit Memo Request Number.

Initially SAP may show:

**Billing Block**

Reason:

Credit Memo usually requires managerial approval because the company is refunding money.

---

### Step 8 – Remove Billing Block

```
VA02

Item Overview

Billing Block = Blank

Save
```

---

### Step 9 – Create Billing

Again run:

```
VF01
```

Billing Type becomes:

**G2**

Save.

---

### Step 10 – Verify

```
VA03

Document Flow
```

Credit Memo process is completed.

---

# 💸 Debit Memo

## Definition

Debit Memo is issued when the **customer has to pay additional money to the company**.

### Examples

* Undercharged invoice
* Price correction
* Additional service charges
* Freight charges added later

---

## Process

Normal Sales Process

```
VA01 (OR)

↓

VL01N

↓

PGI

↓

VF01 (F2)
```

---

### Create Debit Memo Request

```
VA01

Document Type = DR
```

Create **with reference to Billing Document**.

Copy the Billing Document.

Increase:

* Price
* Quantity

(if required)

Save.

Complete incomplete data if prompted.

---

### Billing

```
VF01
```

Billing Type:

**L2**

If Billing Block appears:

Remove it using:

```
VA02
```

Then create Billing again.

---

## Note ⭐

Debit Memo can be created **without reference**, but using a reference document is the recommended business practice.

---

# ❌ Cancellation Invoice

## Definition

Suppose an invoice has already been created but needs to be cancelled due to:

* Wrong customer
* Wrong quantity
* Wrong pricing
* Duplicate invoice
* Customer cancellation

Instead of deleting the invoice, SAP creates a **Cancellation Invoice**.

---

## Transaction

```
VF11
```

---

## Steps

Enter:

Billing Document Number

↓

Execute

↓

SAP automatically proposes:

Original Invoice

*

Cancellation Invoice

↓

Save

---

## Verification

```
VF03 / VF02
```

Billing Type becomes:

**S1**

Meaning:

Invoice has been cancelled.

---

## Why S1?

In **VOFA**,

Billing Type **F2** has

Cancellation Billing Type = **S1**

SAP automatically uses S1 during cancellation.

---

## Check Document Flow

```
VA03

↓

Document Flow
```

---

## Effects of Cancellation

✔ Billing Document is cancelled.

✔ Accounting entries are reversed.

✔ Delivery status becomes **In Process** again.

✔ New Billing Document can be created for the same Delivery.

---

# 📄 Pro Forma Invoice

## Definition

A Pro Forma Invoice is a **dummy invoice** created only for informational purposes.

It is **not a legal accounting document**.

No payment is expected from the customer.

---

## Characteristics

✅ Informational only

✅ No Accounting Document

✅ No FI Posting

✅ No Customer Payment

✅ Can be generated multiple times

---

## Types

| Billing Type | Description                        |
| ------------ | ---------------------------------- |
| **F5**       | Order-related Pro Forma Invoice    |
| **F8**       | Delivery-related Pro Forma Invoice |

---

## Important Points ⭐

* Data is **not transferred to FI**.
* Multiple Pro Forma Invoices can be generated for the same Sales Order or Delivery.
* **F8 can even be created before PGI**, making it useful for customs, export documentation, or advance information to the customer.

---

# 📝 Exercise 1 – Maintain Material Price

Transaction:

```
VK11
```

Condition Type:

```
PR00
```

Enter:

* Sales Organization
* Distribution Channel
* Customer
* Material

Maintain:

Price

Save.

> **Purpose:** Defines the selling price of a material for a specific customer.

---

# 📝 Exercise 2 – Standard Billing

```
VK11
      ↓
VA01
      ↓
VL01N
      ↓
Transfer Order (if WM is active)
      ↓
PGI
      ↓
VF01 (F2)
```

---

# 📝 Exercise 3 – Credit Memo

```
VA01 (CR)
        ↓
Reference Billing Document
        ↓
Modify Quantity/Price
        ↓
Complete Incompletion Log
        ↓
VA02 (Remove Billing Block)
        ↓
VF01
        ↓
Billing Type = G2
```

---

# 📝 Exercise 4 – Debit Memo

```
VA01 (DR)
        ↓
Reference Billing Document
        ↓
Increase Price
        ↓
Complete Incompletion Log
        ↓
VA02
        ↓
Remove Billing Block
        ↓
VF01
        ↓
Billing Type = L2
```

---

# 📝 Exercise 5 – Cancellation Invoice

```
VF11
      ↓
Enter Billing Document
      ↓
Execute
      ↓
Save
```

Billing Type becomes:

**S1**

---

# 📝 Exercise 6 – Pro Forma Invoice

## Order-Based

```
VA01

↓

VF01

↓

Billing Type = F5

↓

Save
```

---

## Delivery-Based

```
VA01

↓

VL01N

↓

Transfer Order

↓

(No PGI Required)

↓

VF01

↓

Billing Type = F8

↓

Save
```

Later:

```
VL02N

↓

PGI

↓

VF01

↓

F2 Standard Invoice
```

Document Flow now contains:

* 📄 Order Pro Forma (F5)
* 🚚 Delivery Pro Forma (F8)
* 💰 Standard Invoice (F2)

---

# 📌 Important Transaction Codes

| T-Code   | Purpose                                   |
| -------- | ----------------------------------------- |
| **VF01** | Create Billing Document                   |
| **VF02** | Change Billing Document                   |
| **VF03** | Display Billing Document                  |
| **VF11** | Cancel Billing Document                   |
| **VOFA** | Define Billing Types                      |
| **VOV8** | Sales Document Type Configuration         |
| **VOV4** | Item Category Determination               |
| **OVAZ** | Assign Sales Area to Sales Document Types |
| **VK11** | Maintain Pricing Condition (PR00)         |
| **VA01** | Create Sales Order                        |
| **VA02** | Change Sales Order                        |
| **VA03** | Display Sales Order                       |
