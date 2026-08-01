# 📦 SAP SD – Material Determination (Material Substitution)

## 📖 What is Material Determination?

Material Determination is also called **Material Substitution**.

It is the process where SAP **automatically replaces one material with another during Sales Order processing**.

### 📝 Example

You enter:

* 📦 **M1**

SAP automatically replaces it with:

* 📦 **M2**

---

## ❓ Why is Material Determination Needed?

It helps avoid manual errors and ensures the correct material is sold.

### ✅ Common Scenarios

### 🛑 1. Product Obsolescence

The old material is no longer manufactured and is replaced by a new version.

Example:

```
Old Material → M1

↓

New Material → M2
```

Whenever the user enters **M1**, SAP automatically substitutes it with **M2**.

---

### 🎉 2. Limited Time Promotions

During promotional periods, one material can automatically be substituted with another promotional material.

---

### 👤 3. Customer-Specific Materials

Different customers may receive different substitute materials based on agreements.

---

# ⚙️ Condition Technique

Material Determination also follows the **Condition Technique**.

It consists of:

* 📋 Condition Table
* 🔍 Access Sequence
* 🏷 Material Determination Condition Type
* ⚙️ Material Determination Procedure
* ✅ Activation of Material Determination Procedure

---

# ⚙️ Configuration

## 1️⃣ Maintain Access Sequence

📍 **Path**

```text
SPRO
→ Sales and Distribution
→ Basic Functions
→ Material Determination
→ Maintain Prerequisites for Material Determination
→ Maintain Access Sequences
```

### Steps

* Create New Access Sequence
* Select the Access Sequence
* Double Click **Accesses**
* Add Condition Tables

| Step | Condition Table |
| ---: | --------------- |
|   10 | 001             |
|   20 | 002             |

Save ✅

---

## 2️⃣ Define Material Determination Condition Type

📍 **Path**

```text
SPRO
→ Sales and Distribution
→ Basic Functions
→ Material Determination
→ Define Condition Types
```

### Steps

* Create New Condition Type
* Assign the Access Sequence
* Save ✅

---

## 3️⃣ Maintain Material Determination Procedure

📍 **Path**

```text
SPRO
→ Sales and Distribution
→ Basic Functions
→ Material Determination
→ Maintain Determination Procedure
```

### Steps

* Create New Procedure
* Assign the Condition Type
* Save ✅

---

## 4️⃣ Activate Material Determination

📍 **Path**

```text
SPRO
→ Sales and Distribution
→ Basic Functions
→ Material Determination
→ Assign Determination Procedure to Sales Document Type
```

Search the Sales Document Type assigned by the trainer.

Assign:

* ✅ Material Determination Procedure

Save ✅

---

## 5️⃣ Define Substitution Reason

Create a new **Substitution Reason**.

### Fields

### ⚠️ Warning

If checked:

* SAP displays a warning before substitution in the Sales Order.

---

### 🔄 Entry

Controls **how substitution happens**.

Examples:

* Automatic
* Manual Selection
* User Choice

---

### 📄 Outcome

Controls **how materials appear in documents**.

Examples:

* Show only substituted material
* Show both original and substituted materials

Save ✅

---

# 📋 Master Data

Create two materials.

Example:

* M1 (Old Material)
* M2 (New Material)

---

# 📝 Create Condition Record

Transaction:

```text
VB11
```

Enter:

* Material Determination Condition Type
* Validity Period
* Substitution Reason
* Material Entered (Old Material)
* Substitute Material (New Material)
* Unit of Measure = EA

Save ✅

---

# 🛒 Testing

Create a Sales Order.

Enter:

* Old Material (M1)

Result:

```
M1
↓

Automatically replaced with

↓

M2
```

---

# 🎯 Quick Revision

✅ Create Access Sequence

⬇️

✅ Create Condition Type

⬇️

✅ Create Determination Procedure

⬇️

✅ Assign Procedure to Sales Document Type

⬇️

✅ Create Substitution Reason

⬇️

✅ Create VB11 Condition Record

⬇️

✅ Create Sales Order & Test

---

# 🚫 SAP SD – Material Listing & Exclusion

## 📖 Material Listing

Listing controls **which materials are allowed** for a customer or customer group.

### Example

Company has:

```
M1
M2
M3
...
MN
```

Customer should purchase only:

```
✅ M2
✅ M5
✅ M8
```

If the customer tries to enter another material, SAP will not allow it.

---

## 📖 Material Exclusion

Material Exclusion controls **which materials are NOT allowed**.

### Example

Customer should never purchase:

```
❌ M1
❌ M2
❌ M3
```

SAP prevents these materials from being entered in the Sales Order.

---

# ⚙️ Condition Technique

Material Listing & Exclusion also use the **Condition Technique**.

* 📋 Condition Table
* 🔍 Access Sequence
* 🏷 Listing/Exclusion Condition Type
* ⚙️ Procedure
* ✅ Activation

---

# ⚙️ Configuration

## 1️⃣ Maintain Access Sequence

📍 **Path**

```text
SPRO
→ Sales and Distribution
→ Basic Functions
→ Listing and Exclusion
→ Maintain Access Sequences
```

