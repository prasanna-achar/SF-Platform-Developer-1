# 02 — SOQL, SOSL & Safe Database DML Operations

> **NotebookLM Study Guide:** Comprehensive breakdown of relationship SOQL, SOSL search syntax, SOQL for loops, DML governor limits rollback, uncatchable exceptions, and security modes for Salesforce Platform Developer I (PD1).

---

## 1. Core Architecture & Mental Model

### SOQL vs. SOSL: When to Use Which?
* **SOQL (Salesforce Object Query Language):**
  - Used when you know **which object** contains the data.
  - Returns a list of records (`List<sObject>`).
  - Supports aggregate functions, relationship joins, and filtering.
* **SOSL (Salesforce Object Search Language):**
  - Used when you do **NOT know which object/field** contains the data, or when performing text searches across multiple objects simultaneously.
  - Uses the search index engine (faster for text searches).
  - Returns a **list of lists of sObjects** (`List<List<sObject>>`).

---

## 2. SOQL Relationship Queries

### 1. Child-to-Parent Queries (Dot Notation):
* Queries child object and traverses upwards to parent.
* Up to **5 levels** of parent relationships in a single query.
* Standard relationships: `Contact.Account.Name`.
* Custom relationships: append `__r` (`Position__r.Department__r.Name`).

```sql
SELECT Id, LastName, Account.Name, Account.Industry FROM Contact WHERE Account.BillingState = 'California'
```

### 2. Parent-to-Child Queries (Nested Subqueries):
* Queries parent object and retrieves child records as a sub-list.
* Only **1 level** of child relationship per query.
* Standard relationships use the **plural relationship name**:
  - Account to Contacts: `(SELECT Id, LastName FROM Contacts)`
  - Account to Opportunities: `(SELECT Id, Amount FROM Opportunities)`
* Custom relationships use the child relationship name appended with `__r`:
  - `(SELECT Id, Name FROM Job_Applications__r)`

```sql
SELECT Name, (SELECT LastName, FirstName FROM Contacts) FROM Account WHERE BillingState = 'Arizona'
```

---

## 3. Querying Multi-Select Picklists

Multi-select picklist filtering uses exact punctuation symbols:
* **Semicolon (`;`) = AND logic:** Matches records containing ALL listed values.
* **Comma (`,`) = OR logic:** Matches records containing ANY of the listed values.

```sql
-- Returns records with BOTH 'Public Relations' AND 'Investor Relations'
SELECT Id, Name FROM Account WHERE Coverage_Area__c INCLUDES ('Public Relations;Investor Relations')

-- Returns records with EITHER 'Public Relations' OR 'Investor Relations'
SELECT Id, Name FROM Account WHERE Coverage_Area__c INCLUDES ('Public Relations', 'Investor Relations')
```

---

## 4. SOSL Syntax & Return Types

```apex
List<List<sObject>> searchResults = [FIND 'San Francisco' IN ALL FIELDS RETURNING Account(Name, Industry), Contact(LastName, Email)];

// Result parsing:
List<Account> accounts = (List<Account>)searchResults[0];
List<Contact> contacts = (List<Contact>)searchResults[1];
```
* **Return Type:** Always **`List<List<sObject>>`**.
* If no records match for an object, an **empty list** is returned at that index.

---

## 5. SOQL For Loops & Heap Management

When processing large query results, standard queries load all records into memory at once, risking the **6 MB heap size limit**.

### The SOQL For Loop Solution:
```apex
// Processes records in memory-efficient batches of 200 records
for (List<Account> acctBatch : [SELECT Id, Name FROM Account WHERE Status__c = 'Active']) {
    for (Account a : acctBatch) {
        a.Description = 'Verified';
    }
    update acctBatch; // DML on 200 records at a time
}
```
* **Heap Optimization:** Records are garbage-collected after each iteration.
* **Total Query Limit:** Still governed by the **50,000 record query limit**!

---

## 6. DML Governor Limits & The Uncatchable Exception Rule

