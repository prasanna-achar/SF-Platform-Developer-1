# 03 — Visualforce Pages, Controllers & Integration

> **NotebookLM Study Guide:** Comprehensive breakdown of Visualforce page architecture, standard vs custom controllers, extensions, security enforcement, and high-frequency exam traps for Salesforce Platform Developer I (PD1).

---

## 1. Core Architecture & Mental Model

### What is Visualforce?
Visualforce is Salesforce's tag-based markup language (similar to HTML/XML) paired with server-side Apex controllers. While modern UI uses LWC, Visualforce is tested heavily on PD1 for **legacy migrations**, **page layout overrides**, **PDF rendering**, and **standard controller extensions**.

### The 3 Visualforce Controller Models:
```
1. Standard Controller       -> 1 object record, zero Apex code.
2. StandardSetController    -> Multiple records (pagination, list views).
3. Custom Controller        -> Apex class implementing all logic without standard controller.
4. Controller Extensions     -> Extends either Standard or Custom Controller with custom Apex methods.
```

---

## 2. Controller Types & Syntax Signatures

### 1. Standard Controller (Single Record)
```html
<apex:page standardController="Opportunity">
    <apex:detail subject="{!Opportunity.Id}" relatedList="true"/>
</apex:page>
```
* **Overriding Standard Buttons:** When overriding standard buttons (e.g. `View` on Opportunity), the Visualforce page **MUST use `standardController="Opportunity"`**!

---

### 2. StandardSetController (List Views & Pagination)
Used to display or work with a collection of records.
```html
<apex:page standardController="Opportunity" recordSetVar="opportunities">
    <apex:pageBlock>
        <apex:pageBlockTable value="{!opportunities}" var="opp">
            <apex:column value="{!opp.Name}"/>
            <apex:column value="{!opp.Amount}"/>
        </apex:pageBlockTable>
        <!-- Built-in Pagination Controls -->
        <apex:commandButton value="Previous" action="{!previous}" rendered="{!hasPrevious}"/>
        <apex:commandButton value="Next" action="{!next}" rendered="{!hasNext}"/>
    </apex:pageBlock>
</apex:page>
```

#### Apex Constructors for StandardSetController:
```apex
// ✅ Valid Constructors:
ApexPages.StandardSetController ssc1 = new ApexPages.StandardSetController(List<sObject>);
ApexPages.StandardSetController ssc2 = new ApexPages.StandardSetController(Database.QueryLocator);

// ❌ Invalid: Map<Id, sObject> and Set<sObject> are NOT accepted!
```

#### Over-10,000 Records Behavior Trap:
* **`Database.QueryLocator`:** If the query returns > 10,000 records, it throws a **`LimitException`**!
* **`List<sObject>`:** If a list with > 10,000 records is passed, it **silently truncates** to 10,000 records without throwing an exception!

---

### 3. Controller Extensions
An Apex class that extends the functionality of a standard or custom controller.
```html
<!-- Multiple extensions comma-separated -->
<apex:page standardController="Account" extensions="AccountExtensionOne,AccountExtensionTwo">
    <apex:outputText value="{!customMessage}"/>
</apex:page>
```

```apex
public with sharing class AccountExtensionOne {
    private final Account acct;
    
    // Constructor MUST take ApexPages.StandardController as parameter!
    public AccountExtensionOne(ApexPages.StandardController stdController) {
        this.acct = (Account)stdController.getRecord();
    }
    
    public String getCustomMessage() {
        return 'Hello from Extension!';
    }
}
```

#### Extension Execution Rules:
* Multiple extensions can be used, separated by commas.
* **Execution order:** Left-to-right (`AccountExtensionOne`, then `AccountExtensionTwo`). If both have a method with the same name, the **leftmost** extension wins!

---

## 3. High-Frequency Visualforce Tags

| Component Tag | Purpose & Capabilities | What It Does NOT Do |
| :--- | :--- | :--- |
| **`<apex:detail>`** | Displays the standard record detail page exactly as configured in Setup (fields, related lists, hover links). | Does NOT require custom layout tags. |
| **`<apex:includeScript>`** | References external JavaScript files uploaded as **Static Resources** (`value="{!$Resource.myScript}"`). | Does NOT auto-escape JS for XSS protection! |
| **`<apex:map>`** | Renders interactive Google maps with `<apex:mapMarker>`. | ❌ No `<apex:google>` or "map mode in charts" exists! |
| **`<apex:include>`** | Embeds one complete Visualforce page inside another. | Not for including scripts (use `<apex:includeScript>`). |
| **`<apex:actionFunction>`** | Invokes an Apex controller method directly from JavaScript. | — |
| **`<apex:actionPoller>`** | Periodically calls an Apex method at a set time interval (minimum 5 seconds). | — |

