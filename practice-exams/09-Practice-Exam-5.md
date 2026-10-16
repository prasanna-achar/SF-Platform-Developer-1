# Practice Exam 5 — Full 60 Questions

**Exam Blueprint:** Developer Fundamentals (27%) · Process Automation & Logic (28%) · User Interface (25%) · Testing, Debugging & Deployment (20%)  
**Time Limit:** 105 minutes  
**Passing Score:** 68%  
**Instructions:** Select ONE best answer unless the question states "Choose 2" or "Choose 3".

---

## Section A: Developer Fundamentals (16 Questions)

**Q1.** A developer is building an integration with an ERP system that stores employee records. The ERP does NOT store Salesforce 18-character record IDs. The developer needs to relate the external employee records (stored as External Objects) to the `Employee__c` custom object in Salesforce using the employee's badge number, which is already stored as a unique External ID field on `Employee__c`.

Which relationship type should the developer use?

- A) External Lookup relationship
- B) Lookup relationship
- C) Indirect Lookup relationship
- D) Master-Detail relationship

---

**Q2.** A developer is creating a custom formula field on the `Invoice__c` object that needs to display the parent Account's `AnnualRevenue` field value. The developer notices that some users who can view `Invoice__c` records do not have read access to the `Account` object.

What will happen when those users view the formula field?

- A) The formula field will display an error: "Insufficient Privileges"
- B) The formula field will display `#N/A`
- C) The formula field will display the `AnnualRevenue` value regardless of their lack of Account access
- D) The formula field will display `null` for users without Account access

---

**Q3.** Universal Containers wants to calculate the average cost of all child `Line_Item__c` records on their parent `Order__c` record. `Order__c` is the master in a Master-Detail relationship. A developer suggests creating a Roll-Up Summary field using the AVERAGE function.

Why will this approach fail?

- A) Roll-Up Summary fields cannot be created on Master objects
- B) Roll-Up Summary fields only support COUNT, SUM, MIN, and MAX — AVERAGE is not a supported function
- C) AVERAGE is supported but only on currency fields
- D) Roll-Up Summary fields cannot reference child records in Master-Detail relationships

---

**Q4.** A developer writes the following Apex code:

```apex
Map<String, SObjectType> gd = Schema.getGlobalDescribe();
Schema.DescribeSObjectResult dsr = gd.get('Case').getDescribe();
Map<Id, RecordTypeInfo> rtMap = dsr.getRecordTypeInfosById();
```

What does `rtMap` contain?

- A) A map of Record Type Ids to RecordTypeInfo objects for the Case object
- B) A map of Record Type developer names to RecordTypeInfo objects
- C) A list of all Record Types across all objects
- D) A map of Record Type labels to their corresponding IDs

---

**Q5.** A developer needs to verify at runtime whether the currently logged-in user has permission to delete records of the `Application__c` custom object before executing a DML delete statement.

Which Apex method should the developer use?

- A) `Schema.DescribeFieldResult.isDeletable()`
- B) `Schema.DescribeSObjectResult.isDeletable()`
- C) `Application__c.sObjectType.isRemovable()`
- D) `UserInfo.hasObjectPermission('Application__c', 'Delete')`

---

**Q6.** A developer is working in Schema Builder and wants to simplify the view of a complex data model by hiding some fields that are cluttering the canvas.

What can the developer do?

- A) Right-click individual fields and select "Hide Field"
- B) Use the "Lightweight Mode" toggle to show only key fields
- C) Hide entire objects from the canvas, but individual fields cannot be hidden
- D) Use Field-Level Security settings within Schema Builder to hide fields

---

**Q7.** Which field data types can be designated as an External ID field on a custom object? (Choose 3)

- A) Text
- B) Number
- C) Picklist
- D) Email
- E) URL
- F) Phone

---

**Q8.** A company has a `Policy__c` custom object that is the detail in a Master-Detail relationship with `Account`. Business users need to assign individual `Policy__c` records to a support Queue for processing.

What should the developer recommend?

- A) Keep the Master-Detail relationship and create a Queue on `Policy__c`
- B) Convert the Master-Detail relationship to a Lookup relationship so that `Policy__c` records have their own Owner field
- C) Create a custom field to store the Queue ID and use a trigger to assign records
- D) Create a Hierarchical relationship between `Policy__c` and User

---

**Q9.** A developer needs to query the metadata of all applications and their tabs programmatically in Apex using the Schema Describe API.

What is the correct return type of `Schema.describeTabs()`?

- A) `List<Schema.DescribeTabResult>`
- B) `Map<String, Schema.DescribeTabResult>`
- C) `List<Schema.DescribeTabSetResult>`
- D) `Schema.DescribeTabResult[]`

---

**Q10.** A developer attempts to delete a custom Number field named `Priority_Score__c` from the `Lead` object but receives an error. Which of the following references would block the deletion? (Choose 2)

- A) An Apex trigger referencing `Priority_Score__c`
- B) A Report filter using `Priority_Score__c`
- C) A Flow that references `Priority_Score__c`
- D) A Dynamic SOQL query string that contains 'Priority_Score__c'

---

**Q11.** A developer attempts to convert the data type of a custom Text field to Number. The field is currently referenced in an Apex class.

