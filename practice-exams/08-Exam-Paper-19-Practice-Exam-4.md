# Exam Paper 19: Full Practice Exam (Paper 19)

**Date**: 2026-10-14  
**Score**: 41/60 (68.33% Raw)  
**Weighted Score**: 68.28% (Passing is 68.00%)  
**Status**: ⚠️ **BARELY PASS (+0.28% buffer)**

---

## 📊 Section Score Breakdown

| Section                             |       Score        |  Weight  |   Result   | Status                                      |
| :---------------------------------- | :----------------: | :------: | :--------: | :------------------------------------------ |
| **Process Automation & Logic**      | **82.35% (14/17)** |   28%    | **23.06%** | 🟢 **Strong (+4.02% over passing)**         |
| **Developer Fundamentals**          | **68.75% (11/16)** |   27%    | **18.56%** | 🟡 **Borderline (+0.20% over passing)**     |
| **Testing, Debugging & Deployment** | **66.67% (8/12)**  |   20%    | **13.33%** | 🟡 **Improved from 50% (+3.33%)**           |
| **User Interface**                  | **53.33% (8/15)**  |   25%    | **13.33%** | 🔴 **CRITICAL DROP (-3.67% below passing)** |
| **TOTAL**                           |     **68.33%**     | **100%** | **68.28%** | ⚠️ **Barely Passing (+0.28%)**              |

---

## 🔍 Key Insights: What Happened in This Exam?

1. **Testing Recovered from 50% → 66.67%**:
   - You got 8/12 right in Testing, Debugging & Deployment (up from 6/12 on Paper 18).
   - You nailed: VS Code in SFDX, Deploying with Default tests (fixing test classes in sandbox), Run Selected Tests (individual 75%), Debugging Triggers in Dev Console, Skipping code coverage benefits, and Execution Units.
   - However, basic logging terminology (Debug Level vs. Trace Flag) still cost you points.

2. **User Interface Collapsed to 53.33% (7 Questions Lost!)**:
   - On Paper 18, UI was 80%. Here, it dropped to 53.33% due to old Visualforce components and Einstein Next Best Action (NBA).
   - Specifically: `<apex:detail>`, `<apex:map>`, StandardListController pagination, multiple extensions syntax, `with sharing` scope, and Strategy Builder elements.

3. **Process Automation Stayed High (82.35%)**:
   - You aced SOSL returns, SOQL relationships, Invocable Variables, Safe Navigation Operator (`?.`), and save order of execution.
   - The only missed questions were: uncatchable governor limit rollback (151 DML), static vs. instance method invocation, and knowing that the 'Create a Case' prebuilt flow exists.

---

## ❌ All 19 Missed Questions — Deep Dive & Trap Rules

---

### 🔴 USER INTERFACE — 7 Missed (The New Leak)

#### 1. Multiple Controller Extensions Syntax

- **Question**: Controller extensions in Visualforce.
- **Your Choice**: "Only one controller extension can be defined on each Visualforce page" ❌
- **Correct Answer**: "An extension can be associated using the 'extensions' attribute on the Visualforce `<apex:page>` component." ✅
- **Rule**:
  ```html
  <apex:page
    standardController="Account"
    extensions="ExtOne,ExtTwo,ExtThree"
  ></apex:page>
  ```

  - Multiple extensions ARE supported using a comma-separated list.
  - Executed left-to-right (first extension overrides subsequent ones if method names collide).
  - Extensions extend standard or custom controllers (NEVER the reverse).

#### 2. Visualforce `<apex:detail>` Component

- **Question**: Visualforce component to display the standard record detail page of an object.
- **Your Choice**: `<apex:recordPage>` ❌
- **Correct Answer**: `<apex:detail>` ✅
- **Rule**:
  - `<apex:detail>`: Renders the standard detail page as configured in Setup page layouts (includes fields, related lists, and hover links).
  - `<apex:dataList>`: Iterates over lists.
  - ❌ `<apex:record>`, `<apex:recordPage>` DO NOT EXIST!

#### 3. StandardListController: When is it Sufficient?

- **Question**: For which situation would a standard list controller be sufficient?
- **Your Choice**: "Running the Visualforce page entirely in system mode" ❌
- **Correct Answer**: "Using object record pagination" ✅
- **Rule**:
  - StandardListController provides built-in pagination: `first()`, `last()`, `next()`, `previous()`, `hasNext`, `hasPrevious`, `getPageNumber()`.
  - Running in system mode, performing callouts, or overriding standard functionality requires a **custom controller** or **extension**.

#### 4. `<apex:map>` Component for Property Locations

- **Question**: How to display a map that shows locations of properties sold by a real estate agent in Visualforce?
- **Your Choice**: "Activate the map mode in Visualforce charts" ❌
- **Correct Answer**: "Generate the map using the `<apex:map>` component" ✅
- **Rule**:
  - Salesforce provides `<apex:map>` and `<apex:mapMarker>` for interactive Google maps.
  - (Note: `<apex:google>` and "map mode in charts" do NOT exist).

