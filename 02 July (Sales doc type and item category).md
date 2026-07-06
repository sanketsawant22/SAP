# 📘 Day 4 – Sales Documents & Item Categories (Foundation of Sales Order)

---

# 🤔 What is a Sales Document?

Whenever a customer interacts with the company, SAP creates a **Sales Document**.

Examples:

* Inquiry
* Quotation
* Sales Order
* Return Order
* Credit Memo Request
* Debit Memo Request

Each of these is a different **Sales Document Type**.

---

# 📄 Sales Document Type (VOV8)

**Definition**

A **Sales Document Type** controls the overall behavior of a sales document.

It tells SAP:

* What kind of document is this?
* Can pricing happen?
* Can delivery happen?
* Can billing happen?
* Which number range should be used?
* Which Item Categories are allowed?
* Which document can be created next?

**Configuration T-Code**

```
SPRO
→ Sales and Distribution
→ Sales
→ Sales Documents
→ Sales Document Header
→ Define Sales Document Types

T-Code : VOV8
```

---

# Common Sales Document Types

| Sales Process       | Standard Document Type | Customized Example |
| ------------------- | ---------------------- | ------------------ |
| Inquiry             | IN                     | ZLIN               |
| Quotation           | QT                     | ZLQT               |
| Standard Order      | OR                     | ZLOR               |
| Return Order        | RE                     | ZLRE               |
| Rush Order          | RO                     | ZLRO               |
| Cash Sales          | BV                     | ZLBV               |
| Credit Memo Request | CR                     | ZLCR               |
| Debit Memo Request  | DR                     | ZLDR               |

### 🧠 Remember

> **Document Type = What kind of sales process is this?**

---

# Why create Z Documents?

SAP standard documents:

* OR
* IN
* QT

Companies usually **copy** the standard document and create a custom version starting with **Z** (or sometimes **Y**).

Example:

```
OR
↓

Copy

↓

ZLOR
```

Reason:

* Standard SAP remains unchanged.
* Company-specific changes can be made safely.

---

# What does VOV8 Control?

Some important settings are:

| Field                 | Purpose                                |
| --------------------- | -------------------------------------- |
| Document Type         | Type of sales document                 |
| Number Range          | Sales document numbering               |
| Document Category     | Inquiry, Order, Quotation, etc.        |
| Delivery Block        | Blocks delivery if required            |
| Billing Block         | Blocks billing if required             |
| Delivery Type         | Which delivery document can be created |
| Billing Type          | Which billing document can be created  |
| Item Number Increment | Item numbering (10, 20, 30...)         |

---

# ⭐ Most Important

Sales Document Type controls **what can happen next**.

Example

```
Inquiry
↓

Quotation
↓

Sales Order
↓

Delivery
↓

Billing
```

SAP checks the document type before allowing the next step.

---

# Example

If you create

```
Inquiry (IN)
```

SAP knows

✔ Customer is only asking.

Therefore

❌ No Delivery

❌ No Billing

---

If you create

```
Sales Order (OR)
```

SAP knows

✔ Delivery Allowed

✔ Billing Allowed

---

# Item Category (VOV7)

Now comes another important topic.

A Sales Order has two parts:

Header

↓

Items

Every line item has an Item Category.

---

Example

```
Sales Order

Header

↓

Laptop
Mouse
Keyboard
```

Each item gets an Item Category.

---

# Definition

Item Category controls the behavior of each item inside the Sales Document.

Configuration

```
SPRO

Sales

Sales Documents

Sales Document Item

Define Item Categories

T-Code : VOV7
```

---

# Common Item Categories

| Item Category | Used For       |
| ------------- | -------------- |
| TAN           | Standard Item  |
| REN           | Return Item    |
| AFN           | Inquiry Item   |
| AGN           | Quotation Item |
| BVN           | Cash Sales     |
| G2N           | Credit Memo    |
| L2N           | Debit Memo     |

These are exactly the values shown in your trainer's sheet.

---