What happens?

- A) The conversion proceeds and the Apex class is automatically updated
- B) The conversion is blocked because the field is referenced in Apex code
- C) The conversion succeeds but the Apex class throws a compile error on next save
- D) The conversion is allowed only if the Apex class is in a managed package

---

**Q12.** A developer wants to create a many-to-many relationship between `Course__c` and `Student__c`. The developer creates a junction object `Enrollment__c` with two Master-Detail relationships. The first MDR created points to `Course__c` and the second points to `Student__c`.

Both the `Course__c` and `Student__c` master records linked to a specific `Enrollment__c` are deleted. What happens to the `Enrollment__c` junction record?

- A) It is moved to the Recycle Bin and can be restored when either master is undeleted
- B) It is permanently deleted and cannot be restored from the Recycle Bin
- C) It remains in the database with null parent references
- D) It is moved to the Recycle Bin but can only be restored if both masters are undeleted simultaneously

---

**Q13.** An external application needs to subscribe to Platform Events published from Salesforce. The application is built using Java.

Which technology should the external application use to subscribe?

- A) REST API polling on a custom endpoint
- B) SOAP API with a WSDL-generated stub
- C) CometD client (EMP Connector) using long-polling on the Streaming API
- D) Bulk API 2.0 with a Platform Event query

---

**Q14.** A developer wants to publish a Platform Event called `Shipment_Update__e` from within a Record-Triggered Flow when an `Order__c` record's status changes to "Shipped".

Which Flow element should the developer use to publish the event?

- A) An Apex Action element calling `EventBus.publish()`
- B) A Create Records element targeting the `Shipment_Update__e` Platform Event
- C) A Send Custom Notification element
- D) A Post to Chatter element configured for Platform Events

---

**Q15.** In Salesforce's multi-tenant architecture, which statement is TRUE about how organizations share resources?

- A) Each organization has its own dedicated database server but shares application code
- B) Organizations share both the application code and the underlying database
- C) Each organization has its own copy of both code and database
- D) Organizations share the application code but each has its own isolated database

---

**Q16.** A developer needs to load 2 million Contact records from an external data warehouse into Salesforce. Which API is best suited for this use case?

- A) REST API
- B) SOAP API
- C) Bulk API 2.0
- D) Streaming API

---

## Section B: Process Automation & Logic (17 Questions)

**Q17.** A developer writes the following Apex code:

```apex
Decimal revenue = 50000.75;
Integer count = 10;
```

What are the data types of `revenue` and `count` in Apex?

- A) Currency and Number
- B) Decimal and Integer
- C) Double and Long
- D) Number and Integer

---

**Q18.** Consider the following Apex method declaration:

```apex
public static calculateTotal(List<LineItem__c> items) {
    Decimal total = 0;
    for (LineItem__c item : items) {
        total += item.Amount__c;
    }
    return total;
}
```

Why does this code fail to compile?

- A) The method is missing the `global` access modifier
- B) The method is missing a return type in the declaration
- C) The `static` keyword cannot be used with `public`
- D) The method cannot accept `List<LineItem__c>` as a parameter

---

**Q19.** A developer needs to safely access a deeply nested field: `contact.Account.ParentAccount.Name`. If any reference in the chain is null, the code should return null instead of throwing a `NullPointerException`.

Which syntax should the developer use?

- A) `contact.Account.ParentAccount?.Name`
- B) `contact?.Account?.ParentAccount?.Name`
- C) `contact.?Account.?ParentAccount.?Name`
- D) `contact.Account.ParentAccount.Name ?? null`

---

**Q20.** A developer writes a SOQL query to search for contacts whose multi-select picklist `Interests__c` contains BOTH "Golf" AND "Tennis".

Which SOQL syntax is correct?

- A) `WHERE Interests__c INCLUDES ('Golf', 'Tennis')`
- B) `WHERE Interests__c INCLUDES ('Golf;Tennis')`
- C) `WHERE Interests__c = 'Golf' AND Interests__c = 'Tennis'`
- D) `WHERE Interests__c CONTAINS ('Golf AND Tennis')`

---

**Q21.** A developer executes the following code in an Apex transaction:

```apex
for (Integer i = 0; i < 200; i++) {
    Contact c = new Contact(LastName = 'Test ' + i);
    try {
        insert c;
    } catch (Exception e) {
        System.debug('Error: ' + e.getMessage());
    }
}
```

How many Contact records will be inserted?

- A) 200
- B) 150
- C) 151
- D) 0

---

**Q22.** A developer needs to find records containing "Cloud Computing" across the `Account`, `Contact`, and `Opportunity` objects simultaneously.

What is the return type of the SOSL query in Apex?

- A) `List<sObject>`
- B) `Map<Id, sObject>`
- C) `List<List<sObject>>`
- D) `sObject[][]`

---

**Q23.** A developer writes the following trigger:

```apex
trigger CaseTrigger on Case (before update) {
    for (Case c : Trigger.new) {
        Case oldCase = Trigger.oldMap.get(c.Id);
        if (c.Status == 'Closed' && oldCase.Status != 'Closed') {
            c.Closed_Date__c = Date.today();
        }
    }
}
```

Why does this trigger NOT require an explicit DML `update` statement?

