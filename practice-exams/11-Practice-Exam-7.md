# Practice Exam 7 — Full 60 Questions

**Exam Blueprint:** Developer Fundamentals (27%) · Process Automation & Logic (28%) · User Interface (25%) · Testing, Debugging & Deployment (20%)  
**Time Limit:** 105 minutes  
**Passing Score:** 68%  
**Instructions:** Select ONE best answer unless the question states "Choose 2" or "Choose 3".

---

## Section A: Developer Fundamentals (16 Questions)

**Q1.** A developer has a `Service_Request__c` object in a Master-Detail relationship with `Account`. The developer is told to create a Roll-Up Summary on Account that calculates the average `Resolution_Time__c` (a Number field) across all child `Service_Request__c` records.

What is the correct approach?

- A) Create a single Roll-Up Summary field using the AVERAGE function
- B) Create two Roll-Up Summary fields (SUM and COUNT of `Resolution_Time__c`) and then a Formula field dividing SUM by COUNT
- C) Use Apex to calculate the average in a trigger
- D) Use a cross-object formula on Account to reference child field values

---

**Q2.** A company has a custom object `Expense_Report__c` (Detail) in a Master-Detail relationship with `Employee__c` (Master). The `Expense_Report__c` has a formula field `Daily_Rate__c` that references `Employee__c.Hourly_Rate__c * 8` (a cross-object formula).

A developer tries to create a Roll-Up Summary field on `Employee__c` to SUM the `Daily_Rate__c` values.

What happens?

- A) The Roll-Up Summary is created successfully
- B) The Roll-Up Summary cannot be saved because Roll-Up Summaries cannot reference cross-object formula fields
- C) The Roll-Up Summary is created but displays $0.00
- D) The Roll-Up Summary throws a runtime error when calculating

---

**Q3.** A developer needs to convert a Lookup relationship on `Case_Note__c` to `Case` into a Master-Detail relationship. Some `Case_Note__c` records have the Lookup field set to null (no parent Case).

What must the developer do FIRST?

- A) Delete all `Case_Note__c` records without a parent Case
- B) Populate the Lookup field on ALL `Case_Note__c` records with a valid parent Case ID before conversion
- C) Convert directly — Salesforce automatically assigns orphan records to a default parent
- D) Change the field type to External Lookup first, then to Master-Detail

---

**Q4.** Three standard lookup relationships in Salesforce support Roll-Up Summary fields natively. Which are they? (Choose 3)

- A) Account → Opportunities
- B) Account → Cases
- C) Opportunity → Opportunity Products (Line Items)
- D) Campaign → Campaign Members
- E) Contact → Cases

---

**Q5.** A developer needs to use Apex to check whether the `LastName` field on the `Contact` object is accessible to the running user before performing a query.

Which code snippet correctly checks field-level accessibility?

- A) `Contact.LastName.getDescribe().isAccessible()`
- B) `Schema.Contact.LastName.isReadable()`
- C) `Schema.DescribeFieldResult.isViewable(Contact.LastName)`
- D) `Contact.sObjectType.getDescribe().isAccessible()`

---

**Q6.** A developer attempts to delete a custom field `Region__c` from `Account`. The field is referenced in a Dashboard filter and a Report column.

What happens?

- A) Deletion is blocked because of the Dashboard filter reference
- B) Deletion is blocked because of the Report column reference
- C) Deletion succeeds — Salesforce automatically removes the field from Reports and Dashboards
- D) Deletion succeeds but the Report shows an error column

---

**Q7.** A developer creates a formula field on `Contact` that references `Account.Owner.Email` — traversing Contact → Account → User (Owner).

How many relationship levels does this formula span?

- A) 1 level
- B) 2 levels
- C) 3 levels
- D) This formula is invalid because you cannot reference User fields in formulas

---

**Q8.** A developer reviews a custom object API name that ends in `__mdt`.

What type of object is this?

- A) A custom object
- B) An External Object
- C) A Custom Metadata Type
- D) A Platform Event

---

**Q9.** A company uses Salesforce Connect to access data from an OData-compliant SAP system. The SAP employee records are displayed as External Objects.

Can the developer create a Master-Detail relationship between the External Object and a Salesforce custom object?

- A) Yes — External Objects support all relationship types
- B) No — External Objects only support Indirect Lookup, External Lookup, and standard Lookup relationships
- C) Yes — but only if the External Object is the detail (child)
- D) Yes — but the developer must use a junction object

---

**Q10.** An external mobile application needs to publish a `Delivery_Status__e` Platform Event to Salesforce using standard HTTP.

Which method and endpoint should the application use?

- A) HTTP GET to `/services/data/vXX.X/query/?q=SELECT+Id+FROM+Delivery_Status__e`
- B) HTTP POST to `/services/data/vXX.X/sobjects/Delivery_Status__e/`
- C) HTTP PUT to `/services/data/vXX.X/sobjects/Delivery_Status__e/`
- D) HTTP POST to `/services/streaming/vXX.X/Delivery_Status__e/`

---

**Q11.** A developer needs to use `Schema.describeTabs()` to get information about all apps and their tabs. The developer expects a list of individual tab descriptions.