#### 5. Custom Controller Running Mode & `with sharing`

- **Question**: What is true regarding the mode that a custom controller runs in?
- **Your Choice**: "Runs in system mode by default, but [with sharing] can be used to run in user mode" ❌
- **Correct Answer**: "Runs in system mode, but [with sharing] can be used to enforce organization-wide defaults, role hierarchy, and sharing rules." ✅
- **CRITICAL TRAP**:
  - `with sharing` enforces **RECORD-LEVEL sharing ONLY** (OWD, Role Hierarchy, Sharing Rules).
  - `with sharing` does **NOT** enforce Object Permissions (CRUD) or Field-Level Security (FLS)!
  - True "User Mode" requires `WITH USER_MODE` or `Security.stripInaccessible()`.

#### 6. Einstein Next Best Action (NBA) Considerations (Choose 3)

- **Question**: Considerations when implementing Einstein Next Best Action.
- **Your Choice**: Missed Strategy Builder vs Flow Builder option; picked Lightning Record Page required ❌
- **Correct Answers**:
  1. Flow Builder must be used to create a flow that gets invoked when a recommendation is accepted.
  2. Strategy Builder or Flow Builder can be used to define a strategy for loading recommendations based on criteria.
  3. The action strategy and its source must be specified in the configuration of the Einstein Next Best Action component.
- **Rule**: NBA components can be placed on Home pages, Experience Cloud, or Visualforce — **NOT** restricted to record pages!

#### 7. Strategy Builder Elements for Recommendations (Choose 3)

- **Question**: Elements utilized when building an NBA strategy.
- **Your Choice**: Generate, Load, Branch Merge ❌
- **Correct Answers**: **Generate**, **Load**, **Branch Selector** ✅
- **Rule**:
  - **Generate**: Invokes an `@InvocableMethod` Apex class to dynamically create recommendations.
  - **Load**: Loads existing Recommendation records from the database.
  - **Branch Selector**: Filters/routes which branch to execute based on criteria (e.g. Profile == 'Sales Agent').
  - **Enhance**: Modifies recommendations on the fly.
  - **Branch Merge**: Combines multiple branches into one.

---

### 🟡 DEVELOPER FUNDAMENTALS — 5 Missed

#### 8. Indirect Lookup vs. External Lookup

- **Question**: Contact records in ERP without Salesforce Account ID; show related contacts on Account detail page via Salesforce Connect.
- **Your Choice**: External Lookup ❌
- **Correct Answer**: **Indirect Lookup** ✅
- **Relationship Matrix for External Objects**:
  ```
  Child: External Object  → Parent: Salesforce Object (matching on Parent's External ID)  = INDIRECT LOOKUP
  Child: External Object  → Parent: Salesforce Object (external system has SF 18-char ID) = LOOKUP
  Child: SF/External Obj  → Parent: External Object                                        = EXTERNAL LOOKUP
  ```

#### 9. DescribeSObjectResult: Record Type Developer Names

- **Question**: Apex method to return a map of developer names of record types.
- **Your Choice**: `getRecordTypeInfos()` ❌
- **Correct Answer**: **`getRecordTypeInfosByDeveloperName()`** ✅
- **Summary**:
  - `getRecordTypeInfosByDeveloperName()`: Returns `Map<String, RecordTypeInfo>` (Key = API Developer Name).
  - `getRecordTypeInfosByName()`: Returns `Map<String, RecordTypeInfo>` (Key = UI Label).
  - `getRecordTypeInfos()`: Returns `List<RecordTypeInfo>` (List, not a Map!).

#### 10. Roll-Up Summaries CANNOT use Cross-Object Formulas

- **Question**: Cost on Product → 'Product Cost Fixed' on Opportunity Product → Rollup total cost on Opportunity.
- **Your Choice**: Configure 'Product Cost Fixed' as a formula field and create roll-up summary ❌
- **Correct Answer**: Build a flow that copies Product 'Cost' to 'Product Cost Fixed' and create roll-up summary on Opportunity ✅
- **Rule**:
  - **Cross-object formula fields CANNOT be used in Roll-up Summary fields!**
  - To summarize related parent data from a child object, copy the value to a regular field using Flow/Apex, then roll it up.

#### 11. Schema Reflection: `describeTabs()` Return Type

- **Question**: Return metadata about custom apps to render in mobile app UI.
- **Your Choice**: `Schema.DescribeTabResult[] r = Schema.describeTabs();` ❌
- **Correct Answer**: **`Schema.DescribeTabSetResult[] r = Schema.describeTabs();`** ✅
- **Rule**:
  - `describeTabs()` returns `List<Schema.DescribeTabSetResult>`.
  - A **TabSet** represents an **App** (contains the list of tabs in that app).
  - A **Tab** represents an individual tab (`DescribeTabResult`).

