# 02 — Aura Components & Event Architecture

> **NotebookLM Study Guide:** Comprehensive breakdown of Aura component bundle structure, component vs. application events, inheritance, and exam traps for Salesforce Platform Developer I (PD1).

---

## 1. Core Architecture & Mental Model

### What is Aura?
The Aura Framework is Salesforce's legacy open-source UI framework. While modern development focuses on Lightning Web Components (LWC), Aura is heavily tested on the PD1 exam—especially around **inter-component communication**, **event propagation**, and **bundle file roles**.

### Coexistence Rule:
* **LWC inside Aura:** YES ✅ (Aura can host LWC components).
* **Aura inside LWC:** NO ❌ (LWC CANNOT host Aura components).

---

## 2. Component Bundle Resources (The 8 Files)

Every Aura component bundle has 8 potential resources, all sharing the same base name:

| Resource File | Extension | Purpose & Rules | Exam Trap Alert |
| :--- | :--- | :--- | :--- |
| **Component** | `.cmp` | Markup definition (HTML-like tags, `<aura:attribute>`). | Top-level tag is `<aura:component>`. |
| **Controller** | `Controller.js` | Client-side controller. Handles UI events and user clicks. | Delegates business logic to Helper; does NOT perform heavy logic. |
| **Helper** | `Helper.js` | Shared utility logic. Reusable functions across controller methods. | Controller functions receive `(component, event, helper)`. |
| **Style** | `.css` | Component-specific CSS. All selectors must start with `.THIS`. | ❌ `.THIS` is required or styling fails! |
| **Documentation**| `.auradoc` | User guide, code examples for component consumers. | — |
| **Renderer** | `Renderer.js` | Overrides default rendering lifecycle (`render`, `afterRender`). | Rarely used in modern apps. |
| **Design** | `.design` | Exposes attributes to **Lightning App Builder** and Experience Cloud. | Uses `<design:attribute>`. |
| **SVG** | `.svg` | Custom icon displayed in Lightning App Builder palette. | — |

---

## 3. Event Architecture: Component vs. Application Events

This is the **#1 most repeated mistake** on the PD1 exam. Memorize this comparison table:

| Characteristic | Component Event (`<aura:event type="COMPONENT">`) | Application Event (`<aura:event type="APPLICATION">`) |
| :--- | :--- | :--- |
| **Definition Tag** | `<aura:event type="COMPONENT">` | `<aura:event type="APPLICATION">` |
| **Firing Mechanism**| `component.getEvent("eventName").fire();` | `$A.get("e.c:eventName").fire();` |
| **Handling Scope** | **Containment Hierarchy ONLY** (Child $\rightarrow$ Parent). | **ANY component on the page** (broadcast model). |
| **Exam Trigger Phrase**| *"Components in a parent-child relationship"* | *"Components that may or may not share a parent"* or *"Unrelated components on a page"* |
| **Phases** | Capture Phase $\rightarrow$ Bubble Phase. | Capture Phase $\rightarrow$ Bubble Phase $\rightarrow$ Default Handler Phase. |
| **Handling Tag** | `<aura:handler name="myEvent" event="c:myEvent" action="{!c.handleEvent}"/>` | `<aura:handler event="c:myAppEvent" action="{!c.handleAppEvent}"/>` (Notice: **NO name attribute**!). |

### The Golden Rule of Aura Events:
* If the components **share a direct hierarchy** (child embedded inside parent) $ightarrow$ Use **Component Event**.
* If components are **independent**, on different parts of the page, or in different tabs $ightarrow$ Use **Application Event** (or Lightning Message Service in modern apps).
* ❌ `reRender`: That is a **Visualforce** tag/attribute (`reRender="blockId"`), NOT an Aura concept!

---

## 4. Lifecycle & Initialization Order

### Initialization Hook:
```html
<aura:handler name="init" value="{!this}" action="{!c.doInit}"/>
```

### Order of Initialization (Bottom-Up):
When an application with nested components loads, the `init` event fires **Innermost Child First**:
```
1. Innermost Child Component `init`
2. Parent Component `init`
3. Top-Level Application `init`
```
*(Think of it like building a pyramid: you must build the foundation/children before the top structure can complete).*

---

## 5. Server-Side Controller Calls from Aura

```javascript
// Controller.js
handleSearch: function(component, event, helper) {
    // 1. Get action reference to @AuraEnabled Apex method
    var action = component.get("c.findAccounts");
    
    // 2. Set parameters
    action.setParams({ searchTerm: component.get("v.searchTerm") });
    
    // 3. Set callback
    action.setCallback(this, function(response) {
        var state = response.getState();
        if (state === "SUCCESS") {
            component.set("v.accounts", response.getReturnValue());
        } else if (state === "ERROR") {
            var errors = response.getError();
            console.error(errors);
        }
    });
    
    // 4. Enqueue the action
    $A.enqueueAction(action);
}
```

---

## 6. High-Frequency Exam Traps & Distractors

| Trap Topic | What Looks Correct (The Distractor) | Why It's Dead Wrong | The Official Exam Rule |
| :--- | :--- | :--- | :--- |
| **Unrelated Components** | "Use Component Event" | Component events cannot bridge unrelated hierarchies. | Must use **Application Event** (`$A.get("e.c:...")`) or **LMS**. |
| **App Event Handler Syntax** | `<aura:handler name="test" event="c:appEvent" ...>` | Application event handlers do NOT use the `name` attribute! | Only component event handlers require `name="eventName"`. |
| **CSS Namespacing** | `.myClass { color: red; }` | CSS without `.THIS` is stripped or ignored by Aura compiler. | Every top-level CSS selector MUST start with `.THIS`. |
| **Design Attributes** | Defining App Builder properties in `.cmp` | Attributes in `.cmp` are not visible to App Builder without `.design`. | Use `<design:attribute name="myAttr" label="Label"/>` in the `.design` file. |
| **Aura in LWC** | "Embed Aura component in LWC" | Violates LWC architecture. | LWC can be contained in Aura; Aura CANNOT be contained in LWC. |

---

## 7. Quick-Fire Summary Cards (NotebookLM Dialogue Ready)

* **Q: How do you fire an Application Event in Aura?**  
  **A:** `var appEvent = $A.get("e.c:myAppEvent"); appEvent.setParams({...}); appEvent.fire();`

* **Q: What is the initialization sequence for nested Aura components?**  
  **A:** Child component `init` executes first, then Parent `init`, then Application `init`.

* **Q: Where do you declare attributes to expose them in Lightning App Builder?**  
  **A:** In the `.design` file of the Aura bundle using `<design:attribute>`.

* **Q: How does Aura enforce scoped CSS within a component?**  
  **A:** By prefixing selectors with `.THIS` in the `.css` file.
