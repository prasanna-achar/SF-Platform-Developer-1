# Exam Paper 18: Practice Exam 3 (Full 60 Questions)

**Date**: 2026-10-14  
**Score**: 42/60 (70.00% Raw)  
**Weighted Score**: 69.97% (Passing score is 68.00%)  
**Time Taken**: 53m 43s  
**Status**: ⚠️ BARELY PASS (+1.97% buffer)

---

## 📊 Section Breakdown

| Section | Score | Weight | Result | Status |
| :--- | :---: | :---: | :---: | :--- |
| **User Interface** | 80.00% (12/15) | 25% | 20.00% | 🟢 Strong (+3.00% over passing) |
| **Process Automation & Logic** | 76.47% (13/17) | 28% | 21.41% | 🟢 Solid (+2.37% over passing) |
| **Developer Fundamentals** | 68.75% (11/16) | 27% | 18.56% | 🟡 On the edge (+0.20% over passing) |
| **Testing, Debugging & Deployment** | **50.00% (6/12)** | **20%** | **10.00%** | 🔴 **CRITICAL ANCHOR (-3.60% below passing)** |
| **TOTAL** | **70.00%** | **100%** | **69.97%** | ⚠️ **Barely Passing** |

---

## 🎯 The Diagnosis: Why Your Score Is Stuck at 70%

1. **Testing is Dragging Your Entire Score Down**:
   - In 60-question exams, you consistently hit 33% to 50% on Testing & Deployment.
   - If you answered just 3 more questions correctly in Testing, your overall score would jump to **75.0%**.
   - If you hit 83% (10/12) in Testing, your total score exceeds **80.0%**.

