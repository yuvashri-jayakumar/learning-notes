<img width="425" height="720" alt="image" src="https://github.com/user-attachments/assets/2b5b9b81-9214-4c7c-8533-838e3e37fbee" />

1. package.json

Defines project dependencies and scripts.

dependencies → packages required to run your app.

devDependencies → tools for development/testing.

scripts → shortcuts like npm start.

2. angular.json

Angular CLI’s configuration file — controls how the app is built, served, and deployed.

3. tsconfig.json /tsconfig.app.json/ tsconfig.spec.json

Base TypeScript configuration.

4. .editorconfig / .gitignore

.editorconfig → keeps consistent coding style.

.gitignore → lists files Git should not track (like node_modules).

🧩 Step 3: Inside src/ folder (Main Application)

This folder contains your actual app code.
```
1. main.ts (Entry point)
import { bootstrapApplication } from '@angular/platform-browser';
import { AppComponent } from './app/app.component';

bootstrapApplication(AppComponent)
  .catch(err => console.error(err));
```

This is the starting point of the app.

It bootstraps (starts) the root component — AppComponent.

2. index.html
 ```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>NewApp</title>
  <base href="/" />
</head>
<body>
  <app-root></app-root>
</body>
</html>
```

This is the HTML page loaded by the browser.

<app-root> is a custom Angular element — replaced at runtime by AppComponent’s template.

3. styles.css

Global styles for the whole app.

4. environments/

Holds environment-specific variables.

environment.ts – for production build

environment.development.ts – for local dev

🧠 Step 4: Inside app/ folder (Core of your app)

2. app.ts

```
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.html',
  styleUrls: ['./app.css']
})
export class AppComponent {
  title = 'new-app';
}

```
Defines a component using @Component.

Has a selector (app-root) used in index.html. Always selector name should have 2 words to avoid clashes with keywords.

Controls data and behavior shown in the UI.

3. app.html

The template that Angular renders.

4. app.css

Styles specific to this component.

5. app.spec.ts

Test file (using Jasmine/Karma).

🔄 Step 5: Application Flow (How Angular Runs)

Let’s connect the dots 🔗
```
Browser loads index.html
        ↓
<app-root> tag found
        ↓
Angular loads main.ts
        ↓
main.ts bootstraps AppComponent (or AppModule)
        ↓
AppComponent loads its HTML + CSS
        ↓
Template is rendered on browser
        ↓
You see "Welcome to new-app!"
```

```
          ┌────────────────────────────┐
          │       Browser loads        │
          │      index.html file       │
          └────────────┬───────────────┘
                       │
                       ▼
        <app-root></app-root> placeholder in index.html
                       │
                       ▼
          ┌────────────────────────────┐
          │       main.ts file         │
          │ (Entry point of Angular)   │
          └────────────┬───────────────┘
                       │
                       ▼
        bootstrapApplication(AppComponent)
                       │
                       ▼
          ┌────────────────────────────┐
          │      AppComponent.ts       │
          │  @Component decorator      │
          │  selector: 'app-root'      │
          └────────────┬───────────────┘
                       │
                       ▼
          ┌────────────────────────────┐
          │   app.component.html       │
          │   app.component.css        │
          │   (View + Style)           │
          └────────────┬───────────────┘
                       │
                       ▼
          Angular renders HTML content
          inside <app-root> in index.html
                       │
                       ▼
          ┌────────────────────────────┐
          │     Browser displays        │
          │  the full Angular App UI    │
          └────────────────────────────┘

```

🧭 TL;DR (Summary Table)
File/Folder	Purpose
angular.json	CLI config for build/serve/test
package.json	Dependencies and scripts
tsconfig.json	TypeScript settings
src/main.ts	App entry point
src/index.html	Root HTML page
src/app/	Application logic (components, modules)
src/environments/	Environment-specific settings
src/styles.css	Global styles
