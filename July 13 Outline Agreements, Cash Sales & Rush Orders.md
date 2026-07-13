# Outline Agreements, Cash Sales & Rush Orders

---

# 📑 Outline Agreements

## 📖 Definition

An **Outline Agreement** is a long-term agreement between a company and a customer that defines the terms for future sales transactions.

Unlike a normal Sales Order, an Outline Agreement **does not immediately trigger delivery or billing**. It acts as a commitment between both parties for future business.

---

# 📚 Types of Outline Agreements

```text
Outline Agreements
│
├── 📅 Scheduling Agreement
│
└── 📄 Contracts
      │
      ├── 📦 Quantity Contract
      ├── 💰 Value Contract
      │      ├── General Value Contract
      │      └── Material Related Value Contract
      ├── 🛠️ Service Contract
      └── 📜 Master Contract
```

---

# 📅 Scheduling Agreement

## 📖 Definition

A **Scheduling Agreement** is an Outline Agreement that contains:

* 📅 Delivery Dates
* 📦 Confirmed Quantities

Unlike a normal Sales Order where **schedule lines are automatically generated**, in a Scheduling Agreement the **schedule lines are entered manually**.

---

## SAP Details

| Setting             | Value                      |
| ------------------- | -------------------------- |
| T-Code              | **VA31**                   |
| Sales Document Type | **LP** *(Before 2023: DS)* |
| Item Category       | **LPN**                    |

---

# 🔄 Process Flow

```text
Scheduling Agreement
        │
        ▼
Manual Schedule Lines
        │
        ▼
Delivery
        │
        ▼
PGI
        │
        ▼
Billing
```

---

# 🧪 Scheduling Agreement Exercise

## Step 1️⃣ Create Scheduling Agreement

```text
VA31
```

Sales Document Type

```text
LP
```

Enter:

* Sales Area
* Customer

---

## Step 2️⃣ Item Overview

Enter:

* Valid From
* Valid To

---

## Step 3️⃣ Material

Enter:

* Material
* Total Quantity

Verify:

* Item Category = **LPN**

---

## Step 4️⃣ Enter Schedule Lines

Go to:

```text
Item Data → Schedule Lines
```

Manually enter:

* 📅 Delivery Date
* 📦 Order Quantity

> **Note:** Each schedule line quantity should be **less than or equal to** the total contract quantity.

Save.

📝 Note the Agreement Number.

---

## Add More Schedule Lines

Use

```text
VA32
```

Open the agreement.

Add additional schedule lines.

Save.

---

# 🚚 Delivery for First Schedule Line

```text
VL01N
```

Enter:

* Delivery Date = Same as **First Schedule Line**

Perform:

* Picking
* Transfer Order
* PGI

Save.

---

# 🧾 Billing

```text
VF01
```

Billing Type:

```text
F2
```

Invoice is generated only for the **first schedule line quantity**.

Save.

---

# 📄 Document Flow

Check Document Flow.

---

# 🚚 Remaining Schedule Lines

Repeat the same process for every schedule line:

```text
VL01N
↓
Transfer Order
↓
PGI
↓
VF01
↓
Billing
```

After all schedule lines are completed:

✅ Scheduling Agreement status becomes **Completed** in Document Flow.

---

# 📜 Contracts

## 📖 Definition

A **Contract** is a mutual agreement between a company and a customer defining future purchases under agreed terms.

Unlike Sales Orders:

❌ No Delivery

❌ No Shipping

❌ No Billing

Contracts only define future commitments.

---

# 📦 Quantity Contract

## 📖 Definition

A **Quantity Contract** is an agreement where the customer promises to purchase a fixed quantity of goods within a specified validity period.

Pricing is fixed for the entire contract duration.

### Example

* Product: TV
* Quantity: **250,000 TVs**
* Validity: 1 Year

Customer may place multiple Sales Orders until the total quantity reaches **250,000**.

---

## Important Concept

The Sales Orders created from a contract are called:

> **Release Orders**

---

## SAP Details

| Setting        | Value    |
| -------------- | -------- |
| T-Code         | **VA41** |
| Sales Document | **QC**   |
| Item Category  | **KMN**  |

---

# 🔄 Process Flow

