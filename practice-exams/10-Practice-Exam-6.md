# Practice Exam 6 — Full 60 Questions

**Exam Blueprint:** Developer Fundamentals (27%) · Process Automation & Logic (28%) · User Interface (25%) · Testing, Debugging & Deployment (20%)  
**Time Limit:** 105 minutes  
**Passing Score:** 68%  
**Instructions:** Select ONE best answer unless the question states "Choose 2" or "Choose 3".

---

## Section A: Developer Fundamentals (16 Questions)

**Q1.** A developer creates a custom object `Claim__c` with a Lookup relationship to `Account`. A business analyst asks: "Can we create a Roll-Up Summary field on `Account` to count the total number of `Claim__c` records?"

Why is this NOT possible?

- A) Roll-Up Summary fields cannot use the COUNT function
- B) Roll-Up Summary fields can only be created on the Master side of a Master-Detail relationship, and Lookup relationships do not support them (except 3 standard exceptions)
- C) `Claim__c` is a custom object and Roll-Up Summary fields only work with standard objects
- D) The `Account` object has reached its maximum number of Roll-Up Summary fields

---

**Q2.** A developer needs to display a geolocation field called `Warehouse_Location__c` on a custom object. Which field syntax correctly accesses the latitude component in Apex?

- A) `record.Warehouse_Location__c.latitude`
- B) `record.Warehouse_Location__c__latitude__s`
- C) `record.Warehouse_Location__latitude__s`
- D) `record.Warehouse_Location__c.getLatitude()`

---

**Q3.** A Salesforce admin tries to create a Hierarchical relationship on a custom object called `Department__c` to link each department to its parent department.

What happens?

- A) The relationship is created successfully
- B) The relationship fails because Hierarchical relationships can ONLY be created on the User object
- C) The relationship fails because `Department__c` is a custom object and Hierarchical requires standard objects
- D) The relationship is created but only supports one level of nesting

---

**Q4.** A developer attempts to use a formula field to concatenate the `Description` field (Long Text Area) of the `Case` object with a text string.

What happens?

- A) The formula evaluates correctly and displays the concatenated result
- B) The formula field cannot be saved because Long Text Area fields cannot be referenced in formulas
- C) The formula evaluates but truncates the Description to 255 characters
- D) The formula field displays an error at runtime: "Unsupported Field Reference"

---

**Q5.** An organization has Multiple Currencies enabled. An Account (in USD) has child Opportunity records in EUR and GBP. A Roll-Up Summary field on Account sums the Opportunity `Amount` field.

In which currency does the Roll-Up Summary display the total?

- A) EUR (the currency of the first child record)
- B) The org's corporate currency
- C) The Master record's (Account's) currency (USD)
- D) Each Opportunity's native currency — the roll-up shows multiple values

---

**Q6.** A developer writes a formula field that references a parent Account's `BillingCity` from a child `Contact` record: `Account.BillingCity`. The formula traverses 3 levels of object relationships.

What is the maximum number of relationship levels a cross-object formula can traverse?

- A) 5 levels
- B) 10 levels
- C) 15 levels
- D) 28 levels (matching SOQL relationship depth)

---

**Q7.** A developer needs to determine which Apex method returns a map of ALL sObject types available in the org, keyed by their API names.

Which method should the developer call?

- A) `Schema.describeSObjects()`
- B) `Schema.getGlobalDescribe()`
- C) `Schema.getGlobalSObjects()`
- D) `Schema.describeAllObjects()`

---

**Q8.** A developer wants to allow data import of child `Invoice__c` records and link them to their parent `Customer__c` records. The external system provides a unique customer code but NOT the 18-character Salesforce record ID.

How should the developer configure the data import?

- A) Map the CSV's customer code column to the `Customer__c.Id` field
- B) Create an External ID field on `Customer__c` for the customer code and use it to match parent records during import
- C) Create a formula field on `Customer__c` to auto-generate the Salesforce ID from the customer code
- D) Use VLOOKUP in the CSV to convert customer codes to Salesforce IDs before import

---

**Q9.** In Agentforce for developers, what is the key difference between Plan Mode and Act Mode?

- A) Plan Mode writes code; Act Mode reviews code
- B) Plan Mode analyzes architecture and drafts strategies without modifying files; Act Mode writes code, creates files, and executes commands
- C) Plan Mode is for production; Act Mode is for sandboxes
- D) There is no difference — they are aliases for the same mode

---

**Q10.** A developer writes an Apex trigger on the `Order_Event__e` Platform Event. Which trigger event is the ONLY event supported on Platform Event objects?

- A) `before insert`
- B) `after insert`
- C) `before update`
- D) `after update`

---

**Q11.** An organization publishes a Platform Event called `Payment_Processed__e`. A Lightning Web Component on the same page needs to subscribe to these events in real time.

Which module should the LWC import to subscribe?

- A) `lightning/platformEvents`
- B) `lightning/empApi`
- C) `@salesforce/eventBus`
- D) `lightning/messageService`

