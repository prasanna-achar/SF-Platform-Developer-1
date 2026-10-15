# 01 — Test Architecture, Data Isolation & Execution

> **NotebookLM Study Guide:** Comprehensive breakdown of Apex test class fundamentals, @testSetup mechanics, test data isolation ("UPR-O"), assertions, and code coverage rules for Salesforce Platform Developer I (PD1).

---

## 1. Core Architecture & Mental Model

### Why Unit Testing in Salesforce?
Because Salesforce is a multi-tenant environment where shared resources are strictly governed, unit tests are mandatory. Salesforce enforces rigorous testing standards before any Apex code can be deployed to a production organization.

### The 3 Core Requirements for Production Deployment:
1. **75% Overall Org-Wide Coverage:** Across all Apex classes and triggers in the organization.
2. **Triggers Require > 0% Coverage:** Every Apex trigger must have at least 1 line of code covered (cannot deploy with 0%).
3. **All Tests Must Pass:** 100% pass rate. If even a single test fails, the entire deployment transaction is rolled back.

> ⚠️ **The Exam Trap:** Lines of code inside `@isTest` classes and test methods **DO NOT count** toward org code coverage calculations!

---

## 2. Test Class Structure & Annotations

### Syntax Template:
```apex
@isTest
private class AccountServiceTest {
    
    // Executes once before ANY test method in this class runs
    @testSetup
    static void setupTestData() {
        Account testAcct = new Account(Name = 'Universal Containers');
        insert testAcct;
        
        Contact testCon = new Contact(
            FirstName = 'Jane',
            LastName = 'Doe',
            AccountId = testAcct.Id
        );
        insert testCon;
    }

    @isTest
    static void testAccountUpdateSuccess() {
        // Query the isolated data created in @testSetup
        Account acct = [SELECT Id, Name, Description FROM Account WHERE Name = 'Universal Containers' LIMIT 1];
        
        Test.startTest();
        acct.Description = 'Updated via Test';
        update acct;
        Test.stopTest();
        
        // Assertions using modern Assert class
        Account updatedAcct = [SELECT Description FROM Account WHERE Id = :acct.Id];
        Assert.areEqual('Updated via Test', updatedAcct.Description, 'Account description should be updated');
    }
}
```

### `@testSetup` Mechanics:
* `@testSetup` runs **once per test class execution**.
* Each individual `@isTest` method receives a **fresh, isolated rollback copy** of the records inserted in `@testSetup`.
* If Method A modifies the account name, Method B still sees the original name created in `@testSetup`.
* **Benefit:** Dramatically reduces test execution time by avoiding redundant DML inserts per test method.

---

## 3. Test Data Isolation & The "UPR-O" Rule

By default, Apex tests do NOT see organization data (`SeeAllData=false`).

### What is Visible WITHOUT `SeeAllData`? (The "UPR-O" Mnemonic):
```
U -> User (Standard & Custom Users)
P -> Profile (All Profiles in the org)
R -> RecordType (RecordType metadata)
O -> Organization (Company information & currency)
+ -> CronTrigger (Scheduled jobs)
```

### What is NOT Visible (Must Be Created in Test Setup):
* ❌ Standard Business Records: `Account`, `Contact`, `Opportunity`, `Case`, `Lead`.
* ❌ Custom Objects: All `__c` records.
* ❌ **Custom Settings (List & Hierarchy):** Must be explicitly inserted in tests! (Common exam trap!).
* ❌ Custom Metadata Records: Read-only in tests unless queried or deployed.

### When is `SeeAllData=true` Strictly Allowed?
* Only when querying metadata objects that cannot be created via DML in tests:
  - `CollaborationGroup` (Chatter groups)
  - `Pricebook2` standard pricebook (prior to `Test.getStandardPricebookId()`)
  - Report / Dashboard / Forecasting metadata

---

## 4. Assertions vs. User Contexts (`System.runAs`)

