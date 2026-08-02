# 📘 OUTPUT DETERMINATION

## 📌 What is Output Determination?

**Output Determination** is the process of deciding:

* 📄 **Which output document** should be generated
* ⏰ **When** it should be generated
* 📤 **How** it should be sent
* 👤 **To whom** it should be sent

---

## 📄 Examples of Output Documents

* 📝 Sales Order Confirmation
* 🚚 Delivery Note
* 🧾 Invoice / Bill
* 📦 Shipping Notification
* 📋 Packing List

---

## 📤 Output Can Be Sent Through

* 🖨 Print
* 📧 Email
* 📠 Fax
* 🔄 EDI (Electronic Data Interchange)

---

# ⭐ Importance of Output Determination

### 👥 Customer Communication

Automatically sends:

* Sales Order Confirmation
* Delivery Note
* Invoice

---

### ⚡ Operational Efficiency

* Automates document generation
* Reduces manual work
* Saves time
* Reduces errors

---

### 📜 Compliance & Transparency

* Generates legal/business documents correctly
* Maintains complete audit trail of outputs

---

# 🔄 Condition Technique

Output determination follows the **Condition Technique**.

It consists of:

1. 📋 Condition Tables
2. 🔍 Access Sequence
3. 📨 Output Types
4. 📑 Output Procedure
5. 🔗 Output Procedure Determination

Output Procedure Determination is done separately for:

* 📄 Sales Document
* 🚚 Delivery Document
* 🧾 Billing Document

---

# 📌 Standard Output Types

| Output Type | Purpose                  |
| ----------- | ------------------------ |
| AF00        | Inquiry                  |
| BA00        | Sales Order Confirmation |
| LD00        | Outbound Delivery        |
| RD00        | Billing Document         |
| LP00        | Scheduling Agreement     |
| RD03        | Cash Sales Invoice       |
| AN00        | Quotation                |

---

# 📝 Condition Records

| Document          | T-Code   |
| ----------------- | -------- |
| Sales Document    | **VV11** |
| Delivery Document | **VV21** |
| Billing Document  | **VV31** |

---

# ⚙ Methods of Output Determination

### 1️⃣ NAST Method (Old)

Traditional output determination process.

---

### 2️⃣ BRF+ (New - S/4HANA)

Business Rules Framework Plus

Modern output management used in S/4HANA.

---

# 🧪 Exercise - Sales Document Output Determination

## Step 1️⃣ Create Access Sequence

**SPRO**

```
Sales & Distribution
   ↓
Basic Functions
   ↓
Output Determination
   ↓
Output Determination Using Condition Technique
   ↓
Maintain Output Determination for Sales Documents
```

Go to

```
Maintain Access Sequences
```

Steps

* New Entries
* Create new Access Sequence
* Select Accesses
* New Entry
* Access No → 10
* Table → 001
* Select Fields
* Press Enter until complete
* Save

---

## Step 2️⃣ Create Output Type

**SPRO**

```
Sales & Distribution
   ↓
Basic Functions
   ↓
Output Determination
   ↓
Output Determination Using Condition Technique
   ↓
Maintain Output Determination for Sales Documents
   ↓
Maintain Output Types
```

Steps

* Edit
* Search **ZG10**
* Copy and create your own Output Type
* Open it

### General Data

Assign the Access Sequence created above.

Save.

---

### Important Tabs

### ⏰ Time

Controls **when** output is generated.

---

### 👤 Partner Function

Controls **to whom** output should be sent.

---

### 🌍 Mail Title & Texts

Language settings for output.

---

### ⚙ Processing Routines

Controls how output is generated.

*(Usually not required during training.)*

---

## Step 3️⃣ Create Output Determination Procedure

```
Maintain Output Determination Procedure
```

Steps

* New Entry
* Create Procedure
* Select Control Data
* Step → 10
* Condition → Output Type created above
* Save

---

## Step 4️⃣ Assign Output Determination Procedure

Go to assignment.

Search required Sales Document Type.

Assign Output Determination Procedure.

Save.

---

## Step 5️⃣ Create Condition Record

**T-Code:** `VV11`

Enter Output Type.

> ❓ Why are Sales Organization and Customer fields shown?

✅ Because the selected **Condition Table** contains these fields.

Fill

* Sales Organization
* Customer
* Function → SP
* Medium → 1
* Dispatch Time → 4
* Language → EN

Select

```
Communication
```

Fill

* Output Device → LP01
* ✔ Release after Output

Save.

---

## Step 6️⃣ Test

Create Sales Order.

Go to

```
Extras
   ↓
Output
   ↓
Header
   ↓
Edit
```

You can see the Output generated.

Save.

---

# 🚚 Output Determination for Delivery

Configuration Path

```
SPRO
   ↓
Logistics Execution
   ↓
Shipping
   ↓
Basic Shipping Functions
   ↓
Output Control
   ↓
Output Determination
   ↓
Maintain Output Determination for Outbound Deliveries
```

---

# 💰 REVENUE ACCOUNT DETERMINATION

## 📌 What is Revenue Account Determination?

When a Billing Document is created, it contains pricing conditions such as:

