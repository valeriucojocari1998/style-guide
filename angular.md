# Angular coding style guide

---

> Alvys' Official Style Guide

Key Sections:

- [Angular coding style guide](#angular-coding-style-guide)
  - [Rule of One](#rule-of-one)
  - [Small functions](#small-functions)
  - [Naming](#naming)
    - [File names separators](#file-names-separators)
    - [Symbols and file names](#symbols-and-file-names)
      - [Symbol Name Examples](#symbol-name-examples)
        - [`app.component.ts`](#appcomponentts)
        - [`trucks.component.ts`](#truckscomponentts)
        - [`truck-list.component.ts`](#truck-listcomponentts)
        - [`truck-detail.component.ts`](#truck-detailcomponentts)
        - [`validation.directive.ts`](#validationdirectivets)
        - [`home.module.ts`](#homemodulets)
        - [`init-cap.pipe.ts`](#init-cappipets)
        - [`user-profile.service.ts`](#user-profileservicets)
    - [Service Names](#service-names)
      - [Service Names Examples](#service-names-examples)
        - [`truck-data.service.ts`](#truck-dataservicets)
        - [`credit.service.ts`](#creditservicets)
    - [Component selectors](#component-selectors)
    - [Directive Selectors](#directive-selectors)
    - [Pipe Names](#pipe-names)
      - [Pipes Names Examples](#pipes-names-examples)
        - [`ellipsis.pipe.ts`](#ellipsispipets)
        - [`init-caps.pipe.ts`](#init-capspipets)
    - [Unit Test File Names](#unit-test-file-names)
    - [_End-to-End_ (E2E) Test File Names](#end-to-end-e2e-test-file-names)
    - [Angular _NgModule_ Names](#angular-ngmodule-names)
      - [Angular _NgModule_ Names Examples](#angular-ngmodule-names-examples)
        - [`app.module.ts`](#appmodulets)
        - [`trucks.module.ts`](#trucksmodulets)
        - [`app-routing.module.ts`](#app-routingmodulets)
  - [Bootstrapping](#bootstrapping)
  - [Application structure and NgModules](#application-structure-and-ngmodules)
    - [_LIFT_](#lift)
    - [Locate](#locate)
    - [Identify](#identify)
    - [Flat](#flat)
    - [_T-DRY_ (Try to be _DRY_)](#t-dry-try-to-be-dry)
    - [Overall structural guidelines](#overall-structural-guidelines)
      - [Project Root Representation](#project-root-representation)
    - [_Folders-by-feature_ structure](#folders-by-feature-structure)
    - [App _root module_](#app-root-module)
    - [Feature modules](#feature-modules)
    - [Shared feature module](#shared-feature-module)
      - [Shared Module short example](#shared-module-short-example)
    - [Lazy Loaded folders](#lazy-loaded-folders)
    - [Pipes](#pipes)
  - [Components](#components)
    - [Components as elements](#components-as-elements)
      - [Components Examples](#components-examples)
    - [Templates and Styles](#templates-and-styles)
      - [Templates and Styles Examples](#templates-and-styles-examples)
    - [Decorate _input_ and _output_ properties](#decorate-input-and-output-properties)
      - [_Input_ and _Output_ Examples](#input-and-output-examples)
    - [Avoid aliasing _inputs_ and _outputs_](#avoid-aliasing-inputs-and-outputs)
    - [Member sequence](#member-sequence)
    - [Delegate complex component logic to services](#delegate-complex-component-logic-to-services)
    - [Don't prefix _output_ properties](#dont-prefix-output-properties)
    - [Put presentation logic in the component class](#put-presentation-logic-in-the-component-class)
    - [Initialize inputs](#initialize-inputs)
  - [Directives](#directives)
    - [Use directives to enhance an element](#use-directives-to-enhance-an-element)
    - [_HostListener_/_HostBinding_ decorators versus _host_ metadata](#hostlistenerhostbinding-decorators-versus-host-metadata)
  - [Services](#services)
    - [Services are singletons](#services-are-singletons)
    - [Single responsibility](#single-responsibility)
    - [Providing a service](#providing-a-service)
    - [Use the @Injectable() class decorator](#use-the-injectable-class-decorator)
  - [Data Services](#data-services)
    - [Talk to the server through a service](#talk-to-the-server-through-a-service)
  - [Lifecycle hooks](#lifecycle-hooks)
    - [Implement lifecycle hook interfaces](#implement-lifecycle-hook-interfaces)


## Rule of One

---

- Define one thing, such as a service or component, per file, limiting files to 400 lines of code.

> Reason: One component per file makes it far easy to read, maintain, and avoid collisions. Moreover, it help with hidden bugs' prevention.

**Bad**

```ts
import { Component, NgModule, OnInit } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

interface Truck {
  id: number;
  name: string;
}

@Component({
  selector: 'app-root',
  template: `
      <h1>{{title}}</h1>
      <pre>{{trucks | json}}</pre>
    `,
  styleUrls: ['app/app.component.css']
})
class AppComponent implements OnInit {
  title = 'Trucks';

  heroes: Hero[] = [];

  ngOnInit() {
    getTrucks().then(trucks => (this.trucks = trucks));
  }
}

@NgModule({
  imports: [BrowserModule],
  declarations: [AppComponent],
  exports: [AppComponent],
  bootstrap: [AppComponent]
})
export class AppModule {}

platformBrowserDynamic().bootstrapModule(AppModule);

const TRUCKS: Truck[] = [
  { id: 1, name: 'Volvo' },
  { id: 2, name: 'Tesla' },
  { id: 3, name: 'Man' }
];

function getTrucks(): Promise<Truck[]> {
  return Promise.resolve(TRUCKS); // TODO: get truck data from the server;
}
```

**Good**

- It is a better practice to redistribute the component and its supporting classes into their own, dedicated files.

> Reason: As the application grows, this rule becomes even more important.

## Small functions

---

- Define small functions, limiting to no more than 100 lines.

> Reason: Small functions are easier to read, maintain, test as well as promoting reuse.

## Naming

---

- Naming conventions are hugely important to maintainability and readability. This guide recommends naming conventions for the file name and the symbol name.

> For more information, check: [Typescript File Naming](./typescript.md/#filename)

- Use consistent names for all symbols. Follow the parttern that describes symbol's feature then its type e.g. `feture.type.ts`
- Names of folders and files should clearly convey their intent e.g. `app/trucks/truck-list.component.ts`

> Reason: Naming conventions help provide a consistent way to find content at a glance. Consistency within the project is vital. Consistency with a team is important. Consistency across the company provides tremendous efficiency.

### File names separators

---

- Use dashes to separate words in the descriptive name.
- Use dots to separate the descriptive name from the type.
- Use conventional type names including `.service`, `.component`, `.pipe`, `.module`, and `.directive`.

> Reason: Type names provide a consistent way to quickly identify what is in the file. Type names make it easy to find a specific file type using an editor or IDE's fuzzy search techniques. Type names provide pattern matching for any automated tasks.

### Symbols and file names

---

- Use consistent names for all assets named after what they represent.
- Use `UpperCamelCase` for all class name
- Match the name of the symbol to the name of the file.
- Append the symbol name with the conventional suffix (such as `Component`,
`Directive`, `Module`, `Pipe`, or `Service`) for a thing of that type.
- Give the filename the conventional suffix (such as `.component.ts`, `.directive.ts`,
`.module.ts`, `.pipe.ts`, or `.service.ts`) for a file of that type.

> Reason: Consistent conventions make it easy to quickly identify
and reference assets of different types.

#### Symbol Name Examples

---

##### `app.component.ts`

```ts
@Component({ ... })
export class AppComponent { }
```

##### `trucks.component.ts`

```ts
@Component({ ... })
export class TrucksComponent { }
```

##### `truck-list.component.ts`

```ts
@Component({ ... })
export class TruckListComponent { }
```

##### `truck-detail.component.ts`

```ts
@Component({ ... })
export class TruckDetailComponent { }
```

##### `validation.directive.ts`

```ts
@Directive({ ... })
export class ValidationDirective { }
```

##### `home.module.ts`

```ts
@NgModule({ ... })
export class HomeModule
```

##### `init-cap.pipe.ts`

```ts
@Pipe({ name: 'initCap' })
export class InitCapPipe implements PipeTransform { }
```

##### `user-profile.service.ts`

```ts
@Injectable()
export class UserProfileService { }
```

### Service Names

---

- Use consistent names for all servies named after their feature
- Suffice a service class name with `Service` e.g. `TruckService` or `DataService`.

> Reason: Provides a consistent way to quickly identify and reference services.

#### Service Names Examples

---

##### `truck-data.service.ts`

```ts
@Injectable()
export class TruckDataService { }
```

##### `credit.service.ts`

```ts
@Injectable()
export class CreditService { }
```

### Component selectors

---

- Use `dashed-case` for naming the element selectors of components.
- Avoid `camelCase` for naming the element selectors.
- Avoid single word names for naming the element selectors.

> Reason: Keeps the element names consistent with the specification for [Custom Elements](https://www.w3.org/TR/custom-elements/).

**Bad**

```ts
@Component({
  selector: 'tohTruckButton',
  templateUrl: './truck-button.component.html'
})
export class TruckButtonComponent {}
```

```ts
@Component({
  selector: 'truck',
  templateUrl: './truck.component.html'
})
export class TruckComponent {}
```

**Good**

```ts
@Component({
  selector: 'toh-truck-button',
  templateUrl: './truck-button.component.html'
})
export class TruckButtonComponent {}
```

### Directive Selectors

---

- Use `lowerCamelCase` for naming the selectors of directives.

> Reason: The HTML parser is case sensitive and recognizes `lowerCamelCase`.

- Use a custom prefix for the selector of directives.
- Spell non-element selectors in lowerCamelCase unless the selector is meant to match a native HTML attribute.
- Dont prefix a directive name with ng, as it is reserved.

> Reason: Prevents name collisions, and are easily identified.

**Bad**

```ts
@Directive({
  selector: '[validate]'
})
export class ValidateDirective {}
```

**Good**

```ts
@Directive({
  selector: '[truckValidate]'
})
export class ValidateDirective {}
```

### Pipe Names

---

- Use consistent names for all pipes, named after their feature.
- The pipe class name should use `UpperCamelCase`, and the corresponding `name` string should use `lowerCamelCase`.
- The `name` string cannot use hyphens (`dash-case` or `kebab-case`).

> Reason: Provides a consistent way to quickly identify and reference pipes.

#### Pipes Names Examples

---

##### `ellipsis.pipe.ts`

```ts
@Pipe({ name: 'ellipsis' })
export class EllipsisPipe implements PipeTransform { }
```

##### `init-caps.pipe.ts`

```ts
@Pipe({ name: 'initCaps' })
export class InitCapsPipe implements PipeTransform { }
```

### Unit Test File Names

---

- Name test specification files the same as the component they test.
- Name test specification files with a suffix of `.spec`.

> Reason: Provides a consistent way to quickly identify tests.

### _End-to-End_ (E2E) Test File Names

---

- Name end-to-end test specification files after the feature they test with a suffix of `.e2e-spec`.

> Reason: Provides a consistent way to quickly identify end-to-end tests.

### Angular _NgModule_ Names

---

- Append the symbol name with the suffix `Module`.
- Give the file name the `module.ts` extension.
- Name the module after the feature and folder it resides in.

> Reason: Provides a consisten way to quickly identify and reference modules.

- Suffix a **RoutingModule** class name with `RoutingModule`.
- Give the file name the `-routing.module.ts`.

> Reason: A `RoutingModule` is a module dedicated exclusively to configuring the Angular router.

#### Angular _NgModule_ Names Examples

---

##### `app.module.ts`

```ts
@NgModule({ ... })
export class AppModule { }
```

##### `trucks.module.ts`

```ts
@NgModule({ ... })
export class TrucksModule { }
```

##### `app-routing.module.ts`

```ts
@NgModule({ ... })
export class AppRoutingModule { }
```

## Bootstrapping

---

- Avoid putting application logic in `main.ts`. Instead, consider placing it in a component or service.

> Reason: Follows a consistent convention for the startup logic of an app. Follows a familiar convention from other technology platforms.

## Application structure and NgModules

---

- Have a near-term view of implementation and a long-term vision. Start small but keep in mind where the application is heading.
- All of the application's code goes in a folder named `src`.
- All feature areas are in their own folder, with their own NgModule.
- All content is one asset per file. Each component, service, and pipe is in its own file.
- All third party vendor scripts are stored in another folder and not in the `src` folder.
- You didn't write them and you don't want them cluttering `src`.
- Use the naming conventions for files in this guide.

### _LIFT_

---

- Structure the aplication such that you can **L**ocate code quickly, **I**dentify the code at a glance, keep the **F**lattest structure you can, and **T**ry to be DRY (**LIFT**).

> Reason: LIFT provides a consistent structure that scales well, is modular, and makes it easier to increase developer efficiency by finding code quickly.

### Locate

---

- Make locating code intuitive and fast
- Place the components in `src/app/component` folder.
- Place the modules in `src/app/module` folder.
- Place the directives in `src/app/directives` folder.
- Place the pipes in `src/app/pipes` folder.
- Place the services in `src/app/services` folder.
- Place shared code (used in several parts of the app) in `src/app/shared` folder.

> Reason: To work efficient you must be able to find files quickly, especially when you don't know the file *name*.

### Identify

---

- Name the file such that you instantly know what it contains and represents.
- Be descriptive with file names and keep the contents of the file to exactly one component.

> Reason: Spend less time hunting and pecking for code, and become more efficient.

### Flat

---

- Keep a flat folder structure as long as possible.
- Consider creating sub-folders when a folder has too many files.

> Reason: it makes locating the needed code easier.

### _T-DRY_ (Try to be _DRY_)

---

- Be DRY (**D**on't **R**epeat **Y**ourself).
- Avoid being so DRY that you sacrifice readability.

> Reason: Being DRY is important, but not crucial if it sacrifices the other elements of LIFT

### Overall structural guidelines

---

- Start small but keep in mind where the application is heading down the road.
- Have a near term view of implementation and a long term vision.
- Put all of the application's code in a folder named `src`.

> Reason: Helps keep the application structure small and easy to maintain in the early stages, while being easy to evolve as the application grows.

#### Project Root Representation

---

```html
src/
|  index.html
|  main.ts
|  styles.scss
|  ...
|
└──assets/
|  |  audio/
|  |  fonts/
|  |  ...
|
└──css/
└──theme/
└──app/
|  |  app-routing.module.ts
|  |  app.component.html
|  |  app.component.ts
|  |  app.module.ts
|  |  ...
|  └──components/
|  |  └──carrier/
|  |  |  |  carrier.component.ts
|  |  |  |  ...
|  |  |
|  |  └──...
|  |
|  └──modules/
|  |  └──carrier/
|  |  |  |  carrier.module.ts
|  |  |  |  ...
|  |  |
|  |  └──...
|  |
|  └──shared/
|  |  └──components/
|  |  └──pipes/
|  |  └──classes/
|  |  └──...
|  |
|  └──...
|
└──...
```

### _Folders-by-feature_ structure

---

- Create foleders named for the feature area they represent

> Reason: A developer must locate the code and identify what each file represents at a glance.

- Create an `NgModule` for each feature area.

> Reason: `NgModule` makes it easy to lazy load routable features, as well as isolate, test, and reuse features.

### App _root module_

---

- Create and `NgModule` in the application's root folder, for example, in `/src/app`.

> Reason: Every application requires at least one root `NgModule`.

### Feature modules

---

- Create and `NgModule` for all distinct features in an application.
- Place the feature module in the `src/app/modules` folder and the feature component in `src/app/components` folder.

> Reason: Each distinct feature has a unical implementation, that can be routed both eagerly and lazily.

### Shared feature module

---

- Declare components, directives, pipes, etc. in the shared module when those items will be re-used and refrenced by the components declared in other feature modules.
- Consider not adding services to `SharedModule`.
- Import all required Modules inside `shared.module.ts`

> Reason: `SharedModule` contains declarations of all the items that are reused. Moreover, it declared components and exports them which makes it easy to use and re-usable component in any part of the code by importing `SharedModule` in the working Module.

#### Shared Module short example

```ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { AppRoutingModule } from '../app-routing.module';

import { TopBarComponent } from './components/top-bar/top-bar.component';
import { ProductListComponent } from './components/product-list/product-list.component';
import { ProductDetailsComponent } from './components/product-details/product-details.component';
import { BottomBarComponent } from './components/bottom-bar/bottom-bar.component';

@NgModule({
  declarations: [
    TopBarComponent,
    ProductListComponent,
    ProductDetailsComponent,
    BottomBarComponent,
  ],
  imports: [
    CommonModule,
    AppRoutingModule,
  ],
  exports: [
    TopBarComponent,
    ProductListComponent,
    ProductDetailsComponent,
    BottomBarComponent,
  ]
})
export class SharedModule { }

```

### Lazy Loaded folders

---

- Put the contents of lazy loaded features in the *lazy loaded folder*

> Reason: The folder makes it easy to identify and isolate the feature content.

- Never import directly lazy loaded folders.

> Reason: Directly importing and using a module will load it immediately when the intention is to load it on demand.

### Pipes

---

- Do not add filtering and sorting logic to pipes
- Pre-compute the filtering and sorting logic in the components or services before binding the model in templates.

> Reason: Filtering and especially sorting are expensive operations. As Angular can call pipe methods many times per second, sorting and filtering operations can degrade the user experience severely for even moderately-sized lists.

## Components

---

### Components as elements

---

- Consider giving components an `selector`, as opposed to `attribute` or `class` selectors.

> Reason: Components have templates containing HTML and optional Angular template syntax. They display content. Developers place components on the page as they would native HTML elements and web components.

#### Components Examples

---

**Bad**

```ts
@Component({
  selector: '[tohTruckButton]',
  templateUrl: './truck-button.component.html'
})
export class HeroButtonComponent {}
```

```html
<div tohTruckButton></div>
```

**Good**

```ts
@Component({
  selector: 'toh-truck-button',
  templateUrl: './truck-button.component.html'
})
export class TruckButtonComponent {}
```

```html
<toh-truck-button></toh-truck-button>
```

### Templates and Styles

---

- Extract templates and styles into a separate file, when more than 3 lines
- Name the template file `[component-name].component.html`, where [component-name] is the component name.
- Name the style file `[component-name].component.css`, where [component-name] is the component name.
- Specify _component-relative_ URLs, prefixed with `./`.

> Reason: In most editors, syntax hints and code snippets aren't available when developing inline templates and styles. The Angular TypeScript Language Service (forthcoming) promises to overcome this deficiency for HTML templates. In those editors that support it; it won't help with CSS styles.
> The `./` prefix is standard syntax for relative URLs; don't depend on Angular's current ability to do without that prefix.

#### Templates and Styles Examples

---

**Bad**

```ts
import { Component } from '@angular/core';

//Imagine the template and style would be much longer in this case.
@Component({
  selector: 'hello-world',
  template: `
    <h2>Hello World</h2>
    <p>This is my first component!</p>
  `,
  styles: [`
  h2 {
     font-size: 100px;
  }
  `]
})
export class HelloWorldComponent {
}
```

**Good**

```ts
import { Component } from '@angular/core';

//Imagine the template and style would be much longer in this case.
@Component({
  selector: 'hello-world',
  templateUrl: 'hello-world.component.html',
  stylesUrls: ['hello-world.component.css']
})
export class HelloWorldComponent {
}
```

### Decorate _input_ and _output_ properties

---

- Use the `@Input()` and `@Output()` class decorators instead of the `inputs` and `outputs` properties of the `@Directive` and `@Component` metadata:
- Consider placing `@Input()` or `@Output()` on the same line as the property it decorates.

> Reason: It is easier and more readable to identify which properties in a class are inputs and outputs, and if you ever need to rename the property or event name associated with `@Input()` or `@Output()`, you can modify it in a single place.

#### _Input_ and _Output_ Examples

---

```ts
@Component({
  selector: 'toh-truck-button',
  template: `<button></button>`,
  inputs: [
    'label'
  ],
  outputs: [
    'truckChange'
  ]
})
export class HeroButtonComponent {
  heroChange = new EventEmitter<any>();
  label: string;
}
```

### Avoid aliasing _inputs_ and _outputs_

---

- Avoid _input_ and _output_ aliases except when it serves an important purpose.

> Reason: Two names for same property is inherently confusing.

### Member sequence

---

- Place properties up top followed by methods
- Place _private_ members after public members, alphabetized.

> Reason: This way it makes the sequence easy to read and helps instantly identify which members of the component serve which purpose.

### Delegate complex component logic to services

---

- Limit logic in a component to only that required for the view. All the other logic should be delegated to services.
- Move reusable logic to services and keep components simple and focused on their intended purpose.

> Reason: Logic may be reused multiple times in several different components. Moreover, logic in a service can more easily be isolated in a unit test, while the calling logic in the component can be easily mocked.

### Don't prefix _output_ properties

---

- Name events without the prefix `on`.
- Name event handler methods with the prefix `on` followed by the event name.

> This is consistent with built-in events such as button clicks.

**Bad**

```ts
@Component({
  selector: 'toh-truck',
  template: `...`
})
export class TruckComponent {
  @Output() onSavedTheDay = new EventEmitter<boolean>();
}
```

```html
<toh-truck (onSavedTheDay)="onSavedTheDay($event)"></toh-truck>
```

**Good**

```ts
@Component({
  selector: 'toh-truck',
  template: `...`
})
export class TruckComponent {
  @Output() savedTheDay = new EventEmitter<boolean>();
}
```

```html
<toh-truck (savedTheDay)="onSavedTheDay($event)"></toh-truck>
```

### Put presentation logic in the component class

---

- Put presentation logic in the component class, and not in the template.

> Reason: Logic will be contained in one place (the component class) instead of being spread in two places. Moreover, keeping the component's presentation logic in the class instead of the template improves testability, maintainability, and reusability.

**Bad**

```ts
@Component({
  selector: 'toh-truck-list',
  template: `
    <section>
      Our list of trucks:
      <toh-truck *ngFor="let truck of trucks" [truck]="truck">
      </toh-truck>
      Total weight: {{totalWeight}}<br>
      Average weight: {{totalWeight / trucks.length}}
    </section>
  `
})
export class TruckListComponent {
  trucks: Truck[];
  totalWeight: number;
}
```

**Good**

```ts
@Component({
  selector: 'toh-truck-list',
  template: `
    <section>
      Our list of trucks:
      <toh-truck *ngFor="let truck of trucks" [truck]="truck">
      </toh-truck>
      Total weight: {{totalWeight}}<br>
      Average weight: {{avgWeight}}
    </section>
  `
})
export class TruckListComponent {
  trucks: Truck[];
  totalWeight: number;

  get avgWeight() {
    return this.totalWeight / this.trucks.length;
  }
}
```

### Initialize inputs

---

- Intialize the value of the _Input_ as TypeScript's `--strictPropertyInitialization` compiler option ensures that a class initializes its properties during construction.

> Reason: By design, Angular treats all `@Input` properties as optional. When possible, you should satisfy `--strictPropertyInitialization` by providing a default value.

```ts
@Component({
  selector: 'toh-truck',
  template: `...`
})
export class TruckComponent {
  @Input() id = 'default_id';
}
```

- If the property is hard to construct a default value for, use `?` to explicitly mark the property as optional.

```ts
@Component({
  selector: 'toh-truck',
  template: `...`
})
export class TruckComponent {
  @Input() id?: string;

  process() {
    if (this.id) {
      // ...
    }
  }
}
```

- If you want to have a required `@Input` field, meaning all your component users are required to pass that attribute. In such cases, use a default value. Just suppressing the TypeScript error with `!` is insufficient and should be avoided because it will prevent the type checker ensure the input value is provided.

**Bad**

```ts
@Component({
  selector: 'toh-truck',
  template: `...`
})
export class TruckComponent {
  @Input() id!: string;
}
```

## Directives

---

### Use directives to enhance an element

---

- Use attribute directives when you have presentation logic without a template.

> Reason: Attribute directives don't have an associated template. Moreover, an element may have more than one attribute directive applied.

```ts
@Directive({
  selector: '[tohHighlight]'
})
export class HighlightDirective {
  @HostListener('mouseover') onMouseEnter() {
    // do highlight work
  }
}
```

```html
<div tohHighlight>Truck</div>
```

### _HostListener_/_HostBinding_ decorators versus _host_ metadata

---

- Consider preferring the `@HostListener` and `@HostBinding` to the
`host` property of the `@Directive` and `@Component` decorators.
- Be consistent in your choice.

> Reason: The property associated with `@HostBinding` or the method associated with `@HostListener` can be modified only in a single place&mdash;in the directive's class.
> If you use the `host` metadata property, you must modify both the property/method declaration in the directive's class and the metadata in the decorator associated with the directive.

```ts
import { Directive, HostBinding, HostListener } from '@angular/core';

@Directive({
  selector: '[tohValidator]'
})
export class ValidatorDirective {
  @HostBinding('attr.role') role = 'button';
  @HostListener('mouseenter') onMouseEnter() {
    // do work
  }
}
```

- Compare with the less preferred `host` metadata alternative.

> Reason: The `host` metadata is only one term to remember and doesn't require extra ES imports.

```ts
import { Directive } from '@angular/core';

@Directive({
  selector: '[tohValidator2]',
  host: {
    '[attr.role]': 'role',
    '(mouseenter)': 'onMouseEnter()'
  }
})
export class Validator2Directive {
  role = 'button';
  onMouseEnter() {
    // do work
  }
}
```

## Services

---

### Services are singletons

---

- Use services as singletons within the same injector. Use them for sharing data and functionality.

> Reason: Services are ideal for sharing methods across a feature area or an app. Moreover, services are ideal for sharing stateful in-memory data.

```ts
export class TruckService {
  constructor(private http: HttpClient) { }

  getTrucks() {
    return this.http.get<Truck[]>('api/trucks');
  }
}
```

### Single responsibility

---

- Create ervices with a single responsibility that is encapsulated by its context.
- Create a new service once the service begins to exceed that singular purpose.

> Reason: When a service has multiple responsibilities, it becomes difficult to test. Moreover, when a service has multiple responsibilities, every component or service that injects it now carries the weight of them all.

### Providing a service

---

- Provide a service with the application root injector in the `@Injectable` decorator of the service.

> Reason: The Angular injector is hierarchical. Also, when you provide the service to a root injector, that instance of the service is shared and available in every class that needs the service. This is ideal when a service is sharing methods or state.
> When you register a service in the `@Injectable` decorator of the service, optimization tools such as those used by the [Angular CLI's](cli) production builds can perform tree shaking and remove services that aren't used by your app.

```ts
@Injectable({
  providedIn: 'root',
})
export class Service {
}
```

### Use the @Injectable() class decorator

---

- Use the `@Injectable()` class decorator instead of the `@Inject` parameter decorator when using types as tokens for the dependencies of a service.

> Reason: The Angular Dependency Injection (DI) mechanism resolves a service's own dependencies based on the declared types of that service's constructor parameters.
> When a service accepts only dependencies associated with type tokens, the `@Injectable()` syntax is much less verbose compared to using `@Inject()` on each individual constructor parameter.

**Bad**

```ts
export class TruckArena {
  constructor(
      @Inject(TruckService) private truckService: TruckService,
      @Inject(HttpClient) private http: HttpClient) {}
}
```

**Good**

```ts
@Injectable()
export class TruckArena {
  constructor(
    private truckService: TruckService,
    private http: HttpClient) {}
}
```

## Data Services

---

### Talk to the server through a service

- Refactor logic for making data operations and interacting with data to a service.
- Make data services responsible for XHR calls, local storage, stashing in memory, or any other data operations.

> Response: The component's responsibility is for the presentation and gathering of information for the view. It should not care how it gets the data, just that it knows who to ask for it. Separating the data services moves the logic on how to get it to the data service, and lets the component be simpler and more focused on the view.
> This makes it easier to test (mock or real) the data calls when testing a component that uses a data service.
> The details of data management, such as headers, HTTP methods,
caching, error handling, and retry logic, are irrelevant to components
and other data consumers.

## Lifecycle hooks

---

- Use Lifecycle hooks to tap into important events exposed by Angular.

### Implement lifecycle hook interfaces

---

- Implement the lifecycle hook interfaces.

> Reason: Lifecycle interfaces prescribe typed method
signatures. Use those signatures to flag spelling and syntax mistakes.

**Bad**

```ts
@Component({
  selector: 'toh-truck-button',
  template: `<button>OK<button>`
})
export class TruckButtonComponent {
  onInit() { //wrong spelling
    console.log('The component is initialized');
  }
}
```

**Good**

```ts
@Component({
  selector: 'toh-truck-button',
  template: `<button>OK</button>`
})
export class TruckButtonComponent implements OnInit {
  ngOnInit() {
    console.log('The component is initialized');
  }
}
```
