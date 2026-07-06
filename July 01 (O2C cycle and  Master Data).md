# 📘 Day 3 – SAP SD Notes

# 🛒 Order-to-Cash (O2C) Cycle | Customer Master | Material Master

---

# 💼 What is O2C (Order-to-Cash)?

**Order-to-Cash (O2C)** is the complete business process from receiving a customer's order until receiving the payment.

It is the core business process in **SAP SD**.

---

# 🔄 O2C Process

```text
Customer Inquiry (Optional)
        │
        ▼
Quotation (Optional)
        │
        ▼
📝 Sales Order
        │
        ▼
📦 Delivery
        │
        ▼
🚚 Post Goods Issue (PGI)
        │
        ▼
🧾 Billing (Invoice)
        │
        ▼
💰 Payment
```

---

# 📌 O2C Transaction Codes

| Process          | T-Code |
| ---------------- | ------ |
| Inquiry          | VA11   |
| Quotation        | VA21   |
| Sales Order      | VA01   |
| Delivery         | VL01N  |
| Post Goods Issue | VL02N  |
| Billing          | VF01   |

> 💡 Inquiry and Quotation are **pre-sales activities** and are optional.

---

# 📝 O2C Process Explained

## 1️⃣ Inquiry *(Optional)*

Customer asks about:

* Product
* Price
* Availability

No commitment from either side.

---

## 2️⃣ Quotation *(Optional)*

Company sends an official offer containing:

* Price
* Quantity
* Validity
* Terms & Conditions

Still no actual sale.

---

## 3️⃣ Sales Order

Customer accepts the quotation (or directly places an order).

Sales Order contains:

* Customer
* Material
* Quantity
* Delivery Date
* Pricing

> 🧠 Sales Order = Customer's confirmed purchase request.

---

## 4️⃣ Delivery

Warehouse prepares the goods.

Activities include:

* Picking
* Packing
* Loading

Delivery document is created.

---

## 5️⃣ Post Goods Issue (PGI)

Goods leave the warehouse.

Effects:

* 📦 Stock decreases
* 📊 Inventory updates
* Ownership transfers to the customer (business perspective)

> 🧠 PGI = Goods are dispatched.

---

## 6️⃣ Billing

Invoice is generated for the customer.

Contains:

* Invoice Number
* Material Details
* Price
* Taxes
* Total Amount

---

## 7️⃣ Payment

Customer pays the invoice.

Finance records the payment and completes the O2C cycle.

---

# 📋 What is Master Data?

**Master Data** is permanent business information used repeatedly in transactions.

It is created once and reused many times.

Examples:

* 👤 Customer Master
* 📦 Material Master

---

# 👤 Customer Master

Customer Master stores all information about a customer.

It is used while creating:

* Sales Orders
* Deliveries
* Billing

---

## Customer Master Contains

* Customer Number
* Customer Name
* Address
* Contact Details
* Payment Terms
* Shipping Details
* Sales Area
* Tax Information

---

## Customer Master Structure

Customer Master consists of three views:

### 🏢 General Data

Common information valid across the organization.

Examples:

* Name
* Address
* Phone
* Email

---

### 💰 Company Code Data

Finance-related information.

Examples:

* Reconciliation Account
* Payment Terms

---

### 🛒 Sales Area Data

Sales-related information.

Examples:

* Sales Organization
* Distribution Channel
* Division
* Shipping Conditions
* Pricing
* Customer Pricing Procedure

---

## 🧠 Remember

> Customer Master = Everything about the customer.

---

# 📦 Material Master

Material Master stores all information about a product.

It is used in:

* Sales
* Purchasing
* Inventory
* Production
* Warehouse Management

---

## Material Master Contains

* Material Number
* Description
* Unit of Measure
* Material Type
* Plant
* Storage Information
* Pricing
* Weight & Dimensions

---

## Material Master Views

Common views include:

* 📋 Basic Data
* 💰 Accounting
* 📦 Sales
* 🏭 MRP
* 🛒 Purchasing
* 📊 Storage
* 🚚 Warehouse

> Different departments use different views of the same material.

---

## 🧠 Remember

> Material Master = Everything about the product.

---

# 🔄 Master Data in O2C

```text
👤 Customer Master
          │
          ▼
     Sales Order
          ▲
          │
📦 Material Master
```

Without Customer Master and Material Master, a Sales Order cannot be created.

---

# 📚 Difference Between Customer Master & Material Master

| 👤 Customer Master              | 📦 Material Master               |
| ------------------------------- | -------------------------------- |
| Stores customer information     | Stores product information       |
| Used in Sales & Billing         | Used across multiple SAP modules |
| Created once, reused many times | Created once, reused many times  |

---

# 💡 Real-Life Example

A customer wants to buy **5 laptops**.

1. 👤 Customer Master identifies **who** is buying.
2. 📦 Material Master identifies **what** is being sold.
3. 📝 Sales Order records the purchase.
4. 📦 Delivery prepares the goods.
5. 🚚 PGI dispatches the goods.
6. 🧾 Billing generates the invoice.
7. 💰 Customer makes the payment.

---

# ⚡ Quick Revision

* ✅ **O2C = Order-to-Cash**
* ✅ Inquiry & Quotation are **optional**.
* ✅ **Sales Order** confirms the customer's purchase.
* ✅ **Delivery** prepares the goods.
* ✅ **PGI** dispatches the goods and reduces stock.
* ✅ **Billing** creates the invoice.
* ✅ **Payment** completes the process.
* ✅ **Customer Master** stores customer details.
* ✅ **Material Master** stores product details.
* ✅ Both Customer Master and Material Master are **Master Data** and are mandatory before creating a Sales Order.

---

# 🎯 Interview Questions

### What is O2C?

**Order-to-Cash (O2C)** is the end-to-end business process that starts with a customer's order and ends when the company receives payment.

---

### What is Master Data?

Master Data is permanent business information that is created once and reused in multiple business transactions.

---

### What is Customer Master?

Customer Master is a central record containing all customer-related information, including general, company code, and sales area data.

---

### What is Material Master?

Material Master is a central record containing all information related to a material or product, used by different departments such as Sales, Purchasing, Inventory, Production, and Finance.

---

# 📝 Important Transaction Codes

| Object                                  | T-Code    |
| --------------------------------------- | --------- |
| Customer Master (Create/Change/Display) | **BP**    |
| List Customers                          | **VCUST** |
| Material Master (Create)                | **MM01**  |
| List Materials                          | **MM60**  |
| Sales Order                             | **VA01**  |
| Delivery                                | **VL01N** |
| Billing                                 | **VF01**  |

> 💡 **Revision Trick:**
> **Customer ➜ Material ➜ Sales Order ➜ Delivery ➜ PGI ➜ Billing ➜ Payment**
> *(Know Who ➜ Know What ➜ Sell ➜ Deliver ➜ Ship ➜ Bill ➜ Get Paid)*