---

**Q12.** A developer needs to choose between REST API and SOAP API for an integration. The integration requires a strictly typed WSDL contract for a legacy Java enterprise system.

Which API should the developer choose?

- A) REST API (supports JSON and XML)
- B) SOAP API (supports XML-only with WSDL contracts)
- C) Bulk API 2.0 (supports asynchronous processing)
- D) Streaming API (supports push notifications)

---

**Q13.** A developer sets an External ID field on a custom object as NOT unique. During an upsert operation, the external ID value matches two existing records.

What happens?

- A) Salesforce updates both matching records
- B) Salesforce updates the first matching record and ignores the second
- C) Salesforce throws a `DUPLICATE_EXTERNAL_ID` exception
- D) Salesforce inserts a new record instead of updating

---

**Q14.** A developer wants to add an Invocable Action to an Agentforce Agent in Agentforce Studio. The Apex method uses `@InvocableMethod`.

What field in the Agent's action configuration tells the reasoning engine WHEN to invoke this action?

- A) Description
- B) Action Instructions
- C) Label
- D) Category

---

**Q15.** A custom object `Warranty__c` has two Master-Detail relationships — the first to `Product__c` and the second to `Customer__c`. The developer confirms `Product__c` is the Primary Master.

What determines which master is "Primary"?

- A) The master with the most records
- B) The first Master-Detail relationship created on the junction object
- C) The master object configured in the "Primary Master" setting
- D) The master with the lookup filter enabled

---

**Q16.** A developer reviews a field on a custom object and sees the API name ends in `__x`.

What does this suffix indicate?

- A) A custom field on a custom object
- B) A managed package field
- C) An External Object
- D) A Platform Event field

---

## Section B: Process Automation & Logic (17 Questions)

**Q17.** A developer writes the following code:

```apex
String companyName = 'Acme Corp';
Boolean isActive = true;
ID recordId = '001xx000003ABC';
```

Which statement about Apex primitives is correct?

- A) `String` values must use double quotes in Apex
- B) `Boolean` only supports `true` and `false` — it cannot be `null`
- C) `ID` accepts both 15-character and 18-character Salesforce record identifiers
- D) `Blob` is a subtype of `String`

---

**Q18.** A developer creates a `Map<Id, Account>` to optimize trigger logic. Which operation does a Map key lookup provide?

- A) O(n) linear search time
- B) O(log n) binary search time
- C) O(1) constant-time lookup
- D) O(n²) quadratic time

---

**Q19.** A developer writes the following parent-to-child SOQL query on Account:

```sql
SELECT Name, (SELECT LastName FROM Contact) FROM Account WHERE Industry = 'Technology'
```

Why does this query fail?

- A) Subqueries cannot be used with WHERE clauses
- B) The child relationship name must use the plural form: `Contacts` (not `Contact`)
- C) Parent-to-child queries are not supported in Apex
- D) The subquery must include the parent's ID field

---

**Q20.** A developer processes large query results and is concerned about hitting the 6 MB heap size limit. They write:

```apex
for (List<Account> batch : [SELECT Id, Name FROM Account WHERE Status__c = 'Active']) {
    for (Account a : batch) {
        a.Description = 'Reviewed';
    }
    update batch;
}
```

What is the primary benefit of using a SOQL for loop in this pattern?

- A) It bypasses the 50,000 record query limit
- B) It processes records in batches of 200, allowing garbage collection between iterations to reduce heap consumption
- C) It automatically runs in an asynchronous context
- D) It bypasses the 150 DML statement limit

---

**Q21.** A developer has the following SOQL query in a class declared as `with sharing`:

```apex
List<Case> cases = [SELECT Id, Subject FROM Case WITH USER_MODE];
```

Which security levels are enforced by `WITH USER_MODE`? (Choose 3)

- A) CRUD (Object Permissions)
- B) FLS (Field-Level Security)
- C) Record-Level Sharing
- D) IP Range Restrictions
- E) Login Hours

---

**Q22.** A developer creates a `before insert` trigger and tries to access `Trigger.newMap`:

```apex
trigger LeadTrigger on Lead (before insert) {
    Map<Id, Lead> newLeadMap = Trigger.newMap;
    // ... logic using newLeadMap
}
```

What happens?

- A) `Trigger.newMap` contains all new Lead records keyed by their temporary IDs
- B) `Trigger.newMap` is null because record IDs have not been assigned yet in `before insert`
- C) `Trigger.newMap` contains records keyed by an auto-generated index
- D) The trigger throws a compile-time error

---

**Q23.** In the save order of execution, a Workflow Rule with a field update fires. What happens next?

- A) The transaction commits immediately
- B) The record re-runs the entire save cycle: Before Triggers → Validation Rules → After Triggers (once only)
- C) Only After Triggers re-run
- D) Only Validation Rules re-run

---

**Q24.** A developer creates two separate triggers on the same `Opportunity` object: `OpportunityTrigger1` and `OpportunityTrigger2`.

In what order do they execute?

