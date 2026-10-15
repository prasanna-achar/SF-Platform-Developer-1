# 01 — Lightning Web Components (LWC) Architecture & Mechanics

> **NotebookLM Study Guide:** Comprehensive breakdown of LWC architecture, reactivity, lifecycle hooks, data communication, and exam traps for Salesforce Platform Developer I (PD1).

---

## 1. Core Architecture & Mental Model

### What is LWC?
Lightning Web Components (LWC) is built directly on modern web standards (Web Components, Custom Elements, Shadow DOM, ECMAScript 6+). Unlike Aura, which runs on an abstraction framework on top of JavaScript, LWC runs natively in the browser engine.

### Key Architectural Pillars:
1. **Shadow DOM Encapsulation:**
   - Elements inside an LWC template are rendered inside a Shadow Root.
   - CSS styles defined in an LWC component are automatically scoped to that component only—they cannot leak out or bleed into child/parent components.
   - Standard DOM traversal (`document.querySelector`) cannot cross into shadow boundaries. Developers must use `this.template.querySelector()`.

2. **Component Bundle Structure:**
   - Every LWC bundle lives in a folder named after the component (camelCase):
     ```
     myComponent/
     ├── myComponent.html        (HTML template - required)
     ├── myComponent.js          (ES6 JavaScript class - required)
     ├── myComponent.js-meta.xml (Configuration & target exposure - required)
     ├── myComponent.css         (Scoped CSS - optional)
     └── myComponent.svg         (Custom icon for App Builder - optional)
     ```
   - ❌ **Exam Trap:** There is NO `.cmp`, `.design`, or `.helper` file in LWC. Those are **Aura-only** artifacts!

3. **Lightning Locker & Lightning Web Security (LWS):**
   - LWS is the modern security architecture replacing Lightning Locker.
   - Enforces cross-namespace isolation, prevents prototype pollution, and restricts access to unsupported global browser APIs (e.g., direct access to `window.top`).

---

## 2. Syntax, APIs & Exact Code Signatures

### Component Definition (.js)
```javascript
import { LightningElement, api, wire, track } from 'lwc';
import getAccountDetails from '@salesforce/apex/AccountController.getAccountDetails';
import { ShowToastEvent } from 'lightning/platformShowToastEvent';

export default class MyComponent extends LightningElement {
    @api recordId; // Automatically populated if placed on a record page
    @api objectApiName;

    @track accountData = {}; // @track needed for object properties & array mutations

    // Reactive Wire Adapter with '$' prefix
    @wire(getAccountDetails, { accountId: '$recordId' })
    wiredAccount({ error, data }) {
        if (data) {
            this.accountData = data;
        } else if (error) {
            this.dispatchEvent(
                new ShowToastEvent({
                    title: 'Error loading account',
                    message: error.body.message,
                    variant: 'error'
                })
            );
        }
    }
}
```

### Reactivity Model: `@api`, `@track`, and Normal Properties
| Decorator / Property | Mutability | Scope | When to Use |
| :--- | :--- | :--- | :--- |
| **Normal Property** | Read/Write internally | Private | Primitive values (`string`, `number`, `boolean`). Changing the value re-renders the component automatically in modern LWC. |
| **`@track`** | Read/Write internally | Private | **Objects and Arrays**. Needed when mutating a property inside an object (e.g. `this.user.name = 'Joe'`) or array elements (`this.items.push(x)`). |
| **`@api`** | **Read-Only to Child** | Public | Exposed to parent components and Lightning App Builder. Set by parent; child cannot reassign its own `@api` property directly. Also used to expose public methods. |

---

## 3. The LWC Component Lifecycle

| Lifecycle Hook | Execution Point | What You CAN Do | What You CANNOT Do |
| :--- | :--- | :--- | :--- |
| **`constructor()`** | Component instantiation. Fires first. | Call `super()`, initialize basic variables. | ❌ Do NOT access UI attributes (`recordId`), inspect DOM, or dispatch events. |
| **`connectedCallback()`** | Component is inserted into the DOM. | Access public `@api` properties, initiate external data fetching, subscribe to LMS. | ❌ Child component DOM is not yet rendered. |
| **`renderedCallback()`** | Called **after every render** and re-render. | Custom DOM manipulations, post-render canvas/library hooks. | ⚠️ **Infinite Loop Trap:** Changing a reactive property here triggers another re-render! Always use a boolean guard (`if (this.hasRendered) return;`). |
| **`disconnectedCallback()`**| Component is removed from the DOM. | Cleanup tasks: unsubscribe from LMS, clear timers, remove global window listeners. | — |
| **`errorCallback(error, stack)`**| Child component throws an error during render or lifecycle. | Capture child error, display fallback UI. (Acts like an error boundary). | — |

---

## 4. Component Communication Architecture

### 1. Child to Parent: Custom Events
* The child instantiates and dispatches a standard DOM `CustomEvent`.
* The parent handles the event declaratively in template: `onmyevent={handleMyEvent}`.

```javascript
// Child JS:
const selectedEvent = new CustomEvent('contactselected', {
    detail: { contactId: this.contact.Id }
});
this.dispatchEvent(selectedEvent);
```

```html
<!-- Parent HTML: -->
<c-contact-card oncontactselected={handleContactSelected}></c-contact-card>
```

