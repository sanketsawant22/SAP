# 🏢 SAP SD – Intercompany Sales Process

## 📖 What is Intercompany Sales?

Intercompany Sales is a sales process between **two company codes** that belong to the **same organization**.

* 🏢 **Ordering Company** → Receives the customer order.
* 🏭 **Delivering Company** → Delivers the goods and bills the ordering company internally.

---

# 🏗 Organizational Structure

## 🏢 Ordering Company

Use your existing organization.

---

## 🏭 Delivering Company

* 🏢 Company Code : **7222**
* 🏢 Sales Org : **7111**
* 🚚 Distribution Channel : **Y1**
* 📦 Division : **Y1**
* 🏭 Plant : **7222**
* 📍 Storage Location : **7111**
* 🚛 Shipping Point : **7111**

---

# ⚙️ Configuration

## 1️⃣ Assign Ordering Sales Org & Distribution Channel to Delivering Plant

📍 **Path**

```
SPRO
→ Enterprise Structure
→ Assignment
→ Sales and Distribution
→ Assign Sales Organization & Distribution Channel to Plant
```

➕ Create New Entry

* 🏢 Sales Org → Ordering Company
* 🚚 Distribution Channel → Ordering Company
* 🏭 Plant → Delivering Company

---

## 2️⃣ Assign Intercompany Billing Type (IV) to Sales Document Type

> ✅ Standard configuration (Normally no changes required)

📍 **Path**

```
SPRO
→ Sales and Distribution
→ Billing
→ Intercompany Billing
→ Define Order Types for Intercompany Billing
```

OR

📍 Check in **VOV8**

* Intercompany Billing = **IV**

---

## 3️⃣ Assign Organization Units by Plant ⭐ (Important)

> ⚠️ Always verify this before testing Intercompany Sales because another user may have changed the configuration.

📍 **Path**

```
SPRO
→ Sales and Distribution
→ Billing
→ Intercompany Billing
→ Assign Organizational Units by Plant
```

Search your:

* 🏭 Plant (Delivering Plant)

Maintain:

* 🏢 Sales Organization → Ordering Company
* 🚚 Distribution Channel
* 📦 Division

Save ✅

---

## 4️⃣ Define Internal Customer for Intercompany Billing

> Create a customer in the **Supplying Company** that represents the **Ordering Company**.

### 📝 Create Customer

### BP

* Organization
* Fill General Data
* Save

---

### FLCU00

* 🏢 Company Code = **Supplying Company Code**

---

### FLCU01

Sales Area:

* 🏢 Sales Org → Supplying Company
* 🚚 Distribution Channel
* 📦 Division

Fill remaining details.

Save ✅

---

### Assign Internal Customer

📍 **Path**

```
SPRO
→ Sales and Distribution
→ Billing
→ Intercompany Billing
→ Define Internal Customer by Sales Organization
```

* Search Ordering Sales Organization
* Assign Internal Customer
* Save

---

## 5️⃣ Add PI01 in Pricing Procedure

📍 Transaction

```
V/08
```

Open your Pricing Procedure.

Create New Entry:

| Field          | Value    |
| -------------- | -------- |
| Step           | **905**  |
| Condition Type | **PI01** |
| Statistical    | ✅        |
| Requirement    | **22**   |
| Account Key    | **ERL**  |
| Subtotal       | **B**    |

Save ✅

---

# 📋 Master Data Configuration

# 👤 1. Customer should exist in both Sales Areas

---

## 🆕 Create Customer

### BP

* Organization
* Role : **000000**
* Fill General Data
* Save

---

### FLCU00

Company Code:

* 🏢 Ordering Company Code

Fill Customer Information.

---

### FLCU01

Sales Area:

* 🏢 Ordering Sales Org
* 🚚 Distribution Channel
* 📦 Division

---

### 🚚 Shipping Tab

Delivery Plant:

* 🌱 Ordering Company Plant

---

### 💵 Billing Tab

* Incoterms → **FOB**
* Terms of Payment → **0001**
* Account Assignment Group → **01**

Save ✅

Customer is now created for the Ordering Company.

---

## 🔄 Extend Customer to Delivering Company

### BP

Enter Customer Number.

---

### FLCU00

Current Company Code = Ordering Company

Click:

```
Switch Company Code
```

Enter:

* 🏢 Delivering Company Code

Save

---

### FLCU01

Click:

```
Switch Sales Area
```

Enter:

* 🏢 Delivering Sales Org
* 🚚 Distribution Channel
* 📦 Division

Fill remaining details.

Save ✅

---

# 📦 2. Material should exist in both Organizational Levels

### Step 1

Create Material in Ordering Company.

---

### Step 2

Extend Material

```
MM01
```

Enter existing Material Number.

At Organizational Levels enter:

* 🏢 Delivering Company
* 🏭 Delivering Plant

Fill remaining views.

Save ✅

---

# 📦 3. Maintain Stock in Delivering Plant

```
MIGO
```

Maintain Stock for:

* 🏭 Delivering Plant (7222)

Save ✅

---

# 💰 4. Maintain Pricing

```
VK11
```

Maintain Price in **Ordering Sales Organization**.

Condition Types:

* 💵 K005
* 💵 K004
* 💵 K007

Also Maintain:

* 💰 **PI01**

  * Use **Delivering Plant** while creating PI01 condition record.

---

# 🛒 Create Sales Order

```
VA01
```

Document Type:

* **OR**

Sales Area:

* Ordering Company

Customer:

* Customer extended to both Sales Areas

Material:

* Material extended to both Companies

---

## 🚚 Shipping Tab

Plant will initially show Ordering Plant.

➡️ Change it to:

* 🏭 Delivering Company Plant

Shipping Point changes automatically. ✅

Save Sales Order.

---

# 📦 Delivery

```
VL01N
```

Shipping Point:

* **7111** (Delivering Company)

Create Delivery.

---

### Header Data

Go to:

```
Header
→ Processing
```

Verify:

```
Intercompany Billing = A (Relevant)
```

✅ If "A - Relevant" appears, configuration is correct.

---

### Perform PGI

```
Post Goods Issue
```

---

# 🧾 Customer Billing

```
VF01
```

Enter:

* Delivery Number

Execute.

Invoice Type:

* **F2**

Save ✅

> This is the customer invoice.

---

# 🧾 Intercompany Billing

Again:

```
VF01
```

Enter:

* Same Delivery Number

Execute.

Invoice Type:

* **IV**

Save ✅

> This is the intercompany invoice (Delivering Company → Ordering Company).

---

# 🔍 Verify Document Flow

Check the complete document flow.

You should see:

```
🛒 Sales Order (OR)
        │
        ▼
📦 Delivery
        │
        ▼
🚚 PGI
        │
        ├────────► 🧾 Customer Billing (F2)
        │
        └────────► 🧾 Intercompany Billing (IV)
```

---

# 🎯 Quick Revision

✅ Create two organizational structures
✅ Assign Ordering Sales Org → Delivering Plant
✅ Verify Billing Type IV
✅ Assign Organization Units by Plant ⭐
✅ Create Internal Customer & Assign It
✅ Add PI01 in Pricing Procedure
✅ Extend Customer to Both Sales Areas
✅ Extend Material to Both Companies
✅ Maintain Stock in Delivering Plant
✅ Maintain Pricing (including PI01)
✅ Create Sales Order (OR)
✅ Create Delivery (VL01N)
✅ Perform PGI
✅ Create Customer Invoice (F2)
✅ Create Intercompany Invoice (IV)
✅ Verify Document Flow