- A) Triggers automatically commit all record changes
- B) In `before` triggers, modifying fields on `Trigger.new` records is automatically saved without DML
- C) The `oldMap` comparison eliminates the need for DML
- D) The trigger uses implicit `Database.update()`

---

**Q24.** In the save order of execution, which statement is correct about the sequence?

- A) Validation Rules execute before Before Triggers
- B) After Triggers execute before Assignment Rules
- C) Flows execute before After Triggers
- D) Escalation Rules execute before Assignment Rules

---

**Q25.** A developer needs to prevent an Apex trigger from executing recursively. The trigger performs an after-update DML operation that could re-fire the same trigger.

What is the recommended approach?

- A) Use a `try/catch` block to catch the recursion exception
- B) Use a static Boolean variable or `Set<Id>` in a trigger handler class to track processed records
- C) Add a `WHERE` clause in the DML to exclude already-processed records
- D) Use `Database.update()` with `allOrNone = false`

---

**Q26.** A developer is building a Flow that needs to make an HTTP POST request to an external REST API to check inventory status. No complex Apex logic is required.

According to the "Declarative-First" principle on PD1, what should the developer use?

- A) Write an Apex class with an `Http` callout
- B) Use a Flow Builder HTTP Callout Action
- C) Use a Platform Event to trigger an external integration
- D) Create a Scheduled Apex class to poll the API

---

**Q27.** An Apex developer needs to create an Invocable Action callable from Flow Builder. The method must accept a list of account IDs and return a list of strings.

Which of the following rules must be followed? (Choose 2)

- A) The method must be annotated with `@InvocableMethod`
- B) The class can contain multiple `@InvocableMethod` annotated methods
- C) The method must accept and return `List<T>` collections
- D) The method must be an instance method (not static)

---

**Q28.** A developer needs to insert a `User` record and an `Account` record in the same Apex transaction and receives a `MixedDmlException`.

How should the developer resolve this?

- A) Wrap both operations in a single `Database.setSavepoint()` transaction
- B) Move the non-setup object DML into an `@future` method
- C) Use `Database.insert()` with `allOrNone = false`
- D) Run the code in Execute Anonymous to bypass the restriction

---

**Q29.** A developer creates a Batch Apex class and needs to maintain a running total of records processed across all batch chunks.

What must the developer add to the class definition?

- A) `implements Database.AllowsCallouts`
- B) `implements Database.Stateful`
- C) `implements Database.RaisesPlatformEvents`
- D) A `@future` method to store intermediate results

---

**Q30.** A developer queries asynchronous job status using SOQL. Which statement about the field names is correct?

- A) `CronTrigger` uses `Status` and `AsyncApexJob` uses `State`
- B) Both `CronTrigger` and `AsyncApexJob` use `Status`
- C) `CronTrigger` uses `State` and `AsyncApexJob` uses `Status`
- D) Both `CronTrigger` and `AsyncApexJob` use `State`

---

**Q31.** A developer needs to compare four asynchronous processing mechanisms. Which statement about `@future` methods is TRUE?

- A) `@future` methods can accept sObject parameters directly
- B) `@future` methods return a Job ID that can be monitored
- C) `@future` methods must be static, return void, and only accept primitive data types
- D) `@future` methods support chaining (calling one future from another)

---

**Q32.** A developer implements the following security enforcement in a SOQL query:

```apex
List<Account> accts = [SELECT Id, Name, Phone FROM Account WITH SECURITY_ENFORCED];
```

What happens if the running user does not have FLS read access to the `Phone` field?

- A) The `Phone` field is silently removed from results but the query succeeds
- B) The query throws a `System.QueryException` immediately
- C) The `Phone` field returns `null` for all records
- D) The query executes successfully but the `Phone` field shows "[Hidden]"

---

**Q33.** A developer writes the following Apex code and expects `result` to contain only fields the user has permission to read:

```apex
SObjectAccessDecision decision = Security.stripInaccessible(AccessType.READABLE, accounts);
List<Account> result = decision.getRecords();
```

What happens to fields the user cannot access?

- A) An exception is thrown for each inaccessible field
- B) The inaccessible fields are silently stripped from the result records
- C) The entire record is excluded from the result list
- D) The inaccessible fields are replaced with default values

---

## Section C: User Interface (15 Questions)

**Q34.** A developer is building a new Lightning Web Component. Which files are REQUIRED in the component bundle? (Choose 3)

- A) `myComponent.html`
- B) `myComponent.js`
- C) `myComponent.js-meta.xml`
- D) `myComponent.css`
- E) `myComponent.helper.js`

---

**Q35.** A developer declares the following property in a Lightning Web Component:

```javascript
@api accountName;
```

A child component attempts to update this property:

```javascript
this.accountName = 'New Name';
```

What happens?

- A) The property is updated successfully
- B) A compile-time error occurs
- C) A runtime error is thrown because `@api` properties are read-only to the owning component
- D) The property updates locally but the change is not reflected in the parent

---

**Q36.** A developer needs to track deep changes inside an object property (e.g., `this.user.address.city = 'London'`) in a Lightning Web Component to trigger re-rendering.

Which decorator should be used?

