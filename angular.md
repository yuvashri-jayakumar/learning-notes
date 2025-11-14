**ANGULAR**

1. JS Framework to build application (interactive, modern web UI)
   
2. contains a lot of tools - build,test and depoy (CLI, debugging tools, plugin) 
 
**Why Angular**

1. declarative code = uses states and no need to find DOM object and udpate. Angular take care of it

2. OOPS - classes, 

3. Use Typescript

4. Components to separate concerns

**Main Blocks**

1. components
2. Services
3. Modules

***COMPONENT***

Every component has a few main parts:

A @Componentdecorator that contains some configuration used by Angular.

An HTML template that controls what renders into the DOM.

A CSS selector that defines how the component is used in HTML.

A TypeScript class with behaviors, such as handling user input or making requests to a server.


**Code script**
```angular
// user-profile.ts
@Component({
  selector: 'user-profile',
  template: `
    <h1>User profile</h1>
    <p>This is the user profile page</p>
  `,
  styles: `h1 { font-size: 3em; } `,
})
export class UserProfile { /* Your component code goes here */ }
```

```angular
// user-profile.ts
@Component({
  selector: 'user-profile',
  templateUrl: 'user-profile.html',
  styleUrl: 'user-profile.css',
})
export class UserProfile {
  // Component behavior is defined in here
}
```

✅ Simple Summary

Selector → how the component appears in HTML  <user-profile></user-profile>

Class Name → the TypeScript class behind the component

They look similar only because of Angular coding conventions, **not because Angular requires them to match**.
```
 ┌────────────────────────────────────────────────────────┐
 │                   Your Code (Developer)                │
 ├────────────────────────────────────────────────────────┤
 │ import { Component } from '@angular/core';             │
 │                                                        │
 │ @Component({                                           │
 │   selector: 'app-hello',                               │
 │   template: '<h1>{{title}}</h1>',                      │
 │   styleUrls: ['./hello.component.css']                 │
 │ })                                                     │
 │ export class HelloComponent {                          │
 │   title = 'Hello Angular';                             │
 │ }                                                      │
 └────────────────────────────────────────────────────────┘

                │
                ▼
  
 ┌────────────────────────────────────────────────────────┐
 │             Step 1️⃣  Decorator Function                │
 │ The @Component function runs at compile time.           │
 │ It attaches metadata (selector, template, styles)       │
 │ to your class using Reflect Metadata API.               │
 │  
 │   function Component(metadata: ComponentMetadata) {
 │    return function (target: Function) {
 │       // Attach metadata to the class
 │       Reflect.defineMetadata('annotations', metadata, target);
 │     };
 │   }
 │
 │ → Essentially stores:                                   │
 │   { selector: 'app-hello', template: '<h1>...</h1>' }   │
 └────────────────────────────────────────────────────────┘

                │
                ▼
 ┌────────────────────────────────────────────────────────┐
 │             Step 2️⃣  Angular Compiler (AOT)            │
 │ ng build / ng serve runs the Angular compiler.          │
 │                                                        │
 │ It reads the @Component metadata and generates          │
 │ a compiled version called “ɵcmp” factory.               │
 │                                                        │
 │ Example in JS:                                          │
 │ HelloComponent.ɵcmp = ɵɵdefineComponent({               │
 │   type: HelloComponent,                                 │
 │   selectors: [["app-hello"]],                           │
 │   template: function (rf, ctx) {                        │
 │     if (rf & 1) ɵɵelementStart(0, "h1"); ...            │
 │   }                                                     │
 │ });                                                     │
 └────────────────────────────────────────────────────────┘
                │
                ▼
 ┌────────────────────────────────────────────────────────┐
 │            Step 3️⃣  Angular Runtime Engine             │
 │ When your app runs:                                    │
 │                                                        │
 │ 1. Angular creates a component instance (HelloComponent)│
 │ 2. Finds `<app-hello>` tag in DOM                      │
 │ 3. Inserts the compiled template (`<h1>{{title}}</h1>`)│
 │ 4. Connects data binding (title → DOM text)             │
 │ 5. Starts change detection to update DOM dynamically.  │
 └────────────────────────────────────────────────────────┘
                │
                ▼
 ┌────────────────────────────────────────────────────────┐
 │             Step 4️⃣  What You See on Screen            │
 │                                                        │
 │   <h1>Hello Angular</h1>                               │
 │                                                        │
 │ Component rendered using compiled metadata and logic.  │
 └────────────────────────────────────────────────────────┘
```

```
Your Code (.ts, .html, .css)
           │
           ▼
   [ COMPILE STAGE ] ng serve
   ├─ Angular Compiler → AOT/JIT
   └─ TypeScript Compiler → .js
           │
           ▼
   [ BUILD STAGE ] ng build
   ├─ Webpack bundles everything
   ├─ Minifies, tree-shakes, hashes files
   └─ Outputs → /dist/your-app/
           │
           ▼
   Browser Loads → index.html + main.js

```

**SIGNAL**

signals to create and manage state

