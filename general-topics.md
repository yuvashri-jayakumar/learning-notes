**🧱 1. What Are Attributes?**

Attributes are part of the HTML markup — they are written inside the HTML tags to set initial values or behavior of an element.

Example:
<input type="text" value="Hello" id="username" disabled>


Here:

type, value, id, and disabled are attributes.

They describe the initial state of the element when the browser parses the HTML.

Attributes always:

Exist in the HTML source code.

Are strings (even if they represent numbers, booleans, etc.).

Are used by the browser to initialize the DOM element.

**⚙️ 2. What Are Properties?**

Properties are part of the DOM (Document Object Model) — they are the JavaScript object representation of the element and can be accessed or modified dynamically.

When HTML is loaded, the browser creates a DOM node (object) for each element, and many of its properties are automatically initialized from the HTML attributes.

Example:
const input = document.querySelector('input');
console.log(input.value);      // "Hello"
console.log(input.disabled);   // true


Here, .value and .disabled are properties of the JavaScript HTMLInputElement object.

Properties:

Represent the current state of the element in memory.

Can be read or changed using JavaScript.

Are not always strings (can be boolean, number, object, etc.).

Change dynamically when the user interacts with the page.


```
<input id="name" value="John">

```
```
const input = document.getElementById('name');
console.log(input.getAttribute('value')); // "John"
console.log(input.value);                 // "John"

input.value = "Mike";

console.log(input.getAttribute('value')); // Still "John"
console.log(input.value);                 // "Mike"
```

**🧩 4. When They Stay in Sync**

Some attributes and properties are synchronized both ways, such as:

**id

class

title**

Example:
```
const div = document.querySelector('div');

div.id = "box";
console.log(div.getAttribute('id')); // "box"

div.setAttribute('id', 'container');
console.log(div.id); // "container"
```

So, for certain attributes, updating one reflects on the other.

| Feature                       | Attribute                           | Property                        |
| ----------------------------- | ----------------------------------- | ------------------------------- |
| Defined in                    | HTML                                | DOM                             |
| Access via                    | `getAttribute()` / `setAttribute()` | Directly via `element.property` |
| Data type                     | String                              | Any type                        |
| Reflects                      | Initial HTML state                  | Current DOM state               |
| Example                       | `<input value="abc">`               | `input.value = "xyz"`           |
| Changes shown in HTML source? | Yes                                 | No                              |
| Changes reflect in real-time? | Only if updated manually            | Yes                             |

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/3b50259b-0c50-4a40-888b-0ac66ff81411" />

=====================================================================================================================================

**🧠 What is zone.js?**

zone.js is a library that Angular uses to **track and manage asynchronous operations** — like:

setTimeout()

Promises

HTTP requests

User events (click, input, etc.)

It creates something called a Zone — a kind of execution context that keeps track of when and where async operations start and finish.

**💡 Why Angular needs it**

Angular’s change detection system (the thing that updates your UI when data changes) needs to know when to run.
But async tasks happen outside of Angular’s normal code flow — for example:

setTimeout(() => {
  this.count++;  // <-- when should Angular update the view?
}, 1000);


Without Zone.js, Angular wouldn’t automatically know when setTimeout finishes to refresh the template.

zone.js monkey-patches all async APIs so it can detect when they start and complete — then it tells Angular:

“Hey, something async just finished! Check if the data changed and update the UI.”

**⚙️ How it works (simplified flow)**

You perform an async task like an HTTP request.

zone.js intercepts it and marks: “I’m watching this async call.”

When the task finishes, zone.js notifies Angular.

Angular’s change detection runs → updates the DOM accordingly.

<img width="931" height="484" alt="image" src="https://github.com/user-attachments/assets/a3069cfb-161b-47b2-8591-8519dd971d90" />


```
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <h2>Count: {{ count }}</h2>
    <button (click)="increaseLater()">Increase After 2s</button>
  `
})
export class AppComponent {
  count = 0;

  increaseLater() {
    setTimeout(() => {
      this.count++; // ✅ Angular auto-detects this change
    }, 2000);
  }
}

```

**🔄 Without Zone.js (manual control)**

In Angular 17+ or standalone setups, you can opt out of Zone.js using:

bootstrapApplication(AppComponent, {
  ngZone: 'noop'
});


Then you must manually trigger change detection (e.g., using ChangeDetectorRef.detectChanges() or signals in Angular 16+).

```
import { Component, ChangeDetectorRef } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <h2>Count: {{ count }}</h2>
    <button (click)="increaseLater()">Increase After 2s</button>
  `
})
export class AppComponent {
  count = 0;

  constructor(private cdr: ChangeDetectorRef) {}

  increaseLater() {
    setTimeout(() => {
      this.count++;
      **this.cdr.detectChanges();** // 🧭 manually tell Angular to refresh
    }, 2000);
  }
}
```
**🧩 3️⃣ Angular without zone.js — using Signals**

