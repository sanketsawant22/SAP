---

# 1. Sales Document Type → VOV8 ⭐⭐⭐⭐⭐

**T-Code:** VOV8

Controls the behavior of the Sales Order.

When creating a Sales Order (VA01)

```
OR
↓
System reads VOV8
```

Important things controlled here:

* Delivery Document Type (LF)
* Billing Type (F2)
* Number Range
* Delivery Block
* Billing Block
* Credit Check
* Reference Mandatory or Not
* Document Pricing Procedure
* Immediate Delivery
* Shipping Conditions
* Incompletion Procedure
* Text Procedure
* Partner Determination Procedure

### Viva Question

**Q:** Where does Delivery Type LF come from?

**Answer**

> Delivery Type is assigned to the Sales Document Type in VOV8.

---

# 2. Item Category Determination → VOV4 ⭐⭐⭐⭐⭐

This is one of the favourite viva questions.

When user enters a material

```
Material = Laptop

System doesn't know whether

TAN
TANN
TAP
KEN
KBN
etc.
```

So SAP determines Item Category using VOV4.

System checks

```
Sales Doc Type
+
Item Category Group
+
Usage
+
Higher Level Item
```

↓

Determines

```
TAN
```

### Example

Sales Order

```
OR
```

Material

```
NORM
```

↓

Result

```
TAN
```

---

### Viva

**Q:** How does TAN come automatically?

Answer

> TAN is determined through Item Category Determination (VOV4) using Sales Document Type and Item Category Group.

---

# 3. Where does Item Category Group come from? ⭐⭐⭐⭐⭐

Many freshers forget this.

Item Category Group is maintained in

```
MM02/MM01

Sales Org 2 View
```

Example

```
NORM

ERLA

LUMF

DIEN
```

Without Item Category Group

↓

No Item Category

↓

Sales Order error.

---

### Viva

**Q:** Where is Item Category Group maintained?

Answer

> Material Master → Sales Org 2 View.

---

# 4. Schedule Line Category Determination → VOV5 ⭐⭐⭐⭐⭐

This is another important one.

Once Item Category is determined

System now determines Schedule Line Category.

It checks

```
Item Category

+

MRP Type
```

↓

Determines

```
CP

CN

CV

etc.
```

Configuration

```
VOV5
```

---

### Viva

**Q**

How does Schedule Line Category CP come automatically?

Answer

> It is determined through VOV5 based on Item Category and MRP Type.

---

# 5. Where does MRP Type come from?

Material Master

```
MM02

MRP1 View
```

Example

```
PD

VB

ND
```

---

# 6. Schedule Line Controls → VOV6 ⭐⭐⭐⭐⭐

Once Schedule Line Category is determined

Actual behavior is controlled in

```
VOV6
```

Controls

* Availability Check
* Transfer of Requirements
* Movement Type (601)
* Purchase Requisition
* Purchase Order
* Requirement Type
* Goods Movement
* Delivery Relevance

---

### Viva

**Q**

Where do we control movement type 601?

Answer

> Schedule Line Category (VOV6).

---

# 7. Item Category Controls → VOV7 ⭐⭐⭐⭐⭐

This is one of the most asked.

Controls

* Billing Relevant
* Delivery Relevant
* Schedule Line Allowed
* Pricing
* Returns
* Packing
* Statistical Item
* Business Item
* Automatic Batch Determination
* Item Relevant for Credit
* Completion Rule

---

Example

```
TAN

Delivery Relevant = Yes

Billing Relevant = Yes

Schedule Line Allowed = Yes
```

---

### Viva

**Q**

Where do we make an item non-delivery relevant?

Answer

> VOV7.

---

# 8. Copy Control ⭐⭐⭐⭐⭐

Many students forget this.

Without Copy Control

Nothing gets copied.

Configurations

```
VTAA
Quotation → Order

VTLA
Order → Delivery

VTFL
Delivery → Billing

VTFA
Order → Billing
```

---

### Viva

**Q**

Why is Delivery not getting created?

Possible reasons

* Copy Control missing (VTLA)
* Item not delivery relevant
* Schedule line not delivery relevant
* No confirmed quantity
* Delivery block
* Credit block