What does the method ACTUALLY return?

- A) `List<Schema.DescribeTabResult>` — one per tab
- B) `List<Schema.DescribeTabSetResult>` — one per application (app), each containing its tabs
- C) `Map<String, List<Schema.DescribeTabResult>>` — keyed by app name
- D) `Schema.DescribeTabResult[]` — an array of tab descriptions

---

**Q12.** A developer creates an External ID field on a custom object and marks it as Unique. During an upsert operation, a record in the CSV file matches an existing Salesforce record by External ID.

What happens?

- A) A new record is inserted and the existing record is left unchanged
- B) The existing record is updated with the values from the CSV
- C) A `DUPLICATE_EXTERNAL_ID` exception is thrown
- D) Both records are merged into one

---

**Q13.** Which function returns the exchange rate for a given currency ISO code relative to the org's corporate currency in a formula field?

- A) `CURRENCY(CurrencyIsoCode)`
- B) `CONVERSIONRATE(CurrencyIsoCode)`
- C) `CURRENCYRATE(CurrencyIsoCode)`
- D) `CURRENTRATE(CurrencyIsoCode)`

---

**Q14.** A developer is told that Formula fields are NOT supported in which of the following contexts? (Choose 2)

- A) Lightning Record Pages
- B) Web-to-Lead forms
- C) SOQL WHERE clauses
- D) Web-to-Case forms

---

**Q15.** A developer reviews the following Apex code:

```apex
Schema.DescribeSObjectResult dsr = Account.sObjectType.getDescribe();
List<Schema.RecordTypeInfo> rtList = dsr.getRecordTypeInfos();
```

What data structure does `getRecordTypeInfos()` return?

- A) `Map<Id, RecordTypeInfo>`
- B) `Map<String, RecordTypeInfo>`
- C) `List<RecordTypeInfo>`
- D) `Set<RecordTypeInfo>`

---

**Q16.** A developer working in an org that uses Knowledge sees a custom field with a `File` data type on a Knowledge article type. The developer attempts to convert this field's data type.

What happens?

- A) The conversion succeeds if the field contains no data
- B) The conversion is blocked because Knowledge `File` fields can NEVER be converted
- C) The conversion succeeds only if converted to Text type
- D) The conversion requires deleting all existing articles first

---

## Section B: Process Automation & Logic (17 Questions)

**Q17.** A developer declares the following variables in Apex:

```apex
List<String> names = new List<String>{'Alice', 'Bob', 'Alice'};
Set<String> uniqueNames = new Set<String>{'Alice', 'Bob', 'Alice'};
```

How many elements does `names` contain vs. `uniqueNames`?

- A) names: 3, uniqueNames: 3
- B) names: 3, uniqueNames: 2
- C) names: 2, uniqueNames: 2
- D) names: 2, uniqueNames: 3

---

**Q18.** A developer needs to write a child-to-parent SOQL query on a custom child object `TimeEntry__c` that has a custom Lookup to `Project__c`. The developer wants to retrieve the Project's Name.

Which syntax is correct?

- A) `SELECT Id, Project__c.Name FROM TimeEntry__c`
- B) `SELECT Id, Project__r.Name FROM TimeEntry__c`
- C) `SELECT Id, (SELECT Name FROM Project__c) FROM TimeEntry__c`
- D) `SELECT Id, Project.Name FROM TimeEntry__c`

---

**Q19.** A developer writes the following code to enforce FLS in a SOQL query:

```apex
List<Account> accts = [SELECT Id, Name, Phone, AnnualRevenue FROM Account WITH SECURITY_ENFORCED];
```

The running user does NOT have FLS read access to `AnnualRevenue`. What happens?

- A) `AnnualRevenue` is returned as `null` for all records
- B) `AnnualRevenue` is silently removed from the query results
- C) The query throws a `System.QueryException`
- D) The query succeeds and returns `AnnualRevenue` because it is a standard field

---

**Q20.** A developer wants to write a trigger that detects when a Contact's email address changes and logs the old value.

Which trigger context variables provide access to the old field values? (Choose 2)

- A) `Trigger.new`
- B) `Trigger.old`
- C) `Trigger.newMap`
- D) `Trigger.oldMap`

---

**Q21.** A developer needs to write a `before update` trigger on `Opportunity` that sets the `Close_Date__c` to today when the Stage changes to "Negotiation". The developer does NOT want to use an explicit DML statement.

Is this possible in a `before` trigger?

- A) No — all field changes require an explicit `update` DML statement
- B) Yes — in `before` triggers, modifying fields directly on `Trigger.new` records is automatically persisted without DML
- C) Yes — but only if the field is not a Date type
- D) No — `before` triggers are read-only and cannot modify records

---

**Q22.** Which of the following trigger events does NOT exist in Salesforce? (Choose 2)

- A) `before insert`
- B) `before undelete`
- C) `after update`
- D) `after merge`

---

**Q23.** A developer writes a class implementing `Database.Batchable<sObject>`. The `start()` method returns a `Database.QueryLocator` with a query that returns 5 million records.

