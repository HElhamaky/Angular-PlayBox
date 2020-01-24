# Components Basics
> Components are so central to how Angular applications are structured that almost every feature is somehow linked to them. It’s impossible to make an Angular application without a component, after all.

* **This chapter covers**
    * The basics of components and their role
    * The `@Component` decorator and its most important properties
    * Rendering a component
    * Passing data into and out of a component using inputs and outputs
    * Customizing components with templates and styling
    * Injecting content into a component using projection

## Composition and lifecycle of a component
>Components have a lifecycle that **begins with their initial instantiation**, and continues with their rendering **until they’re destroyed and removed from the application**.

* Before we can understand the lifecycle, we should look more closely at what goes into a component.
* The composition of components is important to master over time to create more complex and efficient Angular applications.
* Components have several distinct parts that are combined to create the resulting UI that the user can interact with.
* When we generate a component with the CLI, it creates a set of files that contain assets that are combined during rendering. Here’s a list of the primary things that compose a component:
    * **Component Metadata Decorator** —All components must be annotated with the `@Component()` decorator to properly register the component with Angular. The metadata contains numerous properties to help modify the way the component behaves or is rendered.
    * **Controller** —The controller is the class that is decorated with `@Component()`, and it contains all the properties and methods for the component. Most of the logic exists in the controller.
    * **Template** —A component isn’t a component without a template. The markup for a component defines the layout and content of the UI that a user can see, and the rendered version of the template will look at the values from the controller to bind any data.
* These three pieces must exist for any component to be valid. 

>Additionally, there are some optional capabilities that can ride alongside components to enhance them in certain situations.

* The **first two** are **concepts that inject values into the component**, and **the rest** are **concepts that modify the resulting component behavior*, appearance, or interaction with other components:**
    * **Providers and hosts** —Services can be injected directly into a component if they’re not already provided at the root module level. You also have some control over how these services are discovered and where they are made available.
    * **Inputs** —Components can accept data being passed to them using the component inputs, which make it possible for a parent component to bind data directly into a child component, which is a way to pass data down the component tree.
    * **Styles and encapsulation** —Optionally, components can include a set of CSS styles that are meant to apply only to the component. This provides a layer of encapsulation for the design of components, because component styles don’t have to be injected globally. Components can configure the way that styles are injected and encapsulated into the application.
    * **Animations** —Angular provides an animation library that makes it easy to style component transitions and animations that plug into the template, and can define keyframes or animation states to toggle between.
    * **Outputs** —Outputs are properties that are linked to events that can be used to listen for data changes or other events that a parent component might be interested in, and can also be used to share data up the component tree.
    * **Lifecycle hooks** —During the rendering and lifecycle of a component, you can use various hooks to trigger application logic to execute. For example, you can run initialization logic once during the instantiation of the component and tear down logic during the destruction. You can also use these hooks to bring data into the component, so lifecycle hooks work well with both inputs and outputs.
* There are a few more capabilities that aren’t covered here, but you can always find them in the documentation, and we will use them in situations that call for them.

## Component lifecycle
>Components have a lifecycle **from creation to removal**, and understanding that flow will help you design quality components. There can be slight variances in how a component’s lifecycle behaves depending on the build tooling used, but in most cases that tooling will be the Angular CLI.

* The **first** major action is that a component is registered with the App module. 
* This usually happens because we **declare a component** as part of the `NgModule` metadata and occurs **during application bootstrap**, *but components could also be registered dynamically on the fly later while the application is running*. 
* When the component is registered, **it creates a component factory class and stores it for later use**.
* Then, during the application lifecycle, *something will request the component*. 
    * This is typically because *the component was found in a template and the compiler needs the component*, but sometimes components are also requested manually. 
* At this point, an **instance of the component needs to be loaded**. 
* There is a **registry of components that belong to the module**, and Angular will look up the component in question and retrieve its component factory, which is **generated during the compilation using the CLI before the app is run**. 
* This **special class knows how to instantiate a new instance of the component**.
* As the **component is instantiated**, the **metadata is read and the constructor method is fired**. 
* Any **construction logic** will *run early in the component’s life*, and you should be careful **not to put anything that might depend on child components being available**, because *the template won’t have been parsed yet*.
* The **component metadata** will then be fully **processed** by Angular, including the parsing of the component template, styles, and bindings. 
* If the template contains any **child components**, those **will kick off the same lifecycle** for those components as well, but **they won’t block this component** from continuing to render.
* At this point, we’ve **initialized the component**, and a **cycle begins where the child components** become fully rendered, **application state changes**, and **components are updated**.
* During this cycle, lifecycle hooks will fire to alert you to important times when you’ll know it’s safe to do certain actions. 
    * **For example**, there’s a lifecycle hook that lets you know when any of the inputs have changed; another lets you know when all the child components have fully resolved (in case you have logic that depends on them to run).
* At some point, the component may no longer be needed in the application. 
* At that point, **Angular will destroy the component** (and all of its children). 
* Any new instances will need to be **recreated from the component factory class**, as we saw earlier.

## Lifecycle hooks
>During the application rendering process and reaction to user inputs, various hooks can be used to run code at various checkpoints. 

* These hooks are useful when you need to know that certain conditions are true before executing the code, such as ensuring that child components have been initialized, or when changes are detected.
* In chapter 2, we saw the `OnInit` lifecycle hook in action. It runs early in the cycle but after all bindings have been resolved, so it’s safer to know that all data is available for the component.
* Angular will only run a lifecycle hook if it’s defined in the component. 
* Lifecycle hooks aren’t like event listeners—they’re special methods with specific names that are called during the component’s lifecycle if they’re defined. 
* The lifecycle hooks are all named as you see in next table, but when you implement the hook in your controllers, you prefix it with ng as well. OnInit needs to be implemented as the `ngOnInit()` method.