### Steps

* Create New Access Sequence
* Select it
* Double Click **Accesses**
* Add Condition Tables

| Step | Table |
| ---: | ----- |
|   10 | 001   |

Save ✅

---

## 2️⃣ Maintain Listing / Exclusion Types

📍 **Path**

```text
SPRO
→ Sales and Distribution
→ Basic Functions
→ Listing and Exclusion
→ Maintain Listing/Exclusion Types
```

### Steps

* Create New Type
* Assign Access Sequence
* Save ✅

---

## 3️⃣ Maintain Procedure

📍 **Path**

```text
SPRO
→ Sales and Distribution
→ Basic Functions
→ Listing and Exclusion
→ Maintain Procedure
```

### Steps

* Create New Procedure
* Select Procedure
* Go to **Control Data**
* Create New Entry

| Step | Condition Type      |
| ---: | ------------------- |
|   10 | Your Condition Type |

Save ✅

---

## 4️⃣ Activate Listing / Exclusion

> *(Trainer used Exclusion in the exercise.)*

Assign the created Procedure to the Sales Document Type provided by the trainer.

Save ✅

---

# 📦 Condition Records

Transaction:

```text
VB01
```

Enter:

* Listing / Exclusion Type
* Customer
* Materials

Save ✅

---

# 🛒 Testing

Create Sales Order.

* Use assigned Sales Document Type.
* Enter the Customer.

Try entering materials.

### Listing

✅ Only listed materials allowed.

### Exclusion

❌ Excluded materials are not allowed.

---

# 🎯 Quick Revision

✅ Access Sequence

⬇️

✅ Listing/Exclusion Type

⬇️

✅ Procedure

⬇️

✅ Activate for Sales Document Type

⬇️

✅ VB01 Condition Record

⬇️

✅ Test in Sales Order

---

# ⚠️ SAP SD – Incompletion Log

## 📖 What is an Incompletion Log?

An **Incompletion Log** identifies mandatory fields that are missing in a document.

If required information is not entered, SAP can block subsequent processing until the missing data is completed.

---

## ❓ Why is it Needed?

It prevents errors in later stages of the sales process by ensuring all required information is entered before proceeding.

---

## 📍 Incompletion Logs Can Be Maintained At

* 📄 Sales Header Data
* 📦 Sales Item Data
* 📅 Sales Schedule Line Data
* 🚚 Delivery Header Data
* 📦 Delivery Item Data
* 👥 Partner Functions

> ❌ **Billing does not have an Incompletion Log** because it is the final step in the sales process, and all data is copied from the Sales Order and Delivery.

---

# ⚙️ Configuration

There are **3 main configuration steps**.

## 1️⃣ Define Incompletion Procedure

This defines:

* Which fields are mandatory.
* What should happen if they are missing.

---

## 2️⃣ Assign Incompletion Procedure

Assign the created procedure to:

* Sales Document Type
* Item Category
* Schedule Line Category
* Delivery Type
* etc.

---

## 3️⃣ Define Status Groups

Status Groups specify **which subsequent transactions should be blocked** if mandatory fields are incomplete.

Examples:

* 🚫 Delivery
* 🚫 Billing
* 🚫 PGI

---

# 🧪 Exercise

## 1️⃣ Define Incompletion Procedure

📍 **Path**

```text
SPRO
→ Sales and Distribution
→ Basic Functions
→ Log of Incomplete Items
```

Choose the required object (Sales Header, Item, Delivery, etc.).

> Since standard combinations are already used, select an existing procedure (trainer suggested one after alphabet **H**) instead of creating a new one.

### Steps

* Open **Procedure**
* Go to **Fields**
* Remove existing fields (for practice)
* Save
* Create New Entries

Mandatory fields to add:

* 📝 Order Reason
* 📄 Customer Reference
* 🏢 Sales Office

Save ✅

---

## 🔍 Finding Technical Details of a Field

1. Open Sales Order.
2. Go to the required field.
3. Press **F1**.
4. Click **Technical Information** (wrench/tool icon).

You will find:

* 📋 Table Name
* 🏷 Field Name

---

## 📄 Fields to Maintain

| Field      | Description                                      |
| ---------- | ------------------------------------------------ |
| Table Name | From Technical Information                       |
| Field Name | From Technical Information                       |
| Screen     | Screen where the field appears                   |
| Status     | Controls what is blocked if the field is missing |
| Warning    | Shows warning message                            |
| Sequence   | Warning sequence                                 |

Save ✅

---

## 2️⃣ Assign Incompletion Procedure

Select the relevant object.

Assign:

* 📄 Sales Document Type (provided by trainer)
* 📋 Incompletion Procedure

### IC Dialog Box

If checked:

❌ SAP **will not allow the document to be saved** until all mandatory fields are completed.

Save ✅

---

# 🎯 Quick Revision

✅ Define Incompletion Procedure

⬇️

✅ Add Mandatory Fields

* Order Reason
* Customer Reference
* Sales Office

⬇️

✅ Assign Procedure to Sales Document Type

⬇️

✅ Define Status Group

⬇️

✅ Test by creating a Sales Order with missing mandatory fields