#### 12. Importing Child Contacts Linked to Parent via External ID (Choose 2)

- **Question**: Insert contacts from external system linked to Accounts without Salesforce Account IDs.
- **Your Choice**: Picked Apex trigger instead of External ID field on Account ❌
- **Correct Answers**:
  1. Add unique record identifiers from the account records in the external system to the .csv file.
  2. Define a Unique External ID field on the Account object and populate it with unique identifiers.
- **Rule**: Data Loader / Import tools match parent records using the parent's **Unique External ID** field directly from the CSV. No Apex trigger needed!

---

### 🟠 PROCESS AUTOMATION & LOGIC — 3 Missed

#### 13. DML Statement Limit (150) & Uncatchable Exceptions

- **Question**: Loop inserts 200 records one by one inside `try-catch`. What is inserted?
- **Your Choice**: "A limit exception will be caught and one account inserted" ❌
- **Correct Answer**: **"No accounts will be inserted."** ✅
- **Rule**:
  - `System.LimitException` is an uncatchable exception.
  - The `catch(Exception ex)` block **CANNOT catch governor limit exceptions**!
  - When the 151st DML executes, the transaction aborts and all 150 previous inserts are rolled back immediately.

#### 14. Static vs. Instance Methods & List Evaluation

- **Question**: `isAccessible(Id)` (non-static) and `stripInaccessibleRecords(List<sObject>)` (static).
- **Your Choice**: Evaluated `.isEmpty()` instead of `.size() == 1` ❌
- **Correct Answer**:
  ```apex
  new AccountUtilities().isAccessible(myAccount.Id);
  AccountUtilities.stripInaccessibleRecords(new List<Account>{myAccount}).size() == 1;
  ```
- **Rule**:
  - Non-static methods require an instantiated object: `new AccountUtilities().isAccessible(...)`.
  - Static methods are called on the class name: `AccountUtilities.stripInaccessibleRecords(...)`.
  - If accessible, the resulting list contains 1 item (`size() == 1`), so `.isEmpty()` would be `false`!

#### 15. Prebuilt Standard Flows: 'Create a Case'

- **Question**: Call script for agents to capture info, find/create contact, and create case.
- **Your Choice**: "Create a custom flow with Screen, Get Records, and Create Records" ❌
- **Correct Answer**: "Use a prebuilt flow named **'Create a Case'** and customize it if required." ✅
- **Rule**: Salesforce provides out-of-the-box prebuilt flows. Always check for standard templates before building custom from scratch!

---

### 🔴 TESTING, DEBUGGING & DEPLOYMENT — 4 Missed

#### 16. Debug Level vs. Trace Flag Definition

- **Question**: What is a debug level used for?
- **Your Choice**: "Specify the user or entity type for which debug log captures info" ❌
- **Correct Answer**: **"Specify the type and amount of information that is logged in a debug log."** ✅
- **Distinction**:
  - **Debug Level**: Controls the **volume/filters** (NONE, ERROR, WARN, INFO, DEBUG, FINE, FINER, FINEST) across categories (Apex, Database, Workflow).
  - **Trace Flag**: Points to **WHO/WHAT** is being logged (User, Apex Class, Apex Trigger) and for **HOW LONG** (start/expiration time).

#### 17. Platform Integration User Trace Flags & Audit Fields (Choose 2)

- **Question**: Trace flags on Platform Integration User.
- **Your Choice**: Picked "Automated processes run as Platform Integration User" ❌
- **Correct Answers**:
  1. A trace flag can be set on the Platform Integration User to track business processes run by internal applications.
  2. The Platform Integration User is displayed in audit fields when data is synchronized using Salesforce Integration Cloud.
- **Rule**:
  - Automated processes (Flows, Triggers) run as the **Automated Process** user or triggering user, **NOT** Platform Integration User.
  - Platform Integration User handles Einstein bots, Einstein prediction writes, and Integration Cloud sync.

#### 18. Diagnosing Unexpected Field Value Changes (Choose 3)

- **Question**: Field changes unexpectedly during trigger execution; how to diagnose?
- **Your Choice**: Picked Checkpoint Inspector ❌
- **Correct Answers**: **Log Inspector**, **Debug Log**, **User Trace Flag** ✅
- **Rule**:
  - Trace Flags enable Debug Logs; Log Inspector opens and visualizes the log hierarchy.
  - Checkpoint Inspector is only for inspecting heap state and memory at specific code breakpoints.

#### 19. Developer Console Logs Tab Capabilities

- **Question**: What is true about the Logs tab in Developer Console?
- **Your Choice**: "Logs include database events, Apex events, and JavaScript events" ❌
- **Correct Answer**: **"Debug logs of the current user and other users can be displayed in the Logs tab of the Developer Console."** ✅
- **Rule**:
  - Dev Console logs do **NOT** track JavaScript events (use browser console for JS!).
  - To view logs of other users in Dev Console: uncheck **Debug > Show My Current Logs Only**.