### The Hard Limit:
* **Maximum 150 DML Statements per Apex Transaction.**
* **Maximum 10,000 total records processed by DML.**

### The 151 DML Rollback Trap (Papers 18 & 19!):
```apex
for (Integer x = 0; x < 200; x++) {
    Account newAccount = new Account(Name = 'Account-' + x);
    try {
        insert newAccount; // DML inside loop!
    } catch(Exception ex) {
        System.debug('Caught Exception: ' + ex);
    }
}
```

* **Question:** *"How many accounts will be inserted?"*
* **The Trap Choice:** *"150 accounts will be inserted; 151st is caught."* ❌ **DEAD WRONG!**
* **The Reality:** **ZERO (0) ACCOUNTS WILL BE INSERTED!**
* **The Architectural Rule:**
  1. `System.LimitException` is an **uncatchable exception**!
  2. A standard `try/catch` block **CANNOT catch a governor limit exception**.
  3. When the 151st statement executes, the transaction aborts instantly and **rolls back the entire transaction**, undoing all 150 previous inserts!

---

## 7. Apex Security Enforcement Models

| Security Keyword / Mechanism | Enforces CRUD (Object)? | Enforces FLS (Field)? | Enforces Sharing (Record)? | Behavior on Violation |
| :--- | :---: | :---: | :---: | :--- |
| **`with sharing` (Class header)** | ❌ NO | ❌ NO | ✅ **YES** | Hides records outside sharing scope. |
| **`WITH SECURITY_ENFORCED` (SOQL)**| ✅ **YES** | ✅ **YES** | ❌ NO | **Throws `System.QueryException`** immediately. |
| **`WITH USER_MODE` (SOQL/DML)** | ✅ **YES** | ✅ **YES** | ✅ **YES** | Throws `SecurityException` on violation. |
| **`Security.stripInaccessible()`** | ✅ **YES** | ✅ **YES** | ❌ NO | **Silently strips** hidden fields; does NOT throw exception! |

---

## 8. High-Frequency Exam Traps & Distractors

| Trap Topic | What Looks Correct (The Distractor) | Why It's Dead Wrong | The Official Exam Rule |
| :--- | :--- | :--- | :--- |
| **DML Limit Exception** | "Catch block catches LimitException" | Governor limits bypass all try/catch blocks. | Limit exceptions are uncatchable $ightarrow$ **entire transaction rolls back**. |
| **SOSL Return** | `List<sObject>` or `Map<Id, sObject>` | SOSL searches multiple objects simultaneously. | Return type is **`List<List<sObject>>`**. |
| **Standard Subquery** | `(SELECT Name FROM Contact)` | Standard relationships require plural form. | Must use plural: **`(SELECT Name FROM Contacts)`**. |
| **Picklist Querying** | Comma for AND logic | Comma represents OR in multi-select picklists. | **Semicolon (`;`) = AND**, **Comma (`,`) = OR**. |
| **`stripInaccessible`** | Calling on instance | `stripInaccessible` is a static method. | Call on class: **`Security.stripInaccessible(...)`**. |

---

## 9. Quick-Fire Summary Cards (NotebookLM Dialogue Ready)

* **Q: What data type does a SOSL query evaluate to in Apex?**  
  **A:** `List<List<sObject>>`.

* **Q: What happens if an Apex transaction executes 151 individual DML insert statements wrapped in a try/catch block?**  
  **A:** The 151st call throws an uncatchable `LimitException`, aborting the transaction and rolling back all 150 previous inserts (0 records inserted).

* **Q: Which punctuation character represents 'AND' logic when querying a multi-select picklist field in SOQL?**  
  **A:** The semicolon (`;`).

* **Q: How does `Security.stripInaccessible()` differ from `WITH SECURITY_ENFORCED`?**  
  **A:** `Security.stripInaccessible()` gracefully strips fields the user cannot access, whereas `WITH SECURITY_ENFORCED` throws an unrecoverable `QueryException`.
