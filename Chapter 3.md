# App Essentials
* This chapter covers the essentials of Angular applications so that you can understand how everything fits together. It will be a good reference for the fundamentals. It is focused on concepts, and there are no coding projects. 
* You may be eager to jump into coding, and I certainly understand that. I recommend you take the time to read this chapter in its entirety, but you can also start by skimming the first couple paragraphs of each section.

## Entities in Angular
* Angular has several top-level types of entities, these different entities have specific roles and capabilities, and you’ll be using them in various combinations to create your application. 
* Here is a quick overview of the types:
    * **Modules** —Objects that help you to organize dependencies into discrete units
    * **Components** —New elements that will compose the majority of your application’s structure and logic
    * **Directives** —Objects that modify elements to give them new capabilities or change behaviors
    * **Pipes** —Functions that format data before it’s rendered 
    * **Services** —Reusable objects that fill niche roles such as data access or helper utilities 

## Modules
1. **What Are Modules?**
    * Modules are buckets for storing related entities for easy reuse and distribution. Angular itself is composed of several modules, and any external libraries that you consume will also be packaged as modules.
2. **What are Kinds of Modules**?
    * There are two kinds of modules in Angular, and we need to clarify the difference.
        * There are JavaScript modules **(language constructs)** added in ES2015, and then there are Angular modules.
        
3. **What is the difference between JS Modules and Angular Modules?**
    * *JavaScript modules* are language constructs and are a way to separate code into different files that can be loaded as needed. 
        * We leverage JavaScript modules heavily in our code, but they are not Angular modules. Every TypeScript file we wrote in chapter 2 was a JavaScript module because it either imported or exported some values.
    * *Angular modules* are **(logical constructs)** used for organizing similar groups of entities (such as all things needed for the router) and are used by Angular to understand what needs to be loaded as well as what dependencies exist. 
        * Recall from chapter 2 that your application has an App module that holds a reference to all the application logic for Angular to render. 
4. **What types of Angular Modules and Is it a mandatory for the App?**
    * There must always be an App module, but there will likely be additional modules in your application— either **official Angular modules**, **third-party ones**, or other **ones you may create.**
5. **How to declare a Module (with Example)?**
    * A module is declared by **creating a class** and **decorating** it with the `@NgModule` decorator.
    
    ```typescript
    @NgModule({
    //Defines array of components and directives
    declarations: [
    AppComponent,
    SummaryComponent
    ],
    //Defines array of modules to import
    imports: [
    BrowserModule,
    FormsModule,
    HttpModule
    ],
    //Defines array of services to load
    providers: [StocksService],
    //Defines array of components to bootstrap on startup
    bootstrap: [AppComponent]
    })
    export class AppModule { }
    ```
6. **Explain the structure of `@NgModule` decorator?**
    * The `@NgModule` decorator contains the metadata for the App module, and the empty class acts as the **vessel** for storing the data. 
        * The **declarations array** contains a list of all **components and directives** that the application’s main module wants to *make available to the entire application*. 
        * Likewise, the **providers array** contains a list of all of the **services** that you want to *make available to the whole application*.
        * The **imports array** contains a list of the *other modules that this module depends upon*.
        * To start rendering, Angular also needs to know *what component(s) to render on the screen*, and it looks at the **bootstrap array** for this list. Almost always, this will only contain one component, but in some rare cases you may need to render multiple components on load.

## Components
1. **What are Components?**
    * A component is an encapsulated element that maintains its own internal logic for how it desires to render some output. In HTML, a select element can be considered a component.
2. **What are the component design principles?**
    * As a review, here are the key principles of a component, these principles focus on the way that components are best designed and how they behave in Angular:
        * **Encapsulation** —Keep component logic isolated
        * **Isolation** —Keep component internals hidden
        * **Reusability** —Allow component reuse with minimal effort
        * **Event-based** —Emit events during the lifecycle of the component
        * **Customizable** —Possible to style and extend the component
        * **Declarative** —Component used with simple declarative markup


## Directives
1. **What are the benefits of directives?**
    * Directives are a powerful tool to teach `HTML` elements new skills.
    * They take an existing element and apply new capabilities, such as making an image open up in a modal window.
    * Directives can take a normal element and give it additional capabilities that don’t exist naturally.
    * Directives make life much easier because they modify an element to give it a new capability, without having to use JavaScript to reach into the template and modify it on the fly. 
    * We don’t have to use something like jQuery to modify the DOM and put our logic in an external, dissociated location.
