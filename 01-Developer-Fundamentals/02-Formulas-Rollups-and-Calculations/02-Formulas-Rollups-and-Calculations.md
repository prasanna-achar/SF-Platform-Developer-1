# 02 — Formulas, Roll-Up Summaries & Calculation Limits

> **NotebookLM Study Guide:** Comprehensive breakdown of cross-object formulas, roll-up summary mechanics, multi-currency conversion, formula restrictions, and high-frequency exam traps for Salesforce Platform Developer I (PD1).

---

## 1. Core Architecture & Mental Model

### Declarative Calculations in Salesforce:
1. **Formula Fields:** Dynamic, read-only calculated fields evaluated on-the-fly when queried or viewed. They consume zero database storage and update automatically.
2. **Roll-Up Summary Fields:** Persistent summary calculations (SUM, COUNT, MIN, MAX) stored on the Parent/Master record, calculated whenever child detail records are saved or deleted.

---

## 2. Cross-Object Formulas & Capabilities

### Capabilities:
* Can traverse up to **10 relationship levels** upward (Child $\rightarrow$ Parent $\rightarrow$ Grandparent).
* Can span across **Master-Detail or Lookup** relationships.
* **Security Bypass (Exam Trap!):** A cross-object formula can display a parent field value to a user even if that user does **not have access to the parent record** or object permissions!

### Formula Field Restrictions:
* ❌ **CANNOT reference Long Text Area, Rich Text Area, or Encrypted Text fields!**
* ❌ **NOT supported in Web-to-Lead or Web-to-Case forms!**
* ❌ **CANNOT traverse downwards** to query child records (only child-to-parent upward).
* ❌ **CANNOT be used inside Roll-Up Summary fields!**

---

## 3. Roll-Up Summary Rules & Limitations

### The Golden Architecture Rules:
1. **Relationship Requirement:** Can ONLY be created on the Master object in a **Master-Detail relationship**.
2. **Summary Operations:** Supports `COUNT`, `SUM`, `MIN`, `MAX`.
3. **Multi-Currency Behavior:**
   * If an org has Multiple Currencies enabled, the roll-up summary calculates child values and displays the result in the **Master record's currency**!
4. **Detail Formulas in Roll-Ups:**
   * Allowed **UNLESS** the detail formula references a cross-object field OR contains dynamic date functions (`TODAY()`, `NOW()`).

### The 3 Standard Lookup Roll-Up Exceptions (Memorize!):
Roll-up summaries are typically forbidden on Lookup relationships, but Salesforce provides exactly 3 standard exceptions:
```
1. Account     -> Opportunities   (Roll up Total Opp Amount)
2. Opportunity -> Opportunity Products / Line Items (Roll up Total Line Items)
3. Campaign    -> Campaign Members (Roll up Total Members / Responses)
```
*(❌ Account -> Cases and Contact -> Cases DO NOT support roll-up summaries!).*

---

## 4. The Critical "Formula vs. Roll-Up" Exam Trap

### The Scenario (Appeared on Papers 18 & 19):
> *"A custom currency field called 'Cost' on Product needs to be displayed on Opportunity Product as 'Product Cost Fixed', and the total cost of all Opportunity Products must be summarized on the parent Opportunity. How do you meet this requirement?"*

* **The Trap Choice:** *"Configure 'Product Cost Fixed' as a formula field, and create a roll-up summary on Opportunity."* ❌ **DEAD WRONG!**
* **The Reason:** Because `Opportunity Product` looking up `Product.Cost` is a **cross-object formula**. Salesforce explicitly prohibits roll-up summary fields from calculating cross-object formula fields!
* **The Correct Solution:**
  1. Build a **Record-Triggered Flow** that fires when an Opportunity Product is created, copying the Product's `Cost` into a regular custom currency field (`Product Cost Fixed`).
  2. Create a standard **Roll-Up Summary field** on Opportunity summarizing that custom currency field!

---

## 5. Currency Formula Functions

| Function Signature | Purpose | Key Rule |
| :--- | :--- | :--- |
| **`CURRENCYRATE(CurrencyIsoCode)`** | Returns the conversion rate to the corporate currency for the given currency ISO code. | Requires `TEXT(CurrencyIsoCode)` because `CurrencyIsoCode` is a picklist. |
| ❌ `CURRENCY()`, `CURRENTRATE()`, `CONVERSIONRATE()` | **Fake Functions.** | Do not exist in Salesforce formula language! |

---

## 6. High-Frequency Exam Traps & Distractors

| Trap Topic | What Looks Correct (The Distractor) | Why It's Dead Wrong | The Official Exam Rule |
| :--- | :--- | :--- | :--- |
| **Rollup on Cross-Object**| Summarizing a cross-object formula field | Platform throws validation error on save. | Cross-object formulas **cannot be rolled up**. Use Flow to copy value first. |
| **Long Text in Formulas** | Concatenating Description or Case Notes | Long Text Area fields are not supported in formulas. | Formulas **cannot reference Long/Rich Text or Encrypted fields**. |
| **Web-to-Lead Formulas** | Placing formula field on web form | Formulas are read-only and calculated on server. | Formula fields **are not supported on Web-to-Lead/Case forms**. |
| **Standard Rollups** | Creating rollup on Account $\rightarrow$ Case | Case is a standard lookup, not Master-Detail. | Only Account $\rightarrow$ Opp, Opp $\rightarrow$ OppLineItem, Campaign $\rightarrow$ Member. |
| **Cross-Object Depth** | "Formulas can look up 15 levels away" | Governor limit is 10 levels. | Cross-object formulas can reference up to **10 relationships away**. |

---

## 7. Quick-Fire Summary Cards (NotebookLM Dialogue Ready)

* **Q: Can a Roll-Up Summary field summarize a child formula field that references a parent object?**  
  **A:** No. Cross-object formula fields cannot be used in Roll-Up Summary fields.

* **Q: What function returns the exchange rate of a currency ISO code relative to the corporate currency?**  
  **A:** `CURRENCYRATE(CurrencyIsoCode)`.

* **Q: Can a user see data through a cross-object formula field if they do not have read access to the parent record?**  
  **A:** Yes. Cross-object formula fields execute at the system level and display the calculated value regardless of parent record-level security.

* **Q: What are the only three standard lookup relationships that support native Roll-Up Summary fields?**  
  **A:** Account to Opportunity, Opportunity to Opportunity Product, and Campaign to Campaign Member.
