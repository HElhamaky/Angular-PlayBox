## Angular  [ Platform, not a Framework ]

Angular comes with a leaner core library and makes additional features available as
separate packages that can be used as needed. It also has many tools that push it beyond
a simple framework, including the following:

* Dedicated CLI for application development, testing, and deployment
* Offline rendering capabilities on many back-end server platforms
* Desktop-, mobile-, and browser-based application execution environments
* Comprehensive UI component libraries, such as Material Design

### Angular CLI
> `npm install -g @angular/cli`

The CLI has a number of features that aid in the development of Angular apps. Here
are the primary features:
* Generates **new project** scaffolding
* Generates **new application pieces**
* Manages the entire **build toolchain**
* Serves a localhost **development server**
* Incorporates code **linting and formatting** code
* Supports running **unit and e2e** tests

### Server rendering and the compiler
The term server rendering is about the notion that it shouldn’t matter where you run
the JavaScript engine that executes Angular code. It should be possible to run Angular
universally, such as with browser JavaScript engines, NodeJS, or even less common
engines like Java’s Nashorn engine. This greatly increases the ways in which Angular
can be used.

Why does this matter? Let’s explore a few primary use cases:

* Server rendering for faster loading
* Performance in the browser
* SEO
* Multiple platforms

### Mobile and desktop capabilities

The mobile and desktop capabilities of Angular are extensions of the design of the
compiler. The following tools are all outside of Angular’s core but use the design of
Angular to power some powerful design patterns:

* Ionic (mobile)
* NativeScript (mobile)
* React Native (mobile, desktop)
* Windows Universal (desktop)
* Electron (desktop)
* Progressive Web Apps (mobile, desktop)

