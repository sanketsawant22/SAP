# Return Process

## Types of Returns

1. **Administrative Return (Planned Return)**
2. **Counter Return (Unplanned Return)**
3. **Product Recall**
4. **Product Complaint**

---

# 1. Planned Returns (Administrative Returns)

A **planned return** is a return process where the customer informs the company in advance that they want to return the product.

### Reasons

* Defective product
* Damaged product
* Wrong product delivered
* Transit damage
* Late delivery
* Product does not meet customer requirements

After discussing the issue, the company decides how the return will be handled.

The customer may:

* Return the product to the company, or
* Send it directly to another customer/location as instructed by the company.

---

# 2. Unplanned Returns (Counter Returns)

An **unplanned return** generally occurs in the retail industry.

There is **no prior communication** with the company.

The customer directly visits the retailer/store and requests:

* Return
* Replacement
* Refund

This is handled according to the store's return policy.

---

# 3. Product Recall

A **product recall** occurs when the company identifies a defect or safety issue in a product or a particular production batch.

The company recalls all affected products from the market, even if customers have not yet reported any issues.

### Example

A food company recalls a batch because contamination is detected.

---

# 4. Product Complaint

A **product complaint** is raised when the customer experiences an issue after purchasing the product, usually during the warranty period.

Depending on the warranty terms, the company may:

* Repair the product
* Replace the product
* Refund the customer

---

# Return Process in SAP SD

### Normal Sales Process

1. Sales Order
2. Delivery

   * Picking
   * Transfer Order
   * PGI (Post Goods Issue)
3. Billing

After billing, if the customer raises a complaint, the return process begins.

---

# Return Process (Customer Wants Refund)

## Step 1 - Create Return Order

**T-Code:** `VA01`

**Sales Document Type:** `RE`

The Return Order can be created:

* With reference to the previous Sales Order
* With reference to the previous Invoice
* Independently (without reference)

**Important:**

* Return Reason is **mandatory**.
* A billing block is normally present and must be removed before billing.

---

## Step 2 - Create Returns Delivery

**T-Code:** `VL01N`

**Delivery Type:** `LR`

During returns processing:

* **Post Goods Receipt (PGR)** is performed.
* Goods are received back into inventory.

---

## Step 3 - Remove Billing Block

**T-Code:** `VA02`

Before creating the credit memo, remove the billing block from the Return Order.

---

## Step 4 - Create Credit Memo

**T-Code:** `VF01`

**Billing Type:** `RE`

This is an **Order-Related Billing** (controlled from **VOV7**).

Used when the customer wants **money back**.

---

# Replacement Process (Subsequent Delivery)

If the customer wants a **replacement** instead of a refund:

### Step 1

Create a new Sales Order with reference to the Return Order.

**T-Code:** `VA01`

**Sales Document Type:** `SD` / `SDF`

---

### Step 2

Create Outbound Delivery.

**T-Code:** `VL01N`

**Delivery Type:** `LF`

Process includes:

* Picking
* Transfer Order
* PGI

---

### Billing

No billing is created because the replacement is provided free of charge.

---

# Return Process Configuration

| Configuration          | Value                |
| ---------------------- | -------------------- |
| Sales Document         | **RE**               |
| Item Category          | **REN**              |
| Delivery Type          | **LR**               |
| Billing Type           | **RE (Credit Memo)** |
| Schedule Line Category | **DN**               |
| Movement Type          | **651**              |

---

# Subsequent Delivery (Replacement)

| Configuration          | Value          |
| ---------------------- | -------------- |
| Sales Document         | **SD / SDF**   |
| Item Category          | **KLN**        |
| Delivery Type          | **LF**         |
| Billing                | **No Billing** |
| Schedule Line Category | **CP / CN**    |
| Movement Type          | **601**        |

---

# Free of Charge Delivery

Used when the company wants to send products **without charging the customer**.

Examples:

* Product samples
* Promotional items
* Complimentary replacement

| Configuration          | Value          |
| ---------------------- | -------------- |
| Sales Document         | **FD**         |
| Item Category          | **KLN**        |
| Delivery Type          | **LF**         |
| Billing                | **No Billing** |
| Schedule Line Category | **CP / CN**    |
| Movement Type          | **601**        |

---

# ARM (Advanced Return Management)

**Sales Document Type:** `RE2`

**Item Category:** `REN`

In **Advanced Return Management (ARM)**:

* The system automatically creates the Returns Delivery.
* Most return-related documents are generated automatically from the Return Order, reducing manual effort.

---

# Process Flow

## Refund Process

```
Sales Order
      ↓
Delivery
(Picking → TO → PGI)
      ↓
Billing
      ↓
Customer Complaint
      ↓
Return Order (VA01 - RE)
      ↓
Returns Delivery (VL01N - LR)
(Post Goods Receipt)
      ↓
Remove Billing Block (VA02)
      ↓
Credit Memo (VF01 - RE)
```

---

## Replacement Process

```
Sales Order
      ↓
Delivery
      ↓
Billing
      ↓
Customer Complaint
      ↓
Return Order (VA01 - RE)
      ↓
Returns Delivery (VL01N - LR)
      ↓
New Sales Order (VA01 - SD/SDF)
      ↓
Outbound Delivery (VL01N - LF)
      ↓
Picking → TO → PGI
      ↓
No Billing
```

---

# Important T-Codes

| T-Code    | Description                                  |
| --------- | -------------------------------------------- |
| **VA01**  | Create Return Order / Subsequent Sales Order |
| **VA02**  | Change Return Order (Remove Billing Block)   |
| **VL01N** | Create Returns Delivery / Outbound Delivery  |
| **VF01**  | Create Credit Memo Billing                   |

---

# Exam Points to Remember

* **RE** → Return Order
* **LR** → Returns Delivery
* **RE Billing** → Credit Memo
* **Return Reason** is mandatory.
* **Billing Block** must be removed before creating the Credit Memo.
* **Movement Type 651** → Returns
* **Movement Type 601** → Normal Goods Issue / Replacement Delivery
* **Replacement Process** → No Billing
* **Free of Charge Delivery (FD)** → No Billing
* **ARM (RE2)** → Automatically creates return-related documents.