---

## 4. Visualforce Security & Running Modes

### System Mode vs. User Mode:
* Custom controllers run in **System Mode by default**.
* Specifying `public with sharing class MyController`:
  * Enforces **Record-Level Sharing ONLY** (OWD, Role Hierarchy, Sharing Rules).
  * ❌ **DOES NOT enforce CRUD (Object Permissions) or FLS (Field-Level Security)!**
  * Apex still has access to read and update hidden fields unless enforced via `WITH USER_MODE`, `WITH SECURITY_ENFORCED`, or `Security.stripInaccessible()`.

### Person Accounts in Visualforce:
* `IsPersonAccount` is a **Read-Only** field (cannot be assigned in Apex DML).
* In Visualforce pages supporting both Business and Person accounts:
  * Creating/Updating with `Name` populated creates a **Business Account**.
  * Creating/Updating with `LastName` populated creates a **Person Account**.
  * **Best Practice:** Create a **custom name formula field** to render the display name consistently across both types!

### Cross-Site Scripting (XSS) in Visualforce:
Salesforce automatically encodes merge fields (`{!Account.Name}`) in regular text. However, inside `<script>` blocks, developers MUST use:
* **`JSENCODE()`:** Escapes unsafe characters inside JavaScript strings.
* **`HTMLENCODE()`:** Encodes characters before displaying HTML.
* **`JSINHTMLENCODE()`:** Encodes characters inside JS that will be rendered as HTML.

---

## 5. Embedding Visualforce in Lightning Experience

* **Styling:** Set **`lightningStylesheets="true"`** on `<apex:page>` to give legacy Visualforce pages the look and feel of Lightning Experience.
* **Embedding Options:**
  1. Add to an existing **Page Layout** (classic approach).
  2. Embed via the **Visualforce Standard Component** inside Lightning App Builder on a Lightning Record Page.
  3. (Do NOT build a separate custom tab if the goal is to show related info on the record page!).

---

## 6. High-Frequency Exam Traps & Distractors

| Trap Topic | What Looks Correct (The Distractor) | Why It's Dead Wrong | The Official Exam Rule |
| :--- | :--- | :--- | :--- |
| **Standard Button Override** | Using a custom controller | Custom controller cannot replicate standard button context automatically. | **MUST use `standardController="ObjectName"`**. |
| **Multiple Extensions** | "Only one extension allowed" | Multiple extensions are fully supported. | Use comma-separated list: `extensions="ExtA,ExtB"`. |
| **Pagination Controller** | Using custom Apex pagination | StandardListController has native pagination. | Use `<apex:page standardController="Obj" recordSetVar="list">`. |
| **Map Rendering** | `<apex:google>` or "chart map mode" | Fake component / feature. | Use **`<apex:map>`** and **`<apex:mapMarker>`**. |
| **`with sharing` Security** | Assumes it prevents access to hidden fields | `with sharing` only checks record-level access. | CRUD and FLS are **ignored** unless explicitly checked in code. |
| **Person Accounts** | Writing to `IsPersonAccount = true` | Field is strictly read-only. | Set `LastName` and use a **custom formula field** for names. |

---

## 7. Quick-Fire Summary Cards (NotebookLM Dialogue Ready)

* **Q: What attribute must be on `<apex:page>` to use a StandardListController?**  
  **A:** `recordSetVar="variableName"`.

* **Q: How does a Visualforce page adopt Lightning Experience styling?**  
  **A:** Set `lightningStylesheets="true"` on the `<apex:page>` tag.

* **Q: How do you protect dynamic strings from XSS inside a Visualforce `<script>` block?**  
  **A:** Wrap the merge field with the `JSENCODE()` function.

* **Q: What constructor signature must an Apex controller extension have?**  
  **A:** A public constructor accepting `ApexPages.StandardController` (or `ApexPages.StandardSetController`).