2. **Directives Use Case**
    * Imagine you’re building a form in which it’s important that the user doesn’t accidentally click any links to navigate away. 
    * You could create a directive that can disable links depending on whether the user has started to use the form or if they’ve completed it, and internally it would modify the anchor link to disable the href and therefore the clickability of the link.

3. **Directive Example**
    * Here is one that adds a directive that will **render or remove the element** based on the value inside the attribute value (known as an expression):
    ```html
    <div *ngIf="!stocks">
    ```
    * The `*ngIf` is the directive, which is applied as an **attribute to the element**, and it will evaluate the value it’s assigned to. 
    * In this case, if the expression is **truthy**, it will **render** the element—**otherwise** it will **remove** it from the DOM. 
4. **What is `NgIf` role?**
    * `NgIf` gives an element the **ability to conditionally render or be removed**, which is possibly the most common use of JavaScript on the web. 
5. **What are directives categories?**
    * There are **three categories** of directives: 
        * *attribute* directives, 
        * *structural* directives,
        * and *components*. 
    * But components are special because they’re the only type of directive with a template, and therefore I suggest thinking of them as their own type of entity.
6. **How Attribute Directives work?**
    * Attribute directives modify the appearance or behavior of an element. 
    * There are a number of built-in attribute directives, `NgClass` directive is one of them.
    * Typically, they work by *changing the various properties of the element* they are associated with, such as the `NgClass` directive **changing the list of classes attached**. 
    * Most directives are **attribute directives**.
7. **How Structural Attributes work?**
    * Structural directives modify the DOM tree based on some conditions.
    * We saw `NgIf` as a way to **conditionally display a DOM element**, and `NgFor` as a way to **iterate over a list of items** and display them. 
    * There are **fewer** of these types of directives built into Angular because they’re **versatile**. 
    * They work by **adding or removing DOM elements to or from the page**.
8. **How we used directives in stock app?**
    * We used three directives in stock app. 
        * `NgIf` was used to *hide the cards* list until the data had loaded. 
        * `NgFor` was used to *loop over each stock* and create N number of copies.
        * `NgClass` was used to *change the card background*, depending on the positive or negative change for the stock price. But we didn’t go into detail about some of the other directives.
        * Without directives in the stock application, we would have to write JavaScript that would dynamically create multiple summary cards, and that gets harder to manage over time.
9. **What are default directives provided by Angular?**
    * The primary default directives provided by Angular consist of the following (there are also some provided by the Forms and Router modules):
        * **NgClass** — Conditionally *apply a class* to an element.
        * **NgStyle** — Conditionally *apply a set of styles* to an element.
        * **NgIf** — Conditionally *insert or remove* an element from the DOM.
        * **NgFor** — *Iterate* over a collection of items.
        * **NgSwitch** — *Conditionally display* an item from a set of options.

## Pipes
1. **What are Pipes & How it works?**
    * Using pipes, we can **transform the data in the view** during rendering without changing the underlying data value.
    * Often you want to **display data in a different format** than the format it’s stored in.
    * Using a pipe changes the way the data is rendered, but it doesn’t change the value of the property. 
    * It creates a copy of the output, modifies it, and displays the resulting value.
of the property.
2. **How pipes are used?**
    * Angular comes with a set of **default pipes** that cover a number of common use cases.
    * Pipes are **added into template expressions** using the pipe character **(|)** and the **name of the pipe**.
    * The **default pipes** are always available and don’t need to be injected or imported, so we can use them in our templates.
3. **Explain pipes by Example**
    * For example, you could have an expression that looks like this:
        `{{user.registered_date | date:'shortDate'}}`
        * The expression on the left side remains the same, but the addition of the pipe on the right side then **applies a transformation to the value of the expression**. 
        * In this case, it will use the **Date pipe** to format the user’s registration date. 
        * It also **takes an option**; a colon **(:)** denotes that the following value is passed to the pipe as a configuration option. 
        * In this example, you’re passing a **configuration option** that formats the date according to the format of `'shortDate'`.
    * Anytime you’re doing any kind of formatting of data, you should consider whether it can be a pipe. You’ll likely only create a handful of pipes, because they tend to be reusable and easily shared.

## Services
1. **What are Services?**
    * Services are a way to reuse functional pieces of JavaScript logic across your application. 
    * Sometimes these services are a **gateway to access data**, and other times they’re more **like helper functions**, like custom sorting algorithms. 
    * Services are also the ideal place for data access, and component controllers aren’t.
    * Angular provides a number of services out of the box, and many third-party modules will also expose services. 