#### Event Propagation & Shadow DOM Boundary:
* **`bubbles: false, composed: false` (Default):** Event fires on child; does not bubble up DOM. Only parent listener directly on the child tag catches it.
* **`bubbles: true, composed: false`:** Bubbles up inside the child's template, but **stops at the shadow boundary**.
* **`bubbles: true, composed: true`:** **Crosses the Shadow DOM boundary!** Bubbles up the entire DOM tree. Used when child is deeply nested or inside slots.

---

### 2. Parent to Child: `@api` Properties & Methods
* Parent queries child and invokes an `@api` annotated method:
```javascript
// Parent JS:
this.template.querySelector('c-child-modal').openModal();
```

---

### 3. Cross-DOM Communication: Lightning Message Service (LMS)
LMS allows components to communicate across the DOM tree without a parent-child relationship (LWC ↔ Aura ↔ Visualforce ↔ Utility Bar).

#### The 4 Critical LMS Rules:
1. **Message Channel Name:** Must end in **`__c`**!
   ```javascript
   import RECORD_SELECTED_CHANNEL from '@salesforce/messageChannel/RecordSelected__c';
   ```
2. **Import Module:** `lightning/messageService` (NOT `@salesforce/messageService`!).
   ```javascript
   import { publish, subscribe, unsubscribe, MessageContext } from 'lightning/messageService';
   ```
3. **Context Wire:** Must wire `MessageContext`:
   ```javascript
   @wire(MessageContext) messageContext;
   ```
4. **Payload Extraction:** Message fields are read as direct object properties (`message.recordId`), NOT `message.data.recordId`.

---

## 5. Lightning Data Service (LDS) & `lightning/uiRecordApi`

### When to Use LDS Base Components vs. `uiRecordApi` vs. Apex:
* **`<lightning-record-form>`:** Zero code. Auto-renders edit/view form with standard layout or field list.
* **`<lightning-record-view-form>` / `<lightning-record-edit-form>`:** Custom layout, custom styling, individual `<lightning-input-field>` elements.
* **`lightning/uiRecordApi` (`createRecord`, `updateRecord`, `deleteRecord`):** Custom JS logic required before DML without writing server-side Apex.
* **Apex Controller (`@AuraEnabled`):** Complex transactions, multi-object DML, operations exceeding LDS capabilities, bulk saves.

#### Datatable Bulk Save Trap:
```javascript
// ❌ TRAP: uiRecordApi.updateRecord is single record ONLY!
// It CANNOT update an array of draft values directly.
// To save multiple records from a datatable draftValues array,
// you must pass the array to an @AuraEnabled Apex method!
```

#### Updating a Record via `updateRecord`:
```javascript
import { updateRecord } from 'lightning/uiRecordApi';
import ID_FIELD from '@salesforce/schema/Contact.Id';
import FIRSTNAME_FIELD from '@salesforce/schema/Contact.FirstName';

// MUST include the Id field!
const fields = {};
fields[ID_FIELD.fieldApiName] = event.detail.draftValues[0].Id;
fields[FIRSTNAME_FIELD.fieldApiName] = this.updatedFirstName;

const recordInput = { fields };
updateRecord(recordInput).then(() => { /* handle success */ });
```

---

## 6. High-Frequency Exam Traps & Distractors

| Trap Topic | What Looks Correct (The Distractor) | Why It's Dead Wrong | The Official Exam Rule |
| :--- | :--- | :--- | :--- |
| **`refreshApex`** | `refreshApex(this.getAccounts)` | Passing the Apex function does nothing. | You MUST pass the **wired result object**: `refreshApex(this.wiredAccountResult)`. |
| **`createRecord` invocation** | `this.createRecord(input)` | `createRecord` is not a method on `LightningElement`. | It is a standalone imported function: `createRecord(recordInput).then(...)`. |
| **LWC Quick Actions** | `<target>lightning__QuickAction</target>` | Fake target XML tag. | Target is **`lightning__RecordAction`** with `<actionType>ScreenAction</actionType>`. |
| **Aura vs LWC Events** | Choosing Component Event for unrelated components | Component events only travel UP the containment hierarchy. | Use **Lightning Message Service (LMS)** or **Application Events** for unrelated components. |
| **DOM Querying** | `document.getElementById('myId')` | Fails due to Shadow DOM and dynamic ID generation by Salesforce. | Use **`this.template.querySelector('.myClass')`**. |
| **`@api` Mutation** | Child component: `this.myPublicProp = 'new'` | Throws runtime error in console. | `@api` properties are **immutable to the child**. Child must emit an event requesting parent to update it. |

---

## 7. Quick-Fire Summary Cards (NotebookLM Dialogue Ready)

* **Q: How do you access the record ID on a Lightning Record Page in LWC?**  
  **A:** Declare `@api recordId;` in the JS class. Salesforce automatically injects the 18-character ID.

* **Q: What character makes an Apex wire parameter reactive?**  
  **A:** The dollar sign prefix (`'$recordId'`). When `this.recordId` changes, the wire method automatically re-executes.

* **Q: Which lifecycle hook should you use to load third-party JavaScript libraries (e.g., Chart.js)?**  
  **A:** `renderedCallback()`, using `loadScript` and `loadStyle` from `lightning/platformResourceLoader`, wrapped in a guard boolean to run only once.

* **Q: How does LWC publish data to an Aura component or Visualforce page?**  
  **A:** Through a Lightning Message Channel using Lightning Message Service (LMS).

* **Q: If a Custom Event must cross the Shadow DOM boundary to reach an outer parent, what parameters must be set?**  
  **A:** `{ bubbles: true, composed: true }`.