* PR00
* K004
* K005
* K007

These values must be transferred to **Finance (FI)**.

The process of determining **which GL Account** should receive these values is called:

# 💰 Revenue Account Determination

---

## 🔄 Condition Technique

Revenue Account Determination also follows Condition Technique.

It consists of:

1. 📋 Condition Tables
2. 🔍 Access Sequence
3. 🏷 Condition Types
4. 📑 Revenue Account Determination Procedure
5. 💳 Assign GL Accounts

---

## 📌 Standard Condition Types

* KOFI
* KOFK

---

## 📌 Standard Revenue Account Determination Procedure

**KOFI00**

---

# ⭐ How Revenue Account is Determined (Very Important)

Combination of

* Application
* Chart of Accounts
* Condition Type
* Sales Organization
* Customer Account Assignment Group
* Material Account Assignment Group
* Account Key

↓

Determines

✅ GL Account

---

# 🖥 T-Code

**VKOA**

---

## Important Fields

### Application

Always

```
V
```

(V = SD)

---

### Chart of Accounts

Provided by FI.

Training example

```
INT
```

---

### Condition Type

* KOFI
* KOFK

---

### Customer Account Assignment Group

Comes from

```
Customer Master
   ↓
Billing Tab
```

---

### Material Account Assignment Group

Comes from

```
Material Master
   ↓
Sales Org 2
```

---

### Account Key

Comes from

```
V/08
```

---

The above combination determines

✅ GL Account

---

# 🧪 Exercise

## Step 1️⃣ Check Standard Configuration

**SPRO**

```
SAP Reference IMG
   ↓
Sales & Distribution
   ↓
Billing
   ↓
Basic Functions
   ↓
Account Assignment
   ↓
Revenue Account Determination
```

### Define Access Sequence & Account Determination Types

Option 2

Standard already available

* KOFI
* KOFK

No need to create.

---

Option 1

Used to view tables used by

* KOFI
* KOFK

---

## Step 2️⃣ Revenue Account Determination Procedure

**SPRO**

```
SAP Reference IMG
   ↓
Sales & Distribution
   ↓
Billing
   ↓
Basic Functions
   ↓
Account Assignment
   ↓
Revenue Account Determination
```

### Define & Assign Account Determination Procedure

#### Option 1

Search

```
KOFI00
```

You can verify

* KOFI
* KOFK

are assigned.

---

#### Option 2

Assign Procedure to Billing Document.

Check Billing Type

```
F2
```

Already assigned in standard.

No changes required.

---

## Step 3️⃣ Assign GL Accounts

**T-Code**

```
VKOA
```

Create entries

```
V   KOFI   INT   Sales Org   01   01   ERL   800000

V   KOFI   INT   Sales Org   01   01   ERS   889000

V   KOFI   INT   Sales Org   01   01   ERF   281100
```

Save.

---

# ✅ Verify Master Data

## Customer

**BP**

Customer

```
FLCU00
```

Check

* Company Code
* Reconciliation Account

---

```
FLCU01
```

Billing Tab

Check

* Account Assignment Group

---

## Material

**MM02**

Material

Go to

```
Sales Org 2
```

Check

* Account Assignment Group

---

# 🧪 Testing

## Create Sales Order

```
VA01
```

Enter

* Customer
* Material

Save.

---

## Create Delivery

```
VL01N
```

* Picking
* Transfer Order
* PGI

---

## Create Billing

```
VF01
```

Go to

```
Header
   ↓
Conditions
```

Pricing values

(PR00, K004, K007...)

will be posted to GL Accounts.

Complete Billing.

Save.

---

## Verify Accounting Document

Open

```
Document Flow
```

Accounting Document is generated automatically.

Open it to verify

* GL Accounts
* Amount posted to each account

---

# 📝 TEXT DETERMINATION

## 📌 Where Can Text Be Maintained?

* 👤 Customer
* 📦 Material
* 🤝 CMIR
* 📄 Sales Header → Sales Document Type
* 📦 Sales Item → Item Category
* 🚚 Delivery Header → Delivery Document Type
* 📦 Delivery Item → Delivery Item Category
* 🧾 Billing Header → Billing Document Type

---

## 🔄 Condition Technique

Text Determination also follows Condition Technique.

1. 📋 Condition Tables
2. 🔍 Access Sequence
3. 📝 Text Type
4. 📑 Text Determination Procedure
5. 🔗 Assign Text Determination Procedure

---

# 🧪 Exercise

**SPRO**

```
Sales & Distribution
   ↓
Basic Functions
   ↓
Text Determination
```

or

**T-Code**

```
VOTXN
```

---

### Sales Document Header

* Change
* Create Text Procedure (2-character code)

---

### Text IDs Procedure

* New Entry
* Sequence → 10
* Text ID → Created Text
* Save

---

### Text Procedure Assignment

Search Sales Document Type.

Assign Text Procedure.

Save.

---

### Test

Create Sales Order.

Go to

```
Header Data
   ↓
Texts Tab
```

You can verify the Text Procedure assigned.

---

# 🗄 ABAP Database Tables

### T-Code

```
SE16N
```

Used to display SAP database tables and view stored data.