- A) Alphabetically by trigger name
- B) In the order they were created (oldest first)
- C) The execution order is non-deterministic — Salesforce does not guarantee order
- D) The trigger with `before` events always runs before the trigger with `after` events

---

**Q25.** A business analyst asks a developer to send a weekly summary email to all Account owners whose accounts have overdue tasks. The requirement is purely declarative.

What should the developer recommend?

- A) A Batch Apex class scheduled via `System.schedule()`
- B) A Schedule-Triggered Flow
- C) An Apex trigger on the Task object
- D) A Platform Event subscriber

---

**Q26.** A developer needs to expose Apex logic so that it can be called from a Screen Flow. The method accepts a list of `OrderWrapper` objects with properties marked `@InvocableVariable`.

How many `@InvocableMethod` methods can exist in a single Apex class?

- A) Unlimited
- B) 5
- C) 2 (one for input, one for output)
- D) Exactly 1

---

**Q27.** A developer implements Queueable Apex and wants to chain a follow-up job. Inside the `execute()` method, they call `System.enqueueJob()` twice to enqueue two different child jobs.

What happens?

- A) Both child jobs are enqueued and execute sequentially
- B) Only the first `enqueueJob()` call succeeds; the second throws a `LimitException`
- C) Both calls succeed but only the second job actually executes
- D) A compile-time error prevents the second `enqueueJob()` call

---

**Q28.** A developer needs to perform an HTTP callout to a REST API from within an Apex trigger's `after insert` context.

Why does this fail, and what is the correct solution?

- A) Triggers cannot reference `Http` classes; use a Flow HTTP Callout instead
- B) Callouts are prohibited in trigger context because DML has already occurred; move the callout to an `@future(callout=true)` method
- C) The trigger must be in `before insert` context for callouts to work
- D) Add `Database.AllowsCallouts` interface to the trigger

---

**Q29.** A developer writes a Schedulable Apex class and schedules it with the following cron expression:

```apex
System.schedule('Nightly Job', '0 0 2 * * ?', new NightlyCleanup());
```

What time does this job execute?

- A) Every day at midnight (12:00 AM)
- B) Every day at 2:00 AM
- C) Every 2 hours
- D) On the 2nd day of every month

---

**Q30.** A developer is writing a method that needs to call `Database.query()` to execute a SOQL query constructed dynamically from user input.

What is the correct way to use `Database.query()`?

- A) `Database.query('SELECT Id FROM Account WHERE Name = \'' + searchTerm + '\'')`
- B) `Database.query(Account.class, searchTerm)`
- C) `Database.query([SELECT Id FROM Account])`
- D) `Database.search('SELECT Id FROM Account')`

---

**Q31.** A developer adds the following ApexDoc comment to a method:

```apex
/*
 * Calculates tax for a given amount.
 * @param amount The base amount
 * @return Calculated tax
 */
public static Decimal calculateTax(Decimal amount) { ... }
```

Why is this NOT valid ApexDoc?

- A) ApexDoc does not support `@param` tags
- B) The opening delimiter must be `/**` (double asterisk), not `/*` (single asterisk)
- C) The `@return` tag must come before `@param`
- D) ApexDoc comments must be placed after the method, not before

---

**Q32.** A developer declares a variable as follows:

```apex
static Map<Id, Account> accountCache = new Map<Id, Account>();
```

The developer calls the method multiple times within the SAME Apex transaction. What happens to the `accountCache` variable?

- A) It is reset to empty on each method call
- B) It retains its values across all calls within the same transaction because it is `static`
- C) It throws an error because `static` variables cannot be Maps
- D) It is shared across all Apex transactions globally

---

**Q33.** A developer writes the following code:

```apex
Contact con = new Contact(LastName = 'Smith');
insert con;

User u = new User(/* valid user fields */);
insert u;
```

What exception is thrown?

- A) `DmlException`
- B) `MixedDmlException`
- C) `LimitException`
- D) `SecurityException`

---

## Section C: User Interface (15 Questions)

**Q34.** A developer builds a Lightning Web Component and wants it to appear in Lightning App Builder on Record Pages. What target must be specified in the `.js-meta.xml` file?

- A) `<target>lightning__AppPage</target>`
- B) `<target>lightning__RecordPage</target>`
- C) `<target>lightning__QuickAction</target>`
- D) `<target>lightning__RecordAction</target>`

---

**Q35.** In the LWC lifecycle, which hook fires FIRST when a component is created?

- A) `connectedCallback()`
- B) `renderedCallback()`
- C) `constructor()`
- D) `disconnectedCallback()`

---

**Q36.** A developer needs to access the record ID of the current record page in a Lightning Web Component.

How should the developer declare this?

- A) `@track recordId;`
- B) `@api recordId;`
- C) `@wire recordId;`
- D) `const recordId = window.location.pathname.split('/')[1];`

---

**Q37.** A developer writes a `@wire` decorator with a reactive parameter:

```javascript
@wire(getContacts, { accountId: '$recordId' })
wiredContacts;
```