```text
Quantity Contract
        │
        ▼
Release Order
        │
        ▼
Delivery
        │
        ▼
Billing
```

---

# 🧪 Quantity Contract Exercise

## Create Contract

```text
VA41
```

Sales Document Type

```text
QC
```

Enter:

* Customer
* Validity Period
* Material
* Quantity

---

## Pricing

Go to:

```text
Item Data → Conditions
```

Maintain contract pricing.

---

### 📌 Important

There are:

* ❌ No Schedule Lines
* ❌ No Delivery
* ❌ No Billing

Reason:

Item Category **KMN** is **not schedule-line relevant**.

Save.

---

## Create Release Order

```text
VA01
```

Choose:

```text
Create with Reference
```

Enter Contract Number.

SAP automatically copies:

* Customer
* Material
* Pricing

Enter only:

* Required Quantity

Save.

Then perform:

```text
Delivery
↓
PGI
↓
Billing
```

Repeat until contract quantity is fully consumed.

---

# 💰 Value Contract

## 📖 Definition

A **Value Contract** is an agreement where the customer commits to purchasing goods worth a specified monetary value within a defined period.

Unlike Quantity Contracts, the commitment is based on **value**, not quantity.

---

## Process Flow

```text
Value Contract
        │
        ▼
Release Order
        │
        ▼
Delivery
        │
        ▼
Billing
```

---

# Types of Value Contracts

---

# 1️⃣ Material Related Value Contract

Customer specifies the material(s) that can be purchased.

Only those materials are allowed.

| Setting        | Value   |
| -------------- | ------- |
| Sales Document | **WK2** |
| Item Category  | **WKN** |

### Example

Contract Value:

💰 ₹50 Lakhs

Material:

💻 Laptop

Validity:

📅 1 Year

Customer can purchase only **Laptops** until ₹50 Lakhs is reached.

---

# 2️⃣ General Value Contract

Only the contract value is fixed.

Customer is free to purchase **any material**.

| Setting        | Value   |
| -------------- | ------- |
| Sales Document | **WK1** |
| Item Category  | **WKN** |

### Example

Contract Value:

💰 ₹50 Lakhs

Customer may purchase:

* 💻 Laptop
* ⌨️ Keyboard
* 🖨️ Printer
* 🖥️ Monitor

Any products are allowed until ₹50 Lakhs is consumed.

---

# 📊 Quantity Contract vs Value Contract

| Feature        | Quantity Contract     | Value Contract        |
| -------------- | --------------------- | --------------------- |
| Based On       | Quantity              | Monetary Value        |
| Sales Doc      | QC                    | WK1 / WK2             |
| Item Category  | KMN                   | WKN                   |
| Pricing        | Fixed                 | Fixed                 |
| Release Orders | ✅ Yes                 | ✅ Yes                 |
| Delivery       | Through Release Order | Through Release Order |
| Billing        | Through Release Order | Through Release Order |

---

# 🛠️ Service Contract

## 📖 Definition

A **Service Contract** is an agreement to provide **services** instead of physical goods.

No tangible product is sold.

Only services are delivered.

---

## Examples

* 🌐 Wi-Fi Maintenance
* 🧹 Cleaning Services
* 🔧 Equipment Repair
* ❄️ AC Servicing
* 💻 Annual Software Maintenance

---

## Service Material

Create Service Material:

```text
MM01
```

Material Type

```text
DIEN
```

---

## SAP Details

| Setting        | Value   |
| -------------- | ------- |
| Sales Document | **SC**  |
| Item Category  | **WVN** |

---

# ❌ Contract Cancellation

Use:

```text
VA41
```

Sales Document Type:

```text
CQ
```

Specify:

* Cancellation Reason

Save.

### Result

❌ No further Release Orders can be created for the cancelled contract.

---

# 💵 Cash Sales

## 📖 Definition

Cash Sales is a process where:

* Customer pays immediately.
* Delivery is processed immediately.
* Billing is completed immediately.

Everything happens almost at the same time.

### Example

A customer walks into an electronics store:

* 💻 Buys a Laptop
* 💳 Pays immediately
* 📦 Takes the Laptop home

---

## SAP Details