```

import {signal} from '@angular/core';
// Create a signal with the `signal` function.
const firstName = signal('Morgan');
// Read a signal value by calling it— signals are functions.
console.log(firstName());
// Change the value of this signal by calling its `set` method with a new value.
firstName.set('Jaime');
// You can also use the `update` method to change the value
// based on the previous value.
firstName.update(name => name.toUpperCase());

```

**COMPUTED**

A computed signal is read-only; 

it does not have a set or an update method. 

Instead, the value of the computed signal automatically changes when any of the signals it reads change:


```
import {signal, computed} from '@angular/core';
const firstName = signal('Morgan');
const firstNameCapitalized = computed(() => firstName().toUpperCase());
console.log(firstNameCapitalized()); // MORGAN
firstName.set('Jaime');
console.log(firstNameCapitalized()); // JAIME
```
**Reading signals in OnPush components**

When you read a signal within an OnPush component's template, Angular tracks the signal as a dependency of that component.

When the value of that signal changes, Angular automatically marks the component to ensure it gets updated the next time change detection runs.

**EFFECT**

An effect is an operation that runs whenever one or more signal values change. You can create an effect with the effect function:

```
effect(() => {
  console.log(`The current count is: ${count()}`);
});
```

Effects always run at least once.

When an effect runs, it tracks any signal value reads. 

Whenever any of these signal values change, the effect runs again. 

**Use cases**

Logging data being displayed and when it changes, either for analytics or as a debugging tool.

Keeping data in sync with window.localStorage.

Adding custom DOM behavior that can't be expressed with template syntax.

Performing custom rendering to a <canvas>, charting library, or other third party UI library.

```

Component created
      ↓
initializeLogging() called
      ↓
effect() starts watching this.count()
      ↓
count changes → effect re-runs automatically
      ↓
Component destroyed → effect is cleaned up (because injector provided)

```
```
import _ from 'lodash';
const data = signal(['test'], {equal: _.isEqual});
// Even though this is a different array instance, the deep equality
// function will consider the values to be equal, and the signal won't
// trigger any updates.
data.set(['test']);
```
By default, Angular checks equality using **reference equality** (===).
That means:

const arr = ['test'];
data.set(arr);
data.set(arr); // no trigger, same reference
data.set(['test']); // triggers, different array reference


Even though ['test'] looks the same, Angular would treat it as a new value — because it’s a new array instance.

To prevent unnecessary updates (when the contents are equal), you can pass a custom equality comparison function.

🔍 3. What _.isEqual Does

_.isEqual is a Lodash deep equality function.

_.isEqual(a, b)

returns true if two values are deeply equal (arrays, objects, nested structures, etc.), not just reference equal.

So { equal: _.isEqual } tells Angular:

**“Only treat the signal as changed if the new value is not deeply equal to the old one.”**

**Reading without tracking dependencies**

```
effect(() => {
  console.log(`User set to ${currentUser()} and the counter is ${untracked(counter)}`);
});
```

**Effect cleanup functions**

```
effect((onCleanup) => {
  const user = currentUser();
  const timer = setTimeout(() => {
    console.log(`1 second ago, the user became ${user}`);
  }, 1000);
  onCleanup(() => {
    clearTimeout(timer);
  });
});
```
**LINKED SIGNAL**

linkedSignal works similarly to signal with one key difference— instead of passing a default value, you pass a computation function, just like computed. When the value of the computation changes, the value of the linkedSignal changes to the computation result. This helps ensure that the linkedSignal always has a valid value.

```
const shippingOptions = signal(['Ground', 'Air', 'Sea']);
const selectedOption = linkedSignal(() => shippingOptions()[0]);
console.log(selectedOption()); // 'Ground'
selectedOption.set(shippingOptions()[2]);
console.log(selectedOption()); // 'Sea'
shippingOptions.set(['Email', 'Will Call', 'Postal service']);
console.log(selectedOption()); // 'Email'
```
```
linkedSignal<TSource, TValue>({
  source: Signal<TSource>,
  computation: (sourceValue: TSource, previous?: LinkedSignalValue<TValue>) => TValue,
})

```
**DYNAMIC TEMPLATES**

dynamic binding

double curly-braces

```
@Component({
  selector: 'user-profile',
  template:
`<h1>Profile for {{userName()}}</h1>`,  //values
`<button [disabled]="!isValidUserId()">Save changes</button>`, //properties
`<ul [attr.role]="listRole()">` //attributes
})
export class UserProfile {
  userName = signal('pro_programmer_123');
}
```
**Property Binding**

```
@Component({
  selector: 'app-root',
  styleUrls: ['app.css'],
  template: `
    <div [contentEditable]="isEditable"></div>
  `,
})
export class App {
  isEditable = true;
}

```

**USER INTERACTION**
```
@Component({
  /*...*/
  // Add an 'click' event handler that calls the `cancelSubscription` method.
  template: `<button (click)="cancelSubscription($event)">Cancel subscription</button>`,
})
export class UserProfile {
  /* ... */
  cancelSubscription(event: Event) { /* Your event handling code goes here. */  }
}

```
** IF & ELSE **