What is the maximum number of records that can be processed by a single Batch Apex job?

- A) 10,000 records
- B) 50,000 records
- C) 50 million records
- D) Unlimited — limited only by system resources

---

**Q24.** A developer needs to execute a one-time data fix that inserts a `Group` (Setup object) and a `Case` (non-Setup object).

What happens if both DML operations are in the same synchronous method?

- A) Both records are inserted successfully
- B) A `MixedDmlException` is thrown
- C) The `Group` is inserted but the `Case` is rolled back
- D) A `DmlException` is thrown with message "Invalid sObject type"

---

**Q25.** A developer creates a Queueable class that needs to make an HTTP callout to an external service during execution.

What additional interface must the class implement?

- A) `HttpCalloutMock`
- B) `Database.AllowsCallouts`
- C) `Callable`
- D) `Http.Callout`

---

**Q26.** A developer writes the following Schedulable Apex class:

```apex
public class WeeklyReport implements Schedulable {
    public void execute(SchedulableContext sc) {
        Database.executeBatch(new ReportGeneratorBatch());
    }
}
```

The developer calls `System.schedule('Weekly', cronExpr, new WeeklyReport())`.

What does `System.schedule()` return?

- A) `void` — no return value
- B) A Job ID (`ID`) that can be used to monitor or abort the scheduled job
- C) A `CronTrigger` object
- D) A `Boolean` indicating success or failure

---

**Q27.** A developer creates a Record-Triggered Flow on the `Opportunity` object that fires after an Opportunity is updated. The developer also has an `after update` Apex trigger on the same object.

In the save order of execution, which runs FIRST?

- A) The Record-Triggered Flow runs first
- B) The After Trigger runs first
- C) They run simultaneously
- D) The order depends on which was created first

---

**Q28.** A developer builds a Screen Flow that needs to call custom Apex logic for complex shipping calculations. The Apex method uses `@InvocableMethod`.

What must be the access level of the `@InvocableVariable` properties in the input/output wrapper classes?

- A) `private`
- B) `protected`
- C) `public` or `global`
- D) Any access level — the annotation handles visibility

---

**Q29.** A developer needs to handle an error that occurs during a DML operation. They write:

```apex
try {
    insert accounts;
} catch (DmlException e) {
    System.debug('DML error: ' + e.getDmlMessage(0));
}
```

This works for standard DML errors. However, if the insert exceeds the 150 DML statement governor limit, will the `catch` block handle it?

- A) Yes — all exceptions including governor limit exceptions are catchable
- B) No — `System.LimitException` is uncatchable and causes the entire transaction to roll back
- C) Yes — but only if the developer uses `Database.insert()` with `allOrNone = false`
- D) No — but the transaction continues with partial data saved

---

**Q30.** A developer writes the following anonymous Apex code:

```apex
String query = 'SELECT Id, Name FROM Account WHERE Name = :searchTerm';
String searchTerm = 'Acme';
List<Account> results = Database.query(query);
```

What happens when this code executes?

- A) The query succeeds and binds `searchTerm` correctly
- B) The query throws an exception because bind variables (`:searchTerm`) are NOT supported in dynamic SOQL via `Database.query()`
- C) The query returns all accounts regardless of the WHERE clause
- D) The query succeeds only if `searchTerm` is declared before the query string

---

**Q31.** A developer defines the following method:

```apex
global static void processRecords(List<Contact> contacts) {
    // processing logic
}
```

The developer wants to call this from a Flow. They add the `@InvocableMethod` annotation. But the class already contains another method with `@InvocableMethod`.

What happens?

- A) Both methods are available in Flow
- B) A compile-time error occurs because only ONE `@InvocableMethod` is allowed per class
- C) The second method overrides the first
- D) Both methods compile but only the first is visible in Flow

---

**Q32.** A developer writes a custom validation rule on `Opportunity` that checks:

`AND(ISCHANGED(StageName), ISPICKVAL(StageName, "Closed Won"), Amount < 1000)`

In the save order of execution, when does this validation rule evaluate relative to the `before update` trigger?

- A) Before the `before update` trigger
- B) After the `before update` trigger
- C) Simultaneously with the `before update` trigger
- D) After the `after update` trigger

---

**Q33.** A developer writes the following Apex code to process records efficiently:

```apex
Set<Id> accountIds = new Set<Id>();
for (Opportunity opp : Trigger.new) {
    accountIds.add(opp.AccountId);
}
Map<Id, Account> accountMap = new Map<Id, Account>(
    [SELECT Id, Name FROM Account WHERE Id IN :accountIds]
);
```

Why does the developer use a `Set<Id>` and a `Map<Id, Account>` instead of querying inside the loop?

- A) To avoid the 100 SOQL query governor limit by querying once outside the loop
- B) To improve code readability only — there is no performance difference
- C) To prevent `MixedDmlException` errors
- D) To enforce FLS security on the queried records

---

## Section C: User Interface (15 Questions)

**Q34.** A developer needs an LWC component to be available on Home Pages, Record Pages, AND App Pages in Lightning App Builder.