What does the `$` prefix on `'$recordId'` do?

- A) It converts `recordId` to a string before passing it
- B) It makes the wire adapter re-execute automatically whenever `this.recordId` changes
- C) It escapes the variable for use in SOQL
- D) It references a global Salesforce variable

---

**Q38.** A developer needs to query an element inside a Lightning Web Component's template. They write:

```javascript
document.getElementById('myElement');
```

Why does this fail?

- A) `getElementById` is deprecated in ES6
- B) Shadow DOM encapsulation prevents `document` from accessing elements inside the component's shadow root
- C) The element's ID is dynamically generated and differs from what was set in HTML
- D) Both B and C are correct

---

**Q39.** A child LWC component dispatches a Custom Event with default settings (no `bubbles` or `composed` options set).

Which components can handle this event?

- A) Any component on the page
- B) Only the direct parent component that has a listener on the child tag
- C) All ancestor components in the DOM tree
- D) Only sibling components

---

**Q40.** A developer wants to configure a Lightning Web Component as a Quick Action on a record page. Which target and configuration must be specified in `.js-meta.xml`?

- A) `<target>lightning__QuickAction</target>` with no additional configuration
- B) `<target>lightning__RecordAction</target>` with `<actionType>ScreenAction</actionType>`
- C) `<target>lightning__RecordPage</target>` with `<actionType>QuickAction</actionType>`
- D) `<target>lightning__AppPage</target>` with `<actionType>ScreenAction</actionType>`

---

**Q41.** In an Aura component bundle, the developer wants to write reusable helper logic that can be shared across multiple controller functions.

Which file should the developer use?

- A) `Controller.js`
- B) `Helper.js`
- C) `Renderer.js`
- D) `Style.css`

---

**Q42.** A developer needs to fire an Application Event in an Aura component. Which syntax is correct?

- A) `component.getEvent("myAppEvent").fire();`
- B) `$A.get("e.c:myAppEvent").fire();`
- C) `this.dispatchEvent(new CustomEvent("myAppEvent"));`
- D) `document.dispatchEvent(new Event("myAppEvent"));`

---

**Q43.** An Aura component needs to call a server-side Apex method. In the client-side controller, the developer writes:

```javascript
var action = component.get("c.getAccountList");
action.setParams({ searchKey: component.get("v.keyword") });
action.setCallback(this, function(response) { ... });
```

What is the FINAL step to execute this server-side call?

- A) `action.execute();`
- B) `$A.enqueueAction(action);`
- C) `component.fireEvent(action);`
- D) `action.run();`

---

**Q44.** Can a Lightning Web Component contain an Aura component inside its template?

- A) Yes — LWC can host both Aura and other LWC components
- B) No — LWC cannot contain Aura components, but Aura CAN contain LWC components
- C) Yes — but only if the Aura component is converted to a wrapper first
- D) No — neither framework can contain the other

---

**Q45.** A developer creates a Visualforce page with `standardController="Account"` and two extensions: `extensions="ExtensionA,ExtensionB"`. Both extensions define a method called `getTitle()`.

Which extension's `getTitle()` method is executed?

- A) `ExtensionB` (rightmost wins)
- B) `ExtensionA` (leftmost wins)
- C) Both are executed and the results are merged
- D) A compile error occurs because duplicate methods are not allowed

---

**Q46.** A developer needs to build a Visualforce page that displays a paginated list of Contacts with "Previous" and "Next" buttons.

What is the most efficient approach?

- A) Write a custom Apex controller with offset-based SOQL pagination
- B) Use `standardController="Contact"` with `recordSetVar="contacts"` to leverage `StandardSetController` pagination
- C) Use a Lightning Web Component instead — Visualforce cannot paginate
- D) Use a `<apex:repeat>` tag with manual page number tracking

---

**Q47.** A developer is building a Visualforce page that renders within Lightning Experience. The page currently uses classic Salesforce styling.

Which attribute should the developer add to `<apex:page>` to apply Lightning Experience look-and-feel?

- A) `lightning="true"`
- B) `lightningStylesheets="true"`
- C) `renderAs="lightning"`
- D) `theme="lightning"`

---

**Q48.** A Visualforce page uses the `<apex:actionPoller>` component to periodically refresh data from the server.

What is the MINIMUM interval allowed for `<apex:actionPoller>`?

- A) 1 second
- B) 3 seconds
- C) 5 seconds
- D) 10 seconds

---

## Section D: Testing, Debugging & Deployment (12 Questions)

**Q49.** What are the three core requirements for deploying Apex code to a Production organization? (Choose 3)

- A) 75% overall org-wide code coverage
- B) Every trigger must have at least 1% code coverage (>0%)
- C) All test methods must pass (100% pass rate)
- D) Every class must have at least 50% individual coverage
- E) All tests must complete within 10 minutes

---

**Q50.** A developer writes the following test method:

