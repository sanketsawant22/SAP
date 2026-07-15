# 🚚 Third-Party Sales Process (TPS)

---

# 📖 What is Third-Party Sales Process?

**Third-Party Sales (TPS)** is a sales process where **your company sells the product**, but the **product is delivered directly by a third-party vendor (supplier)**.

🏢 Your company **never stores or ships the goods**.

This process is commonly used when a company acts as a:

* 🏪 Dealer
* 📦 Distributor
* 🤝 Reseller

---

# 💡 Example

Suppose:

* 👤 Customer → **Rahul**
* 🏢 Your Company → **ABC Electronics**
* 🏭 Vendor → **Dell**

Rahul wants to buy a **Dell Laptop**.

Instead of purchasing the laptop into ABC's warehouse:

```text
Rahul
   │
   ▼
ABC Electronics (Sales Order)
   │
   ▼
Purchase Requisition (PR)
   │
   ▼
Purchase Order (PO)
   │
   ▼
Dell (Vendor)
   │
   ▼
Laptop Delivered Directly to Rahul
```

### Process

1️⃣ Rahul places an order with **ABC Electronics**.

2️⃣ ABC creates a **Sales Order**.

3️⃣ SAP automatically creates a **Purchase Requisition (PR)**.

4️⃣ MM team converts PR into a **Purchase Order (PO)**.

5️⃣ Dell ships the laptop **directly to Rahul**.

6️⃣ Dell sends the invoice to ABC.

7️⃣ ABC invoices Rahul.

---

# 📂 Types of Third-Party Sales Process

## 🤖 1. Automatic Third-Party Sales Process

The material is **always supplied by a third-party vendor**.

The material is configured in SAP for Third-Party Procurement.

When the Sales Order is created,

✅ SAP automatically creates a **Purchase Requisition (PR).**

---

# ⚙️ Configuration

## 1️⃣ Item Category Determination (VOV4)

**T-Code:** `VOV4`

| Field                  | Value |
| ---------------------- | ----- |
| 📄 Sales Document      | OR    |
| 📦 Item Category Group | BANS  |

Result:

✅ **Item Category = TAS**

---

## 2️⃣ Schedule Line Category Determination (VOV5)

**T-Code:** `VOV5`

| Field            | Value |
| ---------------- | ----- |
| 📦 Item Category | TAS   |
| 📋 MRP Type      | ND    |

Result:

✅ **Schedule Line Category = CS**

---

# 🔗 TPS = SD + MM Integration

Third-Party Sales Process is an integration between:

* 🟢 SD (Sales & Distribution)
* 🔵 MM (Materials Management)

---

# ❓How is the Vendor Processed?

When the Sales Order is saved,

SAP automatically generates a:

📄 **Purchase Requisition Number (PR Number)**

The PR number is visible in the **Schedule Line** of the Sales Order.

The MM Procurement Team uses this PR to create the Purchase Order.

---

# ❓How is PR Generated Automatically?

The automatic PR generation happens because of:

✅ **Schedule Line Category = CS**

Inside Schedule Line Category **CS**, the field:

**Order Type = NB**

`NB` is responsible for automatic creation of the **Purchase Requisition (PR)**.

---

# 🔄 Automatic TPS Flow

```text
Customer
     │
     ▼
Sales Order (VA01 - OR)
     │
     ▼
Automatic PR Generated
     │
     ▼
MM creates Purchase Order (ME21N)
     │
     ▼
Vendor ships goods directly to Customer
     │
     ▼
Vendor Invoice (MIRO)
     │
     ▼
Customer Billing (VF01 - F2)
```

---

# 👨‍💼 Responsibility of Each Team

## 🟢 SD Team

### Create Sales Order

**T-Code:** `VA01`

Sales Document:

**OR**

↓

System automatically creates PR Number.

---

## 🔵 MM Team

Creates **Purchase Order** based on PR.

**T-Code:** `ME21N`

---

## 🏭 Vendor

Vendor delivers goods directly to the customer.

🏢 Company warehouse is **not involved**.

---

## 🔵 MM Team

Vendor sends Invoice.

Company posts **Incoming Invoice**.

**T-Code:** `MIRO`

---

## 🟢 SD Team

Create Customer Invoice.

**T-Code:** `VF01`

Billing Type:

**F2**

> ⚠️ Billing is **Order Related Billing** because there is **no delivery document**.

---

# 🛠️ Manual Third-Party Sales Process

In Manual TPS,

The material is **normally sold from company stock**.

However, in special situations like:

* 📦 Stock shortage
* 🚚 Urgent requirement
* 🏭 Stock unavailable

the company manually converts the order into a Third-Party Sales Process.

Third-party procurement is decided **case-by-case**, not by default.

---

# 📝 Automatic TPS Exercise

## Step 1 - Create Material

**T-Code:** `MM01`

### Material Details

| Field            | Value      |
| ---------------- | ---------- |
| 🏭 Industry      | 4 - Retail |
| 📦 Material Type | FERT       |

### Select Views

* ✅ Basic Data 1
* ✅ Sales Org 1
* ✅ Sales Org 2
* ✅ Sales General/Plant
* ✅ Purchasing

---

### Maintain Values