Which targets should be included in `.js-meta.xml`? (Choose 3)

- A) `lightning__RecordPage`
- B) `lightning__HomePage`
- C) `lightning__AppPage`
- D) `lightning__FlowScreen`
- E) `lightning__UtilityBar`

---

**Q35.** A developer writes the following LWC JavaScript:

```javascript
import { LightningElement, api } from 'lwc';

export default class ChildComponent extends LightningElement {
    @api greeting = 'Hello';

    updateGreeting() {
        this.greeting = 'Goodbye';
    }
}
```

A parent component sets `greeting` to "Hi". When the child's `updateGreeting()` method is called, what happens?

- A) `greeting` updates to "Goodbye" successfully
- B) A runtime error occurs because the child cannot reassign its own `@api` property
- C) `greeting` updates locally but the parent still sees "Hi"
- D) A compile-time error prevents the component from deploying

---

**Q36.** A developer creates a Lightning Web Component that needs to unsubscribe from a Lightning Message Service channel and clear a setInterval timer when the component is removed from the DOM.

Which lifecycle hook should the developer use?

- A) `constructor()`
- B) `connectedCallback()`
- C) `renderedCallback()`
- D) `disconnectedCallback()`

---

**Q37.** An LWC child component throws an unhandled JavaScript error during rendering. The parent component has implemented `errorCallback(error, stack)`.

What happens?

- A) The entire page crashes with a white screen
- B) The parent's `errorCallback` catches the child's error and can display a fallback UI
- C) The error is silently swallowed and the child renders as empty
- D) The Salesforce platform automatically displays a toast error message

---

**Q38.** A developer needs to use the `createRecord` function from `lightning/uiRecordApi` to create a new Contact. They write:

```javascript
this.createRecord(recordInput);
```

Why does this fail?

- A) `createRecord` requires a callback function as the second argument
- B) `createRecord` is a standalone imported function, NOT a method on `LightningElement` — call it as `createRecord(recordInput).then(...)`
- C) `createRecord` only works with custom objects, not standard objects
- D) `createRecord` requires the `@wire` decorator

---

**Q39.** When using Lightning Message Service in an LWC, a developer reads the payload from a received message. The message has a field `recordId`.

How does the developer access this field from the message object?

- A) `message.data.recordId`
- B) `message.detail.recordId`
- C) `message.recordId`
- D) `message.getParam('recordId')`

---

**Q40.** A developer places an LWC component inside an Aura component. The LWC needs to communicate with a Visualforce page also on the same Lightning page.

Which communication mechanism works across ALL THREE frameworks (LWC, Aura, Visualforce)?

- A) Custom Events
- B) Application Events
- C) Lightning Message Service (LMS)
- D) `@api` properties

---

**Q41.** When a nested Aura application loads, in what order do the `init` event handlers fire across parent and child components?

- A) Parent `init` fires first, then child `init`
- B) Child `init` fires first, then parent `init` (innermost first)
- C) All `init` handlers fire simultaneously
- D) The order is random and non-deterministic

---

**Q42.** An Aura component developer handles an Application Event using:

```html
<aura:handler name="myEvent" event="c:myAppEvent" action="{!c.handleIt}"/>
```

The event handler never fires. What is wrong?

- A) Application Event handlers do NOT use the `name` attribute — remove it
- B) The event type should be `COMPONENT` not `APPLICATION`
- C) `$A.get()` must be used instead of `<aura:handler>`
- D) The action should reference the Helper, not the Controller

---

**Q43.** A developer creates a Visualforce page that uses a custom controller. The controller needs to use the `ApexPages.StandardSetController` to paginate a list of Opportunities.

Which constructors are valid for `StandardSetController`? (Choose 2)

- A) `new ApexPages.StandardSetController(List<sObject>)`
- B) `new ApexPages.StandardSetController(Database.QueryLocator)`
- C) `new ApexPages.StandardSetController(Map<Id, sObject>)`
- D) `new ApexPages.StandardSetController(Set<sObject>)`

---

**Q44.** A Visualforce controller extension must work with the `Account` standard controller.

What constructor signature MUST the extension class have?

- A) `public MyExtension()` — a no-argument constructor
- B) `public MyExtension(ApexPages.StandardController stdController)`
- C) `public MyExtension(Account account)`
- D) `public MyExtension(Id recordId)`

---

**Q45.** A developer needs to display error messages from Apex on a Visualforce page. The controller uses `ApexPages.addMessage()` to add messages.

Which component must be included in the Visualforce markup to render these messages?

- A) `<apex:outputText value="{!errors}"/>`
- B) `<apex:pageMessages/>`
- C) `<apex:messages type="error"/>`
- D) `<apex:errorPanel/>`

---

**Q46.** A developer is building a Visualforce page that outputs user-entered data inside a `<script>` block. To prevent Cross-Site Scripting (XSS) attacks, which function should the developer use?

- A) `HTMLENCODE()`
- B) `JSENCODE()`
- C) `URLENCODE()`
- D) `TEXTENCODE()`

---

**Q47.** The Einstein Next Best Action (NBA) pipeline has 4 stages. A developer needs to dynamically generate recommendations using Apex logic within the Strategy Builder.