```apex
@isTest
static void testBatchJob() {
    // Insert test data
    List<Account> accts = new List<Account>();
    for (Integer i = 0; i < 200; i++) {
        accts.add(new Account(Name = 'Test ' + i));
    }
    insert accts;

    Test.startTest();
    Database.executeBatch(new CleanupBatch(), 200);
    Test.stopTest();

    // Assert results
    List<Account> remaining = [SELECT Id FROM Account WHERE Name LIKE 'Test%'];
    Assert.areEqual(0, remaining.size(), 'All test accounts should be deleted');
}
```

Why are the assertions placed AFTER `Test.stopTest()`?

- A) Assertions cannot be placed between `startTest()` and `stopTest()`
- B) `Test.stopTest()` forces the batch job to complete synchronously, so results are only available after it returns
- C) Governor limits prevent assertions during the test block
- D) Assertions before `stopTest()` would see uncommitted data

---

**Q51.** A developer creates an `HttpCalloutMock` implementation to test a REST callout. In what order should the developer set up the test?

- A) 1) Call the method with callout → 2) Register the mock → 3) Assert results
- B) 1) Register the mock with `Test.setMock()` → 2) Call `Test.startTest()` → 3) Execute the method with callout → 4) Call `Test.stopTest()` → 5) Assert results
- C) 1) Call `Test.startTest()` → 2) Register the mock → 3) Execute the callout → 4) Assert → 5) Call `Test.stopTest()`
- D) 1) Assert expected values → 2) Register the mock → 3) Execute the callout

---

**Q52.** A developer sets a Trace Flag on User "Jane Doe" with a Debug Level that has `Apex Code` set to `FINEST` and `Database` set to `FINEST`. After executing a transaction, the debug log is truncated.

What is the most likely cause?

- A) Trace Flags have a maximum duration of 30 minutes
- B) The transaction generated a debug log exceeding the 20 MB maximum log size
- C) The `FINEST` level is invalid for the Database category
- D) Only one category can be set to `FINEST` at a time

---

**Q53.** A developer opens the Execute Anonymous window in Developer Console and runs:

```apex
Account a = new Account(Name = 'Anonymous Test');
insert a;
```

In which security mode does this code execute?

- A) System Mode (ignores all permissions)
- B) User Mode (current user's permissions and sharing rules are enforced)
- C) Admin Mode (elevated privileges)
- D) Test Mode (automatic rollback)

---

**Q54.** In the Developer Console's Checkpoint Inspector, a developer examines the Heap tab. What information does the "State" sub-tab display?

- A) The call stack trace at the checkpoint
- B) The exact field values of an object instance at that point in execution
- C) The CPU time consumed up to the checkpoint
- D) The SOQL query count at the checkpoint

---

**Q55.** A developer needs to deploy a change set from a Sandbox to Production. The change set includes Apex classes but NO Apex triggers.

What is the minimum test level required for this production deployment?

- A) No tests are required since there are no triggers
- B) `RunLocalTests` — all local tests in the org must run and the org must achieve 75% coverage
- C) `RunSpecifiedTests` — only tests for the changed classes must run
- D) Both B and C are valid options for production deployment

---

**Q56.** Which sandbox type copies BOTH metadata AND a sample of production data using a Sandbox Template?

- A) Developer Sandbox
- B) Developer Pro Sandbox
- C) Partial Copy Sandbox
- D) Full Sandbox

---

**Q57.** A developer needs to remove an obsolete Apex class from Production. They attempt to add the class to a Change Set for deletion.

Why does this fail?

- A) Apex classes cannot be included in Change Sets
- B) Change Sets can deploy metadata but CANNOT delete metadata from the target org
- C) The class must first be deprecated before deletion
- D) Only admins can delete Apex classes, not developers

---

**Q58.** A developer wants to create a Managed Package for distribution on AppExchange.

In which organization type MUST the Managed Package be created?

- A) Full Sandbox
- B) Scratch Org
- C) Developer Edition or Partner Developer Edition org
- D) Any Production organization with an Enterprise license

---

**Q59.** A developer needs to view debug logs generated by another user ("Bob") in the Developer Console. By default, only their own logs are visible.

What should the developer do?

- A) Ask Bob to share his Developer Console session
- B) Set a Trace Flag on Bob's user and uncheck `Debug > Show My Current Logs Only`
- C) Run Bob's tests in their own org
- D) Export Bob's logs via the API and import them

---

**Q60.** A developer runs a test suite containing 5 test classes in Developer Console.

How does Developer Console execute this test suite?

- A) Synchronously — one class at a time in alphabetical order
- B) Asynchronously — running 2+ test classes always executes as an asynchronous operation
- C) In parallel threads — each class runs simultaneously
- D) Synchronously if all classes are in the same namespace

---

---

# ✅ Answer Key & Explanations

## Section A: Developer Fundamentals

**Q1. Answer: B) Roll-Up Summary fields can only be created on the Master side of a Master-Detail relationship**  
Roll-Up Summary fields require a Master-Detail relationship. Lookup relationships do NOT support them, with exactly 3 standard exceptions: Account→Opportunity, Opportunity→OpportunityProduct, and Campaign→CampaignMember. Account→Claim__c is a Lookup, not one of the exceptions.

