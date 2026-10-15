# 03 — Schema Builder, Field Modifications & Apex Describe APIs

> **NotebookLM Study Guide:** Comprehensive breakdown of Schema Builder, field deletion rules, type conversion limits, Apex reflection APIs, and exam traps for Salesforce Platform Developer I (PD1).

---

## 1. Core Architecture & Mental Model

Salesforce provides two distinct ways to inspect and manipulate metadata:
1. **Declarative Tool:** **Schema Builder** (visual drag-and-drop canvas for data modeling).
2. **Programmatic API:** **Apex Schema Reflection / Describe APIs** (inspect objects, fields, and record types dynamically in code).

---

## 2. Schema Builder Capabilities & Hard Limits

### What You CAN Do in Schema Builder:
* Drag-and-drop to create custom objects, fields, and relationships.
* View object relationship lines (Master-Detail, Lookup).
* **Filter objects** on the canvas to declutter view.
* **Hide relationships** to optimize canvas performance.
* Pan around using the **Map view**.

### What You CANNOT Do (Major Exam Traps!):
* ❌ **Cannot hide individual fields** (you can only hide entire objects).
* ❌ **There is NO "lightweight mode"** in Schema Builder.
* ❌ Cannot set Field-Level Security (FLS) directly within the Schema Builder canvas.

---

## 3. Custom Field Deletion & Data Type Conversions

### 1. What Blocks Custom Field Deletion?
Salesforce enforces referential integrity. You CANNOT delete a custom field if it is referenced in:
* **Apex Classes or Triggers**
* **Visualforce Pages or Components**
* **Flows or Process Builders**
* **Formula Fields**
* **Sharing Rules** or Lead Assignment Rules

> ✅ **What does NOT block deletion:** Report filters (Salesforce automatically removes the deleted field from reports) and Dynamic SOQL (since dynamic queries are strings evaluated at runtime).

---

### 2. Custom Field Data Type Conversion Restrictions (Choose 3 Trap!):
* ❌ **Referenced in Code:** Cannot change data type if referenced in Apex or Visualforce!
* ❌ **Encrypted Fields:** Cannot change data type of Encrypted fields.
* ❌ **Knowledge 'File' Fields:** Cannot change data type of Knowledge 'File' fields under any circumstances.
* ⚠️ **Data Loss Warning:** Converting from `DateTime` to `Date`, or `Number` to `Text` can result in permanent data truncation or loss.

---

## 4. Apex Describe APIs & Schema Reflection

Dynamic Apex reflection allows code to inspect metadata at runtime without hardcoding object/field names:

```apex
// 1. Describe Apps & Tabs (Global)
List<Schema.DescribeTabSetResult> tabSets = Schema.describeTabs();
// TabSet = App; Tab = individual tab within that App!

// 2. Global SObject Describe
Map<String, SObjectType> globalDescribe = Schema.getGlobalDescribe();

// 3. Describe an SObject
Schema.DescribeSObjectResult dsr = Account.sObjectType.getDescribe();
// Alternative: Schema.DescribeSObjectResult dsr = Schema.SObjectType.Account;

// Check user CRED permissions:
Boolean canDelete = dsr.isDeletable();
Boolean canCreate = dsr.isCreateable();
Boolean canRead   = dsr.isAccessible();

// Record Type Describe Maps (CRITICAL EXAM METHODS!):
Map<String, RecordTypeInfo> rtByDevName = dsr.getRecordTypeInfosByDeveloperName(); // API Names
Map<String, RecordTypeInfo> rtByName    = dsr.getRecordTypeInfosByName();          // UI Labels
List<RecordTypeInfo> rtList            = dsr.getRecordTypeInfos();                // List (not Map!)
// ❌ Note: getRecordTypes() DOES NOT EXIST!

// 4. Describe a Field
Schema.DescribeFieldResult dfr = Account.AnnualRevenue.getDescribe();
DisplayType fieldType = dfr.getType();
Integer maxLength     = dfr.getLength(); // For Text/String fields
Integer maxDigits     = dfr.getDigits(); // For Integer fields
Integer decimalPlaces = dfr.getScale();  // For Double/Currency fields
Boolean treatNullAs0  = dfr.isFormulaTreatNullNumberAsZero();
```

---

## 5. High-Frequency Exam Traps & Distractors

| Trap Topic | What Looks Correct (The Distractor) | Why It's Dead Wrong | The Official Exam Rule |
| :--- | :--- | :--- | :--- |
| **Describe Tabs Return** | `List<Schema.DescribeTabResult>` | DescribeTabResult is for a single tab. | `Schema.describeTabs()` returns `List<Schema.DescribeTabSetResult>`. |
| **Record Type Map** | `dsr.getRecordTypeInfos()` | Returns a `List<RecordTypeInfo>`, not a Map. | Use **`getRecordTypeInfosByDeveloperName()`** to get a Map! |
| **Field Deletion Blocker** | "Report filters block field deletion" | Salesforce auto-removes field from reports. | Blocked by **Apex, Triggers, Flows, and Formulas**. |
| **Knowledge File Field** | "Can convert Knowledge File if not in Apex"| File field conversions are completely locked. | **Cannot convert Knowledge 'File' fields EVER.** |
| **Schema Canvas** | "Hide unused fields in Schema Builder" | Fields cannot be hidden individually. | Only entire **objects** can be hidden from canvas. |

---

## 6. Quick-Fire Summary Cards (NotebookLM Dialogue Ready)

* **Q: Which Apex method returns a map of record types keyed by their API Developer Names?**  
  **A:** `DescribeSObjectResult.getRecordTypeInfosByDeveloperName()`.

* **Q: What data structure does `Schema.describeTabs()` return?**  
  **A:** `List<Schema.DescribeTabSetResult>` (where each TabSet represents an application).

* **Q: Can you hide individual fields in the Schema Builder canvas to improve performance?**  
  **A:** No. You can hide entire objects and relationships, but not individual fields.

* **Q: How can Apex verify at runtime whether the running user has permission to delete an object?**  
  **A:** `Schema.DescribeSObjectResult.isDeletable()`.
