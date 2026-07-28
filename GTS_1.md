
# What is SAP GTS?

**GTS = Global Trade Services**

Think of it like this.

SAP SD asks:

> Can I create a Sales Order?

GTS asks:

> **Should I legally allow this Sales Order?**

That's the biggest difference.

---

Imagine Samsung sells laptops.

Customer:

> Russia

Material:

> Laptop

SAP SD checks

* Customer exists ✅
* Material exists ✅
* Stock exists ✅
* Pricing exists ✅

Sales Order gets created.

But...

Government says

> "You cannot export this product to this country."

SAP SD doesn't know international law.

So before the order proceeds...

SAP GTS checks.

If everything is legal

→ Continue.

If not

→ Block the order.

That's the purpose of GTS.

---

# Why was GTS created?

Imagine 30 years ago.

Most companies traded only inside their own country.

No big issue.

Today companies sell everywhere.

India
USA
Germany
Japan
China
Brazil

Every country has different

* taxes
* customs
* export laws
* import laws
* sanctioned countries
* restricted materials

Managing these manually became impossible.

So SAP created GTS.

---

# Simple Architecture

```
Customer creates Order

↓

SAP S4 / ECC
(SD/MM)

↓

SAP GTS

↓

Checks everything

↓

Approved?

↓

Yes
↓

Continue Delivery

or

Blocked
```

Think of GTS as a **security officer** standing between SAP ERP and the outside world.

---

# Why separate software?

People often ask

Why didn't SAP simply add these features inside SD?

Because trade laws change every month.

Countries keep changing regulations.

Instead of updating ERP every time,

SAP made one central software

SAP GTS.

Many ERP systems can connect to one GTS.

---

# Components of GTS

There are four.

```
SAP GTS

↓

1 Compliance Management

2 Customs Management

3 Risk Management

4 Electronic Compliance Reporting
```

Let's understand each one.

---

# 1 Compliance Management

This is the most important module.

Question:

> "Is this trade legally allowed?"

That's all.

It checks three major things.

---

## A) Sanctioned Party Screening (SPL)

Suppose customer is

```
ABC Corporation
```

But government blacklist contains

```
ABC Corporation
```

GTS immediately blocks.

Reason

Cannot trade with blacklisted companies.

Example

Terrorist organizations

Money laundering

Fake companies

Government blacklist

---

Think of it as

```
Customer

↓

Is customer blacklisted?

↓

Yes

↓

BLOCK
```

---

## B) Legal Control

Some products need licenses.

Example

Military equipment

Nuclear equipment

Satellite equipment

Chemicals

Medicines

Before exporting

Government license required.

GTS checks

License available?

Yes

Continue

No

Blocked.

---

## C) Embargo Check

Embargo means

Government has prohibited trade with a country.

Example

Country X banned exports to Country Y.

Customer belongs to Country Y.

GTS blocks immediately.

No further processing.

---

# Compliance Summary

```
Customer

↓

Blacklisted?

↓

License Required?

↓

Embargo?

↓

Everything OK?

↓

Allow
```

---

# 2 Customs Management

This is after goods are allowed.

Now question becomes

> How do we send goods through customs?

Imagine you're exporting laptops.

Airport customs asks

* Product code?
* Value?
* Country?
* Tax?
* Import duty?

Instead of filling everything manually,

GTS prepares it.

Functions include:

* Import Management
* Export Management
* Product Classification
* Duty Calculation
* Transit
* Customs Documents

---

## Product Classification

Every product gets a customs code.

Example

Laptop

HS Code

847130

Without correct classification

Export may be rejected.

---

## Duty Calculation

Suppose

Product Price

₹1,00,000

Import Duty

10%

GTS calculates

₹10,000

Automatically.

---

## Transit Procedure

Suppose

Germany

↓

France

↓

Italy

Goods travel through multiple countries.

Transit documents are required.

GTS manages them.

---

# 3 Risk Management

This module is more financial.

---

## Letter of Credit (LC)

Imagine

Indian company exports goods to USA.

Question

"What if buyer never pays?"

Bank says

"I guarantee payment."

This guarantee is called

Letter of Credit.

GTS tracks it.

---

## Preference Processing

Countries sign trade agreements.

Example

India exports to a country with reduced duty.

If product qualifies,

Customer pays lower customs duty.

GTS determines this automatically.

---

## Restitution

Mostly used in Europe.

Suppose

Government refunds part of export cost.

GTS calculates refund eligibility.

Mostly EU-specific.

---

# 4 Electronic Compliance Reporting

Mostly for Europe.

Example

Germany wants to know

How much steel entered France?

Companies periodically submit reports.

GTS generates them automatically.

---

# Regulatory Principles

This part confuses everyone.

It simply means

"What laws does GTS follow?"

There are different levels.

---

## International Laws

Applies worldwide.

Example

Chemical Weapons Convention

WTO

HS Convention

---

## Supranational

Applies to a group of countries.

Example

European Union.

---

## National

Country-specific.

USA has

EAR

ITAR

India has its own export laws.

Germany has its own.

Japan has its own.

---

# EAR

Export Administration Regulations

USA controls exports of

Electronics

Software

Technology

Dual-use items

---

# ITAR