Which Strategy Builder element should the developer use?

- A) Load
- B) Generate
- C) Branch Selector
- D) Enhance

---

**Q48.** A developer wants to show Dynamic Forms on a Lightning Record Page. This allows placing individual fields and field sections with conditional visibility.

What does Dynamic Forms REPLACE?

- A) Lightning App Builder
- B) Standard Page Layouts
- C) Visualforce Pages
- D) Lightning Components

---

## Section D: Testing, Debugging & Deployment (12 Questions)

**Q49.** A developer writes a test class and needs to test that a standard user cannot access private records owned by another user.

Which mechanism should the developer use?

- A) `System.assertEquals()` to compare user profiles
- B) `System.runAs(standardUser)` to execute code under the standard user's security context
- C) `Test.setMock()` to mock the user's permissions
- D) `Assert.isFalse(UserInfo.isCurrentUserLicensed())`

---

**Q50.** A developer's test class has `@isTest(SeeAllData=true)`. The test queries `CollaborationGroup` (Chatter groups).

Why might `SeeAllData=true` be acceptable in this specific case?

- A) It is never acceptable — `SeeAllData=true` should never be used
- B) `CollaborationGroup` records cannot be created via DML in test context, so `SeeAllData=true` is the only way to access them
- C) `CollaborationGroup` is a setup object and is always visible in tests
- D) `SeeAllData=true` improves test performance

---

**Q51.** A developer mocks a SOAP web service callout in a test class. Which interface must the mock class implement?

- A) `HttpCalloutMock`
- B) `WebServiceMock`
- C) `SoapCalloutMock`
- D) `WsdlCalloutMock`

---

**Q52.** How many `Test.startTest()` / `Test.stopTest()` pairs are allowed within a single `@isTest` method?

- A) Unlimited
- B) Up to 3
- C) Exactly 1
- D) Up to 5

---

**Q53.** A developer sets a Trace Flag on an Apex class called `OrderProcessor`.

What does this Trace Flag do?

- A) It traces all users who execute the `OrderProcessor` class
- B) It generates debug logs ONLY when `OrderProcessor` executes, regardless of which user triggers it
- C) It applies the Debug Level settings specifically when `OrderProcessor` is part of the execution stack
- D) It disables all other Trace Flags while `OrderProcessor` runs

---

**Q54.** A developer examines a debug log and sees the following lines:

```
EXECUTION_STARTED
  CODE_UNIT_STARTED: MyTrigger
  CODE_UNIT_FINISHED: MyTrigger
EXECUTION_FINISHED
```

What do these 4 lines collectively represent?

- A) Four separate execution units
- B) A single execution unit containing a trigger code unit
- C) Two execution units nested together
- D) An error log showing a failed trigger

---

**Q55.** A developer needs to deploy Apex code to production. They choose the `RunSpecifiedTests` test level and include 2 test classes in the deployment package.

What code coverage rule applies to each Apex class and trigger in the deployment?

- A) Each must have at least 1% coverage
- B) The org-wide average must be 75%
- C) Each individual class and trigger must independently achieve at least 75% coverage
- D) No minimum — as long as the specified tests pass

---

**Q56.** A developer needs to deploy metadata to a completely unrelated Salesforce org (not connected via deployment pipeline).

Which tool should the developer use?

- A) Change Sets
- B) Salesforce CLI / Metadata API
- C) Data Loader
- D) Schema Builder

---

**Q57.** What is the refresh interval for a Full Sandbox?

- A) 1 day
- B) 5 days
- C) 14 days
- D) 29 days

---

**Q58.** A developer creates a new sandbox. They notice that user email addresses now end in `.invalid`.

Why does Salesforce do this?

- A) To indicate the sandbox users are inactive
- B) To prevent test emails from being accidentally sent to real customers
- C) To force users to re-verify their email addresses
- D) To comply with GDPR data masking requirements

---

**Q59.** A developer needs to run a Quick Deploy after successfully validating a deployment to production. The validation was completed 3 days ago.

Can the developer use Quick Deploy?

- A) No — Quick Deploy is only available immediately after validation
- B) Yes — Quick Deploy is available if the validation succeeded within the last 4 days
- C) No — Quick Deploy is only available for Change Sets, not CLI deployments
- D) Yes — Quick Deploy has no time limit

---

**Q60.** A developer is debugging a complex automation that involves triggers, flows, and workflow rules on the `Opportunity` object. They want to see all debug log events including database operations, Apex code execution, and workflow evaluations.

Which Debug Level categories should be set? (Choose 3)

- A) Apex Code
- B) Database
- C) Workflow
- D) Visualforce
- E) Callout

---

---

# ✅ Answer Key & Explanations

## Section A: Developer Fundamentals

**Q1. Answer: B) Create two Roll-Up Summary fields (SUM and COUNT) and a Formula field for the average**  
AVERAGE is NOT a supported Roll-Up Summary function. The supported operations are COUNT, SUM, MIN, and MAX only. To calculate an average, create a SUM roll-up, a COUNT roll-up, and divide them in a formula field.

