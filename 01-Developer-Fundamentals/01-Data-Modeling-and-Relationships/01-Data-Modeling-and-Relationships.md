# 01 — Data Modeling, Object Architecture & Relationships

> **NotebookLM Study Guide:** Comprehensive breakdown of Salesforce data models, relationship types, ownership inheritance, deletion cascades, and high-frequency exam traps for Salesforce Platform Developer I (PD1).

---

## 1. Core Architecture & Mental Model

### Salesforce Multi-Tenant Data Architecture:
In the Force.com multi-tenant architecture, all orgs share the same database tables (`CustomObjects`, `CustomFields`). Salesforce uses metadata-driven architecture and polyglot persistence under the hood to isolate and serve data per tenant.

### The 5 Relationship Types:
```
1. Lookup Relationship          -> Loose coupling, independent ownership, optional parent.
2. Master-Detail Relationship   -> Tight coupling, inherited security & ownership, required parent.
3. Many-to-Many (Junction)      -> 2 Master-Detail relationships on a custom junction object.
4. Hierarchical Relationship    -> Self-lookup strictly on the USER object (User -> Manager).
5. External / Indirect Lookup   -> Connects External Objects (__x) with Salesforce objects.
```

---

## 2. Master-Detail vs. Lookup Relationship Matrix

| Feature / Behavior | Master-Detail Relationship | Lookup Relationship |
| :--- | :--- | :--- |
| **Parent Required?** | **YES.** Must always be populated. | **NO.** Can be optional or required. |
| **Cascade Deletion?** | **YES.** Deleting Master automatically deletes all Detail records. | **NO.** Detail records remain (unless lookup cascade delete is enabled by SF Support). |
| **Record Ownership** | **Inherited from Master.** Detail record has **NO Owner field**! | **Independent.** Child record has its own Owner field. |
| **Security & Sharing** | Detail inherits Master's sharing settings (Read on Master = Read on Detail). | Independent sharing and security per object. |
| **Roll-Up Summaries** | Supported on the Master object. | ❌ NOT supported (except the 3 standard lookup exceptions). |
| **Standard vs. Custom** | **Standard is ALWAYS the Master; Custom is ALWAYS the Detail.** | Any standard or custom object can be parent or child. |
| **Maximum per Object** | Maximum of **2** Master-Detail fields per custom object. | Up to **40** Lookup fields per object. |
| **Reparenting Allowed?**| Disabled by default; can be enabled via checkbox. | Always allowed by default. |

---

## 3. High-Frequency Architecture Rules

### 1. Queues and Record Ownership:
* **The Rule:** A Queue can ONLY own records that have an `Owner` field.
* **The Conflict:** Detail records in a Master-Detail relationship do **NOT** have an Owner field (ownership is inherited from the Master).
* **Exam Trap:** If a business requirement states that child records (such as Claims, Cases, or Approvals) must be assigned to a **Queue**, you **MUST use a Lookup Relationship**, NOT a Master-Detail relationship!

### 2. Hierarchical Relationships:
* A special self-referential lookup that exists **exclusively on the `User` object**.
* Used to link a user to another user (e.g., Manager, Approver).
* ❌ **Exam Trap:** You CANNOT create a Hierarchical relationship on a Custom Object! To link a Custom Object (e.g. `Project__c`) to a User, you **MUST use a standard Lookup relationship** to User.

### 3. Junction Objects (Many-to-Many):
* Created by defining **two Master-Detail relationships** on a custom junction object (e.g., `Job_Application__c` linking `Position__c` and `Candidate__c`).
* **Primary vs. Secondary Master:**
  * **Primary Master:** The *first* Master-Detail relationship created. Determines the custom object's icon, color, and record detail page look.
  * **Secondary Master:** The *second* relationship created.
* **Deletion & Restore Mechanics (Major Exam Question!):**
  * Deleting *either* master record soft-deletes the junction records into the Recycle Bin.
  * Restoring *one* deleted master restores the associated junction records.
  * **The Trap:** If *BOTH* master records are deleted, the junction records are **permanently deleted** and CANNOT be restored from the Recycle Bin!

### 4. External IDs:
* An External ID field contains record identifiers from an external system outside Salesforce (e.g., SAP, Oracle ERP, Workday).
* **Supported Field Types (Choose 3 Trap!):**
  1. **Text**
  2. **Number**
  3. **Email**
  4. **Auto-Number**
  *(❌ Picklist, Phone, and URL CANNOT be External IDs!)*
* **Limits:** Maximum of **25 External ID fields** per custom object.
* **Unique Attribute:** Setting the External ID as `Unique` is optional but strongly recommended. If non-unique and an upsert matches 2+ records, Salesforce throws a `DUPLICATE_EXTERNAL_ID` exception.
* **Importing Child Records:** During data import (e.g., Contacts), child records can be linked to parent Accounts using the parent's Unique External ID in the CSV without needing the Salesforce 18-character Account ID!

---

## 4. High-Frequency Exam Traps & Distractors

| Trap Topic | What Looks Correct (The Distractor) | Why It's Dead Wrong | The Official Exam Rule |
| :--- | :--- | :--- | :--- |
| **Queue Assignment** | "Create Master-Detail relationship for claims queue" | Detail records have no Owner field; queues require an owner. | Must use **Lookup Relationship** if child records need queue assignment. |
| **Manager Field on Project** | "Create Hierarchical relationship from Project to User" | Hierarchical is strictly User-to-User only. | Must use a **Lookup relationship** pointing to User. |
| **Custom Object as Master** | "Custom object is master of Account" | Custom objects can never be the master of standard objects. | Standard object is **always** the Master; Custom is always Detail. |
| **External ID Types** | "External ID must be configured as a Text field type" | Text is not the only valid type. | Allowed: **Text, Number, Email, Auto-Number**. |
| **Converting Lookup to MDR** | "Convert lookup to master-detail directly" | Fails if any child records contain null lookup fields. | Every child record must have a valid parent populated before conversion! |

---

## 5. Quick-Fire Summary Cards (NotebookLM Dialogue Ready)

* **Q: Can a standard object ever be the detail record in a Master-Detail relationship?**  
  **A:** No. A standard object can only be the Master; a custom object is always the Detail.

* **Q: What happens if both parent master records of a junction object are deleted?**  
  **A:** The junction record is permanently deleted and cannot be restored from the Recycle Bin.

* **Q: What data types can be designated as an External ID?**  
  **A:** Text, Number, Email, and Auto-Number.

* **Q: Why can't a record in a Master-Detail relationship be assigned to a Queue?**  
  **A:** Detail records inherit ownership from the Master and do not have an Owner field, whereas Queues require an Owner field.