2. **The 3 Zombie Mistakes (Still Repeating)**:
   - **Application Event**: 4th time missed. (When components "may or may not share a parent", it's ALWAYS an Application Event!).
   - **Person Accounts**: 3rd time missed. (Cannot write to `IsPersonAccount`; use a custom formula field for full name display).
   - **Execute Anonymous Context**: 2nd time missed. (Runs in User Mode as the current user, changes DO commit to the DB).

3. **Missing the "Declarative First" Reflex**:
   - In modern PD1 questions, Salesforce heavily prioritizes declarative options over custom code.
   - Flow Builder HTTP Callout > Apex Callout.
   - Record-Triggered Flow (after-save) > Apex Trigger for publishing Platform Events.

---

## ❌ All 18 Missed Questions & Key Takeaways

### 🔴 Section 1: Testing, Debugging & Deployment (6 Missed)

#### 1. Test Data Access without `SeeAllData=true` (Choose 3)
- **Trap**: Thinking Custom Settings are automatically visible.
- **Rule ("UPR-O")**:
  - ✅ Visible: **User**, **Profile**, **RecordType**, **Organization**, **CronTrigger**.
  - ❌ NOT visible: Custom Objects, standard business records, and **Custom Settings** (must create in `@testSetup` or test method).

#### 2. Debug Log Truncation & Missing Logs (Choose 3)
- **Trap**: Thinking "events not meeting criteria" don't appear in logs.
- **Rule**:
  - Evaluation lines for unmet criteria **are** logged.
  - **Disabled** events never fire and will NOT appear in logs.
  - Debug levels that are too detailed cause log truncation (maximum log size is 20 MB).

#### 3. Unit Test Statements (Choose 2)
- **Trap**: Confusing `System.assert()` with `System.runAs()`.
- **Rule**:
  - `System.assert()` / `Assert` class tests values and conditions.
  - `System.runAs()` executes code in different user contexts (permissions/sharing).
  - **Lines of code in test methods do NOT count toward the 75% code coverage requirement!**

#### 4. Execute Anonymous Statements
- **Trap**: Thinking anonymous code runs in system mode or rolls back.
- **Rule**:
  - Runs in **User Mode** (enforces object and field level permissions).
  - DML operations **ARE committed** to the database (no auto-rollback).
  - **CAN declare and execute user-defined methods** within the anonymous block.

#### 5. Developer Console Test Suites (Choose 3)
- **Trap**: Thinking Suite Manager is used to abort running tests.
- **Rule**:
  - **Suite Manager**: Used to create, edit, and delete test suites.
  - **Rerun Failed Tests**: Re-executes only the failed tests from the selected run.
  - Running a single test class executes synchronously; running multiple test classes executes asynchronously.

#### 6. Change Sets vs. Salesforce CLI (Choose 2)
- **Trap**: Thinking CLI is limited to related orgs.
- **Rule**:
  - **Change Sets**: Limited to related orgs (sandbox to production), GUI only, cannot delete metadata.
  - **Salesforce CLI**: Can deploy between **any** orgs, scriptable/automatable in CI/CD, can delete metadata via destructive changes.

---

### 🟡 Section 2: Developer Fundamentals (5 Missed)

#### 7. CURRENCYRATE Formula Function
- **Trap**: Guessing `CURRENCY()` or `CONVERSIONRATE()`.
- **Rule**:
  - `CURRENCYRATE(CurrencyIsoCode)` converts amounts to the corporate currency.
  - Requires `TEXT(CurrencyIsoCode)` because ISO code is technically a picklist.

#### 8. Aura Event Communication — Unrelated Components
- **Trap**: Choosing `reRender` (which is Visualforce).
- **Rule**:
  - Components that **may or may not share a parent** → **Application Event** (`<aura:event type="APPLICATION">`).
  - Components in a direct parent-child containment hierarchy → **Component Event**.

#### 9. External ID Field Properties (Choose 3)
- **Trap**: Believing External IDs must only be Text.
- **Rule**:
  - Allowed data types: **Text, Number, Email, Auto-Number**.
  - Unique checkbox is **optional** (though recommended to avoid `DUPLICATE_EXTERNAL_ID` exceptions during upsert).

#### 10. Custom Field Data Type Change Limitations (Choose 3)
- **Trap**: Assuming Knowledge File fields can be converted if not referenced in code.
- **Rule**:
  - Cannot change type if referenced in Apex, Visualforce, or Flows.
  - Cannot change type of **Encrypted fields**.
  - Cannot change type of **Knowledge 'File' fields** under any circumstances.
  - Conversions can cause permanent data loss (e.g., DateTime to Date).

#### 11. Formula Field Capabilities & Restrictions (Choose 2)
- **Trap**: Trying to concatenate Long Text Area or Description fields.
- **Rule**:
  - ❌ Formulas CANNOT reference: Long Text Area, Rich Text Area, Encrypted Text.
  - ❌ Formulas are NOT supported in: Web-to-Lead, Web-to-Case.
  - ✅ Formulas CAN use `HYPERLINK()` and `GETSESSIONID()` to pass authenticated session links to external systems.

---

### 🟠 Section 3: Process Automation & Logic (4 Missed)

#### 12. Trigger Event Types (Choose 2)
- **Trap**: Selecting "before undelete".
- **Rule**:
  - Before events: `before insert`, `before update`, `before delete`.
  - After events: `after insert`, `after update`, `after delete`, `after undelete`.
  - **"before undelete" DOES NOT EXIST!** You cannot inspect a deleted record before it is restored.
  - Trigger definition specifies the Apex and API version.

#### 13. ApexDoc Comment Opening Delimiter (Choose 2)
- **Trap**: Thinking `/*` is valid ApexDoc.
- **Rule**:
  - ApexDoc opening delimiter MUST be `/**` (slash + double asterisk).
  - Parameter tags (`@param`) must follow the exact argument order in the method signature.
  - Inline tags (`{@code}`, `{@link}`, `{@hidden}`) must use curly braces.

#### 14. REST Integrations: Declarative vs. Apex Callouts
- **Trap**: Writing custom Apex REST callouts for straightforward HTTP POST operations.
- **Rule**:
  - Use **Flow Builder HTTP Callout Action** for standard declarative integrations without code.
  - Only write Apex callouts (`HttpRequest`, `Http`, `HttpResponse`) when complex transformations or unsupported data structures are required.

#### 15. Platform Event Publishing & Subscriptions
- **Trap**: Choosing an Apex trigger to publish platform events.
- **Rule**:
  - Use an **After-Save Record-Triggered Flow** to publish the Platform Event declaratively upon record creation/update.
  - In LWC, subscribe to the event stream using the `lightning/empApi` module.

---

### 🟢 Section 4: User Interface (3 Missed)

#### 16. Visualforce Integration into Lightning Pages (Choose 2)
- **Trap**: Creating a separate Visualforce Tab.
- **Rule**:
  - Rather than isolating functionality in a custom tab, embed the Visualforce page directly into the **Account Page Layout** or via the **Visualforce standard component on the Lightning Record Page**.

#### 17. StandardSetController Constructor & 10k Records
- **Trap**: Passing a `Map<Id, sObject>` or `Set<sObject>`.
- **Rule**:
  - Valid constructors:
    1. `new ApexPages.StandardSetController(List<sObject>)`
    2. `new ApexPages.StandardSetController(Database.QueryLocator)`
  - Over 10,000 records:
    - `QueryLocator` throws a `LimitException`.
    - `List<sObject>` silently truncates to 10,000 without throwing an exception.

#### 18. Person Accounts in Visualforce Pages
- **Trap**: Attempting to set `IsPersonAccount = true` in Apex or markup.
- **Rule**:
  - `IsPersonAccount` is a read-only system field.
  - Use a **custom formula field** to display a unified name whether the record is a Person Account (`FirstName + LastName`) or a Business Account (`Name`).

---

## 🚀 The Math to Reach 80%+

```
Current State (69.97%):
  Dev Fundamentals: 68.75% × 0.27 = 18.56%
  Process Autom.:   76.47% × 0.28 = 21.41%
  Testing & Deploy: 50.00% × 0.20 = 10.00%  ← THE LEAK
  User Interface:   80.00% × 0.25 = 20.00%
  ----------------------------------------
  Total:                            69.97%

Target State (+3 questions in Testing + 2 repeat traps killed):
  Dev Fundamentals: 75.00% × 0.27 = 20.25%
  Process Autom.:   82.00% × 0.28 = 22.96%
  Testing & Deploy: 75.00% × 0.20 = 15.00%  ← (+5.00% GAIN)
  User Interface:   84.00% × 0.25 = 21.00%
  ----------------------------------------
  Total:                            79.21% ~ 81.00% ✅ SOLID PASS
```