```
<h1>User profile</h1>
@if (isAdmin()) {
  <h2>Admin settings</h2>
  <!-- ... -->
} @else {
  <h2>User settings</h2>
  <!-- ... -->
}
```

** FOR **
```
<h1>User profile</h1>
<ul class="user-badge-list">
  @for (badge of badges(); track badge; let i = $index) {
    <li class="user-badge">{{i}} {{badge}}</li>
  }
</ul>
```


⚙️ 1. Where @if and @else Can Be Used
👉 @if, @else, @for, and @switch are template syntax, not TypeScript code. 
Used in html only


**Variable |	Meaning**
$count - 	Number of items in a collection iterated over

$index -	Index of the current row

$first -	Whether the current row is the first row

$last	- Whether the current row is the last row

$even	- Whether the current row index is even

$odd	- Whether the current row index is odd

```
@Component({
  selector: 'app-root',
  template: `
  @for(user of users;track user.id) {
  <p>{{user.name}}</p>
  }
  `,
})
export class App {
  users = [{id: 0, name: 'Sarah'}, {id: 1, name: 'Amy'}, {id: 2, name: 'Rachel'}, {id: 3, name: 'Jessica'}, {id: 4, name: 'Poornima'}];
}

```
**Dependency Injection**

Reuse code and control behaviors across your application and tests.

**SERVICES**
Services are reusable pieces of code that can be injected.

**A TypeScript decorator** that declares the class as an Angular service via **@Injectable** and allows you to define what part of the application can access the service via the **providedIn** property (which is typically **'root'**) to allow a service to be accessed anywhere within the application.
**A TypeScript class** that defines the desired code that will be accessible when the service is injected

```
import {Injectable} from '@angular/core';
@Injectable({providedIn: 'root'})
export class Calculator {
  add(x: number, y: number) {
    return x + y;
  }
}
```
**How to use a service**

```
import { Component, inject } from '@angular/core';
import { Calculator } from './calculator';
@Component({
  selector: 'app-receipt',
  template: `<h1>The total is {{ totalCost }}</h1>`,
})
export class Receipt {
  private calculator = inject(Calculator);
  totalCost = this.calculator.add(50, 25);
}

```
**INJECTION CONTEXT**

In Angular, Dependency Injection (DI) works only in certain places — that environment or “scope” is called the injection context.

👉 In simple words:

The injection context is the place (or moment) where Angular knows which injector to use to give you dependencies.

Angular DI can only work when Angular is actively running your code inside its control — for example:

Inside a component constructor

Inside a service constructor

Inside a factory function

Inside an inject() call used during component setup (not runtime logic)

If you call inject() outside Angular’s control (for example, inside a random function or callback), Angular throws:
```
NG0203: inject() must be called from an injection context.
```

```

import { Component, inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Component({
  selector: 'app-demo',
  standalone: true,
  template: `<p>Check console</p>`
})
export class DemoComponent {
  // We’re inside Angular’s injection context
  http = inject(HttpClient);

  constructor() {
    console.log('Http injected:', this.http);
  }
}


```
✅ How to Use runInInjectionContext()

Angular 16+ introduced a way to create your own injection context manually.

You can wrap your code in an Angular-controlled context:
```
import { inject, runInInjectionContext, EnvironmentInjector } from '@angular/core';
import { HttpClient } from '@angular/common/http';

export class MyService {
  constructor(private injector: EnvironmentInjector) {
    runInInjectionContext(this.injector, () => {
      const http = inject(HttpClient); // ✅ Works here
      console.log('HttpClient inside custom context:', http);
    });
  }
}
```
🧩 Analogy (Layman Terms)

Imagine Angular’s DI system as a vending machine (injector).

The injection context is when you’re standing in front of that machine.

If you’re outside the building (no machine nearby), you can’t get snacks (dependencies).

runInInjectionContext() is like setting up a temporary machine wherever you are.

✅ In Short

Term	Meaning	Example

**inject()**	Fetch a dependency in DI context	const http = inject(HttpClient)

**Injection Context**	Environment where Angular knows which injector to use	Component, Service, etc.

**runInInjectionContext()**	Manually create DI context	Useful in utility code

**INPUT PROPERTY**

send information from a parent component to a child component.

```
--Child COmponent-- 
import {Component, input} from '@angular/core';

@Component({
  selector: 'app-user',
  template: `
    <p>The user's name is {{name()}}</p>
  `,
})
export class User {
  name = input<String>();
}

--Parent Component --
import {User} from './user';

@Component({
  selector: 'app-root',
  template: `
    <app-user name="Simran"/>
  `,
  imports: [User],
})
export class App {}

```

**DIRECTIVES**
Direcivs doesnt have any templates. 
Components are directives. With templates.

ngModel directive - Used for 2 way binding. 

<img width="927" height="394" alt="image" src="https://github.com/user-attachments/assets/df4edd31-2d93-4ec9-adf2-94117d28f4a1" />


