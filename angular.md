***component***

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