- A) `@api`
- B) `@track`
- C) No decorator is needed — LWC automatically tracks deep object mutations
- D) `@wire`

---

**Q37.** In the LWC lifecycle, which hook fires after every render AND re-render and must include a guard to prevent infinite loops?

- A) `connectedCallback()`
- B) `disconnectedCallback()`
- C) `renderedCallback()`
- D) `constructor()`

---

**Q38.** A developer creates a Custom Event in a deeply nested child LWC that needs to reach a grandparent component outside the child's Shadow DOM boundary.

Which event configuration is required?

- A) `{ bubbles: false, composed: false }`
- B) `{ bubbles: true, composed: false }`
- C) `{ bubbles: true, composed: true }`
- D) `{ bubbles: false, composed: true }`

---

**Q39.** A developer needs to allow an LWC component on a record page to communicate with a Visualforce page embedded in the utility bar. The components have no parent-child relationship.

Which communication mechanism should the developer use?

- A) A Component Event
- B) An Application Event
- C) Lightning Message Service (LMS)
- D) `@api` method invocation

---

**Q40.** A developer imports Lightning Message Service in a Lightning Web Component. Which import path is correct?

- A) `import { publish, subscribe } from '@salesforce/messageService';`
- B) `import { publish, subscribe } from 'lightning/messageService';`
- C) `import { publish, subscribe } from 'c/messageService';`
- D) `import { publish, subscribe } from 'lightning/lms';`

---

**Q41.** A developer needs to save an array of edited rows from a `lightning-datatable` to Salesforce. The developer attempts to pass the `draftValues` array to `updateRecord` from `lightning/uiRecordApi`.

Why does this approach fail?

- A) `updateRecord` only accepts JSON strings, not arrays
- B) `updateRecord` is designed for single-record updates only; bulk saves require an `@AuraEnabled` Apex method
- C) `updateRecord` requires the record ID to be in a separate parameter
- D) The `draftValues` array format is incompatible with the `fields` object structure

---

**Q42.** A developer wants to refresh data fetched by a wire adapter after a user action. Which approach is correct?

- A) Call `refreshApex(this.getAccountDetails)` passing the Apex method reference
- B) Call `refreshApex(this.wiredResult)` passing the cached wired result object
- C) Call `this.connectedCallback()` to re-execute the wire adapter
- D) Re-assign the reactive parameter (e.g., `this.recordId = this.recordId`) to force re-fetch

---

**Q43.** An Aura component developer needs to expose a custom attribute to Lightning App Builder so that admins can configure it when adding the component to a page.

Where should the developer define this attribute?

- A) In the `.cmp` file using `<aura:attribute>`
- B) In the `.design` file using `<design:attribute>`
- C) In the `Controller.js` file as a JavaScript variable
- D) In the `.auradoc` file as a documented parameter

---

**Q44.** Two Aura components on the same Lightning page need to communicate, but they are NOT in a parent-child hierarchy. One component is in the main body and the other is in the sidebar.

Which event type should the developer use?

- A) Component Event with `type="COMPONENT"`
- B) Application Event with `type="APPLICATION"`
- C) DOM Event with `bubbles: true`
- D) Platform Event with `after insert` trigger

---

**Q45.** In an Aura component's `.css` file, a developer writes:

```css
.headerText {
    font-size: 24px;
    color: blue;
}
```

The styles are not being applied. Why?

- A) Aura components do not support custom CSS
- B) The CSS selector must be prefixed with `.THIS` (e.g., `.THIS .headerText`)
- C) The CSS file must be imported using `<ltng:require>`
- D) Inline styles are required instead of CSS files in Aura

---

**Q46.** A developer is building a Visualforce page that overrides the standard "View" button on the Opportunity object.

What attribute MUST be on the `<apex:page>` tag?

- A) `controller="OpportunityViewController"`
- B) `standardController="Opportunity"`
- C) `extensions="OpportunityViewController"`
- D) `recordSetVar="opportunities"`

---

**Q47.** A developer creates a Visualforce page with the following markup:

```html
<apex:page standardController="Contact" recordSetVar="contacts">
    <apex:pageBlockTable value="{!contacts}" var="c">
        <apex:column value="{!c.Name}"/>
    </apex:pageBlockTable>
    <apex:commandButton value="Next" action="{!next}" rendered="{!hasNext}"/>
</apex:page>
```

What does the `recordSetVar` attribute enable?

- A) It binds the page to a single Contact record
- B) It enables the StandardSetController for list views and pagination
- C) It creates a variable to store a DML result set
- D) It enables dynamic binding to any sObject type

---

**Q48.** A developer is using a Visualforce custom controller (no standard controller). The controller class is defined as:

```apex
public with sharing class MyCustomController { ... }
```

Which security is enforced by the `with sharing` keyword?

- A) CRUD (object permissions), FLS (field-level security), and record-level sharing
- B) Record-level sharing only (OWD, Role Hierarchy, Sharing Rules)
- C) CRUD and FLS only, not record-level sharing
- D) No security is enforced — `with sharing` is ignored on custom controllers

---

## Section D: Testing, Debugging & Deployment (12 Questions)

**Q49.** A developer is writing an Apex test class and needs to create test data for `Account`, `Contact`, and a custom `Discount_Setting__c` (List Custom Setting).