**Q2. Answer: C) record.Warehouse_Location__latitude__s**  
Geolocation compound fields use a special suffix convention. The latitude component is accessed as `FieldName__latitude__s` and longitude as `FieldName__longitude__s`. Note the double underscore and lowercase `s` — NOT `__c`.

**Q3. Answer: B) Hierarchical relationships can ONLY be created on the User object**  
Hierarchical relationships are a special self-referential lookup that exists exclusively on the `User` object (e.g., User → Manager). To create a self-referential link on a custom object, use a standard Lookup relationship.

**Q4. Answer: B) The formula field cannot be saved because Long Text Area fields cannot be referenced in formulas**  
Formula fields cannot reference Long Text Area, Rich Text Area, or Encrypted Text fields. The formula editor will prevent saving with a validation error.

**Q5. Answer: C) The Master record's (Account's) currency (USD)**  
When Multiple Currencies is enabled, Roll-Up Summary fields convert child values and display the result in the Master record's currency. So the EUR and GBP Opportunity amounts are converted to USD (the Account's currency).

**Q6. Answer: B) 10 levels**  
Cross-object formulas can traverse up to 10 relationship levels upward (child → parent → grandparent, etc.).

**Q7. Answer: B) Schema.getGlobalDescribe()**  
`Schema.getGlobalDescribe()` returns `Map<String, SObjectType>` containing all sObject types in the org keyed by API name. `describeSObjects()` exists but takes a list of names. `getGlobalSObjects()` and `describeAllObjects()` are fake methods.

**Q8. Answer: B) Create an External ID field on Customer__c for the customer code**  
During data import, child records can be linked to parent records using the parent's External ID field. This eliminates the need to know Salesforce record IDs. The External ID should be marked as Unique for reliable matching.

**Q9. Answer: B) Plan Mode analyzes architecture without modifying files; Act Mode writes code and executes commands**  
Plan Mode is read-only — it inspects schemas, evaluates risks, and creates strategies. Act Mode is the execution phase that modifies files, creates classes, runs CLI commands, and deploys metadata.

**Q10. Answer: B) after insert**  
Platform Event objects only support `after insert` triggers. There is no `before insert`, `update`, or `delete` for Platform Events because event messages are immutable and cannot be updated or deleted.

**Q11. Answer: B) lightning/empApi**  
LWC components subscribe to Platform Events using the `lightning/empApi` module's `subscribe` function. `lightning/messageService` is for LMS (cross-DOM communication), not Platform Events.

**Q12. Answer: B) SOAP API (supports XML-only with WSDL contracts)**  
SOAP API uses XML format exclusively and provides formal WSDL contracts for strictly typed enterprise integrations. REST API is more flexible but doesn't provide WSDL contracts.

**Q13. Answer: C) Salesforce throws a DUPLICATE_EXTERNAL_ID exception**  
When an External ID is NOT set to Unique and an upsert matches multiple records, Salesforce throws a `DUPLICATE_EXTERNAL_ID` exception. It does NOT update either record.

**Q14. Answer: B) Action Instructions**  
The `Action Instructions` field in the Agentforce Agent's action configuration tells the AI reasoning engine when and how to invoke the action. It serves as the natural-language prompt for the agent.

**Q15. Answer: B) The first Master-Detail relationship created on the junction object**  
The Primary Master is determined by the ORDER of creation. The first MDR created becomes the Primary Master and determines the junction object's icon, color, and detail page layout.

**Q16. Answer: C) An External Object**  
The `__x` suffix indicates an External Object, which represents data stored outside of Salesforce accessed in real-time via Salesforce Connect. `__c` = custom object/field, `__e` = Platform Event, `__mdt` = Custom Metadata Type.

---

## Section B: Process Automation & Logic

**Q17. Answer: C) ID accepts both 15-character and 18-character Salesforce record identifiers**  
The Apex `ID` data type accepts both 15-char (case-sensitive) and 18-char (case-insensitive) record IDs. Strings use single quotes (not double). Booleans CAN be null. Blob is NOT a subtype of String.

**Q18. Answer: C) O(1) constant-time lookup**  
Map key lookups in Apex (and generally in hash map implementations) provide O(1) constant-time access, making them essential for trigger bulkification patterns.

**Q19. Answer: B) The child relationship name must use the plural form: Contacts**  
Standard parent-to-child relationship queries must use the plural relationship name. `Contact` is the object name; `Contacts` is the relationship name from Account's perspective.

**Q20. Answer: B) It processes records in batches of 200, allowing garbage collection**  
SOQL for loops process records in internal batches of 200. After each iteration, the processed batch can be garbage collected, reducing heap memory consumption. It does NOT bypass the 50,000 query limit or the 150 DML limit.

