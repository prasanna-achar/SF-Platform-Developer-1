# 05 — Asynchronous Apex Processing & Job Monitoring

> **NotebookLM Study Guide:** Comprehensive breakdown of Future methods, Queueable Apex, Batch Apex, Schedulable Apex, and SOQL job monitoring for Salesforce Platform Developer I (PD1).

---

## 1. Core Architecture & Mental Model

### Why Asynchronous Processing?
Asynchronous Apex executes in the background when server resources are available. It delivers:
* **Higher Governor Limits:** 60,000 ms CPU time (vs 10,000 ms sync) and 12 MB heap size (vs 6 MB sync).
* **Decoupled Execution:** User UI remains snappy without waiting for heavy batch calculations or external callouts.

---

## 2. The 4 Asynchronous Processing Tools (Comparison Matrix)

| Feature | `@future` Methods | Queueable Apex | Batch Apex | Schedulable Apex |
| :--- | :--- | :--- | :--- | :--- |
| **Interface / Annotation**| `@future` or `@future(callout=true)` | `implements Queueable` | `implements Database.Batchable<sObject>` | `implements Schedulable` |
| **Parameter Types** | **Primitives ONLY** (`Id`, `String`, etc.) | Complex types & **sObjects** | `Database.QueryLocator` or `Iterable` | `SchedulableContext` |
| **Returns Job ID?** | ❌ NO | ✅ **YES** (`System.enqueueJob()`) | ✅ **YES** (`Database.executeBatch()`) | ✅ **YES** (`System.schedule()`) |
| **Supports Chaining?** | ❌ NO (cannot call future from future) | ✅ **YES** (1 child job per parent) | ❌ NO (can only chain in `finish()`) | ❌ NO |
| **Primary Use Case** | Mixed DML (Setup + non-Setup objects), simple callouts. | Sequential background jobs, passing sObjects. | Large data volumes (**up to 50M records**). | Recurring time-based runs (Cron expressions). |

---

## 3. Asynchronous Deep Dives

### 1. Future Methods (`@future`):
```apex
public class AccountRemover {
    // Must be static and return void!
    @future(callout=true)
    public static void processCallout(List<Id> accountIds) {
        // Primitive arguments only!
    }
}
```
* **Mixed DML Error:** Occurs when inserting a Setup object (`User`, `UserRole`, `Group`) and a Non-Setup object (`Account`, `Contact`) in the same synchronous transaction. Moving one operation to an `@future` method resolves the error!

---

### 2. Queueable Apex:
```apex
public class ProcessOrdersQueueable implements Queueable, Database.AllowsCallouts {
    private List<Order> orders;

    public ProcessOrdersQueueable(List<Order> orders) {
        this.orders = orders;
    }

    public void execute(QueueableContext context) {
        // Process sObjects...
        
        // Chaining another job:
        if (!Test.isRunningTest()) {
            System.enqueueJob(new FollowUpQueueable());
        }
    }
}

// Enqueueing:
ID jobId = System.enqueueJob(new ProcessOrdersQueueable(orderList));
```

---

### 3. Batch Apex (`Database.Batchable`):
Used to process millions of records in chunks (default 200 records per chunk):

```apex
public class InactiveAccountBatch implements Database.Batchable<sObject>, Database.Stateful {
    public Integer totalProcessed = 0; // State preserved across batches because of Database.Stateful!

    public Database.QueryLocator start(Database.BatchableContext bc) {
        return Database.getQueryLocator('SELECT Id FROM Account WHERE Active__c = \'No\'');
    }

    public void execute(Database.BatchableContext bc, List<Account> scope) {
        totalProcessed += scope.size();
        delete scope;
    }

    public void finish(Database.BatchableContext bc) {
        System.debug('Total deleted: ' + totalProcessed);
        // Can chain another batch here!
    }
}
```

* **`Database.Stateful` Rule:** Without `Database.Stateful`, instance variables reset to initial values between chunk executions. With it, state is preserved across all batches!

---

### 4. Schedulable Apex:
```apex
public class DailyCleanupSchedule implements Schedulable {
    public void execute(SchedulableContext sc) {
        Database.executeBatch(new InactiveAccountBatch(), 200);
    }
}

// Scheduling via Cron:
String cronExp = '0 0 1 * * ?'; // Every day at 1:00 AM
System.schedule('Daily Account Cleanup', cronExp, new DailyCleanupSchedule());
```

---

## 4. Monitoring Asynchronous Jobs via SOQL

This is a **major syntax exam trap**:

```sql
-- 1. Monitoring Cron Scheduled Jobs:
-- ⚠️ Field is 'State', NOT 'Status'!
SELECT Id, State, TimesTriggered, NextFireTime FROM CronTrigger WHERE Id = :cronId

-- 2. Monitoring Asynchronous Jobs (Batch, Queueable, Future):
-- ⚠️ Field is 'Status', NOT 'State'!
SELECT Id, Status, JobType, NumberOfErrors, TotalJobItems FROM AsyncApexJob WHERE JobType = 'BatchApex'
```

---

## 5. High-Frequency Exam Traps & Distractors

| Trap Topic | What Looks Correct (The Distractor) | Why It's Dead Wrong | The Official Exam Rule |
| :--- | :--- | :--- | :--- |
| **Cron SOQL Field** | `SELECT Status FROM CronTrigger` | Field does not exist on CronTrigger. | CronTrigger field is **`State`**! |
| **Async SOQL Field** | `SELECT State FROM AsyncApexJob` | Field does not exist on AsyncApexJob. | AsyncApexJob field is **`Status`**! |
| **Future Parameters** | Passing `List<Account>` to `@future` | Future methods only accept primitives. | Must pass **`List<Id>`** and query inside method. |
| **Batch State** | Expecting class variables to retain state | Resets to default values per batch chunk. | Must implement **`Database.Stateful`**. |
| **Queueable Chaining** | Calling multiple `enqueueJob` in 1 queueable | Synchronous transactions allow 1 child job only. | Only **1 child job** can be enqueued from a parent. |

---

## 6. Quick-Fire Summary Cards (NotebookLM Dialogue Ready)

* **Q: Why would an Apex developer use Queueable Apex instead of an `@future` method?**  
  **A:** When they need to pass complex types/sObjects, obtain a Job ID, or chain sequential background jobs.

* **Q: Which SOQL field tracks the execution status on `CronTrigger` versus `AsyncApexJob`?**  
  **A:** `CronTrigger` uses `State`, while `AsyncApexJob` uses `Status`.

* **Q: How can a Batch Apex class maintain a cumulative record counter across all batch chunks?**  
  **A:** By declaring `implements Database.Stateful` on the batch class definition.

* **Q: How does a developer resolve a "Mixed DML" error between Setup and non-Setup objects?**  
  **A:** By separating the DML operations into an asynchronous thread using an `@future` method or `Queueable` job.
