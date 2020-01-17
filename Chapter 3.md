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

## How Angular begins to render an app