## Angular  [a platform, not a framework]

Angular comes with a leaner core library and makes additional features available as
separate packages that can be used as needed. It also has many tools that push it beyond
a simple framework, including the following:

* Dedicated CLI for application development, testing, and deployment
* Offline rendering capabilities on many back-end server platforms
* Desktop-, mobile-, and browser-based application execution environments
* Comprehensive UI component libraries, such as Material Design

### Angular CLI
`npm install -g @angular/cli`

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
* [Angular Material](https://github.com/angular/material2)
* [Covalent](https://teradata.github.io/covalent)
* [Clarity](https://vmware.github.io/clarity)
* [ng-bootstrap](https://ng-bootstrap.github.io)
* [Kendo UI](https://www.telerik.com/kendo-angular-ui/)
* [PrimeNG](www.primefaces.org/primeng/)
* [Wijmo](http://wijmo.com/angular2/)
* [Ionic](http://ionic.io)
* [Fuel-UI](http://fuelinteractive.github.io/fuel-ui/)

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
    * Neither HTML nor JavaScript has traditionally had a **native means** to load additional files or assets during the lifecycle of the application.
    * Today we have modules and module loaders in JavaScript, which **give a native way** to load and execute code throughout the **entire lifecycle of the app**, not just on page load.
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
* Decorators
* Modules
* Template literals