# What does Item Category Control?

This is one of the favorite interview questions.

Item Category decides:

* 📦 Delivery Allowed?
* 💰 Billing Allowed?
* 📈 Pricing Active?
* 📦 Schedule Lines Required?
* 📊 Inventory Updated?
* 🔄 Returns Process?
* 🎁 Free Goods?

---

# Example

## TAN

Standard Sales Item

```
Pricing
✔

Delivery
✔

Billing
✔

Schedule Line
✔
```

Normal sales order.

---

## AFN

Inquiry Item

```
Pricing
Optional

Delivery
❌

Billing
❌
```

Customer is only asking.

---

## AGN

Quotation Item

```
Pricing
✔

Delivery
❌

Billing
❌
```

Only quotation.

---

## REN

Return Item

```
Customer returns goods

Reverse process
```

---

# Relation Between Document Type & Item Category ⭐⭐⭐

This is one of the most important concepts in SAP SD.

When you create a Sales Order, SAP automatically determines the Item Category.

It does **not** ask the user to enter it manually.

SAP uses this logic:

```text
Sales Document Type
        +
Item Category Group (Material Master)
        +
Usage
        +
Higher Level Item
        ↓
Item Category
```

Example:

```
Sales Document Type = OR

Material Item Category Group = NORM

↓

Item Category = TAN
```

Another:

```
Sales Document Type = IN

↓

Item Category = AFN
```

---

# ⭐ Where does Item Category Group come from?

This is exactly the type of question that appears in interviews.

| Field                  | Source                                     |
| ---------------------- | ------------------------------------------ |
| Sales Document Type    | VOV8                                       |
| Item Category Group    | Material Master (Sales: Sales Org. 2 View) |
| Item Category          | Automatically determined by SAP            |
| Schedule Line Category | Determined from Item Category + MRP Type   |

---

# Future Process Impact

Changing VOV8 or VOV7 changes how the entire O2C process behaves.

Example:

### If Delivery is disabled in Item Category

```
Sales Order

↓

Delivery

❌ Cannot Create
```

---

### If Billing is disabled

```
Sales Order

↓

Delivery

↓

Billing

❌ Not Allowed
```

---

### If Pricing is disabled

```
Sales Order

↓

Price = 0
```

---

### If Schedule Line is disabled

```
No stock check

No delivery
```

---

# Real SAP Flow

```text
Customer
      │
      ▼
Sales Document Type (VOV8)
      │
      ▼
Material Master
(Item Category Group)
      │
      ▼
Item Category Determination
      │
      ▼
Item Category (VOV7)
      │
      ▼
Controls
✔ Pricing
✔ Delivery
✔ Billing
✔ Inventory
✔ Schedule Line
```

---

# 🎯 Interview Questions

### What is a Sales Document Type?

A Sales Document Type defines the overall behavior of a sales document, such as inquiry, quotation, or sales order. It controls numbering, delivery, billing, pricing, and other header-level settings.

---

### What is an Item Category?

An Item Category controls the behavior of an individual line item in a sales document. It determines whether the item is relevant for delivery, billing, pricing, and schedule lines.

---

### How is Item Category determined?

```text
Sales Document Type
        +
Item Category Group (Material Master)
        +
Usage
        +
Higher Level Item
        ↓
Item Category
```

---

### Where does Item Category Group come from?

**Material Master → Sales: Sales Org. 2 View**

---

## 🧠 Memory Trick

* 📄 **VOV8** = **Header Control** (What kind of document is this?)
* 📦 **VOV7** = **Item Control** (How should each item behave?)

> Think of it this way:
>
> * **Sales Document Type (VOV8)** decides the rules for the **entire document**.
> * **Item Category (VOV7)** decides the rules for **each individual line item**.

This foundation will become extremely important when you study **Sales Orders (VA01), Copy Control, Schedule Line Categories (VOV6), Delivery (VL01N), and Billing (VF01)** because SAP determines many behaviors automatically based on these configuration settings.
