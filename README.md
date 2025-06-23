# Declarative-apex

# Declarative Apex in Lightning Web Components (LWC)

## 🔥 What is Declarative Apex?

**Declarative Apex** in LWC refers to calling Apex methods using the `@wire` decorator or using built-in base components like `lightning-record-form`. It allows you to **automatically retrieve or bind data** to your components **without writing complex logic** in JavaScript.

In simpler terms: You just **declare what data you want**, and the system fetches it for you.

---

## 💡 Why Use Declarative Apex?

| Benefit             | Description                            |
| ------------------- | -------------------------------------- |
| 🚀 Easy to use      | Just specify what data you need.       |
| 📒 Less Code        | No `.then()` or `.catch()` needed.     |
| ✨ Automatic Refresh | Reacts when data or parameters change. |
| 🔐 Secure           | Respects field-level security (FLS).   |

---

## 🚀 Common Ways to Use Declarative Apex

1. Using `@wire` with `getRecord`, `getObjectInfo`, or custom Apex.
2. Using base components like `lightning-record-form`.

---

## 📅 Example 1: Using `@wire` with `getRecord`

### 📁 JavaScript: `accountWire.js`

```js
import { LightningElement, wire } from 'lwc';
import { getRecord } from 'lightning/uiRecordApi';

const FIELDS = ['Account.Name', 'Account.Industry'];

export default class AccountWire extends LightningElement {
    recordId = '001xx000003DGXzAAO';

    @wire(getRecord, { recordId: '$recordId', fields: FIELDS })
    account;
}
```

### 📁 HTML: `accountWire.html`

```html
<template>
    <lightning-card title="Account Info (Declarative)">
        <template if:true={account.data}>
            <p>Name: {account.data.fields.Name.value}</p>
            <p>Industry: {account.data.fields.Industry.value}</p>
        </template>
        <template if:true={account.error}>
            <p>Error loading account</p>
        </template>
    </lightning-card>
</template>
```

### 🔍 Explanation:

* `@wire` tells Salesforce to fetch data reactively.
* `account.data.fields.Name.value` gives the actual value.
* Automatically updates if `recordId` changes.

---

## 📅 Example 2: Using `lightning-record-form`

```html
<template>
    <lightning-record-form
        record-id="001xx000003DGXzAAO"
        object-api-name="Account"
        fields="Name, Industry"
        mode="view">
    </lightning-record-form>
</template>
```

### 🔍 Explanation:

* You don’t need any JavaScript at all.
* It handles layout, data fetching, FLS, and error handling.

---

## 🧵 When to Use Declarative Apex?

| Use Case                  | Use Declarative? |
| ------------------------- | ---------------- |
| Display simple fields     | ✅ Yes            |
| Auto-load record data     | ✅ Yes            |
| Form with built-in layout | ✅ Yes            |
| Fetch data after a click  | ❌ Use Imperative |
| Perform complex logic     | ❌ Use Imperative |

---

## 🔧 Project Structure

```
force-app
└── main
    └── default
        ├── lwc
        │   └── accountWire
        │       ├── accountWire.js
        │       ├── accountWire.html
        │       └── accountWire.js-meta.xml
```

---

## 🚀 Bonus Tips

* Use `@wire` with built-in LDS functions when possible (like `getRecord`).
* Avoid using `@wire` with Apex methods that perform write/update/delete.
* Use `@wire` with parameters to make the component reactive.

```js
@wire(myMethod, { param1: '$value1' })
data;
```

* Use `account.data`, `account.error` in your HTML to handle both outcomes.

---

## 👍 Conclusion

Declarative Apex is **beginner-friendly**, **secure**, and **efficient**. If your goal is to display data, especially record fields, go declarative first. It saves you time and avoids boilerplate code.

Use Declarative when:

* You want **auto-fetched data**.
* You’re using **standard objects**.
* You want to **respect FLS** and make your code **cleaner**.

For any complex logic or UI control, fall back to Imperative Apex.