### 1. Assertions: Validating Behavior
* **Modern API:** `Assert.areEqual(expected, actual, 'message')`, `Assert.isTrue(condition)`, `Assert.isFalse()`, `Assert.isNull()`.
* **Legacy API:** `System.assertEquals(expected, actual)`, `System.assert(condition)`.
* **Exam Trap:** System.assert tests **values and boolean conditions**—it does NOT test user contexts or security permissions!

### 2. `System.runAs(User u)`: Testing User Contexts
* `System.runAs` is a special method available **only inside test methods**.
* **Purpose:** Executes code under the security, role, and sharing context of a specific user.
* Enforces: Record sharing rules, OWDs, and Role Hierarchy.
* Does NOT count toward governor limits (creating a user and calling `runAs` is limit-exempt for DML).

```apex
@isTest
static void testSharingEnforcement() {
    User standardUser = [SELECT Id FROM User WHERE Profile.Name = 'Standard User' LIMIT 1];
    
    System.runAs(standardUser) {
        // This query runs with standardUser's sharing permissions applied!
        List<Private_Record__c> records = [SELECT Id FROM Private_Record__c];
        Assert.areEqual(0, records.size(), 'User should not have access to private records');
    }
}
```

---

## 5. Developer Console Test Execution & Test Suites

### Running Tests:
1. **Developer Console:** `Test > New Run` (select classes or suites).
2. **Setup Menu:** `Custom Code > Apex Test Execution` (can run all tests or selected tests).
3. **Salesforce CLI / VS Code:** `sf apex run test --code-coverage`.

### Developer Console Test Suites & Suite Manager:
* **Test Suite:** A collection of Apex test classes grouped together to execute in a single batch.
* **Suite Manager:** Used to **create, edit, and delete** test suites. (❌ NOT used to create classes or abort runs!).
* **Execution Rules:**
  - Running a single test class can execute **synchronously** (if configured).
  - Running a test suite or 2+ classes is **ALWAYS asynchronous**.

### Skipping Code Coverage in Developer Console:
* Under `Test > New Run`, check **"Skip Code Coverage"**.
* **Benefits:**
  1. Tests run much faster.
  2. Immediate pass/fail feedback for unit testing.
  3. No coverage data is calculated or saved to the database.

---

## 6. High-Frequency Exam Traps & Distractors

| Trap Topic | What Looks Correct (The Distractor) | Why It's Dead Wrong | The Official Exam Rule |
| :--- | :--- | :--- | :--- |
| **Test Data Visibility** | "Custom Settings are visible without SeeAllData" | Custom Settings are data records, not metadata. | Custom Settings **MUST be inserted in `@testSetup`**. Only "UPR-O" is visible. |
| **`System.assert` Scope** | "Use System.assert() to test different user profiles" | Assertions only validate values. | Use **`System.runAs(user)`** to switch user context. |
| **Test Method Coverage** | "Lines in test classes count toward the 75% requirement" | Test lines are strictly excluded from coverage. | Only production classes and triggers count toward the 75% metric. |
| **Suite Manager Role** | "Suite Manager is used to abort running tests" | Suite Manager manages suite definitions. | Aborting tests is done from the **Apex Test Queue** or Dev Console Progress tab. |
| **Trigger Deploy Coverage** | "A trigger can be deployed with 0% if org total is 85%" | Every trigger has a hard minimum. | Every trigger **MUST have at least 1% coverage**! |

---

## 7. Quick-Fire Summary Cards (NotebookLM Dialogue Ready)

* **Q: What standard objects are accessible in test methods without `SeeAllData=true`?**  
  **A:** User, Profile, RecordType, Organization, and CronTrigger ("UPR-O").

* **Q: How does `@testSetup` handle test data between different test methods in the same class?**  
  **A:** It executes once; every test method receives an isolated, independent rollback copy of that data.

* **Q: What happens if you skip code coverage during an Apex test run in Developer Console?**  
  **A:** Tests execute faster, return immediate pass/fail results, and store zero code coverage metrics in the org.

* **Q: Can you deploy an Apex trigger with 0% code coverage if the overall org coverage is 80%?**  
  **A:** No. Every trigger must have at least 1% code coverage to deploy to production.
