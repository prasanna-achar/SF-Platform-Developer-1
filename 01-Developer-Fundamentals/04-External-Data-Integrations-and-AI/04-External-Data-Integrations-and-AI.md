# 04 — External Data, Integrations & Agentforce

> **NotebookLM Study Guide:** Comprehensive breakdown of Salesforce Connect, relationship mapping for External Objects, integration APIs, Platform Events, Agentforce modes, and exam traps for Salesforce Platform Developer I (PD1).

---

## 1. Core Architecture & Mental Model

Modern Salesforce architectures frequently require reading and updating data that resides in external ERP, SAP, or legacy data lakes without physical data duplication inside Salesforce.

---

## 2. Salesforce Connect & External Objects (`__x`)

External Objects (API name suffix `__x`) provide real-time access to external data via OData protocols using Salesforce Connect.

### The 3 External Relationship Types (Memorize This Matrix!):

| Relationship Type | Child Object | Parent Object | Match Condition | When to Use |
| :--- | :--- | :--- | :--- | :--- |
| **Indirect Lookup** | **External Object** | **Standard or Custom Object** | Matches against Parent's **Unique External ID** field. | When external system does NOT store the 18-char Salesforce ID. |
| **Lookup** | **External Object** | **Standard or Custom Object** | Matches against Parent's **18-char Salesforce ID**. | When external system stores the Salesforce Record ID. |
| **External Lookup** | **Standard, Custom, or External** | **External Object** | Matches against Parent External Object ID column. | When parent record resides outside Salesforce. |

> ⚠️ **The Exam Rule:** Master-Detail relationships **CANNOT** be created with External Objects!

---

## 3. Integration API Selection Matrix

| API Name | Format | Communication Style | Primary Use Case & Scale |
| :--- | :--- | :--- | :--- |
| **REST API** | JSON, XML | Synchronous request/response | Mobile apps, lightweight web integrations, interactive UI requests. |
| **SOAP API** | **XML Only** | Synchronous contract-based (WSDL) | Enterprise server-to-server integrations requiring formal contracts. |
| **Bulk API 2.0** | CSV, JSON | Asynchronous batch processing | Large data volumes (**> 50,000 to millions of records**). |
| **Streaming API** | JSON (Bayeux / CometD) | Push / Pub-Sub notifications | Real-time event notifications (Platform Events, Change Data Capture). |

---

## 4. Platform Events (`__e`) Architecture

* **Definition:** Defined under `Platform Events` in Setup (suffix `__e`). Event messages cannot be updated or deleted.
* **Publishing Platform Events:**
  1. **Declarative (Preferred on PD1):** After-Save Record-Triggered Flow using 'Create Records' on the event object.
  2. **Programmatic (Apex):** `EventBus.publish(myEventList);`.
  3. **External Apps:** Standard **REST API HTTP POST** to `/services/data/vXX.X/sobjects/MyEvent__e/`.
* **Subscribing to Platform Events:**
  1. **Declarative:** Platform Event-Triggered Flow or Process.
  2. **Programmatic (Apex):** `after insert` Apex Trigger on `MyEvent__e`.
  3. **LWC:** `lightning/empApi` module (`subscribe` function).
  4. **External Apps:** CometD client or EMP Connector using long-polling.

---

## 5. Agentforce & Modern Developer AI Tools

Salesforce's modern AI engine for developers enforces strict architectural boundaries:

| Mode / Concept | What It Does | What It CANNOT Do |
| :--- | :--- | :--- |
| **Plan Mode** | Analyzes workspace, inspects schemas, evaluates architectural risks, writes implementation strategy. | ❌ **CANNOT modify, create, or delete any files!** |
| **Act Mode** | Executes code edits, creates classes/components, runs CLI commands, deploys metadata. | Requires explicit plan approval. |
| **Context Types** | Real-time inputs: `Terminal Context` (CLI logs), `Problem Context` (compiler errors), `File Context` (active code), `sObject Context` (schema). | — |
| **Invocable Actions**| Added in Agentforce Studio (Setup). **Action Instructions** field dictates when the reasoning engine invokes the action. | Methods must use `@InvocableMethod`. |

---

## 6. High-Frequency Exam Traps & Distractors

| Trap Topic | What Looks Correct (The Distractor) | Why It's Dead Wrong | The Official Exam Rule |
| :--- | :--- | :--- | :--- |
| **ERP Contacts to Account**| External Lookup | External lookup is when parent is outside SF. | Child external to parent SF without SF ID = **Indirect Lookup**! |
| **Publishing Event from App**| "Use SOAP create() POST request" | SOAP does not use POST endpoints directly. | Use standard **REST API POST** to the event's sObject endpoint. |
| **Flow vs Apex Callout** | "Write Apex callout for simple POST" | PD1 always favors declarative options. | Use **Flow Builder HTTP Callout Action**. |
| **Agentforce Modes** | "Debug Mode" or "Analysis Mode" | Fake mode names. | Only **Plan Mode** and **Act Mode** exist. |
| **MDR on External Obj** | "Create Master-Detail relationship" | External objects do not support MDR. | External objects support **Indirect, External, or Lookup** only. |

---

## 7. Quick-Fire Summary Cards (NotebookLM Dialogue Ready)

* **Q: Which relationship links a child external object to a parent Salesforce standard object when no 18-character Salesforce ID exists in the external database?**  
  **A:** Indirect Lookup relationship (matching against the parent's Unique External ID).

* **Q: How does an external web application publish a Platform Event into Salesforce using standard HTTP?**  
  **A:** By making an HTTP POST request to the Salesforce REST API sobjects endpoint for that event (`/services/data/vXX.X/sobjects/Event_Name__e/`).

* **Q: What is the primary difference between Plan Mode and Act Mode in Agentforce?**  
  **A:** Plan Mode analyzes architecture and drafts strategies without touching files; Act Mode writes code, modifies files, and executes commands.

* **Q: Which API is specifically optimized for inserting or querying more than 50,000 records asynchronously?**  
  **A:** Bulk API 2.0.
