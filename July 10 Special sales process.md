# 📘 Module 4 – Special Sales Process (SAP SD)

---

# 🚀 Special Sales Processes

Special Sales Processes in SAP SD:

* 📦 Consignment Sales Process
* 💵 Cash Sales
* ⚡ Rush Orders
* 📑 Outline Agreements
* 🔄 Returns Process
* 🏢 Intercompany Sales Process
* 🚚 Third-Party Sales Process

---

# 📦 1. Consignment Sales Process

## 📖 Definition

A **Consignment Sales Process** is a special sales process where the company sends goods to the customer's location, **but ownership remains with the company** until the customer actually uses or sells the goods.

### Common Industries

* 💊 Pharmaceuticals
* 🚗 Automobile Spare Parts
* 🏥 Hospitals
* 🛒 Retail Stores

The stock kept at the customer's location is called:

> **Consignment Stock (Special Stock)**

---

# 🔄 Four Consignment Processes

```
Company Stock
      │
      ▼
1️⃣ Fill-Up
      │
      ▼
Customer Consignment Stock
      │
      ├────────► 2️⃣ Issue (Customer Uses Stock)
      │
      ├────────► 3️⃣ Pickup (Unused Stock Returned)
      │
      └────────► 4️⃣ Return (Used Goods Returned Due to Defect)
```

---

# 1️⃣ Consignment Fill-Up

## Purpose

Send goods to the customer's premises.

### Business Flow

* Goods move from **Company Unrestricted Stock** ➜ **Customer Consignment Stock**
* Ownership remains with the company
* Customer is **NOT billed**

### Configuration

| Setting                | Value       |
| ---------------------- | ----------- |
| 📄 Sales Document Type | **KB**      |
| 📦 Item Category       | **KBN**     |
| 📅 Schedule Line       | **E0 / E1** |
| 🚚 Delivery            | ✅ Yes       |
| 📋 Picking             | ✅ Yes       |
| 📤 PGI                 | ✅ Yes       |
| 💰 Pricing             | ❌ No        |
| 🧾 Billing             | ❌ No        |
| 📦 Movement Type       | **631**     |

---

# 2️⃣ Consignment Issue

## Purpose

Customer actually consumes or sells the goods.

### Business Flow

* Ownership transfers to customer
* Customer is billed
* Consignment stock reduces

### Configuration

| Setting                | Value       |
| ---------------------- | ----------- |
| 📄 Sales Document Type | **KE**      |
| 📦 Item Category       | **KEN**     |
| 📅 Schedule Line       | **C0 / C1** |
| 🚚 Delivery            | ✅ Yes       |
| 📋 Picking             | ❌ No        |
| 📤 PGI                 | ✅ Yes       |
| 💰 Pricing             | ✅ Yes       |
| 🧾 Billing             | ✅ Yes       |
| 📦 Movement Type       | **633**     |

---

# 3️⃣ Consignment Pickup

## Purpose

Company takes back unused goods from customer.

### Business Flow

* Unused stock returns to company
* No billing

### Configuration

| Setting                     | Value       |
| --------------------------- | ----------- |
| 📄 Sales Document Type      | **KA**      |
| 📦 Item Category            | **KAN**     |
| 📅 Schedule Line            | **F0 / F1** |
| 🚚 Delivery                 | ✅ Yes       |
| 📋 Picking                  | ❌ No        |
| 📥 PGR (Post Goods Receipt) | ✅ Yes       |
| 💰 Pricing                  | ❌ No        |
| 🧾 Billing                  | ❌ No        |
| 📦 Movement Type            | **632**     |

---

# 4️⃣ Consignment Return

## Purpose

Customer returns consumed goods (usually because of defects).

### Business Flow

* Customer returns already consumed goods
* Billing adjustment required

### Configuration

| Setting                | Value   |
| ---------------------- | ------- |
| 📄 Sales Document Type | **KR**  |
| 📦 Item Category       | **KRN** |
| 📅 Schedule Line       | **D0**  |
| 🚚 Delivery            | ✅ Yes   |
| 📋 Picking             | ❌ No    |
| 📥 PGR                 | ✅ Yes   |
| 💰 Pricing             | ✅ Yes   |
| 🧾 Billing             | ✅ Yes   |
| 📦 Movement Type       | **634** |

---

# 🧪 Practical Exercise

## Step 1️⃣ Check Initial Stock

Use:

```
MMBE
```

Record the available stock for the material that will be used.

---

## Step 2️⃣ Assign Sales Document Types

Use:

```
OVAZ
```

Assign the following Sales Document Types to your:

* Sales Organization
* Distribution Channel
* Division

| Sales Document | Description         |
| -------------- | ------------------- |
| KB             | Consignment Fill-Up |
| KE             | Consignment Issue   |
| KA             | Consignment Pickup  |
| KR             | Consignment Return  |
| BV             | Cash Sales          |
| RO             | Rush Order          |

Save.

---

# 🚚 Consignment Fill-Up Exercise

## Create Sales Order

```
VA01
```

Sales Document Type:

```
KB
```

Fill:

* Customer
* Material
* Quantity = 100

Check:

* Item Category = **KBN**