|Lifecycle hook|Role|
|--------------|----|
|**OnChanges** | Fires any time the input bindings have changed. It will give you an object (SimpleChange) that includes the current and previous values so you can inspect what’s changed. This is most useful to read changes in binding values.
|**OnInit**| This runs once after the component has fully initialized (though not necessarily when all child components are ready), which is after the first OnChanges hook. This is the best place to do any initialization code, such as loading data from APIs.
|**OnDestroy**| Before a component is completely removed, the OnDestroy hook allows you to run some logic. This is most useful if you need to stop listening for incoming data or clear a timer.
|**DoCheck**| Any time that change detection runs to determine whether the application needs to be updated, the DoCheck lifecycle hook lets you implement your own type of change detection.
|**AfterContentInit**| When any content children have been fully initialized, this hook will allow you to do any initial work necessary to finish setting up the content children components, such as if you need to verify whether content passed in was valid or not.
|**AfterContentChecked**| Every time that Angular checks the content children, this can run so you can implement additional change detection logic.
|**AfterViewInit**| This hook lets you run logic after all View Children have been initially rendered. This lets you know when the whole component tree has fully initialized and can be manipulated.
|**AfterViewChecked**| When Angular checks the component view and any View Children have been checked, you can implement additional logic to determine whether changes occurred.

* The `OnInit`, `OnChanges`, and `OnDestroy` hooks are the most commonly used lifecycle hooks. 
* The `DoCheck`, `AfterContentChecked`, and `AfterViewChecked` hooks are most useful to keep track of logic that needs to run during any change detection process, and respond if necessary. 
* The `OnInit`, `AfterContentInit`, and `AfterViewInit` hooks are primarily useful to run logic during the component’s initial rendering to set it up, and each one ensures a different level of component integrity (such as it’s ready or if the child components are also ready).
* You may be wondering about the differences between a **Content Child** and **View Child**. 
* Let’s briefly talk about **how components are nested** and **how that impacts the rendering of a component**.

## Nesting components
>Because Angular is a tree of components, you’ll be nesting components inside one another. 

* There are two ways to nest components and they’re named differently based on how they’re rendered. Take a look again at the chapter example in figure 4.1, and you’ll see how components are nested inside one another to create a more complex interface.
* Most often, components are nested by being declared in the template of another component. 
* Any component that’s nested inside another’s template is called a View Child, so named because the template represents the view of the component and therefore is a child inside that view. 
* A View Child is declared inside the component template.
* Occasionally a component accepts content to be inserted into its template, and this is known as a Content Child, so named because these components are inserted as content inside the component rather than being directly declared in the template. 
* A Content Child is declared between the opening and closing tags when a component is used.
* Let’s take an example to be sure we can see the difference. Imagine we have a `UserProfile` component with the following template:

```html
<user-avatar [avatar]="avatar"></user-avatar>
<ng-content></ng-content>
```
* The first line is using a component, UserAvatar, which is a View Child. 
* Notice how it’s declared in the template of the component. 
* Then there’s this `NgContent` element— which I cover in more detail later, but suffice it to say it’s a place to render additional content. 
* Any component passed in through `NgContent` would be considered a **Content Child**. 
* When we use the `UserProfile` component, its use would look something like this:

```html
<user-profile [avatar]="user.avatar">
<user-details [user]="user"></user-details>
</user-profile>
```
* When we use the UserProfile component, we’re passing another component, UserDetails, into the component by declaring it between the opening and closing tags. 
* This is how a Content Child is passed into a component and then is put where the `NgContent` element sits in the UserProfile component.
* In our example here, we had two nested components, `UserAvatar` as a View Child and `UserDetails` as a Content Child. 
* The distinction is where they’re declared and has nothing to do with the component design themselves.
* Generally, the code of a single component should focus on its own business and not have to worry a lot about child components (of either type). 
* But there are some use cases where you’ll build a component that needs to distinguish between these types of components and its children. 
* The concern is always making components too coupled, but sometimes it’s unavoidable or even desirable (such as a Tabs and Tab component working together to make a tabbed interface), so this distinction can be important.
* Now that we’ve covered a lot of the high-level concepts behind components, we can get back to our example. 
* The next step is to create our second component and get some data generated to display.

## Types of components
>Fundamentally there’s only one type of component, but I like to think about components
as four categories, based on their roles. 

* All components are declared and function in fundamentally the same way, but they can be instantiated differently, may or may not contain state, or have coupling to other aspects of the application that make them distinct from other components.
* I think these classifications are useful to describe the roles of components and give general guidance about how they should be designed. 
* This shouldn’t be considered a rigid set of rules to follow—you’ll certainly build components that don’t fit perfectly into these guidelines, and that’s perfectly acceptable. 
* Keep these ideas in the back of your mind, and I believe they will be helpful. Here are the four roles of components, and the names I’ve given them:
    * **App component** —This is the root app component, and you only get one of these per application.
    * **Display component** —This is a stateless component that reflects the values passed into it, making it highly reusable.
    * **Data component** —This is a component that helps get data into the application by loading it from external sources.
    * **Route component** —When using the router, each route will render a component, and this makes the component intrinsically linked to the route.
* Angular doesn’t provide a standard nomenclature like this for various roles of components, so you won’t be able to go searching for related content based on these names.
* The real value is in understanding that it’s typically best to give your components specific roles instead of trying to make them do too many things. Let’s take a closer look at these different roles and why they’re designated as separate groups.