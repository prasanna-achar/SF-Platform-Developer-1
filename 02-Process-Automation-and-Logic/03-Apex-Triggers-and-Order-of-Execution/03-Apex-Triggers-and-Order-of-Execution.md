# 03 — Apex Triggers & Transaction Save Order of Execution

> **NotebookLM Study Guide:** Comprehensive breakdown of trigger events, context variables, recursion prevention, and the official 20-step save order of execution for Salesforce Platform Developer I (PD1).

---

## 1. Core Architecture & Mental Model

### What is an Apex Trigger?
An Apex trigger is apex code that executes before or after specific data manipulation language (DML) events occur on Salesforce records.

### The 7 Trigger Events:
```
BEFORE EVENTS (3):    AFTER EVENTS (4):
• before insert       • after insert
• before update       • after update
• before delete       • after delete
                      • after undelete
```

> ❌ **CRITICAL EXAM TRAP:** **`before undelete` DOES NOT EXIST!** You cannot inspect or modify a deleted record before it has been restored from the Recycle Bin. Undelete only has an `after` event.

---

## 2. Trigger Context Variables Matrix

| Context Variable | Type | `insert` | `update` | `delete` | `undelete` | Key Use Case & Rules |
| :--- | :--- | :---: | :---: | :---: | :---: | :--- |
| **`Trigger.new`** | `List<sObject>` | ✅ | ✅ | ❌ | ✅ | New versions of records. In `before` triggers, fields can be modified without DML! |
| **`Trigger.old`** | `List<sObject>` | ❌ | ✅ | ✅ | ❌ | Old versions of records. Read-only. |
| **`Trigger.newMap`** | `Map<Id, sObject>` | ❌ (Before)<br>✅ (After) | ✅ | ❌ | ✅ | Keyed by record `Id`. In `before insert`, `Id` is null so `newMap` is null! |
| **`Trigger.oldMap`** | `Map<Id, sObject>` | ❌ | ✅ | ✅ | ❌ | Old record map keyed by `Id`. Used to detect field value changes. |
| **`Trigger.isExecuting`** | `Boolean` | ✅ | ✅ | ✅ | ✅ | Returns true if current code is executing within a trigger context. |

---

## 3. Detecting Field Changes in Triggers

To run logic only when a field value actually changes:
```apex
trigger OpportunityTrigger on Opportunity (before update) {
    for (Opportunity opp : Trigger.new) {
        Opportunity oldOpp = Trigger.oldMap.get(opp.Id);
        
        // Detect if Stage has changed to 'Closed Won'
        if (opp.StageName == 'Closed Won' && oldOpp.StageName != 'Closed Won') {
            opp.CloseDate = Date.today(); // Modifying Trigger.new in before update requires NO DML!
        }
    }
}
```

---

## 4. The Official Save Order of Execution (Memorize This Flow!)

When a record is saved with an `insert`, `update`, or `upsert`, Salesforce executes automations in this strict order:

```
 1. System-Level Validation (checks required fields, layout rules, field formats).
 2. BEFORE Triggers execute.
 3. Custom Validation Rules execute (and system validation again).
 4. Duplicate Rules execute.
 5. Record saved to database (assigned ID, but NOT yet committed!).
 6. AFTER Triggers execute.
 7. Assignment Rules execute.
 8. Auto-Response Rules execute.
 9. Workflow Rules execute.
    └── If field update occurs: Re-runs Before Trigger + Validation + After Trigger ONCE!
10. Escalation Rules execute.
11. Flows execute (Record-Triggered After-Save Flows and Processes).
12. Entitlement Rules execute.
13. Roll-Up Summary Calculations update Parent records.
    └── Parent updates run the entire save cycle on the parent object!
14. Criteria-Based Sharing rules evaluated.
15. DML Committed to the database.
16. Post-Commit Actions execute (Outbound Messages, Emails sent).
```

### High-Yield Sequence Exam Traps:
* **Before Triggers fire BEFORE Validation Rules!** (You can fix or format fields in a before trigger before validation rules evaluate them).
* **Assignment Rules fire BEFORE Escalation Rules!**
* **Escalation Rules fire BEFORE Processes/Flows!**
* **After Triggers fire BEFORE Assignment Rules!**

---

## 5. Trigger Architecture Best Practices

1. **One Trigger Per Object:** Never create multiple triggers on the same object (the order of execution between triggers is non-deterministic).
2. **Logic-Less Triggers:** Delegate all processing to a **Trigger Handler Class**.
3. **Bulkification:** Assume `Trigger.new` always contains up to **200 records**. Never execute SOQL or DML inside a loop!
4. **Recursion Control:** Prevent infinite trigger looping using a static `Set<Id>` or `Boolean` flag in the handler class.

```apex
public class AccountTriggerHandler {
    private static Set<Id> processedIds = new Set<Id>();

    public static void handleAfterUpdate(List<Account> newAccounts) {
        List<Account> toProcess = new List<Account>();
        for (Account a : newAccounts) {
            if (!processedIds.contains(a.Id)) {
                toProcess.add(a);
                processedIds.add(a.Id);
            }
        }
        // Run logic on toProcess...
    }
}
```

---

## 6. High-Frequency Exam Traps & Distractors

| Trap Topic | What Looks Correct (The Distractor) | Why It's Dead Wrong | The Official Exam Rule |
| :--- | :--- | :--- | :--- |
| **`before undelete`** | "Use before undelete trigger to inspect deleted data" | Trigger event does not exist. | Only **`after undelete`** exists in Salesforce! |
| **`Trigger.newMap` in Insert**| Querying `Trigger.newMap` in `before insert` | Record IDs have not been generated yet. | `Trigger.newMap` is **null** in `before insert`. |
| **DML in Before Trigger**| Calling `update Trigger.new` in before update | Modifying fields on `Trigger.new` is automatic. | Calling DML on `Trigger.new` in before triggers causes **recursion error**. |
| **Execution Order** | Validation rules run before Before Triggers | Before triggers run first. | **Before Triggers $ightarrow$ Validation Rules $ightarrow$ After Triggers**. |
| **Automation Order** | Escalation rules run before Assignment rules | Assignment rules run first. | **After Trigger $ightarrow$ Assignment $ightarrow$ Escalation $ightarrow$ Flow**. |

---

## 7. Quick-Fire Summary Cards (NotebookLM Dialogue Ready)

* **Q: Which trigger event does not exist in Salesforce Apex?**  
  **A:** `before undelete` (only `after undelete` is supported).

* **Q: In which trigger context is `Trigger.newMap` null?**  
  **A:** In `before insert` (because records have not received their 18-character IDs yet) and all `delete` triggers.

* **Q: Do Validation Rules execute before or after Before Triggers?**  
  **A:** Validation Rules execute **after** Before Triggers.

* **Q: Between Assignment Rules, Escalation Rules, and Record-Triggered Flows, what is the exact execution order?**  
  **A:** Assignment Rules execute first, then Escalation Rules, then Flows.