**Q2. Answer: B) Roll-Up Summaries cannot reference cross-object formula fields**  
If a detail formula field contains a cross-object reference (referencing a parent field), it CANNOT be used in a Roll-Up Summary field. Salesforce throws a validation error when attempting to save. The workaround is to use a Flow to copy the value into a regular field first.

**Q3. Answer: B) Populate the Lookup field on ALL records with a valid parent before conversion**  
Converting a Lookup to Master-Detail requires every child record to have a valid parent populated. If any child records have null values in the Lookup field, the conversion fails. All orphan records must be assigned a parent first.

**Q4. Answer: A, C, D) Account→Opportunities, Opportunity→OpportunityProducts, Campaign→CampaignMembers**  
These are the only three standard lookup relationships that natively support Roll-Up Summary fields. Account→Cases and Contact→Cases do NOT support roll-up summaries.

**Q5. Answer: A) Contact.LastName.getDescribe().isAccessible()**  
`Contact.LastName.getDescribe()` returns a `DescribeFieldResult`, and `isAccessible()` checks whether the running user has read access to that field. `isReadable()` and `isViewable()` are fake methods.

**Q6. Answer: C) Deletion succeeds — Salesforce automatically removes the field from Reports and Dashboards**  
Report and Dashboard references do NOT block field deletion. Salesforce automatically cleans up these references. Field deletion IS blocked by Apex, Triggers, Flows, Visualforce, and Formula fields.

**Q7. Answer: B) 2 levels**  
Contact → Account is level 1, Account → Owner (User) is level 2. The formula spans 2 relationship levels. Cross-object formulas support up to 10 levels.

**Q8. Answer: C) A Custom Metadata Type**  
The `__mdt` suffix indicates a Custom Metadata Type. `__c` = custom object/field, `__x` = External Object, `__e` = Platform Event.

**Q9. Answer: B) No — External Objects only support Indirect Lookup, External Lookup, and standard Lookup**  
Master-Detail relationships CANNOT be created with External Objects. External Objects only support Indirect Lookup, External Lookup, and standard Lookup relationships.

