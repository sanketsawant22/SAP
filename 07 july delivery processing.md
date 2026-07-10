
# DELIVERY PROCESSING (SAP SD)

## Delivery Activities

Delivery processing consists of the following activities:

* Picking *(Optional)*
* Packing *(Optional)*
* Loading *(Optional)*
* Shipment / Transportation *(Optional, depends on business scenario)*
* **Post Goods Issue (PGI)** *(Mandatory before billing)*

> **Note:** Shipment is mainly used when SAP Transportation Management (TM) or Shipment Processing is implemented.

---

# Packaging

Packaging materials are created using material type:

**VERP** → Packaging Material

Examples:

* Carton Box
* Wooden Box
* Plastic Container
* Pallet

> **Correction:** It is **VERP**, not VERT.

---

# PGI (Post Goods Issue)

### Definition

Post Goods Issue (PGI) is the process where goods officially leave the company's inventory and are issued to the customer.

From this point:

* Goods leave company stock.
* Ownership (legal responsibility) is considered transferred to the customer according to the business process.
* Delivery becomes ready for billing.

---

# Effects of PGI

After PGI, SAP performs several updates automatically:

1. Inventory stock is reduced.
2. Inventory value is reduced.
3. Material Document is generated.
4. Accounting (FI) document is generated (if valuation is active).
5. Billing Due List is updated (delivery becomes billing relevant).
6. Delivery Due List is updated.
7. Document Flow is updated.
8. Goods Movement status becomes **Completed**.
9. Delivery status is updated.

> **Interview Point:** PGI creates both Inventory and Financial postings.

---

# Structure of Delivery Document

Delivery document contains only:

## Header Data

Controlled by:

* Delivery Document Type

Examples:

* Shipping Point
* Route
* Delivery Date
* Partner Data

---

## Item Data

Controlled by:

* Delivery Item Category

Examples:

* Material
* Quantity
* Picking Status
* Packing Status
* Batch
* Storage Location

---

### Schedule Line

Delivery documents **do not have Schedule Line data.**

Reason:

The schedule line in the Sales Order represents the confirmed quantity and delivery date.

When the Delivery Document is created, those confirmed schedule line quantities become delivery items.

Therefore, no separate schedule line exists in delivery documents.

---

# Delivery Document Types

Transaction Code:

**OVLK** → Define Delivery Types

Standard Delivery Types:

| Delivery Type | Description                                    |
| ------------- | ---------------------------------------------- |
| LF            | Standard Delivery (with Sales Order reference) |
| LO            | Delivery without Order Reference               |
| LR            | Returns Delivery                               |
| LP            | Delivery from Project                          |
| NL            | Stock Transfer / Replenishment Delivery        |

---

## Delivery Type controls

Delivery Type controls many settings like:

* Number Range
* Item Number Increment
* Storage Location Determination
* Route Determination
* Text Procedure
* Partner Determination
* Output Determination
* Delivery Split Criteria
* Document Flow

These settings determine system behavior while creating delivery documents.

---

## How to check Delivery Document Type

VL03N / VL02N

Header → Administration tab

There you can see the Delivery Document Type (LF, LO, etc.)

---

# Delivery Document Determination

Example:

Sales Order Type = **OR**

While creating Delivery,

SAP automatically creates Delivery Type **LF**.

Why?

Because in Sales Order configuration (**VOV8**),

Shipping section contains:

Delivery Type = **LF**

Process:

Sales Order (OR)

↓

SAP checks VOV8 configuration

↓

Finds Delivery Type = LF

↓

Creates Delivery Document as LF

---

# Delivery Item Category

Sales Order Item Category and Delivery Item Category are completely different.

Although names (like TAN) may be the same, they have different configurations.

Sales Order Item Category

Transaction:

**VOV7**

Delivery Item Category

Transaction:

**0VLP**

---

## Example

Suppose Delivery Item Category TAN is configured as:

Picking Relevant = No

Then:

Sales Order will still be created successfully.

But during Delivery,

Picking fields will not be available because Delivery Item Category controls delivery behavior.

This setting affects only Delivery processing.

---

# Minimum Delivery Quantity

In Delivery Item Category (0VLP),

Minimum Quantity indicator can be maintained.

Example:

If Indicator = B

System checks Minimum Delivery Quantity maintained in:

Material Master

↓

Sales Org 1 View

↓

Minimum Delivery Quantity

If picked quantity is below the minimum allowed quantity,

System gives an error.

---

# Packaging Control

Configured in:

**0VLP**

Possible options:

