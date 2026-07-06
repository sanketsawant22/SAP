# 📘 Day 2 – SAP SD Notes

# 🔗 Definition vs Assignment | Plant & Storage Location

---

# 🤔 Definition vs Assignment

When you open **SPRO**, you'll mainly work with:

* ✅ **Definition**
* ✅ **Assignment**

---

# 🏗️ Definition

**Definition = Create an object.**

SAP only knows that the object exists; it is **not connected** to anything yet.

### Objects you define

* 🏢 Company Code
* 🛒 Sales Organization
* 🛍 Distribution Channel
* 📱 Division
* 🏭 Plant
* 📦 Storage Location
* 🏬 Sales Office
* 👨‍💼 Sales Group
* 📦 Shipping Point

### 🧠 Remember

> **Definition = Create**

---

# 🔗 Assignment

**Assignment = Connect existing objects.**

Example:

```text
Company Code
      │
      ▼
Sales Organization
```

Now SAP knows:

> This Sales Organization belongs to this Company Code.

### Common Assignments

* Company Code → Sales Organization
* Company Code → Plant
* Plant → Storage Location

### 🧠 Remember

> **Assignment = Connect**

---

# 🏭 Plant

A **Plant** is a physical location where products are manufactured, stored, or distributed.

Examples:

* 🏭 Factory
* 📦 Warehouse
* 🚚 Distribution Center

### Example

```text
Plant: Y100
Jack Pune Plant
```

### 🧠 Remember

> **Plant = Where products exist**

---

# 📦 Storage Location

A **Storage Location** is a specific area inside a Plant where stock is stored.

Example:

```text
Pune Plant
│
├── Finished Goods
├── Raw Material
├── Spare Parts
└── Returned Goods
```

### 🧠 Remember

> **Plant = Building**
> **Storage Location = Room inside the building**

---

# ⭐ Plant vs Storage Location

| 🏭 Plant                        | 📦 Storage Location  |
| ------------------------------- | -------------------- |
| Physical location               | Area inside a Plant  |
| Factory / Warehouse             | Specific stock area  |
| Can have many Storage Locations | Belongs to one Plant |

---

# 🔗 Enterprise Structure

```text
                    🏢 Company Code
                     /          \
                    /            \
             🏭 Plant      🛒 Sales Organization
                 │                 │
        📦 Storage Location   ┌────┴────┐
                              │         │
                      🛍 Distribution  📱 Division
                          Channel
                              │
                              ▼
                        ⭐ Sales Area
                              │
                              ▼
                        🏬 Sales Office
                              │
                              ▼
                        👨‍💼 Sales Group
                              │
                              ▼
                        📦 Shipping Point
```

---

# 🤔 Why do we need both Plant & Sales Organization?

Every sales process involves different organizational units.

```text
Customer
     │
     ▼
🛒 Sales Organization
(Takes the order)
     │
     ▼
🏭 Plant
(Checks stock)
     │
     ▼
📦 Shipping Point
(Dispatches goods)
     │
     ▼
Customer receives product
```

### 🧠 Easy Remember

* 🛒 Sales Organization = Takes Orders
* 🏭 Plant = Stores/Produces Goods
* 📦 Shipping Point = Ships Goods

---

# 📚 Quick Memory Table

| SAP Object              | Remember As          |
| ----------------------- | -------------------- |
| 🏢 Company Code         | Legal Company        |
| 🛒 Sales Organization   | Sales Department     |
| 🛍 Distribution Channel | Selling Method       |
| 📱 Division             | Product Category     |
| 🏭 Plant                | Factory / Warehouse  |
| 📦 Storage Location     | Section inside Plant |
| 🏬 Sales Office         | Regional Office      |
| 👨‍💼 Sales Group       | Sales Team           |
| 📦 Shipping Point       | Dispatch Point       |

---

# ⚡ Quick Revision

* ✅ **Definition = Create organizational units**
* ✅ **Assignment = Connect organizational units**
* ✅ **Plant = Physical location where goods are stored/produced**
* ✅ **Storage Location = Specific stock area inside a Plant**
* ✅ **Company Code → Plant** (Assignment)
* ✅ **Plant → Storage Location** (Assignment)
* ✅ **Sales Organization** handles sales.
* ✅ **Plant** provides the goods.
* ✅ **Shipping Point** dispatches the goods.

This version keeps all the important concepts, removes repeated explanations, and is compact enough to revise quickly before practice sessions or interviews.