**Q10. Answer: B) HTTP POST to /services/data/vXX.X/sobjects/Delivery_Status__e/**  
External applications publish Platform Events by making a standard REST API HTTP POST request to the event's sObject endpoint. GET is for queries, PUT is for updates, and the `/streaming/` path is for subscriptions.

**Q11. Answer: B) List<Schema.DescribeTabSetResult> — one per application**  
`Schema.describeTabs()` returns `List<Schema.DescribeTabSetResult>` where each element represents an application (TabSet), and each contains its constituent tabs.

**Q12. Answer: B) The existing record is updated with the values from the CSV**  
During an upsert, when a unique External ID matches exactly one existing record, the record is UPDATED with the new values. `DUPLICATE_EXTERNAL_ID` only occurs when a non-unique External ID matches multiple records.

**Q13. Answer: C) CURRENCYRATE(CurrencyIsoCode)**  
`CURRENCYRATE()` is the only valid function for retrieving exchange rates in formulas. `CURRENCY()`, `CONVERSIONRATE()`, and `CURRENTRATE()` do not exist.

**Q14. Answer: B, D) Web-to-Lead and Web-to-Case forms**  
Formula fields are read-only and calculated server-side. They cannot be placed on Web-to-Lead or Web-to-Case forms, which are external-facing HTML forms that submit data.

**Q15. Answer: C) List<RecordTypeInfo>**  
`getRecordTypeInfos()` returns a `List<RecordTypeInfo>`, NOT a Map. To get a Map, use `getRecordTypeInfosByDeveloperName()` (keyed by dev name) or `getRecordTypeInfosById()` (keyed by ID).

**Q16. Answer: B) Knowledge File fields can NEVER be converted**  
Knowledge `File` type fields are completely locked — their data type cannot be changed under any circumstances, regardless of whether they contain data.

---

## Section B: Process Automation & Logic

**Q17. Answer: B) names: 3, uniqueNames: 2**  
`List<String>` allows duplicates, so `names` has 3 elements (Alice, Bob, Alice). `Set<String>` enforces uniqueness, so `uniqueNames` has only 2 elements (Alice, Bob — the duplicate Alice is discarded).

**Q18. Answer: B) SELECT Id, Project__r.Name FROM TimeEntry__c**  
For custom relationship references in child-to-parent SOQL queries, use the `__r` suffix (relationship name) instead of `__c` (field ID). `Project__c` is the field storing the ID; `Project__r` is the relationship reference for dot-notation traversal.

**Q19. Answer: C) The query throws a System.QueryException**  
`WITH SECURITY_ENFORCED` checks both CRUD and FLS. If the user lacks FLS read access to ANY field in the SELECT clause, the query immediately throws a `QueryException`. It does NOT silently strip fields.

**Q20. Answer: B, D) Trigger.old and Trigger.oldMap**  
`Trigger.old` (List) and `Trigger.oldMap` (Map) contain the old versions of records before the update. Use `Trigger.oldMap.get(record.Id).Email` to compare old vs. new field values. `Trigger.new` and `Trigger.newMap` contain the new/updated versions.

**Q21. Answer: B) Yes — modifying Trigger.new in before triggers is automatically saved**  
In `before` triggers, you can directly modify field values on `Trigger.new` records. These changes are automatically included in the database save without needing an explicit DML statement. This is a core Salesforce platform behavior.

**Q22. Answer: B, D) before undelete and after merge**  
`before undelete` does NOT exist — only `after undelete` exists. `after merge` also does NOT exist as a trigger event. Merge operations fire `delete` triggers on the losing records and `update` triggers on the winning record.

**Q23. Answer: C) 50 million records**  
When `start()` returns a `Database.QueryLocator`, the batch can process up to 50 million records (bypassing the standard 50,000 SOQL query limit). With `Iterable`, the standard 50,000 limit applies.

**Q24. Answer: B) A MixedDmlException is thrown**  
`Group` is a Setup object. `Case` is a non-Setup object. Performing DML on both in the same synchronous transaction throws a `MixedDmlException`. The fix is to move one operation to an `@future` method.

**Q25. Answer: B) Database.AllowsCallouts**  
Queueable classes that need to make HTTP callouts must implement `Database.AllowsCallouts`. Without this interface, any callout attempt throws a `CalloutException`.

**Q26. Answer: B) A Job ID (ID) that can be used to monitor or abort the job**  
`System.schedule()` returns an `ID` representing the scheduled job. This ID maps to a `CronTrigger` record and can be used to query job status or abort the job.

**Q27. Answer: B) The After Trigger runs first**  
In the save order of execution: After Triggers execute at step 6, then Assignment Rules, Auto-Response Rules, Workflow Rules, Escalation Rules, and finally Record-Triggered Flows (step 11). Flows execute AFTER triggers.

**Q28. Answer: C) public or global**  
Variables annotated with `@InvocableVariable` must be declared as `public` or `global` to be accessible from Flow. Private or protected access levels prevent Flow from reading/writing these properties.

**Q29. Answer: B) No — LimitException is uncatchable and the entire transaction rolls back**  
`System.LimitException` (governor limit violations) are UNCATCHABLE by any `try/catch` block. When a governor limit is exceeded, the entire transaction immediately aborts and all changes roll back.

**Q30. Answer: A) The query succeeds and binds searchTerm correctly**  
Dynamic SOQL via `Database.query()` DOES support bind variables using the colon (`:`) notation. The variable must be in scope where `Database.query()` is called. This is a common misconception — bind variables work in dynamic SOQL.

**Q31. Answer: B) A compile-time error because only ONE @InvocableMethod is allowed per class**  
Salesforce enforces a strict rule: only ONE `@InvocableMethod` annotation is permitted per Apex class. Attempting to add a second causes a compile-time error.

**Q32. Answer: B) After the before update trigger**  
In the save order: System Validation → Before Triggers → Custom Validation Rules → After Triggers. Validation Rules evaluate AFTER Before Triggers, so a before trigger can fix field values before validation rules check them.

**Q33. Answer: A) To avoid the 100 SOQL query governor limit by querying once outside the loop**  
Placing a SOQL query inside a loop (per record) would hit the 100 SOQL query limit. The developer collects all needed IDs into a Set, performs a single bulk query, and stores results in a Map for O(1) lookups — this is the "bulkification" pattern.

---

## Section C: User Interface

**Q34. Answer: A, B, C) RecordPage, HomePage, AppPage**  
To make an LWC available on all three page types, include all three targets in `.js-meta.xml`: `lightning__RecordPage`, `lightning__HomePage`, and `lightning__AppPage`.

**Q35. Answer: B) A runtime error because the child cannot reassign its own @api property**  
`@api` properties are public and can be SET by the parent component, but the child component CANNOT reassign them. Doing so throws a runtime error. The child should dispatch an event requesting the parent to update the value.

**Q36. Answer: D) disconnectedCallback()**  
`disconnectedCallback()` fires when a component is removed from the DOM. This is the correct place to clean up subscriptions (LMS unsubscribe), clear timers (clearInterval), and remove global event listeners.

**Q37. Answer: B) The parent's errorCallback catches the error and can display fallback UI**  
`errorCallback(error, stack)` acts as an error boundary for child component errors during rendering or lifecycle. The parent can catch the error and render alternative UI instead of crashing the page.

**Q38. Answer: B) createRecord is a standalone imported function, not a method on LightningElement**  
`createRecord` is imported from `lightning/uiRecordApi` and called as a standalone function: `createRecord(recordInput).then(...)`. It is NOT a method on `this` (the component instance).

**Q39. Answer: C) message.recordId**  
In LMS, message fields are accessed as direct properties on the message object: `message.recordId`. NOT `message.data.recordId` or `message.detail.recordId`.

**Q40. Answer: C) Lightning Message Service (LMS)**  
LMS is the ONLY communication mechanism that works across ALL three UI frameworks: LWC, Aura, and Visualforce. Custom Events and Application Events don't reach Visualforce. `@api` only works in parent-child LWC.

**Q41. Answer: B) Child init fires first, then parent init (innermost first)**  
Aura initialization follows a bottom-up pattern. The innermost child component's `init` event fires first, then parent, then application. Think of building a house: foundation first.

**Q42. Answer: A) Application Event handlers do NOT use the name attribute — remove it**  
This is a classic trap. Application Event handlers use `<aura:handler event="c:myAppEvent" action="..."/>` WITHOUT the `name` attribute. The `name` attribute is only used for Component Events.

**Q43. Answer: A, B) List<sObject> and Database.QueryLocator**  
`StandardSetController` accepts either a `List<sObject>` or a `Database.QueryLocator`. `Map<Id, sObject>` and `Set<sObject>` are NOT valid constructor parameters.

**Q44. Answer: B) public MyExtension(ApexPages.StandardController stdController)**  
Controller extensions MUST have a constructor that accepts `ApexPages.StandardController` (for single-record pages) or `ApexPages.StandardSetController` (for list pages). A no-arg constructor won't work.

**Q45. Answer: B) <apex:pageMessages/>**  
`<apex:pageMessages/>` is the standard Visualforce component that renders messages added via `ApexPages.addMessage()`. Without it, the messages exist in memory but are never displayed on the page.

**Q46. Answer: B) JSENCODE()**  
When outputting user data inside a `<script>` block, use `JSENCODE()` to escape unsafe characters for JavaScript context. `HTMLENCODE()` is for HTML body output. `URLENCODE()` is for URL parameters. `TEXTENCODE()` doesn't exist.

**Q47. Answer: B) Generate**  
The `Generate` element in Strategy Builder calls an Apex class annotated with `@InvocableMethod` to dynamically create recommendations. `Load` loads existing static Recommendation records. `Branch Selector` routes based on conditions. `Enhance` modifies attributes.

**Q48. Answer: B) Standard Page Layouts**  
Dynamic Forms replaces the monolithic standard Page Layout system, allowing individual field placement, field sections, and conditional visibility rules directly in Lightning App Builder without the constraints of traditional page layouts.

---

## Section D: Testing, Debugging & Deployment

**Q49. Answer: B) System.runAs(standardUser)**  
`System.runAs()` is the correct mechanism for testing user context, security, and sharing. It executes code under a specific user's permissions. `System.assertEquals()` only validates values — it cannot switch user contexts.

**Q50. Answer: B) CollaborationGroup cannot be created via DML in test context**  
`SeeAllData=true` is acceptable when querying metadata objects that cannot be created in tests, such as `CollaborationGroup` (Chatter groups), standard Pricebook2, and Report/Dashboard metadata.

**Q51. Answer: B) WebServiceMock**  
SOAP callout mocking uses the `WebServiceMock` interface with its `doReply()` method. `HttpCalloutMock` is for REST callouts. `SoapCalloutMock` and `WsdlCalloutMock` don't exist.

**Q52. Answer: C) Exactly 1**  
Only ONE `Test.startTest()` / `Test.stopTest()` pair is allowed per test method. Calling `startTest()` a second time throws a `LimitException`.

**Q53. Answer: C) It applies Debug Level settings when OrderProcessor is in the execution stack**  
A Trace Flag on an Apex class applies the associated Debug Level's logging settings specifically when that class is part of the transaction's execution path.

**Q54. Answer: B) A single execution unit containing a trigger code unit**  
`EXECUTION_STARTED` + `EXECUTION_FINISHED` delimit a single execution unit. `CODE_UNIT_STARTED` + `CODE_UNIT_FINISHED` are nested within, representing the trigger code unit. All 4 lines together form one execution unit.

**Q55. Answer: C) Each individual class and trigger must achieve at least 75% coverage**  
With `RunSpecifiedTests`, the coverage is evaluated per-component, not org-wide. Every Apex class and trigger in the deployment must independently achieve ≥75% code coverage from the specified tests.

**Q56. Answer: B) Salesforce CLI / Metadata API**  
Change Sets can only deploy to RELATED orgs (connected via the deployment pipeline). Salesforce CLI / Metadata API can deploy to ANY org, including completely unrelated ones.

**Q57. Answer: D) 29 days**  
Full Sandbox refresh interval is 29 days. Developer/Developer Pro = 1 day. Partial Copy = 5 days. Full = 29 days.

**Q58. Answer: B) To prevent test emails from being accidentally sent to real customers**  
Salesforce appends `.invalid` to sandbox user emails and restricts email deliverability to prevent test automation, workflows, and flows from accidentally sending emails to real customers during testing.

**Q59. Answer: B) Yes — Quick Deploy is available if validation succeeded within the last 4 days**  
Quick Deploy is available as long as the validation completed successfully within the last 4 days (96 hours). After 4 days, the validation results expire and a new validation is required.

**Q60. Answer: A, B, C) Apex Code, Database, Workflow**  
For debugging triggers (Apex Code), database operations (Database), and workflow/flow evaluations (Workflow), these three Debug Level categories should be set to an appropriate level (e.g., FINE or FINER). Visualforce and Callout categories are not relevant for this scenario.
