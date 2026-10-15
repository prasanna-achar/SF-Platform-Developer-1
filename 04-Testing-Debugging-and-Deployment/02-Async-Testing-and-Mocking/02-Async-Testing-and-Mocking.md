# 02 — Asynchronous Testing & Web Service Mocking

> **NotebookLM Study Guide:** Comprehensive breakdown of Test.startTest/stopTest mechanics, asynchronous processing validation, and HTTP/SOAP callout mocking for Salesforce Platform Developer I (PD1).

---

## 1. Core Architecture & Mental Model

### The Testing Dilemma:
In production, asynchronous Apex (`@future`, `Queueable`, `Database.Batchable`, `Schedulable`) runs in the background whenever system resources permit. However, unit tests execute synchronously and need immediate assertion checks.
Furthermore, Salesforce strictly prohibits live HTTP/SOAP callouts during unit test execution (`CalloutException: You have uncommitted work pending or live callouts not allowed`).

### The Solution:
1. **`Test.startTest()` / `Test.stopTest()`:** Forces background asynchronous threads to complete immediately.
2. **`HttpCalloutMock` & `WebServiceMock`:** Simulates external endpoint responses without touching the network.

---

## 2. `Test.startTest()` & `Test.stopTest()` Mechanics

### The Two Critical Behaviors:
1. **Resets Governor Limits:**
   - Any limits consumed *before* `Test.startTest()` (such as test data creation) are reset to zero.
   - Code between `startTest()` and `stopTest()` runs with a **brand new, pristine set of governor limits**.
2. **Forces Asynchronous Execution:**
   - When code calls an `@future` method, enqueues a `Queueable`, or executes `Database.executeBatch()`, Salesforce queues the job.
   - Calling **`Test.stopTest()` forces all queued asynchronous jobs to execute synchronously before returning**!

```apex
@isTest
static void testQueueableExecution() {
    Account acct = new Account(Name = 'Async Test');
    insert acct;

    Test.startTest();
    // Enqueue background job
    System.enqueueJob(new UpdateAccountQueueable(acct.Id));
    
    // In background, job is queued but NOT yet executed!
    Test.stopTest(); // FORCES QUEUEABLE TO COMPLETE SYNCHRONOUSLY!

    // Now safe to assert the result!
    Account updatedAcct = [SELECT Description FROM Account WHERE Id = :acct.Id];
    Assert.areEqual('Processed by Queueable', updatedAcct.Description);
}
```

> ⚠️ **Hard Exam Rule:** Only **ONE** `Test.startTest()` and `Test.stopTest()` pair is permitted per test method! Calling it twice throws a runtime exception.

---

## 3. Mocking REST HTTP Callouts (`HttpCalloutMock`)

Unit tests cannot make live calls to external endpoints. You must provide a mock implementation.

### Step 1: Create the Mock Class
Implement the `HttpCalloutMock` interface and its single method: `respond(HTTPRequest req)`.

```apex
@isTest
public class OrderServiceMock implements HttpCalloutMock {
    public HTTPResponse respond(HTTPRequest req) {
        // Assert request properties if needed
        Assert.areEqual('POST', req.getMethod());
        Assert.areEqual('callout:OrderAPI/v1/orders', req.getEndpoint());

        // Construct fake HTTPResponse
        HTTPResponse res = new HTTPResponse();
        res.setHeader('Content-Type', 'application/json');
        res.setBody('{"status": "SUCCESS", "orderId": "EXT-9988"}');
        res.setStatusCode(200);
        return res;
    }
}
```

### Step 2: Register the Mock in Your Test Method
Use `Test.setMock(HttpCalloutMock.class, new MyMockClass());` **before** the callout code executes:

```apex
@isTest
static void testCalloutSuccess() {
    // 1. Register Mock
    Test.setMock(HttpCalloutMock.class, new OrderServiceMock());

    // 2. Execute method that makes the callout
    Test.startTest();
    String orderStatus = OrderService.sendOrderToERP('001xx000003AAA');
    Test.stopTest();

    // 3. Assert result
    Assert.areEqual('SUCCESS', orderStatus);
}
```

---

## 4. Static Resource Callout Mocks

When external endpoints return large or complex JSON/XML payloads, use a **Static Resource**:

```apex
@isTest
static void testStaticResourceMock() {
    // 1. Instantiate StaticResourceCalloutMock
    StaticResourceCalloutMock mock = new StaticResourceCalloutMock();
    mock.setStaticResource('MockERPOrderResponse'); // Name of Static Resource file
    mock.setStatusCode(200);
    mock.setHeader('Content-Type', 'application/json');

    // 2. Register mock
    Test.setMock(HttpCalloutMock.class, mock);

    Test.startTest();
    OrderService.processOrders();
    Test.stopTest();
    
    // Assert results...
}
```

---

## 5. Mocking SOAP Web Services (`WebServiceMock`)

For WSDL-generated classes (SOAP integrations), implement the `WebServiceMock` interface:

```apex
@isTest
public class StockQuoteSoapMock implements WebServiceMock {
    public void doReply(
        Object stub,
        Object request,
        Map<String, Object> response,
        String endpoint,
        String soapAction,
        String requestName,
        String responseNS,
        String responseName,
        String responseType
    ) {
        // Populate the response map with mock response object
        docSample.stockQuoteResponse_element resElement = new docSample.stockQuoteResponse_element();
        resElement.price = 150.25;
        response.put('response_x', resElement);
    }
}
```
* Register using: `Test.setMock(WebServiceMock.class, new StockQuoteSoapMock());`.

---

## 6. High-Frequency Exam Traps & Distractors

| Trap Topic | What Looks Correct (The Distractor) | Why It's Dead Wrong | The Official Exam Rule |
| :--- | :--- | :--- | :--- |
| **Verifying Async Apex** | Querying async results before `Test.stopTest()` | Asynchronous threads do not run until `stopTest()` completes. | Always assert async results **AFTER `Test.stopTest()`**. |
| **Multiple `startTest`** | Using `startTest()` twice in one test method | Violates system governor rules. | Only **ONE** `startTest()` / `stopTest()` pair per test method. |
| **Live Callouts in Tests** | Setting a Remote Site Setting for testing | Remote Site Settings do not bypass the test callout restriction. | Tests **MUST use `HttpCalloutMock` or `WebServiceMock`**. |
| **Mocking Registration** | Calling `Test.setMock` after `Test.startTest` | Must be registered before callout executes. | Best practice is registering mock immediately before `Test.startTest()`. |

---

## 7. Quick-Fire Summary Cards (NotebookLM Dialogue Ready)

* **Q: What happens to asynchronous processes when `Test.stopTest()` is reached?**  
  **A:** All queued `@future`, `Queueable`, and `Batchable` jobs are forced to execute synchronously.

* **Q: How many times can `Test.startTest()` be called within a single test method?**  
  **A:** Exactly once. Calling it a second time throws a LimitException.

* **Q: Which interface is used to mock REST API callouts in Apex tests?**  
  **A:** `HttpCalloutMock`, implementing the `respond(HTTPRequest req)` method.

* **Q: How can you mock large JSON API responses without hardcoding strings in Apex test classes?**  
  **A:** Use `StaticResourceCalloutMock` pointing to a pre-uploaded Static Resource JSON file.