Which records must be explicitly inserted in the test's `@testSetup` method? (Choose 2)

- A) Account
- B) Profile
- C) Contact
- D) Discount_Setting__c (List Custom Setting)

---

**Q50.** A developer creates a test class with an `@testSetup` method that inserts 5 Account records. Test Method A updates the name of one Account. Test Method B queries the same Account.

What name does Test Method B see?

- A) The updated name set by Test Method A
- B) The original name set in `@testSetup`
- C) `null` because Test Method A's changes are committed
- D) An error because multiple methods cannot share `@testSetup` data

---

**Q51.** A developer is testing an Apex class that makes an HTTP callout to an external REST API. The developer writes the test without mocking the callout.

What happens when the test runs?

- A) The test makes a live HTTP call and succeeds if the endpoint is reachable
- B) The test throws a `CalloutException` because live callouts are not allowed in test context
- C) The test silently skips the callout and continues execution
- D) The test fails only if the Remote Site Setting is not configured

---

**Q52.** A developer is testing a Queueable Apex job. In the test method, the developer enqueues the job and immediately asserts the results BEFORE calling `Test.stopTest()`.

What happens?

- A) The assertion succeeds because the Queueable executes immediately when enqueued
- B) The assertion fails because the Queueable has not yet executed — it only runs synchronously when `Test.stopTest()` is called
- C) The test throws a `QueueableException`
- D) The assertion succeeds because test context forces synchronous execution

---

**Q53.** What is the functional difference between a Debug Level and a Trace Flag?

- A) Debug Level specifies the target user; Trace Flag specifies the logging detail
- B) Debug Level specifies the type and amount of information logged; Trace Flag specifies who/what is monitored and for how long
- C) They are the same concept with different names
- D) Debug Level applies to production only; Trace Flag applies to sandboxes only

---

**Q54.** A developer runs Apex code in the Developer Console's Execute Anonymous window. The code inserts 3 Account records without wrapping them in a `try/catch` block, and the insert succeeds.

What happens to those Account records after the anonymous block finishes execution?

- A) They are automatically rolled back because Execute Anonymous is a test context
- B) They are committed permanently to the database
- C) They are only committed if the developer explicitly calls `Database.setSavepoint()`
- D) They remain pending until approved by an admin

---

**Q55.** A developer is debugging an issue where a Workflow Rule's criteria evaluates to `false` and the rule does not fire. The developer sets a Trace Flag and reviews the debug log.

Will the Workflow Rule evaluation appear in the log?

- A) No — only rules that fire generate log entries
- B) Yes — the evaluation step is logged even when the criteria is not met
- C) Only if the Debug Level for Workflow is set to FINEST
- D) No — Workflow Rules are client-side and are not captured in server debug logs

---

**Q56.** A developer needs to deploy an Apex trigger and its test class to production using a Change Set with the "Run Specified Tests" option. The trigger has 70% code coverage from the specified test class.

What happens during deployment?

- A) Deployment succeeds because 70% exceeds the org-wide 75% minimum
- B) Deployment fails because with "Run Specified Tests", each component must individually achieve at least 75% coverage
- C) Deployment succeeds because triggers only need 1% coverage
- D) Deployment fails because Change Sets do not support the "Run Specified Tests" option

---

**Q57.** A developer needs to deploy a metadata change to production that includes deleting an obsolete custom field.

Which deployment tool supports destructive changes (metadata deletion)?

- A) Change Sets
- B) Salesforce CLI / Metadata API with `destructiveChanges.xml`
- C) Data Loader
- D) Schema Builder

---

**Q58.** A developer creates a Scratch Org for a feature branch. Another developer on the same team needs to collaborate on the same feature.

What should the second developer do?

- A) Log into the first developer's scratch org using shared credentials
- B) Create their own scratch org from the Dev Hub and pull the latest source from version control
- C) Clone the first developer's scratch org using the Salesforce CLI
- D) Use the Partial Copy Sandbox instead since scratch orgs don't support collaboration

---

**Q59.** What is the maximum lifespan of a Scratch Org?

- A) 7 days (cannot be extended)
- B) Up to 30 days (configurable; defaults to 7 days)
- C) 90 days
- D) Unlimited until manually deleted

---

**Q60.** A developer has a test class that fails intermittently in production. They suspect it is dependent on existing organization data. The test class has `@isTest(SeeAllData=true)` on it.

Which standard objects are accessible to test methods even WITHOUT `SeeAllData=true`? (Choose 2)

- A) Account
- B) User
- C) Opportunity
- D) Profile

---

---

# ✅ Answer Key & Explanations

## Section A: Developer Fundamentals

**Q1. Answer: C) Indirect Lookup relationship**  
When the child is an External Object and the parent is a Salesforce object, and the external system does NOT store the Salesforce 18-char ID, you use an Indirect Lookup. It matches against the parent's unique External ID field. External Lookup is when the *parent* is external. A standard Lookup requires the external system to store the Salesforce ID.

**Q2. Answer: C) The formula field will display the AnnualRevenue value regardless of their lack of Account access**  
Cross-object formula fields execute at the system level and display the calculated value even if the user does not have read access to the parent record or object. This is a well-known security behavior — formulas bypass FLS and record-level access for display purposes.