| Setting            | Value             |
| ------------------ | ----------------- |
| Sales Document     | **BV / CS**       |
| Item Category      | **BVN**           |
| Immediate Delivery | ✅ Yes             |
| Delivery Type      | **BV**            |
| Billing Type       | **BV**            |
| Billing Reference  | **Order Related** |

---

# 🔄 Cash Sales Process Flow

```text
Sales Order
       │
       ├──► Delivery Created Automatically
       │
       ▼
Picking
       │
       ▼
PGI
       │
       ▼
Billing
```

---

# 🧪 Cash Sales Exercise

## Create Sales Order

```text
VA01
```

Sales Document Type

```text
BV
```

Enter:

* Customer
* Material
* Quantity

Verify:

Item Category = **BVN**

Save.

📝 Note:

* Sales Order Number
* Delivery Number *(created automatically)*

---

## Complete Delivery

```text
VL02N
```

Enter Delivery Number.

Perform:

* Picking
* Transfer Order
* PGI

Save.

---

## Billing

```text
VF01
```

Reference:

**Sales Order Number**

Billing Type:

```text
BV
```

Complete Billing.

Save.

---

## Check Document Flow

Verify the complete process.

---

# ⚡ Rush Order

## 📖 Definition

A **Rush Order** is used when the customer urgently requires goods.

The order receives the highest priority so delivery begins immediately.

### Example

🏥 A hospital urgently needs medical equipment.

The company processes the order immediately to avoid delays.

---

## SAP Details

| Setting            | Value                |
| ------------------ | -------------------- |
| Sales Document     | **RO**               |
| Item Category      | **TAN**              |
| Immediate Delivery | ✅ Yes                |
| Delivery Type      | **LF**               |
| Billing Type       | **F2**               |
| Billing Reference  | **Delivery Related** |

---

# 🔄 Rush Order Process

```text
Sales Order
      │
      ▼
Immediate Delivery
      │
      ▼
Picking
      │
      ▼
PGI
      │
      ▼
Billing
```

---

# 🧪 Rush Order Exercise

The process is almost identical to **Cash Sales**.

Only the Sales Document Type changes.

```text
VA01
```

Sales Document Type:

```text
RO
```

Then perform:

```text
Delivery
↓
Picking
↓
Transfer Order
↓
PGI
↓
Billing (VF01)
```

---

# 📊 Cash Sales vs Rush Order

| Feature           | 💵 Cash Sales         | ⚡ Rush Order                   |
| ----------------- | --------------------- | ------------------------------ |
| Customer Payment  | Immediate             | May be Immediate or Later      |
| Sales Doc Type    | BV / CS               | RO                             |
| Item Category     | BVN                   | TAN                            |
| Delivery Type     | BV                    | LF                             |
| Billing Type      | BV                    | F2                             |
| Billing Reference | Order Related         | Delivery Related               |
| Delivery          | Immediate             | Immediate                      |
| Picking Required  | ✅ Yes                 | ✅ Yes                          |
| PGI               | ✅ Yes                 | ✅ Yes                          |
| Typical Example   | Retail Store Purchase | Emergency Customer Requirement |

---

# 🎯 Exam Points to Remember

## 📅 Scheduling Agreement

* ✅ Schedule lines are **entered manually**.
* ✅ Multiple deliveries can be created from a single agreement.
* ✅ Each schedule line is delivered and billed separately.

## 📦 Quantity Contract

* ✅ Based on **quantity**.
* ✅ Multiple **Release Orders** can be created until the agreed quantity is fulfilled.
* ✅ No delivery or billing directly from the contract.

## 💰 Value Contract

* ✅ Based on **monetary value**, not quantity.
* ✅ **WK1** = General Value Contract (any material).
* ✅ **WK2** = Material Related Value Contract (specific material only).

## 🛠️ Service Contract

* ✅ Used for **services**, not physical goods.
* ✅ Service material type = **DIEN**.
* ✅ Sales Document = **SC**.

## 💵 Cash Sales

* ✅ Customer pays immediately.
* ✅ Delivery document is created automatically.
* ✅ Billing is **Order Related**.

## ⚡ Rush Order

* ✅ Used for urgent customer requirements.
* ✅ Delivery starts immediately.
* ✅ Billing is **Delivery Related**.
* ✅ Uses standard delivery type **LF** and billing type **F2**.