**Q21. Answer: A, B, C) CRUD, FLS, and Record-Level Sharing**  
`WITH USER_MODE` enforces all three security levels: CRUD (object permissions), FLS (field-level security), AND record-level sharing. This is the most comprehensive security enforcement option available in SOQL/DML.

**Q22. Answer: B) Trigger.newMap is null in before insert**  
In `before insert`, records have NOT been assigned IDs yet (they don't exist in the database). Since `Trigger.newMap` is a `Map<Id, sObject>` keyed by record ID, and IDs are null, the map is null.

**Q23. Answer: B) The record re-runs the entire save cycle (Before Triggers → Validation → After Triggers) once**  
When a Workflow Rule performs a field update, Salesforce re-evaluates the record through Before Triggers, Validation Rules, and After Triggers ONE additional time. This is a well-known re-trigger behavior.

**Q24. Answer: C) The execution order is non-deterministic**  
When multiple triggers exist on the same object, Salesforce does NOT guarantee execution order. This is why the best practice is to have ONE trigger per object that delegates to a handler class.

**Q25. Answer: B) A Schedule-Triggered Flow**  
PD1 follows the Declarative-First principle. A Schedule-Triggered Flow is the declarative solution for time-based recurring automation. Batch Apex is programmatic and should only be chosen when Flow cannot meet the requirement.

**Q26. Answer: D) Exactly 1**  
Only ONE `@InvocableMethod` method is allowed per Apex class. The compiler throws an error if you attempt to add a second one.

**Q27. Answer: B) Only the first enqueueJob() succeeds; the second throws a LimitException**  
In a synchronous transaction, only 1 child Queueable job can be enqueued from a parent Queueable. Attempting to enqueue a second throws a `LimitException` (too many queueable jobs added to the queue).

**Q28. Answer: B) Callouts are prohibited after DML; move the callout to an @future(callout=true) method**  
Apex prohibits callouts in the same transaction where DML has been performed (and trigger context always has pending DML). The solution is to move the callout to an `@future(callout=true)` method or a Queueable implementing `Database.AllowsCallouts`.

**Q29. Answer: B) Every day at 2:00 AM**  
The cron expression `'0 0 2 * * ?'` means: seconds=0, minutes=0, hours=2, every day, every month, any day of week. This fires at 2:00 AM daily.

**Q30. Answer: A) Database.query('SELECT Id FROM Account WHERE Name = ...')**  
`Database.query()` accepts a String parameter containing a dynamic SOQL query. It returns `List<sObject>`. Option C passes an inline SOQL array (not a string). `Database.search()` is for SOSL, not SOQL.

**Q31. Answer: B) The opening delimiter must be /\*\* (double asterisk), not /\* (single asterisk)**  
ApexDoc requires the opening delimiter `/**` (slash + double asterisk). A single `/*` is just a standard multi-line comment and will NOT generate ApexDoc documentation.

**Q32. Answer: B) It retains its values across all calls within the same transaction**  
`static` variables in Apex persist for the duration of the transaction (single request/response cycle). They are shared across all method calls within that transaction but reset between separate transactions.

**Q33. Answer: B) MixedDmlException**  
Inserting a non-Setup object (Contact) and a Setup object (User) in the same synchronous transaction throws a `MixedDmlException`. The fix is to move one DML operation to an `@future` method or Queueable.

---

## Section C: User Interface

**Q34. Answer: B) lightning__RecordPage**  
To make an LWC available on Record Pages in Lightning App Builder, the `.js-meta.xml` must include `<target>lightning__RecordPage</target>`. `lightning__AppPage` is for App Pages, and `lightning__RecordAction` is for Quick Actions.

**Q35. Answer: C) constructor()**  
The `constructor()` fires first during component instantiation. It must call `super()`. You cannot access `@api` properties, inspect the DOM, or dispatch events in the constructor.

**Q36. Answer: B) @api recordId;**  
Declaring `@api recordId;` in the LWC JS class allows Salesforce to automatically inject the 18-character record ID when the component is placed on a Record Page. `@track` is private, `@wire` is for data services.

**Q37. Answer: B) It makes the wire adapter re-execute automatically when this.recordId changes**  
The `$` prefix creates a reactive binding. Whenever the value of `this.recordId` changes, the wire adapter automatically re-invokes with the new value, fetching fresh data.

**Q38. Answer: D) Both B and C are correct**  
Shadow DOM encapsulation prevents `document.querySelector/getElementById` from reaching inside component shadow roots. Additionally, Salesforce dynamically generates element IDs, so the ID in the DOM won't match what was declared in HTML. Use `this.template.querySelector()` instead.

**Q39. Answer: B) Only the direct parent component with a listener on the child tag**  
By default (no `bubbles`, no `composed`), a Custom Event fires on the child element and does NOT bubble. Only the parent component that has a handler directly on the child tag (`onmyevent={handler}`) can catch it.

**Q40. Answer: B) lightning__RecordAction with actionType ScreenAction**  
LWC Quick Actions use `<target>lightning__RecordAction</target>` with `<actionType>ScreenAction</actionType>`. `lightning__QuickAction` is a fake target that doesn't exist.