This answer impresses examiners because it shows troubleshooting knowledge rather than memorized theory.

---

# 9. Delivery Type Configuration

Configuration

```
OVLK
```

Controls

* Delivery Number Range
* Picking
* Packing
* Goods Issue
* Delivery Split
* Text Procedure

---

### Viva

Where is Delivery Type configured?

Answer

> OVLK.

---

# 10. Billing Type Configuration

Configuration

```
VOFA
```

Controls

* Number Range
* Posting Block
* Cancellation Billing Type
* Account Determination
* Billing Category

---

### Viva

Where is Billing Type configured?

Answer

> VOFA.

---

# 11. Partner Determination

Configuration

```
VOPA
```

Controls

```
Sold-to

Ship-to

Bill-to

Payer
```

---

### Viva

How does Ship-to Party come automatically?

Answer

> Through Partner Determination Procedure configured in VOPA.

---

# 12. Output Determination

Configuration

```
NACE
```

Controls

```
Invoice Print

Delivery Print

Order Confirmation

Email Output
```

---

# 13. Pricing Procedure Determination ⭐⭐⭐⭐⭐

Configuration

```
OVKK
```

System checks

```
Sales Area

+

Customer Pricing Procedure

+

Document Pricing Procedure
```

↓

Determines

```
Pricing Procedure
```

---

### Viva

How does SAP know which Pricing Procedure to use?

Answer

> Through OVKK using Sales Area, Customer Pricing Procedure, and Document Pricing Procedure.

---

# 14. Shipping Point Determination ⭐⭐⭐⭐⭐

One of the most practical questions.

System checks

```
Shipping Condition
(Customer Master)

+

Loading Group
(Material Master)

+

Delivering Plant
(Material Master)
```

↓

Determines

```
Shipping Point
```

Configuration

```
OVL2
```

---

### Viva

How is Shipping Point determined?

Answer

> Through OVL2 using Shipping Condition, Loading Group, and Delivering Plant.

---

# 15. Route Determination ⭐⭐⭐⭐

Configuration

```
OVTZ
```

System checks

```
Shipping Condition

+

Transportation Group

+

Departure Zone

+

Destination Zone
```

↓

Determines

```
Route
```

---

# 16. Plant Determination ⭐⭐⭐⭐

Configuration

```
OVXC (assign plant)

Customer-Material Info Record (if maintained)
```

Generally, the delivering plant is determined from the **Customer-Material Info Record**, **Customer Master**, or **Material Master** based on configuration.

---

# 17. Storage Location Determination

Configuration

```
MALA

RETA

etc.
```

Uses

```
Plant

Shipping Point

Storage Condition
```

↓

Determines

```
Storage Location
```

---

# 18. Availability Check

Configuration

```
OVZ9
```

Checks

```
Stock

Sales Orders

Reservations

Purchase Orders

Production Orders
```

---

# 19. Credit Management

Configuration

```
OVA8
```

Controls

```
Credit Limit

Risk Category

Credit Group
```

---

# 20. Incompletion Procedure

Configuration

```
OVA2

OVA3

OVA4
```

If mandatory data is missing

```
↓

Billing blocked

Delivery blocked
```

---

# 🔥 The determination flow you should remember

```
Sales Order (VA01)
        │
        ▼
Sales Document Type
        │
      VOV8
        │
        ├── Delivery Type (LF)
        ├── Billing Type (F2)
        ├── Doc Pricing Procedure
        └── Other controls
        │
        ▼
Material Entered
        │
        ▼
Item Category Determination
      VOV4
        │
        ▼
Item Category (TAN)
        │
        ▼
Item Category Control
      VOV7
        │
        ├── Delivery Relevant?
        ├── Billing Relevant?
        └── Schedule Line Allowed?
        │
        ▼
Schedule Line Determination
      VOV5
        │
        ▼
Schedule Line Category (CP)
        │
        ▼
Schedule Line Control
      VOV6
        │
        ├── Movement Type (601)
        ├── Availability Check
        └── Transfer of Requirements
```