**Q3. Answer: B) Roll-Up Summary fields only support COUNT, SUM, MIN, and MAX — AVERAGE is not a supported function**  
AVERAGE is NOT a valid Roll-Up Summary operation. To calculate an average, create two roll-up summaries (SUM and COUNT) and then use a formula field: `SUM / COUNT`.

**Q4. Answer: A) A map of Record Type Ids to RecordTypeInfo objects for the Case object**  
`getRecordTypeInfosById()` returns a `Map<Id, RecordTypeInfo>`. The method name literally says "ById" — the keys are Record Type Ids. `getRecordTypeInfosByDeveloperName()` and `getRecordTypeInfosByName()` return maps keyed by developer name and label respectively.

**Q5. Answer: B) Schema.DescribeSObjectResult.isDeletable()**  
`isDeletable()` is a method on `DescribeSObjectResult` that checks whether the running user has delete permission on the object. `isRemovable()` and `hasObjectPermission()` do not exist.

**Q6. Answer: C) Hide entire objects from the canvas, but individual fields cannot be hidden**  
Schema Builder allows hiding entire objects and relationship lines from the canvas, but there is no option to hide individual fields. There is also no "Lightweight Mode" feature.

**Q7. Answer: A) Text, B) Number, D) Email**  
The valid External ID field types are Text, Number, Email, and Auto-Number. Picklist, URL, and Phone cannot be External IDs.

**Q8. Answer: B) Convert the Master-Detail relationship to a Lookup relationship**  
Detail records in a Master-Detail relationship do NOT have an Owner field (ownership is inherited from the Master). Queues require an Owner field to assign records. Converting to Lookup gives `Policy__c` its own Owner field, enabling Queue assignment.

**Q9. Answer: C) List<Schema.DescribeTabSetResult>**  
`Schema.describeTabs()` returns `List<Schema.DescribeTabSetResult>` where each `DescribeTabSetResult` represents an application (App) and contains individual tabs. `DescribeTabResult` represents a single tab, not the return type of the method.

**Q10. Answer: A) An Apex trigger, C) A Flow**  
Apex code (classes, triggers), Flows, Visualforce pages, and Formula fields block field deletion. Report filters do NOT block deletion — Salesforce automatically removes the field from reports. Dynamic SOQL is a runtime string and does not create a metadata dependency.

**Q11. Answer: B) The conversion is blocked because the field is referenced in Apex code**  
You cannot change the data type of a custom field if it is referenced in Apex classes or Visualforce pages. The platform prevents the conversion to avoid compile errors.

**Q12. Answer: B) It is permanently deleted and cannot be restored from the Recycle Bin**  
When BOTH master records of a junction object are deleted, the junction record is permanently deleted and cannot be recovered. If only one master is deleted, the junction goes to the Recycle Bin and can be restored when that master is undeleted.

**Q13. Answer: C) CometD client (EMP Connector) using long-polling on the Streaming API**  
External applications subscribe to Platform Events using CometD/EMP Connector via the Streaming API with long-polling (Bayeux protocol). REST API doesn't support event subscription. SOAP API and Bulk API are not designed for event streaming.

**Q14. Answer: B) A Create Records element targeting the Shipment_Update__e Platform Event**  
The declarative way to publish a Platform Event from a Flow is to use the Create Records element and select the Platform Event object. This is the "Declarative-First" approach preferred on PD1.

**Q15. Answer: B) Organizations share both the application code and the underlying database**  
In Salesforce's multi-tenant architecture, all orgs share the SAME application code AND the SAME database infrastructure. Governor limits exist precisely because resources are shared.

**Q16. Answer: C) Bulk API 2.0**  
Bulk API 2.0 is specifically optimized for loading large data volumes (50,000 to millions of records) asynchronously in CSV or JSON format.

---

## Section B: Process Automation & Logic

**Q17. Answer: B) Decimal and Integer**  
`Currency` and `Number` are NOT valid Apex data types — those are declarative field types in the Salesforce UI. In Apex code, use `Decimal` for currency values and `Integer` for whole numbers.

**Q18. Answer: B) The method is missing a return type in the declaration**  
Every Apex method MUST declare a return type. This method returns a `Decimal` value but has no return type in the signature. It should be `public static Decimal calculateTotal(...)`. The return type is always required — even `void` must be explicitly stated.

**Q19. Answer: B) contact?.Account?.ParentAccount?.Name**  
The safe navigation operator `?.` is placed between the object and the property/method. If any reference on the left evaluates to null, it returns null instead of throwing a NullPointerException. Option C has the syntax wrong (period before question mark).

**Q20. Answer: B) WHERE Interests__c INCLUDES ('Golf;Tennis')**  
In multi-select picklist SOQL queries, the semicolon (`;`) represents AND logic (both values must be present). The comma (`,`) represents OR logic (any value matches). So `'Golf;Tennis'` means BOTH must be present.

**Q21. Answer: D) 0**  
The 151st `insert` statement exceeds the 150 DML governor limit and throws an uncatchable `System.LimitException`. The `try/catch` block CANNOT catch governor limit exceptions. The entire transaction is rolled back, undoing all 150 previous inserts. Zero records are inserted.