**Q41. Answer: B) Helper.js**  
The `Helper.js` file in an Aura bundle contains reusable shared logic accessible across multiple controller functions. The controller delegates to the helper for business logic.

**Q42. Answer: B) $A.get("e.c:myAppEvent").fire();**  
Application Events in Aura are fired using `$A.get("e.c:eventName")` to get a reference, set params, and call `.fire()`. `component.getEvent()` is for Component Events only. `dispatchEvent(CustomEvent)` is LWC syntax.

**Q43. Answer: B) $A.enqueueAction(action);**  
After setting up the action, parameters, and callback, the final step is `$A.enqueueAction(action)` to add the server call to the action queue. There is no `.execute()` or `.run()` method.

**Q44. Answer: B) No — LWC cannot contain Aura, but Aura CAN contain LWC**  
This is a one-way relationship: Aura components can embed LWC components. But LWC components CANNOT embed Aura components. This is due to LWC's direct-to-browser architecture.

**Q45. Answer: B) ExtensionA (leftmost wins)**  
When multiple extensions define the same method, the leftmost extension in the comma-separated list takes precedence. `ExtensionA` is listed first, so its `getTitle()` executes.

**Q46. Answer: B) Use standardController with recordSetVar to leverage StandardSetController**  
`recordSetVar` activates the `StandardSetController`, which provides built-in pagination (`next`, `previous`, `hasNext`, `hasPrevious`) without any custom Apex code.

**Q47. Answer: B) lightningStylesheets="true"**  
Adding `lightningStylesheets="true"` to the `<apex:page>` tag applies Lightning Design System (SLDS) styling to Visualforce components, giving them the Lightning Experience look and feel.

**Q48. Answer: C) 5 seconds**  
The minimum interval for `<apex:actionPoller>` is 5 seconds. Setting it lower throws a validation error. This prevents excessive server load from rapid polling.

---

## Section D: Testing, Debugging & Deployment

**Q49. Answer: A, B, C) 75% org coverage, triggers > 0%, all tests pass**  
The three requirements for production deployment are: (1) 75% overall org-wide coverage, (2) every trigger must have >0% coverage, and (3) all tests must pass with a 100% pass rate. There is no requirement for individual class coverage (except with `RunSpecifiedTests`).

**Q50. Answer: B) Test.stopTest() forces the batch to complete synchronously**  
Batch Apex (and all async operations) are queued during `startTest()` but only execute when `stopTest()` is called. Assertions must come AFTER `stopTest()` because that's when the batch actually runs and commits results.

**Q51. Answer: B) Register mock → startTest → execute callout → stopTest → assert**  
The mock must be registered BEFORE the callout executes. Best practice: register with `Test.setMock()` first, then `Test.startTest()`, execute the method, `Test.stopTest()`, then assert.

**Q52. Answer: B) The log exceeds the 20 MB maximum log size**  
Setting multiple categories to `FINEST` generates extremely verbose logging. When the log exceeds 20 MB, Salesforce truncates it. The solution is to reduce logging levels for categories you don't need.

**Q53. Answer: B) User Mode**  
Execute Anonymous runs in User Mode — the current user's permissions and sharing rules are enforced. This is different from standard Apex classes/triggers which run in System Mode by default.

**Q54. Answer: B) The exact field values of an object instance at that point**  
The Checkpoint Inspector's Heap → State sub-tab shows the exact runtime values of all fields on an object instance at the moment the checkpoint was captured.

**Q55. Answer: D) Both B and C are valid options**  
When deploying Apex to production, you can choose `RunLocalTests` (runs all non-managed-package tests, needs 75% org coverage) OR `RunSpecifiedTests` (runs only specified test classes, needs 75% individual coverage per component). Both are valid for production.

**Q56. Answer: C) Partial Copy Sandbox**  
Partial Copy Sandbox copies metadata + a sample of production data (using a Sandbox Template to define which objects to copy). Developer and Developer Pro copy metadata only. Full Sandbox copies everything.

**Q57. Answer: B) Change Sets cannot delete metadata from the target org**  
Change Sets can only deploy (add/update) metadata — they CANNOT delete metadata from the target org. To delete metadata, use Salesforce CLI with `destructiveChanges.xml`.

**Q58. Answer: C) Developer Edition or Partner Developer Edition org**  
Managed Packages CANNOT be created in Sandboxes or Scratch Orgs. They must be created in a Developer Edition or Partner Developer Edition organization.

**Q59. Answer: B) Set a Trace Flag on Bob and uncheck "Show My Current Logs Only"**  
To see another user's logs: (1) Create a Trace Flag targeting Bob's user, (2) In Developer Console, uncheck `Debug > Show My Current Logs Only` to see all users' logs.

**Q60. Answer: B) Asynchronously — running 2+ test classes always executes asynchronously**  
When running a test suite or 2+ test classes simultaneously, Developer Console ALWAYS executes them asynchronously. Only a single test class run can be configured for synchronous execution.
