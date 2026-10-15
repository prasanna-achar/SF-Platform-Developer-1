# 01 — Apex Core Syntax, OOP Fundamentals & Method Declarations

> **NotebookLM Study Guide:** Comprehensive breakdown of Apex primitive types, collections, method signature requirements, static vs instance invocation, safe navigation, and ApexDoc rules for Salesforce Platform Developer I (PD1).

---

## 1. Core Architecture & Mental Model

### What is Apex?
Apex is a strongly typed, object-oriented programming language designed specifically for the Force.com multi-tenant platform. It compiles to Java-like bytecode executed by the Salesforce server runtime engine.

### Multi-Tenant Execution Characteristics:
* **Tight Database Integration:** Direct inline SOQL queries and DML operations.
* **Governed by Strict Limits:** CPU time limits (10s sync / 60s async), memory limits (heap 6MB sync / 12MB async).
* **Case-Insensitive:** Variable names, method names, SOQL statements, and class names are case-insensitive.

---

## 2. Primitive Data Types & Collections

### Valid Primitive Types:
* **`Integer`:** 32-bit whole number without decimal point (-2,147,483,648 to 2,147,483,647).
* **`Long`:** 64-bit whole number.
* **`Double` / `Decimal`:** Numbers with decimal points. `Decimal` handles exact currency/financial rounding.
* **`String`:** Any sequence of characters in single quotes (`'Hello World'`).
* **`Boolean`:** `true`, `false`, or `null`.
* **`ID`:** Any valid 18-character (or 15-character) Salesforce record identifier.
* **`Blob`:** Binary data collection stored as a single object (e.g. file attachments, crypto keys).
* **`Date`, `Time`, `DateTime`:** Standard date and time primitives.
* **`Enum`:** An abstract data type consisting of a predefined set of named values.

> ❌ **CRITICAL EXAM TRAP:** A `'Number'`, `'Text'`, or `'Currency'` primitive data type **DOES NOT EXIST** in Apex! (Those are declarative UI field types, not Apex programming data types).

### Collections:
* **`List<T>`:** Ordered collection of elements. Allows duplicates. Accessed by 0-based index.
* **`Set<T>`:** Unordered collection of **unique** elements. No duplicates. Ideal for gathering IDs for SOQL queries.
* **`Map<Key, Value>`:** Key-value pairs where keys are unique. Unlocks $O(1)$ lookups. Essential for trigger bulkification.

---

## 3. Apex Method Definition Rules

```apex
[public | protected | global | private] [static | virtual | abstract] ReturnType methodName([ParamType paramName, ...]) {
    // Body
}
```

### What is ALWAYS Required?
* **Return Type is ALWAYS REQUIRED.** If the method returns nothing, **`void`** must be explicitly declared as the return type!
* **Access Modifiers:** Optional for general methods (defaults to `private`), but required for `abstract` and `virtual` methods in modern API versions.
* **Parameters:** Optional (methods can take zero or multiple parameters).

### Static vs. Instance Method Invocation:
* **Static Members:** Associated with the **class itself**. Accessed directly: `ClassName.staticMethod()` or `ClassName.staticVariable`. Cannot be called on an instance!
* **Instance Members:** Associated with a specific **object instance**. Must instantiate first: `new ClassName().instanceMethod()`.

---

## 4. Safe Navigation Operator (`?.`)

Replaces tedious and verbose null-checking ladders:
```apex
// Old verbose syntax:
String street = null;
if (con != null && con.Account != null && con.Account.BillingAddress != null) {
    street = con.Account.BillingAddress.getStreet();
}

// Modern Safe Navigation Operator:
String street = con?.Account?.BillingAddress?.getStreet();
// If ANY reference on the left of '?.' evaluates to null, it returns null without throwing a NullPointerException!
```

---

## 5. ApexDoc Commenting Standards

ApexDoc provides automated API documentation generation:
* **Opening Delimiter:** MUST be **`/**`** (slash followed by **DOUBLE asterisk**).
  * ❌ **Exam Trap:** `/*` is a standard multi-line comment, **NOT valid ApexDoc**!
* **Closing Delimiter:** `*/` (asterisk + slash).
* **`@param` Tags:** Must match the **exact sequential order** of parameters defined in the method signature.
* **Inline Tags:** Tags like `{@code}`, `{@link}`, and `{@hidden}` **MUST be wrapped in curly braces**!

```apex
/**
 * Processes shipment items and calculates customs duties.
 * @param items List of package items to inspect
 * @param shippingCountry Destination country code
 * @return Total duty calculated
 * @see {@link TaxCalculationService}
 */
public static Decimal calculateDuty(List<Item> items, String shippingCountry) {
    // ...
}
```

---

## 6. High-Frequency Exam Traps & Distractors

| Trap Topic | What Looks Correct (The Distractor) | Why It's Dead Wrong | The Official Exam Rule |
| :--- | :--- | :--- | :--- |
| **Data Types** | Declaring variable as `Currency` or `Number` | Fake Apex data types. | Use **`Decimal`** for currency and **`Integer`** or **`Double`** for numbers. |
| **ApexDoc Delimiter** | Opening with `/*` | Standard comment, not ApexDoc. | Opening delimiter MUST be **`/**`**. |
| **Required Elements** | "Access modifier is always required" | Access modifier defaults to private. | **Return type is ALWAYS required** (even `void`). |
| **Static Invocation** | `new Class().staticMethod()` | Throws "Static method cannot be referenced from non-static context". | Must call directly on class: **`Class.staticMethod()`**. |
| **Null Navigation** | `con.Account.?Name` | Syntax error (period before question mark). | Syntax is **`?.`** (question mark THEN period). |

---

## 7. Quick-Fire Summary Cards (NotebookLM Dialogue Ready)

* **Q: Which component is strictly mandatory when declaring any method in Apex?**  
  **A:** The return type (e.g., `void`, `Integer`, `String`).

* **Q: Do the data types 'Number' and 'Currency' exist in Apex code?**  
  **A:** No. Apex uses `Integer`, `Long`, `Double`, and `Decimal`.

* **Q: What is the exact opening delimiter required for ApexDoc documentation?**  
  **A:** A forward slash followed by two asterisks (`/**`).

* **Q: How does the safe navigation operator (`?.`) handle a null reference on its left side?**  
  **A:** It short-circuits and immediately returns `null` instead of throwing a `NullPointerException`.