| Field                       | Value       |
| --------------------------- | ----------- |
| Description                 | As required |
| Base Unit                   | EA          |
| Material Group              | 01          |
| General Item Category Group | **BANS**    |

Save the material.

📝 Note the Material Number.

---

## Step 2 - Create Sales Order

**T-Code:** `VA01`

Sales Document:

**OR**

Process is same as a normal Sales Order.

---

### System Determines

| Field                     | Value             |
| ------------------------- | ----------------- |
| 📦 Item Category          | TAS *(from VOV4)* |
| 📋 Schedule Line Category | CS *(from VOV5)*  |

Save the Sales Order.

📝 Note the Sales Order Number.

---

## Step 3 - Check Purchase Requisition

After saving the Sales Order,

SAP automatically creates a **Purchase Requisition (PR).**

To view it:

`VA02`

↓

Item Data

↓

Schedule Lines Tab

↓

PR Number

---

## Step 4

The MM Team creates the Purchase Order using this PR.

---

# 📝 Manual TPS Exercise

Create a normal Sales Order.

Initially,

| Field         | Value  |
| ------------- | ------ |
| Item Category | TAN    |
| Schedule Line | Normal |

Suppose stock is unavailable.

We now want to convert this order into TPS.

---

### Configuration (Information Purpose)

**T-Code:** `VOV4`

Sales Document:

**OR**

Item Category Group:

**NORM**

Add another allowed Item Category:

✅ TAS

---

Now during Sales Order creation,

You can manually change:

**TAN ➜ TAS**

Once changed,

The order becomes a **Third-Party Sales Order**.

---

# 📊 Automatic vs Manual TPS

| Feature                            | 🤖 Automatic TPS | 🛠️ Manual TPS          |
| ---------------------------------- | ---------------- | ----------------------- |
| Material always supplied by vendor | ✅ Yes            | ❌ No                    |
| Item Category Group                | BANS             | NORM                    |
| Item Category                      | TAS              | TAN → TAS (Manual)      |
| PR Generated Automatically         | ✅ Yes            | ✅ After changing to TAS |
| Decision                           | Fixed            | Case-by-case            |
| Company Stock                      | Not Used         | Normally Used           |

---

# 🏢 Inter Company Sales Process

## 📖 Definition

**Inter Company Sales** means:

Sales between **two Company Codes** belonging to the **same Client/Company**.

---

# 💰 Inter Company Billing

The billing transaction between two Company Codes is called:

**Inter Company Billing**

Billing Type:

✅ **IV**

---

# ⚙️ Configuration

### 1️⃣ Assign Sales Organization & Distribution Channel

Assign the:

* Sales Organization
* Distribution Channel

of the **Ordering Company Code**

to the

**Plant of the Delivering Company Code.**

---

### 2️⃣ Assign Billing Type

Assign

✅ **Billing Type IV**

to the required Sales Document Type.

---

### 3️⃣ Assign Organizational Units

Assign Organizational Units by Plant.

---

### 4️⃣ Define Internal Customer

Create and maintain the **Internal Customer** for Inter Company Billing.

---

### 5️⃣ Pricing Procedure

Maintain:

**Condition Type → PI01**

**T-Code:** `V/08`

Requirement:

**22**

---

# 📂 Master Data Requirements

For Inter Company Sales:

### 👤 Customer Master

Customer must be extended to **both Sales Areas**.

---

### 📦 Material Master

Material Master must be maintained at **both Organizational Levels**.

---

### 🏭 Stock

Material stock must be available in the **Delivering Plant**.

---

# 🎯 Important Configuration Summary

## 🚚 Automatic TPS

| Configuration             | Value              |
| ------------------------- | ------------------ |
| 📄 Sales Document         | OR                 |
| 📦 Item Category Group    | BANS               |
| 📦 Item Category          | TAS                |
| 📋 Schedule Line Category | CS                 |
| 📄 PR Order Type          | NB                 |
| 💰 Billing                | F2 (Order Related) |

---

## 🛠️ Manual TPS

| Configuration            | Value          |
| ------------------------ | -------------- |
| 📄 Sales Document        | OR             |
| 📦 Default Item Category | TAN            |
| 🔄 Manual Change         | TAN → TAS      |
| 📋 Schedule Line         | CS (after TAS) |

---

## 🏢 Inter Company Sales

| Configuration                 | Value |
| ----------------------------- | ----- |
| 💰 Inter Company Billing Type | IV    |
| 💵 Pricing Condition          | PI01  |
| 📋 Requirement                | 22    |

---

# ⭐ Exam Tips

✅ **TPS = SD + MM Integration**

✅ **BANS → TAS → CS** (Most Important sequence)

✅ **CS** Schedule Line contains **Order Type = NB**, which automatically generates the **Purchase Requisition (PR)**.

✅ In TPS, **Vendor delivers goods directly to the customer**.

✅ **No Outbound Delivery (VL01N)** is created by your company in TPS.

✅ Customer Billing is **Order Related Billing (F2)** because there is no delivery document.

✅ **Automatic TPS** uses **BANS**, while **Manual TPS** starts with **TAN** and is manually changed to **TAS**.

✅ **Inter Company Billing Type = IV**.

✅ **Condition Type = PI01** with **Requirement = 22** is maintained in the Pricing Procedure (`V/08`).
