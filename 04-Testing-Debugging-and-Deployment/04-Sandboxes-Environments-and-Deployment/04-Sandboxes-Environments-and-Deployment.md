# 04 — Sandboxes, Environments & Deployment Mechanics

> **NotebookLM Study Guide:** Comprehensive breakdown of sandbox tiers, scratch orgs, Change Sets vs. Salesforce CLI, code coverage rules during deployment, and high-frequency exam traps for Salesforce Platform Developer I (PD1).

---

## 1. Core Architecture & Mental Model

### Environment Strategy in Salesforce:
Salesforce enforces separation of development, testing, staging, and production environments using **Sandboxes** and **Scratch Orgs**.

---

## 2. Sandbox Tiers & Refresh Cycles (Memorize This Table!)

| Sandbox Type | Storage (Data / File) | Contents Copied | Refresh Interval | Primary Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **Developer Sandbox** | 200 MB / 200 MB | Metadata ONLY | **1 Day** (Daily) | Individual coding and unit testing. |
| **Developer Pro Sandbox**| 1 GB / 1 GB | Metadata ONLY | **1 Day** (Daily) | Integration testing and QA with larger data volumes. |
| **Partial Copy Sandbox** | 5 GB / 5 GB | Metadata + **Sample Data** (uses Sandbox Template) | **5 Days** | UAT, batch testing, performance dry runs. |
| **Full Sandbox** | Same as Production | Metadata + **ALL Production Data** (can use template) | **29 Days** | Final pre-production staging, load testing, regression. |

### Sandbox Email Deliverability Rule:
When a sandbox is created or refreshed:
* All user email addresses have **`.invalid`** appended to the end (`john@acme.com` $\rightarrow$ `john@acme.com.invalid`).
* Email deliverability is defaulted to **"System Email Only"** or **"No Access"** to prevent accidental test emails to real customers.

### Sandbox Cloning:
* You can clone an existing sandbox instead of creating from production to preserve work-in-progress customization and test data across developer sandboxes. Faster than deploying via change sets!

---

## 3. Scratch Orgs (Salesforce DX)

| Property | Scratch Org Architecture | Exam Trap |
| :--- | :--- | :--- |
| **Source** | Created from a **Dev Hub** org (must be enabled in Production). | Cannot create without Dev Hub. |
| **Configuration** | Configured via a **scratch org definition file** (`project-scratch-def.json`). | Can enable features and settings declaratively. |
| **Lifespan** | Ephemeral: **1 to 30 days** (defaults to 7 days; expires automatically). | Temporary environments. |
| **Data** | Starts completely empty (source-driven development). | No production data copied. |
| **Sharing** | **Individual only.** Each developer has their own scratch org. | ❌ NOT shared across a developer team! |

> ⚠️ **Package Creation Trap:** Managed Packages **CANNOT be created in Sandboxes or Scratch Orgs**! Managed packages can ONLY be created in **Developer Edition** or **Partner Developer Edition** orgs.

---

## 4. Deployment Tools: Change Sets vs. Salesforce CLI

| Feature | Change Sets | Salesforce CLI / Metadata API |
| :--- | :--- | :--- |
| **Target Orgs** | **Related orgs ONLY** (connected via deployment pipeline, e.g. Sandbox to Prod). | **ANY org** (unrelated orgs, scratch orgs, dev hubs). |
| **Interface** | Point-and-click Web UI. | Command-line interface (`sf project deploy start`). |
| **Automation** | ❌ Cannot be scripted or scheduled. | ✅ Fully scriptable in CI/CD pipelines (GitHub Actions, Jenkins). |
| **Destructive Changes**| ❌ **Cannot delete metadata** from target org! | ✅ **Can delete metadata** via destructive changes XML! |
| **Validation** | Supports validation before deployment. | Supports validation via `--dry-run`. |
| **Quick Deploy** | Available if validation succeeded within last 4 days. | Available via `--quick-deploy`. |

---

## 5. Deployment Test Levels & Code Coverage Enforcement

When deploying Apex to production via Change Sets or Metadata API:

| Test Level | Behavior | When Used |
| :--- | :--- | :--- |
| **NoTestRun** | No tests run. | Allowed in **Sandboxes only** (prohibited in Production!). |
| **RunSpecifiedTests**| Runs ONLY the test classes specified in the deployment package. | Fast targeted deployment to Production. |
| **RunLocalTests** | Runs all tests in the org **except** managed package tests. | Default option for Production Change Sets. |
| **RunAllTestsInOrg** | Runs all tests in the org, **including managed package tests**. | Comprehensive pre-release validation. |

### "Run Specified Tests" Rules (High-Frequency Exam Trap!):
1. **Test Classes ONLY:** You can only pass test **classes**, NOT individual test methods!
2. **Individual 75% Coverage Requirement:**
   - When choosing `RunSpecifiedTests`, **EVERY single Apex class and trigger included in the deployment package must achieve $\ge 75\%$ code coverage independently!**
   - This is strictly enforced regardless of the org-wide total coverage.

---

## 6. High-Frequency Exam Traps & Distractors

| Trap Topic | What Looks Correct (The Distractor) | Why It's Dead Wrong | The Official Exam Rule |
| :--- | :--- | :--- | :--- |
| **Deleting Metadata** | "Use change sets to delete an obsolete custom field" | Change sets cannot perform destructive changes. | Must use **Salesforce CLI / Metadata API destructiveChanges.xml** or delete manually in Setup. |
| **Run Specified Tests** | Specifying individual test methods: `MyTestClass.testMethodOne` | Deployment APIs do not accept method-level targets. | Can **only pass test class names** (`MyTestClass`). |
| **Scratch Org Sharing** | "Share scratch org across the dev team" | Scratch orgs are designed for single-developer branching. | Each developer spins up their **own scratch org**. |
| **Package Creation** | "Create managed package in Full Sandbox" | Sandboxes cannot create managed package namespaces. | Managed packages must be built in **Developer Edition** orgs. |
| **Partial Copy Refresh** | Refresh interval of 1 day | Partial copy refreshes every 5 days. | Developer = 1 day, Partial = 5 days, Full = 29 days. |

---

## 7. Quick-Fire Summary Cards (NotebookLM Dialogue Ready)

* **Q: What is the refresh interval and data limit of a Partial Copy Sandbox?**  
  **A:** Refresh interval is 5 days; storage limit is 5 GB of data using a Sandbox Template.

* **Q: Which deployment tool can delete metadata components from a production organization?**  
  **A:** Salesforce CLI / Metadata API using `destructiveChanges.xml` (Change Sets cannot delete metadata).

* **Q: What code coverage rule is enforced when deploying using the `RunSpecifiedTests` option?**  
  **A:** Every individual Apex class and trigger in the deployment package must independently achieve at least 75% coverage.

* **Q: In which organization type must a developer build and release a Managed Package?**  
  **A:** A Developer Edition or Partner Developer Edition organization (never in a Sandbox or Scratch Org).