2. **Explain Services by Example**

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
let service: string = 'https://angular2-in-action-api.herokuapp.com';

@Injectable()

export class StocksService {

    constructor(private http: HttpClient) {}
        load(symbols) {
            if (symbols) {
                return this.http.get(service + '/stocks/snapshot?symbols=' + symbols.join());
            }
        }
    }
```
* First **HttpClient is imported** to the file and **then it’s injected** into the StocksService through the controller. 
* The `load()` method uses the `HttpClient` service to make a `GET` request, but it constructs the proper `URL` to *call*, making it easier to consume elsewhere in the application.
* Services are ideal for placing this type of logic that simplifies the usage of code and makes it **possible to reuse**.
* The principle of **separation of concerns** applies in Angular, and keeping individual entities focused on a single set of tasks is important for **maintainability and testability**.

## How Angular begins to render an app?

* In Angular, the CLI generates a fairly lightweight app. It could be slightly smaller, but it’s easiest to consider the generated app as the base for future development, and I’ll often refer to it as the `base app`. 
    * We’ll focus on the app that’s generated when you run `ng new app-name`.
    * Angular has an app **bootstrapping mechanism** that kicks off the rendering. 
    * Immediately upon loading the page, the **bootstrapper** is called to begin Angular execution. 
* **You may be asking yourself, how does the bootstrap begin?** 
    * The CLI uses **webpack** to build, and it **compiles** all the JavaScript and adds it as script tags to the bottom of the **index.html** on build. This is when it will run the code to begin your app.
    * Now that Angular has started, it **loads your App module** and reads through any additional dependencies that need to be **loaded and bootstrapped**. 
    * In the `base app`, the Browser **module is loaded** into the application **before execution** happens.
    * Then Angular **renders the App component**, which is the root element of your application. 
    * As this App component renders, any **child components are also rendered** as part of the component tree.
    * This is like the **DOM** tree, except any special Angular template syntax has to be rendered by Angular. 
    * As it renders, it will also **resolve bindings** and set up event listeners for anything that declares it. 
    * Once this has been completed, the full application should be **rendered out and available** for the user to begin interacting with. 
    * The **lifecycle** of the application **continues** as the user begins to use the application, and the application will begin to react. 
    * As a user **navigates around** (Routing), the components on the screen will be removed, and new components will get loaded and rendered. 
    * The cycle of reacting to the user and rendering the component tree continues until the application is closed.

## Types of compilers
* **What are types of Angular compilers?**
    * Angular provides two types of compilers 
        * Just-in-Time `JiT` compiler
        * Ahead-of-Time `AoT` compiler.
* **What are differences between these compilers?**
    * The primary difference is 
        * tooling and timing for the compiler, and 
        * that can change the way your application behaves and 
        * how it’s served to the user.
* **Explain the JiT compilation process!**
    * With JiT compilation, it means that the **compiling of the application happens in the browser *only after the assets* have all been loaded**.
    * That means there will be a **lag between initially loading the page and being able to see the content**.
    * You saw that in the chapter 2 example, because there is a fairly basic **“loading”** message displayed **until everything is ready** and the compiler has run.
* **Explain the AoT compilation process!**
    * AoT, is a way to render the content **before sending** it to the browser. 
    * This means the user will be sent exactly what’s needed to display the content **without any kind of loading message** once the application assets have loaded.
* **How really the two compilation processes differs?**
    * The other big difference is that with `JiT`, the application **must also load the compiler library before the application can execute**, whereas the AoT version is **able to drop this payload from being sent**, causing a faster load experience.
* **What are the benefits of AoT?**
    * With AoT, we have the ability to perform a number of interesting **optimizations**, because the application is **compiled before serving**. 
    * The other possibility it provides is **server-side rendering** for applications, which can be useful for pre-rendering applications with user-specific data.
* **Production Tip**
    * You should work to ensure your applications compile with the AoT compiler, because anytime you build your application for production it will use the AoT compiler. 
* **Release Note**
    * It is possible that in future releases of Angular, the `JiT` compiler may be removed entirely once AoT compilation is fast enough for development.
    * We’ll use `JiT` for all development in this book because it’s much faster to render and preview the application.

## Dependency injection
1. **What is the problem solved by DI?**
    * All but the most basic code **relies on using objects** from other parts of the application.
    * **The problem is** that the larger your code base becomes, the harder it is to ensure that individual parts are encapsulated while still being easy to access. 
    * Therefore, many programming languages or frameworks have some mechanism to **facilitate tracking and sharing objects**.
    * There are many approaches to **structuring your code** in a way that allows you to **easily share objects**. 
2. **What is Dependency injection?**
    * Dependency injection (DI) is a **pattern for obtaining objects** that uses a registry to maintain a list of available objects and a service that allows you to request the object you need. 
    * Rather than having to pass around objects, you can **ask for what you need** when you need it.
3. **How DI is different from using JavaScript module imports and exports?**
4. **Why do we need another method to pass code around when JavaScript now has modules?**
    * Dependency injection shouldn’t be confused with JavaScript module imports. 
    * There is a need for Angular to be able **to keep track** of what parts of the application need a particular service. 
    * **JavaScript has no awareness of how dependencies are linked together**, which can be useful information in understanding how to best assemble dependencies. 
    * Also, injecting a dependency with Angular will **resolve any additional dependencies**.
5. **How Dependency injection system is constructed?**
    * There are a few key pieces to the DI system. 
    * **The injector**. 
        * This is the **service** that Angular provides for **requesting and registering** dependencies. 
        * The injector is often at **work behind the scenes**, but occasionally is used directly. 
        * Most of the time, you’ll **invoke the injector** by **declaring a type annotation on the property**.
        * For Example we injected the `HttpClient` service like this:` constructor(private http: HttpClient) {}`
        * Because we declare the type as HttpClient, the application will use the injector to ensure that the http property contains an instance of the HttpClient service. 
        * This seems like magic, but it’s merely a way to alias the dependency you would like to request without directly calling the injector API.
    * **The Providers**. 
        * Providers are responsible for **creating the instance of the object requested**. 
        * The **injector** knows the list of available providers, and based on the name, **it calls a factory function** from the provider and **returns the requested object**.
        * Anything that has been registered with an NgModule’s providers array is available to be injected in your application code.
        * Providers **don’t have to be exposed to the root module** and instead can be made visible only to a particular component or component tree.
6. **How to Inject a Dependency?**
    * You can inject anywhere, but I prefer to use the TypeScript approach, as we saw earlier, where the **constructor properties are annotated with the specific type of service to inject**.
    * **Alternatively**, you could use the `@Inject` decorator to **inject the Http service**, like this: `constructor(private @Inject(HttpClient) http) {}`
        * This decorator [`@Inject`], wires up the dependency injection the same way as the TypeScript typing information. Either way you’ll get the same result.
## Change detection
1. **What is Change Detection?**
    * Change detection is the mechanism that allows components to be updated when data changes in a parent component, and ensure views and data are in sync.
    * Changes always come down **from the model into the view**, and Angular employs a **unidirectional propagation** of changes **from parents down to children**. **[Tree pushes data down to children and events bubble data up.]**
    * This helps **ensure** that **if a parent changes, any children are also checked**, due to potential linked data.
2. **Who is responsible for the CD mechanism?**
    * Angular will run a change detection process to check whether values have changed since the last time the process ran. 
    * JavaScript has no guaranteed way to notify about any change to an object, so Angular runs this process instead. 
3. **How Change Detection mechanism is constructed?**
    * Angular creates a *special class*, known as a **change detector**, when it renders a component. 
    * **This class is in charge of keeping track of the state of the component data and detecting whether any values have changed between the times the change detection ran.** 
    * **When a value change is detected in a component, it will update the component and potentially any child components as well.** 
4. **What are Angular modes for triggering the changes?**
    * Angular has **two ways** for changes to be triggered. 
    * The **Default mode** 
        * **Traverse the entire tree** looking for changes with each change detection process. 
    * The **OnPush mode** 
        * Tells Angular that the **component only cares about changes** to any values that are input into the component **from its parent**, and gives Angular the ability to **skip checking** the component during change detection **if** it already knows the **parent hasn’t changed**.
5. **How Change Detection is triggered?**
    * Change detection is triggered by 
        * Events, 
        * Receiving HTTP responses,
        * Timers/ intervals. 
    * The best way to think of it is that **anytime something asynchronous occurs, the change detection process begins** to determine what may have changed, because **synchronous calls are already handled** during the normal rendering flow of Angular.
    * **Think of it like this:** You can turn on your car, but until you put it in gear, push the pedal, or brake, the vehicle is in an idle state, waiting for the driver to give it something to do.