**Q22. Answer: C) List<List<sObject>>**  
SOSL queries search across multiple objects simultaneously and return `List<List<sObject>>`. Each inner list corresponds to one object specified in the RETURNING clause.

**Q23. Answer: B) In before triggers, modifying fields on Trigger.new records is automatically saved without DML**  
In `before` triggers, you can directly modify field values on `Trigger.new` records. These modifications are automatically saved to the database — no explicit DML statement is needed. Calling DML on `Trigger.new` in a before trigger would cause recursion.

**Q24. Answer: B) After Triggers execute before Assignment Rules**  
The save order is: System Validation → Before Triggers → Validation Rules → Record Saved → After Triggers → Assignment Rules → Escalation Rules → Flows. So After Triggers fire BEFORE Assignment Rules.

**Q25. Answer: B) Use a static Boolean variable or Set<Id> in a trigger handler class**  
The standard pattern is to use a static variable (Boolean flag or `Set<Id>`) in a separate handler class that persists across trigger re-entries within the same transaction. Each time the trigger fires, it checks if the record has already been processed.

**Q26. Answer: B) Use a Flow Builder HTTP Callout Action**  
PD1 follows the "Declarative-First" principle. Since the requirement is a simple HTTP POST with no complex logic, the Flow Builder HTTP Callout Action is the correct answer. Writing Apex for a simple REST call is incorrect on the exam.

**Q27. Answer: A) and C)**  
`@InvocableMethod` is required, and the method must accept `List<T>` parameters and return `List<R>` (or void) to support bulk Flow operations. Only ONE `@InvocableMethod` is allowed per class (B is wrong). The method must be `static` (D is wrong).

**Q28. Answer: B) Move the non-setup object DML into an @future method**  
A `MixedDmlException` occurs when inserting a Setup object (User, UserRole, Group) and a non-Setup object (Account, Contact) in the same synchronous transaction. Separating them into asynchronous threads via `@future` or Queueable resolves the error.

**Q29. Answer: B) implements Database.Stateful**  
Without `Database.Stateful`, instance variables reset between batch chunk executions. Adding `Database.Stateful` preserves state (like running totals) across all execute() invocations.

**Q30. Answer: C) CronTrigger uses State and AsyncApexJob uses Status**  
This is a notorious syntax trap. `CronTrigger` has a field called `State` (not Status), while `AsyncApexJob` has a field called `Status` (not State). Mixing them up causes SOQL errors.

**Q31. Answer: C) @future methods must be static, return void, and only accept primitive data types**  
`@future` methods have strict constraints: must be `static`, must return `void`, and can only accept primitive types (String, Id, Integer, etc.) — NOT sObjects. They also do NOT return a Job ID and cannot be chained.

**Q32. Answer: B) The query throws a System.QueryException immediately**  
`WITH SECURITY_ENFORCED` checks both CRUD and FLS permissions. If the running user lacks read access to any field in the SELECT clause, the query immediately throws a `QueryException` — it does NOT silently strip fields.

**Q33. Answer: B) The inaccessible fields are silently stripped from the result records**  
`Security.stripInaccessible()` gracefully removes fields the user cannot access from the result set without throwing any exception. This is the key difference from `WITH SECURITY_ENFORCED`, which throws an exception.

---

## Section C: User Interface

**Q34. Answer: A, B, C)**  
Every LWC bundle requires: `.html` (template), `.js` (ES6 class), and `.js-meta.xml` (configuration/targets). The `.css` file is optional. There is NO `.helper.js` file in LWC — that's an Aura-only concept.

**Q35. Answer: C) A runtime error is thrown because @api properties are read-only to the owning component**  
`@api` properties are public and can be SET by the parent component, but the child component that owns the property cannot reassign it. Attempting to do so throws a runtime error. The child must dispatch an event requesting the parent to update it.

**Q36. Answer: B) @track**  
Normal reactive properties automatically trigger re-rendering for primitive value changes. But for deep object mutations (like `this.user.address.city = 'London'`), the `@track` decorator is required to observe nested property changes.

**Q37. Answer: C) renderedCallback()**  
`renderedCallback()` fires after every render and re-render. If you change a reactive property inside it, it triggers another re-render, creating an infinite loop. Always use a boolean guard like `if (this.hasRendered) return;`.

**Q38. Answer: C) { bubbles: true, composed: true }**  
By default, custom events do NOT cross Shadow DOM boundaries. Setting `bubbles: true` makes the event bubble up, and `composed: true` allows it to cross the Shadow DOM boundary to reach ancestor components outside the child's shadow root.

**Q39. Answer: C) Lightning Message Service (LMS)**  
LMS allows communication across the entire DOM tree between unrelated components — including LWC, Aura, and Visualforce pages. Component Events only work in parent-child hierarchies. Application Events are Aura-only.

**Q40. Answer: B) import { publish, subscribe } from 'lightning/messageService';**  
The correct import path is `lightning/messageService` — NOT `@salesforce/messageService`. This is a common exam trap.

**Q41. Answer: B) updateRecord is designed for single-record updates only**  
`updateRecord` from `lightning/uiRecordApi` can only update one record at a time. To save multiple draft rows from a datatable, you must pass the array to an `@AuraEnabled` Apex method that performs bulk DML.

