# JavaScript Code Standards & Best Practices

> **Presentation Summary:** *Good code isn't code that merely works — it's code that is easy to read, understand, test, maintain, and modify.*

---

## Table of Contents
1. [What are Code Standards?](#1-what-are-code-standards)
2. [Why Do We Need Code Standards?](#2-why-do-we-need-code-standards)
3. [Naming Conventions](#3-naming-conventions)
4. [Use Meaningful Names](#4-use-meaningful-names)
5. [Avoid Magic Numbers](#5-avoid-magic-numbers)
6. [Use `const` and `let` Correctly](#6-use-const-and-let-correctly)
7. [Keep Functions Small](#7-keep-functions-small)
8. [Avoid Duplicate Code — DRY](#8-avoid-duplicate-code--dry)
9. [Don't Overuse Comments](#9-dont-overuse-comments)
10. [Use Strict Equality](#10-use-strict-equality)
11. [Avoid Deeply Nested Code](#11-avoid-deeply-nested-code)
12. [Use Optional Chaining](#12-use-optional-chaining)
13. [Use Nullish Coalescing](#13-use-nullish-coalescing)
14. [Prefer Modern Array Methods](#14-prefer-modern-array-methods)
15. [Don't Mutate Data Unnecessarily](#15-dont-mutate-data-unnecessarily)
16. [Handle Errors Properly](#16-handle-errors-properly)
17. [Use `async`/`await` for Asynchronous Code](#17-use-asyncawait-for-asynchronous-code)
18. [Avoid Hardcoding Configuration](#18-avoid-hardcoding-configuration)
19. [Separate Concerns](#19-separate-concerns)
20. [Avoid Unnecessary Complexity](#20-avoid-unnecessary-complexity)
21. [Don't Create Abstractions Too Early](#21-dont-create-abstractions-too-early)
22. [Use ESLint](#22-use-eslint)
23. [Use Prettier for Formatting](#23-use-prettier-for-formatting)
24. [Code Review Best Practices](#24-code-review-best-practices)
25. [Don't Commit Secrets](#25-dont-commit-secrets)
26. [Write Testable Code](#26-write-testable-code)
27. [A Very Important Principle: KISS](#27-a-very-important-principle-kiss)
28. [YAGNI — You Aren't Gonna Need It](#28-yagni)
29. [DRY vs KISS — Important Difference](#29-dry-vs-kiss--important-difference)
30. [Validate Types Before Function Calls](#30-validate-types-before-function-calls)
31. [Always Handle Else in If Statements](#31-always-handle-else-in-if-statements)
32. [Overall Best-Practice Checklist](#32-overall-best-practice-checklist)
33. [How I'd Teach This Tomorrow (Interactive Teaching Guide)](#33-how-id-teach-this-tomorrow)

---

## 1. What are Code Standards?

Code standards are a set of rules and guidelines that developers follow to keep code:
- **Consistent**
- **Readable**
- **Maintainable**
- **Scalable**
- **Less error-prone**
- **Easier for teams to collaborate on**

### Simple Example

❌ **Without standards:**
```javascript
const username="Stuti"
function getuser(){
return username
}
```

✅ **With standards:**
```javascript
const username = "Stuti";

function getUser() {
  return username;
}
```
*The second code snippet is much easier to read and follows consistent formatting and naming.*

---

## 2. Why Do We Need Code Standards?

Imagine a project with 10 developers. If everyone writes code differently:
- `getUser()`
- `GetUser()`
- `get_user()`
- `getuser()`

It quickly becomes difficult to understand, search, and maintain. Code standards help the whole team follow the same style and conventions.

### Main Benefits

| Benefit | Meaning |
| :--- | :--- |
| **Readability** | Code is easier to understand for everyone on the team. |
| **Maintainability** | Code is easier to modify, update, and debug later. |
| **Consistency** | Everyone follows similar patterns across the codebase. |
| **Fewer Bugs** | Prevents common structural and scope mistakes. |
| **Collaboration** | Smoother onboarding for new developers and better reviews. |
| **Scalability** | Large projects remain manageable as the team and features grow. |

---

## 3. Naming Conventions

Naming is one of the most critical aspects of code standards.

### Variables → `camelCase`
```javascript
const firstName = "Stuti";
const userAge = 22;
const totalAmount = 500;
```

❌ **Avoid:**
```javascript
const first_name = "Stuti";
const userage = 22;
const TOTALAMOUNT = 500;
```

### Functions → `camelCase`
Prefer **verb-based names** because functions perform actions:
```javascript
function getUser() {}
function calculateTotal() {}
function validateEmail() {}
```

**Common Action Verbs:**
- `getUser()` / `createUser()` / `deleteUser()`
- `calculatePrice()`
- `validateForm()`

### Constants → `UPPER_SNAKE_CASE`
Constants that represent fixed values or global configuration are written in `UPPER_SNAKE_CASE`.
```javascript
const MAX_RETRY_COUNT = 3;
const API_BASE_URL = "https://api.example.com/v1";
```

> **Note:** Don't blindly uppercase every `const`. Standard scoped local variables declared with `const` should still use `camelCase`:
```javascript
const userName = "Stuti";
```

---

## 4. Use Meaningful Names

❌ **Bad:**
```javascript
const x = 500;
const d = 10;
const u = getUser();
```
*What is `x`? What is `d`? What does `u` hold?*

✅ **Better:**
```javascript
const totalAmount = 500;
const discount = 10;
const user = getUser();
```

> 💡 **Golden Rule:** The name should explain **what** the value represents without needing extra comments.

---

## 5. Avoid Magic Numbers

A **magic number** is a hardcoded number directly written in code without context or explanation.

❌ **Bad:**
```javascript
if (age >= 18) {
  // ...
}

// In complex code:
if (retryCount > 3) {
  // ...
}
```
*What is `3`? Why is `3` the limit?*

✅ **Good:**
```javascript
const MAX_RETRIES = 3;

if (retryCount > MAX_RETRIES) {
  // ...
}
```
*Now the business intention is clear, and changing the retry limit later requires editing only one spot.*

---

## 6. Use `const` and `let` Correctly

- Prefer **`const`** by default when values won't be reassigned.
  ```javascript
  const userName = "Stuti";
  ```
- Use **`let`** only when the variable value needs to change.
  ```javascript
  let count = 0;
  count++;
  ```
- **Avoid `var`** completely in modern JavaScript.
  ```javascript
  var name = "Stuti"; // ❌ Outdated, scope leaks
  const name = "Stuti"; // ✅ Block scoped
  ```

---

## 7. Keep Functions Small

A function should ideally have **one clear responsibility** (Single Responsibility Principle — SRP).

❌ **One giant function:**
```javascript
function processUser() {
  // validate user
  // calculate salary
  // save to database
  // send email
  // generate report
  // log activity
}
```
*This is difficult to test, debug, reuse, and maintain.*

✅ **Break it down into focused helpers:**
```javascript
function validateUser() {}
function calculateSalary() {}
function saveUser() {}
function sendEmail() {}
function generateReport() {}

// Orchestrator function
function processUser() {
  validateUser();
  calculateSalary();
  saveUser();
  sendEmail();
  generateReport();
}
```

---

## 8. Avoid Duplicate Code — DRY

**DRY = Don't Repeat Yourself**

❌ **Duplicate Logic:**
```javascript
const user1Total = price1 + tax;
const user2Total = price2 + tax;
const user3Total = price3 + tax;
```
*If tax calculation rules change, you have to update multiple lines.*

✅ **Encapsulated Function:**
```javascript
function calculateTotal(price, tax) {
  return price + tax;
}

const user1Total = calculateTotal(price1, tax);
const user2Total = calculateTotal(price2, tax);
const user3Total = calculateTotal(price3, tax);
```

---

## 9. Don't Overuse Comments

Comments should explain **why** something is done, not state the obvious **what**.

❌ **Obvious / Redundant Comment:**
```javascript
// Increment count by 1
count++;

// Call API
callApi();
```

✅ **Helpful Context Comment:**
```javascript
// Retry because the payment service can temporarily fail due to rate limits.
retryCount++;

// API occasionally returns 503 during deployment windows, so retry request once.
```

---

## 10. Use Strict Equality

Always prefer strict equality (`===` and `!==`) over loose equality (`==` and `!=`).

```javascript
5 === "5" // false (checks value AND type)
5 == "5"  // true (type coercion can cause hidden bugs)
```

✅ **Best Practice:**
```javascript
if (userId === 10) {
  // Explicit type safety
}
```

---

## 11. Avoid Deeply Nested Code

Deeply nested `if` statements create arrow code (hard to follow cognitive load).

❌ **Deeply Nested:**
```javascript
if (user) {
  if (user.isActive) {
    if (user.hasPermission) {
      if (user.role === "admin") {
        // do something
      }
    }
  }
}
```

✅ **Guard Clauses / Early Returns:**
```javascript
if (!user) return;
if (!user.isActive) return;
if (!user.hasPermission) return;
if (user.role !== "admin") return;

// do something
```

---

## 12. Use Optional Chaining

Use optional chaining (`?.`) to safely access deeply nested properties without long guard conditionals.

❌ **Verbose check:**
```javascript
if (user && user.address && user.address.city) {
  console.log(user.address.city);
}
```

✅ **Clean Optional Chaining:**
```javascript
console.log(user?.address?.city);
```

---

## 13. Use Nullish Coalescing

The nullish coalescing operator (`??`) provides fallback values specifically for `null` or `undefined`, unlike `||` which triggers on any falsy value (`0`, `""`, `false`).

❌ **Using `||` (Problematic for valid falsy values):**
```javascript
const name = user.name || "Guest"; 
// If user.name is "", it evaluates to "Guest"
```

✅ **Using `??`:**
```javascript
const name = user.name ?? "Guest";
// Falls back ONLY if user.name is null or undefined
```

---

## 14. Prefer Modern Array Methods

Avoid imperative `for` loops when declarative array methods express intent more clearly.

❌ **Imperative Loop:**
```javascript
const names = [];
for (const user of users) {
  names.push(user.name);
}
```

✅ **Declarative Method:**
```javascript
const names = users.map(user => user.name);
```

### Common Array Methods Cheat Sheet

| Method | Purpose | Example Use Case |
| :--- | :--- | :--- |
| `map()` | Transform data | Extract fields or convert objects |
| `filter()` | Select items | Filter users by active status |
| `find()` | Find one matching item | Look up user by ID |
| `some()` | Check if at least one matches | Check if any item is out of stock |
| `every()` | Check if all items match | Check if all form fields are valid |
| `reduce()` | Accumulate values | Calculate cart total price |
| `forEach()` | Perform side effect per item | Send notification to each active user |

---

## 15. Don't Mutate Data Unnecessarily

Direct mutation can cause unexpected side effects and state bugs (especially in React/Redux).

❌ **Direct Mutation:**
```javascript
user.name = "John";
```

✅ **Immutability (Creating a new object with spread operator):**
```javascript
const updatedUser = {
  ...user,
  name: "John"
};
```

---

## 16. Handle Errors Properly

Never swallow errors with an empty `catch` block.

❌ **Empty Catch (Swallowing Errors):**
```javascript
try {
  await fetchData();
} catch (error) {
}
```

✅ **Proper Error Handling:**
```javascript
try {
  await fetchData();
} catch (error) {
  console.error("Failed to fetch user:", error);
  // Optional: send to monitoring service (e.g. Sentry, Datadog)
}
```

---

## 17. Use `async`/`await` for Asynchronous Code

`async`/`await` flattens asynchronous logic, making it read sequentially like synchronous code.

❌ **Nested Promise Chains:**
```javascript
getUser()
  .then(user => getOrders(user.id))
  .then(orders => processOrders(orders))
  .catch(error => console.error(error));
```

✅ **Clean `async`/`await`:**
```javascript
try {
  const user = await getUser();
  const orders = await getOrders(user.id);
  processOrders(orders);
} catch (error) {
  console.error("Failed processing orders:", error);
}
```
*Note: Promises and `.then()` are not bad, but `async`/`await` provides higher readability.*

---

## 18. Avoid Hardcoding Configuration

❌ **Hardcoded URL:**
```javascript
fetch("https://production-api.example.com/users");
```

✅ **Environment Configuration:**
```javascript
const API_BASE_URL = process.env.API_BASE_URL;

fetch(`${API_BASE_URL}/users`);
```
*Allows seamless switching between Development, Staging, and Production environments.*

---

## 19. Separate Concerns

Organize files by functional layer so components focus only on their primary duty.

### Recommended Folder Structure:
```text
src/
├── components/   # UI Layout & View components
├── services/     # API calls & Network logic
├── utils/        # Helper functions & formatting
├── hooks/        # React / Custom State logic
├── api/          # Endpoints configuration
└── constants/    # Fixed variables & Configs
```

**Example:**
```javascript
// services/userService.js
export async function getUser(userId) {
  const response = await fetch(`/api/users/${userId}`);
  return response.json();
}

// In component:
const user = await getUser(userId);
```

---

## 20. Avoid Unnecessary Complexity

Readable code is almost always preferable to clever or overly condensed code.

❌ **Over-clever One-liner:**
```javascript
const result = users?.filter(Boolean)?.map(u => u?.name)?.filter(Boolean);
```

✅ **Clear & Legible:**
```javascript
const validUsers = users.filter(Boolean);
const names = validUsers.map(user => user.name);
```

---

## 21. Don't Create Abstractions Too Early

Avoid premature abstraction ("Abstract Factory Pattern" for single-use cases).

❌ **Premature Abstraction:**
```javascript
// Adding layers before understanding full requirements:
BaseService
AbstractUserService
GenericDataService
ServiceFactory
```

✅ **Rule:** Build what is currently needed first. Create abstractions only when repeating a proven pattern 3+ times.

---

## 22. Use ESLint

ESLint analyzes JavaScript statically to enforce code quality rules and catch syntax/logic violations automatically.

### Common Rule Configurations:
- `no-unused-vars`: Warns on unused variables.
- `no-console`: Prevents leaving debug logs in production.
- `eqeqeq`: Enforces `===` over `==`.
- `semi`: Enforces consistent semicolons.
- `quotes`: Enforces single or double quotes consistency.

---

## 23. Use Prettier for Formatting

While ESLint handles code quality, **Prettier** handles code formatting automatically on save or pre-commit.

### Comparison Table

| Tool | Primary Purpose | Examples |
| :--- | :--- | :--- |
| **ESLint** | Code quality & problem detection | Catching unused variables, scope leaks, loose equality |
| **Prettier** | Code formatting & visual layout | Line width, indentation, comma placement, quote styles |

> 🤝 **Best Practice:** Use both together (`eslint-config-prettier`).

---

## 24. Code Review Best Practices

Code reviews are a collaborative process to elevate quality and share knowledge across the team.

### Code Review Checklist:
- 📖 **Readability:** Can I understand this easily? Are variable/function names clear?
- 🧠 **Logic:** Is the logic correct? Are edge cases handled?
- 🛠️ **Maintainability:** Is there duplicate code? Is any function oversized?
- 🔒 **Security:** Is user input validated/sanitized? Are secrets exposed?
- ⚡ **Performance:** Are there redundant API calls or heavy operations in render loops?
- 🧪 **Testing:** Is critical business logic covered by automated tests?

---

## 25. Don't Commit Secrets

❌ **Never commit sensitive credentials into source control:**
```javascript
const API_KEY = "abc123secret";
const DB_PASSWORD = "mypassword";
```

✅ **Use Environment Variables & `.gitignore`:**
```javascript
const API_KEY = process.env.API_KEY;
```
*Ensure `.env` files containing actual production keys are added to `.gitignore`.*

---

## 26. Write Testable Code

Keep functions small, pure, and free of unexpected side effects so they can be unit-tested effortlessly.

❌ **Hard to test:**
```javascript
function processEverything() {
  // 200 lines doing calculations, DOM manipulation, network calls
}
```

✅ **Easy to test isolated units:**
```javascript
function calculateTax(amount) { return amount * 0.18; }
function calculateDiscount(amount) { return amount > 1000 ? 50 : 0; }
function calculateFinalAmount(amount) { 
  return amount + calculateTax(amount) - calculateDiscount(amount); 
}
```

---

## 27. A Very Important Principle: KISS

**KISS = Keep It Simple, Stupid**

```javascript
// ✅ Simple & Direct:
function isAdult(age) {
  return age >= 18;
}
```
*The goal isn't to write the shortest or cleverest code — it's to write simple code that is easy to understand and maintain.*

---

## 28. YAGNI

**YAGNI = You Aren't Gonna Need It**

Don't build functionality or complex fallback configurations just because you *might* need them in the future. Build what is required now.

---

## 29. DRY vs KISS — Important Difference

| Principle | Focus | Catch / Warning |
| :--- | :--- | :--- |
| **DRY** (Don't Repeat Yourself) | Eliminating duplicate logic | Don't force DRY on code that looks similar but represents different business domain rules. |
| **KISS** (Keep It Simple) | Avoiding unnecessary complexity | Don't create premature abstractions just to save a few lines of repetition. |

> ⚠️ *Premature abstraction for the sake of DRY can lead to overly complex code that violates KISS.*

---

## 30. Validate Types Before Function Calls

Always check the types of variables before passing them as arguments to functions. This prevents unexpected runtime errors and makes the code more robust.

❌ **Bad:**
```javascript
function processData(data) {
  return data.map(item => item.value);
}

processData(null); // Uncaught TypeError
```

✅ **Good:**
```javascript
function processData(data) {
  if (!Array.isArray(data)) {
    return [];
  }
  return data.map(item => item.value);
}

// Or validate before passing:
if (Array.isArray(myVariable)) {
  processData(myVariable);
}
```

---

## 31. Always Handle Else in If Statements

Always consider and handle the `else` case of an `if` statement to ensure no scenario is left unaccounted for.

❌ **Bad:**
```javascript
if (user.isActive) {
  grantAccess();
}
// What happens if user is not active? We are left in the dark.
```

✅ **Good:**
```javascript
if (user.isActive) {
  grantAccess();
} else {
  denyAccess();
}
```
*Even if the `else` case just logs a warning or explicitly does nothing, making it explicit clarifies the developer's intent.*

---

## 32. Overall Best-Practice Checklist

Use this checklist during your code reviews:

- [ ] Are variable and function names meaningful and descriptive?
- [ ] Am I using `const`/`let` instead of `var`?
- [ ] Am I using strict equality (`===`) instead of loose equality (`==`)?
- [ ] Does each function have single responsibility (SRP)?
- [ ] Have I avoided duplicate logic (DRY)?
- [ ] Is the code readable without needing excessive explanatory comments?
- [ ] Have I avoided deep nesting using guard clauses / early returns?
- [ ] Are errors properly caught and logged/handled?
- [ ] Are secrets and environment configurations separated from code?
- [ ] Is critical business logic broken down into testable functions?
- [ ] Have I avoided unnecessary complexity (KISS & YAGNI)?
- [ ] Does ESLint pass without warnings or errors?
- [ ] Is formatting consistent with Prettier?
- [ ] Are important edge cases (null/undefined/empty arrays) handled?
- [ ] Are types checked before variables are passed to functions?
- [ ] Is the `else` case handled for every `if` statement?

---

## 33. How I'd Teach This Tomorrow

### Interactive Session Plan: *Show Bad Code → Spot the Flaws → Step-by-Step Refactor*

Instead of reading 30 rules, open with this live code snippet and ask your audience:  
👉 **"How many problems can you find in this code?"**

#### ❌ Bad Code (Starting Point):
```javascript
var x = 10;

function calc(a, b) {
  if (a) {
    if (b) {
      if (a == b) {
        console.log("same");
      }
    }
  }
}
```

#### 🔍 Identified Flaws:
1. `var x` — Scope pollution (should be `const`).
2. Magic number `10` — Unclear meaning.
3. Cryptic function name `calc` & parameter names `a`, `b`.
4. Deep nesting (`if` inside `if` inside `if`).
5. Loose equality `==` instead of `===`.
6. Unused return value / console side effect.

#### ✅ Improved Code (Final Refactored Version):
```javascript
const DEFAULT_VALUE = 10;

function areValuesEqual(firstValue, secondValue) {
  if (!firstValue || !secondValue) {
    return false;
  }

  return firstValue === secondValue;
}
```

### Concepts Covered Naturally in This Single Exercise:
1. `var` → `const`
2. Meaningful names
3. Naming conventions
4. Magic numbers
5. Strict equality (`===`)
6. Guard clauses / early returns
7. Small, pure functions
8. Code simplicity & testability

---

### 🎯 One-Line Summary for Presentation Slide
> **"Good code isn't code that merely works — it's code that is easy to read, understand, test, maintain, and modify."**