* Packing Allowed
* Packing Mandatory
* Packing Not Allowed

This determines whether packing activity is optional or compulsory during delivery.

---

# Item Category Determination

## Sales Order

Sales Order Item Category is determined using:

* Sales Document Type
* Item Category Group
* Usage
* Higher Level Item Category

---

## Delivery with Order Reference

When Delivery is created from a Sales Order,

Delivery Item Category is copied/determined from the Sales Order configuration.

---

## Delivery without Order Reference

Delivery Type:

LO

Standard Item Category:

DLN

Since there is no Sales Order,

SAP performs **Delivery Item Category Determination** instead of copying from Sales Order.

---

# Delivery Processing Checks

Before Delivery can be created,

SAP performs several checks.

## Header Level

* No Delivery Block should exist.
* Shipping Point must be determined.
* Delivery Type should be valid.

---

## Item / Schedule Line Level

* Delivery Block should be blank.
* Material should be Delivery Relevant.
* Confirmed Quantity should exist.
* Schedule Line should be Delivery Relevant.
* Material should be Available (Availability Check).

If any of these fail,

Delivery cannot be created.

---

# Exercise 1

## Delivery Without Order Reference

Transaction:

VL01N

Choose:

**Delivery without Order Reference**

Enter:

* Shipping Point
* Delivery Type = LO
* Sales Organization
* Distribution Channel
* Division

Continue.

Enter:

* Ship-to Party
* Material
* Quantity

Go to Picking tab

Enter:

* Storage Location
* Picked Quantity

Save

Perform PGI.

---

# PGI Reversal

Purpose:

Reverse a previously completed PGI.

Transaction:

VL09

Steps:

Enter:

* Shipping Point
* Delivery Number

Execute.

Select Delivery.

Click:

Reverse.

System displays success message.

Verification:

VL03N / VL02N

Document Flow

A **Reversal Goods Movement** document is created.

Goods Issue status changes back accordingly.

---

# Prerequisites for PGI Reversal

1. Entire Delivery is reversed.

Partial PGI reversal is not possible.

2. Billing must NOT be completed.

If Billing is completed,

First cancel Billing

↓

Then reverse PGI

---

# Effects of PGI Reversal

SAP automatically:

* Creates Reversal Material Document.
* Reverses Goods Movement.
* Increases Inventory Stock.
* Updates Delivery Status.
* Sets Goods Movement Status to **Not Yet Started**.
* Updates Sales Order Status back to **In Process**.
* Updates Document Flow.

---

# Transfer Order (Warehouse Management)

Transfer Order is required when Warehouse Management (WM) is active.

Instead of entering Pick Quantity manually,

Warehouse personnel create a Transfer Order.

After confirmation,

SAP automatically updates Pick Quantity.

---

# Configuration

Assign Warehouse to Plant

SPRO

Enterprise Structure

↓

Assignment

↓

Logistics Execution

↓

Assign Warehouse Number to Plant/Storage Location

Example:

Warehouse = 001 or 011 (depends on system)

---

Lean WM Configuration

SPRO

↓

Logistics Execution

↓

Shipping

↓

Picking

↓

Lean WM

↓

Assign Warehouse Number to Plant/Storage Location

Maintain:

* Warehouse Number
* Picking Storage Type (e.g., 005)

---

# Exercise

Create Sales Order

VA01

↓

Create Delivery

VL01N

↓

Enter Storage Location

↓

System detects Warehouse Management.

Manual Pick Quantity is disabled.

↓

Create Transfer Order.

---

## Method 1

Transaction:

LT03

Enter:

* Warehouse Number
* Delivery Number

Create Transfer Order.

---

## Method 2

VL02N

Subsequent Functions

↓

Create Transfer Order

↓

Post

↓

Transfer Order is created.

---

After Transfer Order

Return to VL02N.

System automatically updates:

* Picked Quantity
* Picking Status = **C (Completely Picked)**
* WM Activity Status = **C (Transfer Order Confirmed)**

Now perform PGI.

---

# Important Transactions

| T-Code | Purpose                            |
| ------ | ---------------------------------- |
| VL01N  | Create Delivery                    |
| VL02N  | Change Delivery                    |
| VL03N  | Display Delivery                   |
| VL09   | Reverse PGI                        |
| OVLK   | Define Delivery Types              |
| VOV7   | Define Sales Order Item Categories |
| 0VLP   | Define Delivery Item Categories    |
| LT03   | Create Transfer Order              |


These notes are now aligned with standard SAP SD concepts while preserving the practical workflow you learned in class.