🧠 What are Signals?

Signals are reactive variables that Angular watches automatically.
When a signal’s value changes, Angular re-renders only the parts of the template that depend on it — no zones, no manual change detection.

```
import { Component, signal } from '@angular/core';
import { bootstrapApplication } from '@angular/platform-browser';

@Component({
  selector: 'app-root',
  template: `
    <h2>Count: {{ count() }}</h2>
    <button (click)="increaseLater()">Increase After 2s</button>
  `
})
export class AppComponent {
  // Declare a reactive signal
  count = signal(0);

  increaseLater() {
    setTimeout(() => {
      this.count.update(c => c + 1); // 🚀 updates signal
    }, 2000);
  }
}

// Bootstrap without zone.js
bootstrapApplication(AppComponent, {
  ngZone: 'noop'
});
```

<img width="980" height="409" alt="image" src="https://github.com/user-attachments/assets/ce88ca6d-ea97-4194-b53e-1ec407badc66" />

<img width="1055" height="491" alt="image" src="https://github.com/user-attachments/assets/c886b7b4-d813-4899-bbbb-5a41fa66cca4" />


| Approach               | Auto Change Detection | Needs Zone.js | Best For                     |
| ---------------------- | --------------------- | ------------- | ---------------------------- |
| Default Angular        | ✅ Yes                 | ✅ Yes         | Simpler apps                 |
| Manual detectChanges() | ⚙️ Manual             | ❌ No          | Advanced/manual control      |
| Signals                | ✅ Yes                 | ❌ No          | Modern reactive Angular apps |

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/01ae7ac4-dc2c-443c-96f7-035c6a09fc29" />

============================================================================================================================================
**@Input vs signal input**

**🧩 Overview**


| Feature                           | `@Input()`                | Signal Inputs (`input()`)      |
| --------------------------------- | ------------------------- | ------------------------------ |
| Introduced in                     | Angular (from start)      | Angular 17                     |
| Reactive?                         | ❌ No (manual tracking)    | ✅ Yes (reactive signal)        |
| Type                              | Property decorator        | Function that returns a signal |
| Updates trigger change detection? | ✅ Yes                     | ✅ Yes                          |
| Works with Signals API?           | ❌ No                      | ✅ Fully integrated             |
| Syntax                            | `@Input() prop!: string;` | `prop = input<string>();`      |


**🧠 1️⃣ Traditional @Input()**

@Input() is a decorator that tells Angular a property should receive data from a parent component to a child component.

🔹 Child Component — child.component.ts
```
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<h3>Welcome, {{ name }}</h3>`
})
export class ChildComponent {
  @Input() name!: string;  // <-- decorator used here
}
```
🔹 Parent Component — parent.component.html
```
<app-child [name]="userName"></app-child>
```
🔹 Parent Component — parent.component.ts
```
export class ParentComponent {
  userName = 'Yuva';
}
```

**🧠 2️⃣ Signal-based Inputs (input())**

Angular 17+ introduced signal inputs that make component inputs reactive and signal-compatible.

You import input() from @angular/core.

Example:
```
import { Component, input } from '@angular/core';

@Component({
  selector: 'user-card',
  template: `<p>User: {{ name() }}</p>`
})
export class UserCardComponent {
  name = input<string>(); // 👈 a signal input
}

Usage (same as before):
<user-card [name]="username"></user-card>
```

✅ Now, inside your component:

name is a signal.

You can react to its changes directly, without lifecycle hooks.

| Use Case                            | Use `@Input()` | Use `input()` (signal input) |
| ----------------------------------- | -------------- | ---------------------------- |
| Legacy / existing apps              | ✅ Yes          | ❌ Optional                   |
| Using Signals for reactivity        | ❌ No           | ✅ Yes                        |
| Need computed or effect-based logic | ❌ Harder       | ✅ Native support             |
| Want fine-grained updates           | ❌ No           | ✅ Yes                        |


| Concept                | `@Input()`                   | `input()`             |
| ---------------------- | ---------------------------- | --------------------- |
| Reactivity             | Imperative (lifecycle hooks) | Declarative (signals) |
| Detect changes         | `ngOnChanges()`              | Automatic via signals |
| Use in computed/effect | ❌ Not supported              | ✅ Yes                 |



**push vs unshift**

Push - Add elements to array at end
Unshift - Add array elements to start

**Content Projection**

**<ng-content/>** to wrap another component html in shared component. 
This will not replace the child html component.ninstead, it will use the child component just like that. 
