# 04 — Declarative Automation, Invocable Apex & Platform Events

> **NotebookLM Study Guide:** Comprehensive breakdown of the Declarative-First philosophy, Flow types, Invocable Method contracts, Platform Event streaming, and exam traps for Salesforce Platform Developer I (PD1).

---

## 1. Core Architecture & Mental Model

### The "Declarative-First" Principle (Golden PD1 Philosophy):
Salesforce heavily prioritizes clicks-over-code. If a business requirement can be met using a standard declarative tool (Flow Builder, Validation Rule, Formula), **choosing Apex is considered an incorrect answer on the PD1 exam**!

### When to Use Flow vs. When to Use Apex:
* **Use Flow Builder:**
  - Automated field updates and task creation.
  - Guided step-by-step agent scripts (Screen Flows).
  - Outbound REST integrations with standard JSON payloads (**Flow HTTP Callout Action**).
  - Standard record-triggered notifications and Platform Event publishing.
* **Use Apex:**
  - Complex transactions exceeding Flow CPU limits.
  - Complex multi-system transactional rollbacks (`Savepoint`).
  - Highly custom algorithms or data structures not supported in Flow.

---

## 2. Invocable Apex: Bridging Flow and Code

When Flow requires custom logic, developers build **Invocable Actions**:

```apex
public with sharing class ShippingCalculator {

    // Custom Input/Output Wrapper Class
    public class ShippingRequest {
        @InvocableVariable(label='Package Weight' required=true)
        public Decimal weight;

        @InvocableVariable(label='Destination Postal Code' required=true)
        public String postalCode;
    }

    public class ShippingResult {
        @InvocableVariable(label='Shipping Cost')
        public Decimal shippingCost;
    }

    @InvocableMethod(label='Calculate Shipping Rate' description='Computes carrier rate')
    public static List<ShippingResult> calculateRates(List<ShippingRequest> requests) {
        List<ShippingResult> results = new List<ShippingResult>();
        for (ShippingRequest req : requests) {
            ShippingResult res = new ShippingResult();
            res.shippingCost = req.weight * 2.50;
            results.add(res);
        }
        return results;
    }
}
```

### The Invocable Contract Rules:
1. **Method Annotation:** Must use **`@InvocableMethod`**.
2. **Parameters & Return:** Must accept a **`List<T>`** and return a **`List<R>`** (or `void`) to support bulk flow operations.
3. **One Per Class:** Only **ONE `@InvocableMethod`** per Apex class is allowed!
4. **Variable Annotation:** Class member variables passed to/from Flow must be annotated with **`@InvocableVariable`** and declared `public` or `global`.

---

## 3. Platform Events (`__e`) Architecture & Communication

Platform Events deliver secure, scalable, and decoupled event-driven architectures across systems.

### Publishing Platform Events:
* **Declarative (Preferred):** Create an After-Save Record-Triggered Flow. Add a **Create Records** element targeting the Platform Event (`Order_Event__e`).
* **Programmatic (Apex):**
  ```apex
  Order_Event__e evt = new Order_Event__e(OrderId__c = 'ORD-123', Status__c = 'Complete');
  Database.SaveResult sr = EventBus.publish(evt);
  ```
* **External Systems:** Standard HTTP POST to the event REST endpoint: `/services/data/vXX.X/sobjects/Order_Event__e/`.

### Subscribing to Platform Events:
* **Apex Trigger:** `trigger OrderEventTrigger on Order_Event__e (after insert) { ... }`. (Platform events only support `after insert`!).
* **Flow:** Platform Event-Triggered Flow.
* **Lightning Web Components:** `lightning/empApi` module (`subscribe` function).
* **External Systems:** CometD client (using Bayeux protocol and long-polling) or Salesforce EMP Connector.

---

## 4. Prebuilt Standard Flows (The Hidden Exam Trap)

Salesforce includes prebuilt flow templates out-of-the-box:
* **'Create a Case':** Prebuilt standard flow for support agents. Walks through gathering customer info, searching existing contacts, creating contacts if missing, and generating cases.
* **Exam Rule:** If an exam question asks how to implement a call script that checks for existing contacts and logs cases, the answer is **"Use the prebuilt flow 'Create a Case' and customize it"**, NOT "Build a custom flow from scratch"!

---

## 5. High-Frequency Exam Traps & Distractors

| Trap Topic | What Looks Correct (The Distractor) | Why It's Dead Wrong | The Official Exam Rule |
| :--- | :--- | :--- | :--- |
| **HTTP Callout Choice** | "Write an Apex HTTP callout" | Flow has native callout capability. | Use **Flow Builder HTTP Callout Action**. |
| **Publishing Platform Event**| "Write an Apex trigger on Opportunity" | Flow can publish declaratively. | Use **Record-Triggered Flow (after-save)** to publish event. |
| **Invocable Signature** | Taking a single object: `process(Item i)` | Flow passes inputs in bulk. | Invocable methods **MUST accept and return `List<T>`**. |
| **Multiple Invocables** | Adding 2 `@InvocableMethod` in one class | Compiler prohibits multiple invocables per class. | Only **ONE `@InvocableMethod`** per Apex class. |
| **Prebuilt Flows** | "Build a custom screen flow from scratch" | Salesforce provides standard templates. | Leverage prebuilt standard flow **'Create a Case'**. |

---

## 6. Quick-Fire Summary Cards (NotebookLM Dialogue Ready)

* **Q: Why must `@InvocableMethod` signatures in Apex always accept and return `List<T>` collections?**  
  **A:** Because Flows execute in bulk; Apex must receive a batch of interview inputs and return corresponding outputs.

* **Q: Which trigger event is the only event supported on a Platform Event object?**  
  **A:** `after insert`.

* **Q: How does a Lightning Web Component subscribe to real-time Platform Events?**  
  **A:** By importing the `subscribe` method from the `lightning/empApi` module.

* **Q: What is the recommended declarative way to integrate with an external REST API without writing code?**  
  **A:** Flow Builder HTTP Callout Action.