Save the order.

📝 Note the Sales Order Number.

---

## Create Delivery

```
VL01N
```

Perform:

* Picking ✅
* Transfer Order ✅
* PGI ✅

Save.

---

# 📊 Verify Stock

Use:

```
MMBE
```

Observe:

✅ Company stock decreases

✅ New section appears:

```
Customer Consignment Stock
```

The transferred quantity is visible there.

---

# 🚚 Consignment Issue Exercise

## Create Sales Order

```
VA01
```

Sales Document Type:

```
KE
```

Enter:

* Customer
* Same Material
* Quantity ≤ Consignment Stock

Check:

* Item Category = **KEN**

Save.

📝 Note Order Number.

---

## Create Delivery

```
VL01N
```

No:

* ❌ Picking
* ❌ Transfer Order

Perform:

* ✅ PGI

Save.

---

## Verify Stock

```
MMBE
```

Customer Consignment Stock decreases by the issued quantity.

---

## Billing

```
VF01
```

Create Billing Document.

Save.

📝 Note Billing Number.

---

## Check Document Flow

Verify the complete document flow.

---

# 🔄 Consignment Pickup Exercise

## Create Sales Order

```
VA01
```

Sales Document:

```
KA
```

Enter:

* Customer
* Same Material
* Quantity ≤ Consignment Stock

---

### Incompletion Log

Edit:

```
Order Reason = Sales Call
```

Continue.

Save.

📝 Note Order Number.

---

## Delivery

```
VL01A
```

Perform:

* ❌ No Picking
* ✅ PGR

Save.

---

## Verify Stock

```
MMBE
```

Customer Consignment Stock decreases.

Company Stock increases.

---

## Check Document Flow

Verify complete process.

---

# 🔄 Consignment Return Exercise

## Create Sales Order

```
VA01
```

Sales Document:

```
KR
```

Enter:

* Customer
* Same Material
* Return Quantity

Verify:

| Field         | Value |
| ------------- | ----- |
| Item Category | KRN   |
| Schedule Line | D0    |

---

### Incompletion Log

Set:

```
Order Reason = Poor Quality
```

Save.

---

## Delivery

```
VL01N
```

Perform:

* ❌ No Picking
* ❌ No Transfer Order
* ✅ PGR

Save.

---

## Billing

```
VF01
```

Billing is created **with reference to the Sales Order**.

This behavior is configured in:

```
VOV8
```

For Order Category:

```
KR
```

---

### ⚠️ Common Error

You may receive:

> Sales Order is blocked.

Solution:

```
VA02
```

Remove the block.

Save.

Return to:

```
VF01
```

Billing Type automatically becomes:

```
RE
```

Complete billing.

Save.

---

## Verify

* 📄 Document Flow
* 📦 Stock using MMBE

Customer Consignment Stock is updated according to the returned quantity.

---

### 💡 Important Note

If you want the returned stock to come back into **Company Stock**, perform a **Consignment Pickup (KA)** for that quantity.

---

# ⚙️ Important Configuration Transactions

| T-Code   | Purpose                                        |
| -------- | ---------------------------------------------- |
| **VOV8** | Sales Document Type Configuration              |
| **VOV7** | Item Category Configuration                    |
| **VOV6** | Schedule Line Category Configuration           |
| **0VLP** | Delivery Item Category / Picking Configuration |

---

# 🧠 What Controls What?

| Function         | Controlled By |
| ---------------- | ------------- |
| 🧾 Billing       | VOV8 + VOV7   |
| 💰 Pricing       | VOV7          |
| 📤 PGI           | VOV6          |
| 📋 Picking       | 0VLP          |
| 🚚 Delivery      | VOV8 + VOV6   |
| 📦 Movement Type | VOV6          |

---

# 📚 Quick Revision Table

| Process    | Doc Type | Item Cat | Schedule Line | Billing | Pricing | Picking | Movement Type |
| ---------- | -------- | -------- | ------------- | ------- | ------- | ------- | ------------- |
| 📦 Fill-Up | KB       | KBN      | E0 / E1       | ❌       | ❌       | ✅       | 631           |
| 💰 Issue   | KE       | KEN      | C0 / C1       | ✅       | ✅       | ❌       | 633           |
| ↩️ Pickup  | KA       | KAN      | F0 / F1       | ❌       | ❌       | ❌       | 632           |
| 🔄 Return  | KR       | KRN      | D0            | ✅       | ✅       | ❌       | 634           |

---

# 🎯 Exam Points to Remember

* ✅ Consignment Stock is **owned by the company**, even though it is stored at the customer's location.
* ✅ Billing happens **only** during **Consignment Issue** and **Consignment Return**.
* ✅ Ownership transfers only during **Consignment Issue**.
* ✅ Fill-Up and Pickup do **not** create billing documents.
* ✅ MMBE is the best transaction to verify Consignment Stock.
* ✅ Movement Types:

  * **631 → Fill-Up**
  * **633 → Issue**
  * **632 → Pickup**
  * **634 → Return**
* ✅ During Consignment Return, billing is created with reference to the **Sales Order**, and if the order is blocked, remove the block in **VA02** before creating the billing document.