**Q42. Answer: B) Call refreshApex(this.wiredResult) passing the cached wired result object**  
`refreshApex` must receive the cached wired result object (the provisioned value from the wire adapter), NOT the Apex method function reference. Passing the function does nothing.

**Q43. Answer: B) In the .design file using <design:attribute>**  
To expose an Aura component attribute to Lightning App Builder for admin configuration, you must define it in the `.design` file using `<design:attribute>`. Defining it only in `.cmp` with `<aura:attribute>` makes it available programmatically but NOT in App Builder.

**Q44. Answer: B) Application Event with type="APPLICATION"**  
Components that do NOT share a parent-child hierarchy cannot use Component Events (which only travel up the containment hierarchy). Application Events broadcast to ALL listening components on the page, regardless of their position.

**Q45. Answer: B) The CSS selector must be prefixed with .THIS**  
In Aura components, every top-level CSS selector MUST be prefixed with `.THIS` to properly scope styles to the component. Without `.THIS`, the Aura framework ignores or strips the styles.

**Q46. Answer: B) standardController="Opportunity"**  
When overriding standard buttons (View, Edit, New, etc.), the Visualforce page MUST use `standardController` pointing to the object whose button is being overridden. A custom controller alone cannot replicate the standard button context.

**Q47. Answer: B) It enables the StandardSetController for list views and pagination**  
The `recordSetVar` attribute activates the `StandardSetController`, which provides built-in pagination methods (`next`, `previous`, `hasNext`, `hasPrevious`) and enables the page to work with collections of records.

**Q48. Answer: B) Record-level sharing only (OWD, Role Hierarchy, Sharing Rules)**  
`with sharing` on a custom controller enforces ONLY record-level sharing rules. It does NOT enforce CRUD (object permissions) or FLS (field-level security). To enforce CRUD/FLS, you must use `WITH SECURITY_ENFORCED`, `WITH USER_MODE`, or `Security.stripInaccessible()`.

---

## Section D: Testing, Debugging & Deployment

**Q49. Answer: A) Account and D) Discount_Setting__c**  
By default, tests cannot see org data (`SeeAllData=false`). Standard business objects (Account, Contact) and Custom Settings MUST be explicitly created in test setup. However, User and Profile are always visible without `SeeAllData` (UPR-O rule). So Account AND the Custom Setting need explicit insertion.

**Q50. Answer: B) The original name set in @testSetup**  
Each test method receives a fresh, independent rollback copy of the `@testSetup` data. Test Method A's modifications are rolled back before Test Method B executes. Each method always starts with the original data.

**Q51. Answer: B) The test throws a CalloutException**  
Salesforce strictly prohibits live HTTP/SOAP callouts during test execution. If no mock is registered, the callout throws a `CalloutException`. You must use `HttpCalloutMock` or `WebServiceMock` with `Test.setMock()`.

**Q52. Answer: B) The assertion fails because the Queueable has not yet executed**  
Asynchronous jobs (Queueable, @future, Batch) are queued but do NOT execute until `Test.stopTest()` is called. Asserting before `stopTest()` means the job hasn't run yet, so the assertion fails.

**Q53. Answer: B) Debug Level specifies the type and amount of information logged; Trace Flag specifies who/what is monitored and for how long**  
Debug Level = WHAT and HOW MUCH to log (categories and verbosity levels). Trace Flag = WHO/WHAT is being traced (User, Class, or Trigger), which Debug Level to apply, and the time duration.

**Q54. Answer: B) They are committed permanently to the database**  
Execute Anonymous commits all successful DML operations to the database immediately. Unlike test methods, Execute Anonymous does NOT automatically roll back changes. The records persist.

**Q55. Answer: B) Yes — the evaluation step is logged even when the criteria is not met**  
When logging is active, rule evaluation events are recorded in the debug log even if the criteria evaluates to false. Only disabled/deactivated automations generate zero log lines.

**Q56. Answer: B) Deployment fails because with "Run Specified Tests", each component must individually achieve at least 75% coverage**  
When using `RunSpecifiedTests`, every individual Apex class and trigger in the deployment package must independently achieve ≥75% code coverage. The trigger's 70% is below this threshold, so deployment fails.

**Q57. Answer: B) Salesforce CLI / Metadata API with destructiveChanges.xml**  
Change Sets CANNOT delete metadata from a target org. Only Salesforce CLI (or the Metadata API) supports destructive changes via the `destructiveChanges.xml` manifest.

**Q58. Answer: B) Create their own scratch org from the Dev Hub and pull the latest source from version control**  
Scratch orgs are individual — each developer gets their own. They share code through version control (Git), not by sharing scratch orgs. The second developer creates their own scratch org and pulls the feature branch.

**Q59. Answer: B) Up to 30 days (configurable; defaults to 7 days)**  
Scratch orgs are ephemeral and last between 1 and 30 days. The default is 7 days. They expire automatically after the configured duration.

**Q60. Answer: B) User and D) Profile**  
Without `SeeAllData=true`, test methods can still access: User, Profile, RecordType, Organization, and CronTrigger (the "UPR-O" mnemonic). Standard business objects like Account and Opportunity must be created in test setup.