### UI libraries
* [Angular Material](https://github.com/angular/material2) , [Covalent](https://teradata.github.io/covalent)
. [Clarity](https://vmware.github.io/clarity)
, [ng-bootstrap](https://ng-bootstrap.github.io)
* [Kendo UI](https://www.telerik.com/kendo-angular-ui/)
, [PrimeNG](www.primefaces.org/primeng/)
, [Wijmo](http://wijmo.com/angular2/)
, [Ionic](http://ionic.io)
, [Fuel-UI](http://fuelinteractive.github.io/fuel-ui/)

## Component architecture
In many ways, a component is a way to create custom HTML elements in your
application.

HTML itself is a language of components. Each element has a certain **role** and **functionality**,
and they’re all easily **nested** to create more **complex functionality**. They’re
**isolated** but still easily **manipulated** to do whatever is needed at the moment. Some elements work in **tandem**. For example, INPUTs are used inside of a FORM to describe a
set of **input controls**. Many elements can also **emit events** when things happen; a FORM
can **emit** an event **when** the form is **submitted**, for example. This allows you to **wire up**
additional logic to **manipulate** HTML elements based on the events that **fire** the fundamentals
of front-end application development.

### Components’ key characteristics

Components have some concepts that drive their design and architecture. This section
will explore these concepts in more detail, but also keep an eye open for how Angular
applies these concepts to practice throughout the book:

* **Encapsulation** —Keeping component logic in a single place
* **Isolation** —Keeping component internals hidden from external actors
* **Reusability** —Allowing component reuse with minimal effort
* **Evented** —Emitting events during the lifecycle of the component
* **Customizable** —Making it possible to style and extend the component
* **Declarative** —Using a component with simple declarative markup

The World Wide Web Consortium (W3C), the primary standards body for the web, is
developing an official **Web Component specification**. Several standards are required in
order to implement the full vision of web components:

* **Custom elements** (encapsulation, declarative, reusability, evented)

    * It gives us a declarative way to create a reusable component, which encapsulates the internal mechanics of the component away from the rest of the application, but can emit events to enable other components to hook into the lifecycle.

* **Shadow DOM** (isolation, encapsulation, customizable)
    * Shadow DOM is an isolated Document Object Model (DOM) tree that’s detached from the typical CSS inheritance, allowing you to create a barrier between markup inside and outside of the Shadow DOM.
    * Shadow DOM allows us to write CSS and HTML that gets rendered without having the ability to modify other styles.
    * Shadow DOM enables **the best form of encapsulation available in the browser** for *styles* and *templates*. It’s able to **isolate** the internals of a component in such a way that outside styles and scripts won’t accidentally attach and modify it. It does provide some customization features that allow you to **communicate** across the shadow boundary.

* **Templates** (encapsulation, isolation)
    * Our custom elements need to have some kind of internal structure, and often we’ll need to be able to reuse this markup.
    * Templates are often used with the Shadow DOM because it allows you to define the template and then inject it into the shadow root. Without templates, the Shadow DOM APIs would require us to inject content node by node.
    * They provide a layer of encapsulation that lets you define a template that remains inactive until it’s needed and therefore isolates the template from the rest of the application.

* **JavaScript modules** (encapsulation, isolation, reusability)
    * Neither HTML nor JavaScript has traditionally had a **native means to load additional files or assets** during the lifecycle of the application.
    * Today we have *modules* and *module loaders* in JavaScript, which **give a native way** to load and execute code throughout the **entire lifecycle of the app**, not just on page load.
    * Inherently, **modules aren’t strictly a component technology**. Modules are an **isolated piece of JavaScript** that can be used to *generate a component*, *create a reusable service*, or *do anything* else JavaScript can do.
    * In JavaScript, a **module is any file of JavaScript code that contains the export keyword**.
    * These modules are powerful because  
        * They **encapsulate** the contents of a single JavaScript file into a single coherent whole. 
        * They **isolate** the code and allow the developer to conditionally export values to share. 
        * They also support **reusability** by defining common mechanics for sharing values in a JavaScript application.
    * Executing an Angular application is fundamentally **loading a module** that contains the **application bootstrapping logic**, which in turn starts to load and trigger additional modules.

## Modern JavaScript and Angular
Angular is designed to take advantage of many features that are fairly recent to the
web platform. Most of these became part of the JavaScript specification in 2015.

* Classes
    * Classes were introduced as a new way to **define an object**, which is in fact a **function**. 
    * Classes are used to create *components*, *directives*, *pipes*, and *services*.
    * Using the class keyword, the class MyComponent is created.
    * Classes are syntactic sugar for creating objects in JavaScript.
    * Inside of the class there’s a special method called **constructor()**.
        * It’s executed immediately when a new copy of the object is created.
        * As long as you name a method constructor(), it will be used during creation.
    * Classes are also useful because they help ensure that the keyword *this* **references the object itself**.
        * The keyword this is a common barrier in JavaScript, and classes help ensure that it behaves more consistently.
    

* Modules
    * The *export* keyword denotes the **file as a module**. 
    * Any module is **isolated** into a private space, and unless a value is **exported**, it won’t be available for another file or module to use. 
    * This breaks away from the **global scope** that JavaScript has for values and provides a proper **separation** between modules.
    
    
* Decorators [**@Component**]
    * Decorators is a way to add metadata to the class. 
    * Decorators always start with the **@** symbol, and Angular uses these decorators to understand what type of class has been declared. 
    * In this case, it’s a component, and Angular will know how to render a component based on this decorator. 
    * There are several other ones, such as **Injectable** and **Pipe**.
    * Decorator **accepts an object** that contains the metadata associated with the component itself. 
    

* Template literals

## Observables

* observables are a newer pattern for JavaScript applications to manage asynchronous activities.
* RxJS is the library we’ll use to help us implement observables in our applications.
* Promises are another construct to help deal with asynchronous calls, which are useful
for making API requests
    * Promises have a major **limitation** in that they’re only useful for **one call cycle**. 
    * For example, if you wanted to have a promise return a value on an event like a user click, that promise would resolve on the **first click**.
* If you are interested in **handling every user click** action. Normally, you’d use an **event listener** for this, and that allows you to **handle events over time**.
* This is an important distinction:
    * **Observables** are like event handlers in that they continue to process data over
    time and allow you to continuously **handle that stream of data**.
* Reactive programming is the higher-level name for what observables provide, which is a pattern for dealing with asynchronous data streams.
* Asynchronous Data Streams
    * A user typing keystrokes into a form input is really a stream of individual characters.
    * Timers and intervals generate a stream of activity over time. 
    * Websockets pass data as a stream over time.

### Using Observables
To use observables, you **subscribe** to the stream of data and **pass a function** that will run every time there’s a new piece of data.

```javascript
this.http.get('/api/user').subscribe(user => {
    // Do something with every user record
}, (error) => {
    // Handle the error
})
```
* This snippet is using the **HTTP library** to make a **get request**, which returns an **observable**.
* Then we **subscribe** to that observable, and our **callback function** fires when the
data is returned or the error is handled. 
* It’s not very different from a **promise**, except that an **observable could continue to send data**. 

#### Let’s take a different example:

```javascript
this.keyboardService.keypress().subscribe(key => {
    // Do something with every key record
}, (error) => {
    // Handle the error
})
```
* In this example, imagine **keyboardService.keypress()** returns an **observable**, and
it **emits details** about what **key was pressed**. This is *like* an **event listener**, *except* that it **comes in a stream**.


#### Another interesting capability of observables is that they are **composable into many combinations**. 
* Observables can be combined, flattened into one, filtered, and more so you can **combine two observable streams** and **handle** the data they emit **in one place**.

For More on Reactive Programming using Observables I recommend [RxJS in Action](www.manning.com/books/rxjs-in-action).

## TypeScript and Angular
TypeScript is a superset of JavaScript that introduces the ability to **enforce typing** information. It can be used with **any version of JavaScript**, so you can use it with anything **ES3** or newer.

* The basic value proposition of TypeScript is it can **force restrictions** on what types of
values variables hold. 
    * For example, a variable may **only hold a number** or it may **hold an array of strings**.
* JavaScript has types (don’t let anyone tell you otherwise!), but **variables
aren’t typed**, so you can store any type of value in any variable.
* This also gave birth to the many types of comparison operators, such as == for **loose equality** or === for **strict equality**.
* TypeScript can help catch many simple syntax errors before they affect your application.

Sometimes you can write valid JavaScript, but the real world shows that valid syntax
doesn’t always mean valid behavior. 

#### Take this example:

```javascript
var bill = 20;
var tip = document.getElementById('tip').value; // Contains '5'
console.log(bill + tip); // 205
```

* This snippet shows a simple tip calculator example where you take the value from an
input element and add it to the bill to get the total payment amount.
* The problem here is that the tip variable is actually a string (because it’s text input).
* Adding a number and a string together is perhaps one of the most common pitfalls for new JavaScript developers, though it can happen to anyone. 

#### If you used TypeScript to enforce types, this code could be written to alert about this common error >>

```javascript
var bill: number = 20;
var tip: number = document.getElementById('tip').value; // 5, error!
var total: number = bill + tip; // error!
```
* Here we’re using TypeScript to declare that all these variables must each hold a number
value by using **:number**. 
* This is a simple syntax that sits inside of JavaScript to tell TypeScript what type of value the variable should hold. 
* The tip value will **error because it’s being assigned a string**, and then the total value will error because it **attempts to add a number and a string type**, which results in a string.

#### Without TypeScript, you’re responsible for doing a strict comparator check of every value before it’s used.


### Primary reasons to use TypeScript
* Adds clarity to your code
* Enables a smarter editor [**automatic IntelliSense**]
* Catches errors before you run code
* Entirely optional

#### You can learn all what you need to know about [TypeScript](www.typescriptlang.org/ docs/tutorial.html).



## Summary
This chapter introduced you to Angular as a **development platform**, not just an application
framework. There are so many features and capabilities with Angular. Here’s a
quick summary:

* Angular is a **platform**, with many key elements such as *tooling*, *UI libraries*, and
*testing* built in or easily incorporated into your application projects.
* Applications are essentially **combinations of components**. These components
build upon the core principles of *encapsulation*, *isolation*, and *reusability*, which
should have events, be *customizable*, and be d*eclarative*.
* **ES6 and TypeScript** provide a lot of the underpinnings for Angular’s architecture
and syntax, making it a powerful framework without having to build a lot of custom
language capabilities.
