International Traffic in Arms Regulations

Deals with

Weapons

Military equipment

Defense technology

Very strict.

---

# Free Trade Zone

Suppose

India imports from Nepal.

If both countries signed an agreement,

Tax may become

0%

or reduced.

Example from your notes

```
NAFTA

USA

Canada

Mexico
```

Today it is called

USMCA.

---

# GTS Architecture

This is important.

```
SAP S4

↓

Sales Orders

Purchase Orders

Customers

Materials

Vendors

↓

RFC / ALE

↓

SAP GTS

↓

Checks legality

↓

IDoc

↓

EDI

↓

Customs Authority
```

Let's simplify.

---

## Feeder System

Feeder System means

SAP ERP

or

SAP S4

This is where business happens.

Sales Orders

Purchase Orders

Materials

Customers

Everything originates here.

---

## SAP GTS

Only checks trade-related information.

It is not where Sales Orders are created.

---

# RFC

Remote Function Call.

Think of it as

One SAP system calling another SAP system.

Example

ERP says

"Hey GTS,

Please check Sales Order 10001."

That's RFC.

---

# ALE

Application Link Enabling.

Used to exchange business data.

Mostly via IDocs.

---

# IDoc

IDoc = SAP's standard message format.

Instead of copying database tables,

SAP sends an IDoc.

Example

Customer Master

↓

IDoc

↓

GTS

---

# Why Logical System?

Suppose

Client

100

200

300

all connect to one GTS.

If only client numbers were used,

There would be confusion.

So SAP uses

Logical System.

Example

```
ECCCLNT100

ECCCLNT200

ECCCLNT300
```

Each one is unique.

Defined in

```
BD54
```

---

# RFC Destination

Transaction

```
SM59
```

This tells SAP

"How do I reach another SAP system?"

Like saving someone's phone number before calling them.

---

# ALE Distribution Model

Transaction

```
BD64
```

This decides

"What data should be sent?"

Example

Send

✔ Customers

✔ Vendors

✔ Materials

Don't send

Payroll

Employees

Finance

---

# Master Data Transfer

Suppose

Material price changes.

Material description changes.

Customer address changes.

Do we resend everything?

No.

SAP creates

Change Pointer.

Only changed data gets transferred.

Very efficient.

---

Message Types

| Master Data | Message Type       |
| ----------- | ------------------ |
| Material    | /SAPSLL/MATMAS_SLL |
| Vendor      | /SAPSLL/CREMAS_SLL |
| Customer    | /SAPSLL/DEBMAS_SLL |

These names are worth memorizing—they're frequently asked in SAP GTS quizzes.

---

# Why some data is created directly in GTS?

Not everything exists in ERP.

Things like

* Customs Offices
* SPL Lists
* Classification Codes
* Licenses
* Bills of Product

belong only to international trade.

So they're maintained directly in GTS.

---

# Organization Structure

ERP

```
Company Code

↓

Plant
```

GTS creates its own organization.

```
Foreign Trade Organization

↓

Legal Unit
```

Why?

Because legal rules are usually country-based.

Example

Company Code

1000

2000

Both are in India.

Instead of repeating India's trade rules,

Assign both Company Codes to one

Foreign Trade Organization.

Less maintenance.

---

# How GTS Fits with What You Already Know

You already know this flow:

```
Inquiry

↓

Quotation

↓

Sales Order

↓

Delivery

↓

PGI

↓

Billing
```

Now insert GTS:

```
Inquiry

↓

Quotation

↓

Sales Order

↓

SAP GTS

↓

Compliance Check

↓

Approved?

↓

Delivery

↓

PGI

↓

Billing
```

If GTS blocks the order,

the business process stops until the issue (license, embargo, sanctioned party, etc.) is resolved.

---

# T-Codes from Today's Session

| T-Code                 | Purpose                    |
| ---------------------- | -------------------------- |
| `/SAPSLL/MENU_LEGALR3` | Feeder system GTS settings |
| `BD54`                 | Define Logical Systems     |
| `SCC4`                 | View Clients               |
| `SM59`                 | RFC Destinations           |
| `BD64`                 | ALE Distribution Model     |
| `/SAPSLL/CREMAS_DIRR3` | Manual Vendor Transfer     |
| `/SAPSLL/DEBMAS_DIRR3` | Manual Customer Transfer   |

---

# 10 Things to Remember Before Tomorrow

1. **GTS = Global Trade Services**.
2. **SAP ERP/S4 is the feeder system**; business transactions start there.
3. **GTS checks whether international trade is legally allowed** before processing continues.
4. **Compliance Management** = Sanctioned Party Screening + Legal Control + Embargo Checks.
5. **Customs Management** = Product Classification + Duty Calculation + Import/Export + Transit.
6. **Risk Management** = Letter of Credit + Preference Processing + Restitution.
7. **RFC** connects SAP systems, while **ALE/IDocs** transfer business data.
8. **Logical Systems (BD54)** uniquely identify connected SAP clients.
9. **Master data transfer** uses **Change Pointers** and message types like `MATMAS_SLL`, `CREMAS_SLL`, and `DEBMAS_SLL`.
10. **One GTS system can serve multiple ERP clients**, which is why logical systems are used instead of client numbers alone.

---
