# 03 — Debug Logs, Trace Flags & Developer Console

> **NotebookLM Study Guide:** Comprehensive breakdown of Debug Levels, Trace Flags, Developer Console log analysis, execution units, and Execute Anonymous mechanics for Salesforce Platform Developer I (PD1).

---

## 1. Core Architecture & Mental Model

### What is a Debug Log?
A Debug Log records database operations, system processes, and errors that occur when executing transactions.
* **Maximum Log Size:** **20 MB**. If a transaction generates more than 20 MB, the log is **truncated**!
* **Retention:** Stored for **24 hours** (or maximum of 1,000 MB total logs per org). Older logs are automatically overwritten.

---

## 2. Debug Levels vs. Trace Flags (The Core Distinction)

This is a **major exam point**. Know the exact difference:

| Concept | What It Specifies | Key Attributes & Settings |
| :--- | :--- | :--- |
| **Debug Level** | **The TYPE and AMOUNT of information logged.** | Set of log levels (`NONE`, `ERROR`, `WARN`, `INFO`, `DEBUG`, `FINE`, `FINER`, `FINEST`) across categories: *Apex Code, Apex Profiling, Callouts, Database, System, Validation, Visualforce, Workflow*. |
| **Trace Flag** | **WHO or WHAT is being monitored, and for HOW LONG.** | Identifies the Target Entity (`User`, `Apex Class`, `Apex Trigger`), Debug Level, Start Time, and Expiration Time. |

### Special Trace Flag: Platform Integration User
* Created and managed automatically by Salesforce internally (has **no license**, **cannot expire**).
* Set trace flag on this user to track **internal Salesforce background operations**:
  1. When an **Einstein Bot** creates cases.
  2. When **Einstein** writes prediction data to records.
  3. When data is synchronized using **Salesforce Integration Cloud**.
* ❌ Does NOT trace record-triggered flows or assignment rules (those run as the triggering user or Automated Process).

---

## 3. Log Truncation & Event Logging Mechanics

### Why Do Logs Get Truncated?
* When logging levels (e.g. `FINEST` on Database and Apex) cause the log to exceed the **20 MB limit**.
* Once truncated, you only see the beginning of the transaction; the critical exception at the end may be lost!

### Events Not Meeting Criteria vs. Disabled Events:
* **Criteria NOT Met:** (e.g., Workflow Rule or Flow decision criteria evaluates to false) $ightarrow$ **STILL LOGGED!** (The evaluation step itself is recorded).
* **Disabled Events:** (e.g., Trigger or Flow is deactivated) $ightarrow$ **NEVER LOGGED!** (Never fires, generates zero log lines).

---

## 4. Developer Console: Panels & Tools

```
Developer Console Architecture:
├── Logs Tab            -> Lists recent debug logs (filter by user/operation).
├── Tests Tab           -> Runs tests, monitors test pass/fail and suite execution.
├── Checkpoints Tab     -> Memory snapshots (Heap tab & Symbols tab).
└── Query Editor        -> SOQL/SOSL queries against the database.
```

### 1. The Logs Tab:
* By default, shows logs for the logged-in user only.
* **To see other users' logs:** Uncheck **`Debug > Show My Current Logs Only`**.
* ❌ **Exam Trap:** Debug logs do **NOT** track client-side JavaScript events! (Use Chrome DevTools for JS).

### 2. Execution Units & Log Hierarchy:
* An **Execution Unit** begins with `EXECUTION_STARTED`, contains nested `CODE_UNIT_STARTED` / `CODE_UNIT_FINISHED`, and ends with `EXECUTION_FINISHED`. All 4 lines represent parts of the single execution unit.

### 3. Checkpoint Inspector:
Set a checkpoint on a line of code to capture in-memory state:
* **Heap Tab:**
  - **Types:** Shows instantiated classes/objects and memory consumed in bytes.
  - **Instances:** Lists individual object instances with unique memory IDs.
  - **State:** Displays the exact field values of an object instance at that checkpoint.
* **Symbols Tab:** Tree view of all variable symbols in memory.

---

## 5. Execute Anonymous Truth Table

Running code via `Debug > Open Execute Anonymous Window`:

| Property | Execute Anonymous Rule | Why Developers Miss It |
| :--- | :--- | :--- |
| **Execution Context** | Runs in **USER MODE** (current user's permissions and sharing rules enforced). | Trigger/Class code runs in System Mode by default, but Anonymous runs as **User**! |
| **Database Commits** | Changes **ARE COMMITTED** to the database immediately. | ❌ It does NOT roll back automatically! |
| **User Methods** | **CAN declare and call user-defined methods** within the anonymous block. | Misconception that only inline statements are allowed. |
| **Supported Tools** | Developer Console, VS Code, Code Builder, SOAP API (`executeAnonymous`), Tooling API. | Multiple IDEs support it. |

---

## 6. High-Frequency Exam Traps & Distractors

| Trap Topic | What Looks Correct (The Distractor) | Why It's Dead Wrong | The Official Exam Rule |
| :--- | :--- | :--- | :--- |
| **Debug Level Purpose** | "Specifies the user being traced" | That is the definition of a Trace Flag! | Debug Level specifies the **type and volume of information logged**. |
| **Debugging Automation** | Using Checkpoint Inspector for workflow bugs | Checkpoints inspect heap memory variables. | Use **User Trace Flag, Debug Log, and Log Inspector**. |
| **Dev Console Logs** | "Logs include JavaScript events" | Developer Console logs are 100% server-side. | Client-side JS errors must be debugged in the **browser console**. |
| **Execute Anonymous Mode** | "Runs in System Mode" | Anonymous Apex evaluates under running user context. | Runs strictly in **User Mode** with sharing and permissions enforced. |

---

## 7. Quick-Fire Summary Cards (NotebookLM Dialogue Ready)

* **Q: What is the maximum file size of a Salesforce debug log before it truncates?**  
  **A:** 20 Megabytes (20 MB).

* **Q: What is the functional difference between a Debug Level and a Trace Flag?**  
  **A:** A Debug Level defines the volume and filters of logged categories; a Trace Flag assigns that Debug Level to a specific target user, class, or trigger.

* **Q: How does Execute Anonymous handle database DML commits?**  
  **A:** All successful DML operations commit permanently to the database; there is no automatic rollback.

* **Q: How do you view debug logs generated by other users in the Developer Console?**  
  **A:** Uncheck the option `Debug > Show My Current Logs Only` in the Developer Console.
