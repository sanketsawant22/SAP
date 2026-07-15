# 🔄 Return Process

---

# 📦 Types of Return Process

### 📝 1. Administrative Return (Planned Return)

Customer informs the company **before** returning the product.

### 🛒 2. Counter Return (Unplanned Return)

Customer directly returns the product at the store without prior communication.

### ⚠️ 3. Product Recall

Company recalls products due to quality or safety issues.

### 💬 4. Product Complaint

Customer reports an issue after purchase (usually during warranty).

---

# 📝 Planned Returns (Administrative Return)

A **planned return** is when the customer informs the company in advance that they want to return the product.

### 📌 Possible Reasons

* ❌ Defective product
* 📦 Damaged product
* 🔄 Wrong product delivered
* 🚚 Transit damage
* ⏰ Late delivery
* 😕 Product does not meet customer requirements

### 📋 Company Decision

After discussing the issue, the company decides how the return will be processed.

The customer may:

* 📤 Return the product to the company
* 🚛 Send it directly to another customer or another location (as instructed by the company)

---

# 🛒 Unplanned Returns (Counter Return)

Usually seen in the **Retail Industry**.

* No prior discussion with the company.
* Customer directly visits the retailer/store.
* Requests:

  * 💰 Refund
  * 🔄 Return
  * 🔁 Exchange

The return is processed according to the **store's return policy**.

---

# ⚠️ Product Recall

A **Product Recall** happens when the company identifies a defect or safety issue in a product or production batch.

The company recalls **all affected products**, even if customers haven't reported any problems yet.

### 📌 Example

🥫 A food company discovers contamination in one production batch and recalls all products from that batch.

---

# 💬 Product Complaint

A customer reports a problem after purchasing the product, usually during the **warranty period**.

Depending on company policy and warranty:

* 🔧 Repair the product
* 🔄 Replace the product
* 💰 Refund the customer

---

# 🔄 Return Process in SAP SD

### Normal Sales Flow

```
1️⃣ Sales Order
        ↓
2️⃣ Delivery
   • Picking
   • Transfer Order
   • PGI
        ↓
3️⃣ Billing
```

Customer later raises a complaint.

---

# 📄 Step 4 - Return Order

**T-Code:** `VA01`

**Sales Document Type:** `RE`

### ✅ Return Order can be created:

* With reference to previous Sales Order
* With reference to previous Invoice
* Independently (without reference)

> ⚠️ **Return Reason** is a **mandatory field** and must be maintained.

---

# 🚛 Step 5 - Returns Delivery

**T-Code:** `VL01N`

**Delivery Type:** `LR`

### Process

* Post Goods Receipt (PGR) is performed.
* Goods move back into company stock.

> ⚠️ Before billing, remove the **Billing Block** from the Return Order using **VA02**.

---

# 💰 Case 1 - Customer Wants Refund

### Step 6A - Billing

**T-Code:** `VF01`

**Billing Type:** `RE`

Billing is **Order Related** (controlled from **VOV7**).

Result:

➡️ **Credit Memo** is generated and money is returned to the customer.

---

# 🔁 Case 2 - Customer Wants Replacement

Instead of refunding money...

Create a **new Sales Order** with reference to the Return Order.

### Step 6B - Sales Order

**T-Code:** `VA01`

Sales Document Type:

* `SD`
* `SDF`

Created **with reference to Return Order (RE).**

---

### Step 7 - Outbound Delivery

**T-Code:** `VL01N`

**Delivery Type:** `LF`

Process:

* 📦 Picking
* 🚚 Transfer Order
* ✅ PGI

> ❌ **No Billing** is created because this is a **Replacement Delivery**.

---

# 📋 Configuration Summary

## 🔄 Return Process

| Configuration             | Value                |
| ------------------------- | -------------------- |
| 📄 Sales Document         | **RE**               |
| 📦 Item Category          | **REN**              |
| 🚛 Delivery Type          | **LR**               |
| 💰 Billing                | **RE (Credit Memo)** |
| 📅 Schedule Line Category | **DN**               |
| 📦 Movement Type          | **651**              |

---

## 🔁 Subsequent Delivery (Replacement)

| Configuration             | Value          |
| ------------------------- | -------------- |
| 📄 Sales Document         | **SD / SDF**   |
| 📦 Item Category          | **KLN**        |
| 🚛 Delivery Type          | **LF**         |
| 💰 Billing                | **No Billing** |
| 📅 Schedule Line Category | **CP / CN**    |
| 📦 Movement Type          | **601**        |

---

# 🎁 Free of Charge Delivery

Used when the company sends products **without charging the customer** (e.g., samples, goodwill replacement).

| Configuration             | Value          |
| ------------------------- | -------------- |
| 📄 Sales Document         | **FD**         |
| 📦 Item Category          | **KLN**        |
| 🚛 Delivery Type          | **LF**         |
| 💰 Billing                | **No Billing** |
| 📅 Schedule Line Category | **CP / CN**    |
| 📦 Movement Type          | **601**        |

---

# 🤖 ARM (Advanced Return Management)

ARM automates the return process.

| Configuration     | Value   |
| ----------------- | ------- |
| 📄 Sales Document | **RE2** |
| 📦 Item Category  | **REN** |

### Advantage

✅ System automatically creates:

* Return Delivery
* Follow-up documents
* Other return-related processes

This reduces manual work and speeds up return processing.

---

# 📌 Complete Return Flow

## 💰 Refund Scenario

```
Sales Order
      ↓
Delivery
      ↓
Billing
      ↓
Customer Complaint
      ↓
Return Order (RE)
      ↓
Returns Delivery (LR)
      ↓
Remove Billing Block (VA02)
      ↓
Billing (RE)
      ↓
Credit Memo 💰
```

---

## 🔁 Replacement Scenario

```
Sales Order
      ↓
Delivery
      ↓
Billing
      ↓
Customer Complaint
      ↓
Return Order (RE)
      ↓
Returns Delivery (LR)
      ↓
New Sales Order (SD/SDF)
      ↓
Outbound Delivery (LF)
      ↓
Replacement Product Delivered 📦

❌ No Billing
```

---

# 🎯 Important T-Codes

| T-Code       | Purpose                                       |
| ------------ | --------------------------------------------- |
| 📝 **VA01**  | Create Return Order / Replacement Sales Order |
| ✏️ **VA02**  | Remove Billing Block from Return Order        |
| 🚛 **VL01N** | Create Returns Delivery / Outbound Delivery   |
| 💰 **VF01**  | Create Credit Memo Billing                    |

---

# ⭐ Exam Tips

✅ Return Reason is **Mandatory** in Return Order (`RE`).

✅ Remove the **Billing Block** in **VA02** before creating the Credit Memo.

✅ **Refund** → Create **Credit Memo** (`VF01`).

✅ **Replacement** → Create a **new Sales Order** with reference to the Return Order.

✅ **Replacement Delivery** and **Free of Charge Delivery** both have **No Billing**.

✅ **ARM (RE2)** automates the return process by automatically creating return-related documents.
