# Angular Fundamentals

## Components

1. What is Component?
    * A component encapsulates Data, HTML and Logic for a view.
    * Every application has at least one component(Called AppComponent).
2. What is Module?
    * A module is a container for a group of related components.
    * Every application ahs at least one Module(Called AppModule).
3. How to use components?
    * Create a component `ng g c component-name`
    * Add an element in an HTML MarkUp
    
## Templates
1. **How Interpolation works?**
    * Interpolation resolves a binding and displays the resulting value as a string in the page.
    * The binding works by taking an expression, evaluating it, and replacing the binding with the result.

## Directives
1. **What are the benefits of directives?**
    * Directives are a powerful tool to teach `HTML` elements new skills.
    * They take an existing element and apply new capabilities, such as making an image open up in a modal window.
    * Directives can take a normal element and give it additional capabilities that don’t exist naturally.
    
2. **Directive Example**
    * Here is one that adds a directive that will **render or remove the element** based on the value inside the attribute value (known as an expression):
    ```html
    <div *ngIf="!stocks">
    ```
    * The `*ngIf` is the directive, which is applied as an **attribute to the element**, and it will evaluate the value it’s assigned to. 
    * In this case, if the expression is **truthy**, it will **render** the element—**otherwise** it will **remove** it from the DOM. 
3. **What is `NgIf` role?**
    * `NgIf` gives an element the **ability to conditionally render or be removed**, which is possibly the most common use of JavaScript on the web. 

## Services 
1. Why you shouldn't couple components to specific endpoint?
    * First reason: This will make this component testing dependent on this endpoint which is not best practice.
    * Second reason: Everytime you need this service you will have to implement it again in a repetitve manner which is not best practice.
    * Third reason: A component shouldn't contain any logic rather than the presentation logic.
* How to create a Service?
    * You can generate new service using CLI command `ng g s service-name`.
* What is a service in a very basic way?
    * Basically it is an exported class without any decorators.
* What is meant by Dependency Injection?
    * 