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

### App component
>The App component is a special case component. As you know, every Angular application starts by rendering a component, and if you used the CLI it will be the **AppComponent** (but could be named anything if you changed it).

* Here are the guidelines I recommend for your App component:
    * **Keep it simple** —If possible, don’t put any logic into the component. Think of it more like a container. It’s easier to reuse and optimize the rest of your components if the App component doesn’t have complex behaviors they depend upon.
    * **Use for application layout scaffolding** —The template is the primary part of the component, and you’ll see later in this chapter how we create the primary application layout in this component.
    * **Avoid loading data** —Usually you will avoid loading data in this component, because I like to load data closer to the component that uses that data. You might load some global data (perhaps something like a user session), though that could also be done separately. On less complex applications, you might load data because it’s more complicated to abstract it on smaller applications.
* In my opinion, the best rule is to keep the App component as simple as possible. Typically, I have only a template and an empty controller. 
* The intention is to avoid doing too much “global”-type logic and configuration in the app controller to increase the modularity of your other components and keep the logic inside components that need it.

### Display component
>The Display component role is likely to be the most common one that you create or consume as you build your Angular expertise. These are components that generally are useful for rendering out content and are typically given the necessary data to display.

* Most third-party components will be in this role because it’s the most decoupled type of component.
* Here are the primary guidelines I suggest for a Display component:
    * **Decouple** —Ensure that the component has no real coupling to other components, except that data may be passed into it as an input when requested.
    * **Make it only as flexible as necessary** —Avoid making these components overly complex and adding a lot of configuration and options out of the box. Over time, you might enhance them, but I find it’s best to start simple and add later.
    * **Don’t load data** —Always accept data through an input binding instead of loading data dynamically through HTTP or through a service.
    * **Have a clean API** —Accept input bindings to obtain data into the component and emit events for any actions that need to be pushed back up to other components.
    * **Optionally use a service for configuration** —Sometimes you may need to provide configuration defaults, and instead of having to declare the preferences with every use of the component, you can use a service that sets application defaults.
Sometimes it may feel like overkill to make your components more isolated and specific
for displaying output. The more encapsulated and isolated the component is, the
easier it will be to reuse.
I often find that when I start to refactor some code, I begin by identifying individual
aspects of my code that could be standalone display components. I might notice a lot of
repeated snippets of code that mostly have the same capabilities and refactor them into
a single component.
It’s also common for these components to have a template and little to no logic in
the controller. That’s perfectly acceptable, because it allows you to easily reuse a template
snippet across your application.

### Data component
>The Data component oversees loading or managing data. Most applications need data from some external source (HTTP or user input), and it’s best to contain that inside a Data component versus a Display component. 

* I find that most developers build these first and eventually start to abstract out pieces into either route or display component types.
* Data components are primarily about handling, retrieving, or accepting data. Typically, they rely on a service to handle the loading of data, as we saw in chapter 2. 
* Here are some considerations for a Data component:
    * **Use appropriate lifecycle hooks** —To do the initial data loading, always leverage the best lifecycle hook for when to trigger the loading or persistence of data. We’ll look at this more later in this chapter.
    * **Don’t worry about reusability** —These components are not likely to be reused because they have a special role to manage data, which is difficult to decouple.
    * **Set up display components** —Think about how this component can load data needed by other display components and handle any data from user interactions.
    * **Isolate business logic inside** —This can be a great place to store your application business logic, because anytime you manage data, you’re likely dealing with a specialized implementation that works for a specific use case.
* I try to limit the number of components that deal with data so I can avoid making too many specialized components. 
* Each application is different, and perhaps you also leverage a nice UI library and don’t need to make many of your own display components, so it’s possible that the code you write for an application may be weighted toward data components (while the overall application will still have a lot of display components provided by the third-party module).

### Route component 
>The Route component is any component that’s linked directly to a route. These componentsaren’t very reusable and are typically created specifically for a specific route in an application.

* These components also often follow the principles of a Data component because routes often require loading more data for the new view. 
* The reason I distinguish between them is that a single route could render out multiple data components, such as in a dashboard that has several components loading metric information.
* A route component should primarily follow these guidelines:
    * **Template scaffolding for the route** —The route will render this component, so this is the most logical place to put the template that’s associated with the route.
    * **Load data or rely on data components** —Depending on the complexity of your route, the route component may load data for the route or rely on one or more data components to do that for it. If you’re unsure, I’d suggest loading data initially in the Route component and decoupling as your view gets more complex.
    * **Handles route parameters** —As you navigate, there are likely to be router parameters (such as the ID of the content item being viewed), and this is the best place to handle those parameters, which often determine what content to load from the back end.
* Every route that you can navigate to is linked to a component, so the number of route components you create is directly linked to the number of routes you add into your application.
* You could route to the same component for different routes, but that isn’t common.

## Creating a Data component
>We’re going to start by making a component that will help us manage data. This is a
dashboard for a data center, so the data it provides is largely numeric values of various
metrics that are important to determine the health of the data center. 

* We’ll create a component, aptly named the Dashboard component, which will host our data and display it in the app.
* We’ll have the raw data print to the screen for the moment until we create other components.
* At the end of this section your app should look like figure 4.6.
* Start by generating a new component using the CLI. 
* Then we’ll add the logic into the controller and see a few lifecycle hooks in action. This will be a good example of a Data component because it will handle the data for the entire application and not deal too much with the display of content: `ng generate component dashboard`
* Now open the **src/app/dashboard/dashboard.component.ts** file and replace its contents with what you see in the following listing.

```typescript
import { Component, OnInit, OnDestroy } from '@angular/core';
//Declares the interfaces for a Metric and Node data type
interface Metric {
    used: number,
    available: number
    };

interface Node {
    name: string,
    cpu: Metric,
    mem: Metric
    };

@Component({
    selector: 'app-dashboard',
    templateUrl: './dashboard.component.html',
    styleUrls: ['./dashboard.component.css']
    })

//Creates controller class; declare must implement OnInit and OnDestroy
export class DashboardComponent implements OnInit, OnDestroy {
    //Defines properties and grants them types
    cpu: Metric;
    mem: Metric;
    cluster1: Node[];
    cluster2: Node[];
    interval: any;
                                                              
    //Implements the NgOnInit lifecycle hook to initialize the data, and sets up interval    
    ngOnInit(): void {
        this.generateData();
                      
        this.interval = setInterval(() => { 
            this.generateData();
        }, 15000);
    }

    //Clears the interval using NgOnDestroy to free up memory
    ngOnDestroy(): void {
    clearInterval(this.interval);
    }

    generateData(): void {
        this.cluster1 = [];
        this.cluster2 = [];
        this.cpu = { used: 0, available: 0 };
        this.mem = { used: 0, available: 0 };
        for (let i = 1; i < 4; i++) this.cluster1.push(this.randomNode(i));
        for (let i = 4; i < 7; i++) this.cluster2.push(this.randomNode(i));
    }

    private randomNode(i): Node {
        let node = {
            name: 'node' + i,
            cpu: { available: 16, used: this.randomInteger(0, 16) },
            mem: { available: 48, used: this.randomInteger(0, 48) }
        };
        this.cpu.used += node.cpu.used;
        this.cpu.available += node.cpu.available;
        this.mem.used += node.mem.used;
        this.mem.available += node.mem.available;
        return node;
    }

    private randomInteger(min: number = 0, max: number = 100): number {
    return Math.floor(Math.random() * max) + 1;
    }
}

```
* We start by importing the OnInit and OnDestroy interfaces, which we use when we create our controller to add better intelligence to the TypeScript compiler about what constitutes a valid controller implementation. In this case, OnInit and OnDestroy are interfaces that tell TypeScript that it must implement the ngOnInit and ngOnDestroy methods, respectively.
* For additional clarity, the Metric and Node interfaces describe the structures we’re using for our data. It’s optional, but I recommend leveraging interfaces for proper enforcement of code and because interfaces help over time with maintaining code.
* The component has five properties, four of which contain values for our dashboard and the last of which is used to maintain a reference to the interval so it can be cleared later.
* Then we use the NgOnInit lifecycle hook to generate the data, and also set up the interval that will generate a new data set every 15 seconds. The NgOnDestroy lifecycle hook is also used then to clear the interval when the Dashboard component is removed from the application. Anytime you use an interval, you’ll want to be sure to clear the interval if you no longer need it, or you’ll create a memory leak over time because the interval would continue to exist even after navigating to another page.
* The rest of the code is a set of methods used to generate the data, which I won’t go over in detail. It will generate memory and CPU metrics for six nodes across two clusters and aggregate the total utilization metrics as well. How this data is leveraged will be more obvious as we build the next few components.
* Now in the component template, which is found in the file **src/app/dashboard/dashboard.component.html**, let’s bind the raw data to the screen so we can see it as it’s generated. 
* Replace the contents of that file with this single interpolation binding that will bind the data for one of the clusters:
```html
<p>{{cluster1 | json}}</p>
```
* We haven’t added the dashboard to our application yet, so open up **src/app/app.component.html** and add the dashboard to the bottom of the template like so. It should look like what you saw in figure 4.3 earlier in this section:
```html
<app-navbar></app-navbar>
<app-dashboard></app-dashboard>
```
* That’s it for the Dashboard component—for the moment. We’ll add more to the Dashboard later in this chapter as we build more components to handle the displaying of the data, starting with the next component, which will display the CPU and memory metrics and will accept data through an input.

## Using inputs with components 
> When you create your own components, you can define properties that can accept input through property bindings. Any default HTML element properties can also be bound to a component, but you can define additional properties that the component can use to manage its lifecycle or display.

* We saw this in chapter 2, and the following is a snippet we used to bind stock data into the Summary component. You see the brackets around the stock attribute to indicate that the value is binding to the data found in the stock property:
```html
<summary [stock]="stock"></summary>
```
* By default, all properties of a component aren’t immediately bind-able, as you saw earlier.
* They must be declared as an input to allow the property to be bound. One reason is that we want encapsulation of our components, and it’s best not to automatically expose all properties. Another reason is that Angular needs to know which properties exist so it can properly handle the wiring of values into a component.
* In our application, we want to display some metric data of the entire data center.
* This is shown in figure 4.7, which is the result of the work we’ll do in this section. 
* The role of these components is to display the CPU and Memory metric information of the entire data center. 
* Because the only difference between these components is the data, we’ll make it reusable, using inputs to pass data into the component.

### Input basics
>Let’s build our component that has an input and see it in action before we dig into it further. Use the CLI to generate a new component like you see here: `ng generate component metric`

* We’ll start by looking at the component controller, so open the **src/app/metric/metric.component.ts** file and update it with what you see in the following listing.

```typescript
import { Component, Input } from '@angular/core';

@Component({
    selector: 'app-metric',
    templateUrl: './metric.component.html',
    styleUrls: ['./metric.component.css']
})

export class MetricComponent {
    @Input() title: string = '';
    @Input() description: string = '';
    @Input('used') value: number = 0;
    @Input('available') max: number = 100;
    
    isDanger() {
        return this.value / this.max > 0.7;
            }
}
```
* The Metric component first imports the Input decorator, which is used to decorate any property that you want to define as an input. In this case, we have four inputs declared, which means that all the properties of this component are made available for binding.
* Then we implement a simple method called isDanger() that will tell us whether the utilization is above 70% or not, so we can display the metric in a different way. 
* The first two properties are the title and description. The Input decorator sits in front and will make each of the properties available for binding based on the name of the property. 
* Though I’ve declared a typing for each of the properties, it’s important to note that they don’t verify that the input matches that type at runtime. You’ll still need to sanitize the inputs or handle invalid values.
* The last two properties pass an optional value into the Input decorator, which is a way to change the name of the property that’s used in the binding from the name used internally for the component. That allows us to alias the used attribute to the value property, and the available attribute to the max property. 
* When you bind to the Metric component, you’ll use the binding like [used]="node.used", but inside the component it will use the property value.
* You may hear some refer to the component API, and what they’re talking about is the component inputs. Because this is the primary way for passing data into a component, it’s much like a contract for how to consume the component in your application. 
* Being able to rename the input can be beneficial for code clarity because it allows you to expose bindings differently from the internal implementation. But I recommend making the names consistent for simplicity and debugging.
* I also recommend that you ensure that your input bindings and property names are as clear as possible. Making your property names short to save characters is rarely worth it, because developers no longer know what that property is about without digging deeper into the code.
* As you build more components and work on larger projects, you’ll likely start to create components that are reused across projects and applications. This is a core principle of Angular as well—to enable easy reuse of components.
* Now let’s set up the Metric component template, and we’ll even use the ng-bootstrap Progress Bar component as an example of how to consume another component that has inputs. 
* Open the **src/app/metric/metric.component.html** file and update it to the contents of the following listing.

```html
<div class="card card-block">
    <div class="card-body">
        <nav class="navbar navbar-dark bg-primary mb-1" [ngClass]="{'bg-danger':
        isDanger(), 'bg-success': !isDanger()}">
            <h1 class="navbar-brand mb-0">{{title}}</h1>
        </nav>
    
        <h4 class="card-title">{{value}}/{{max}} ({{value / max |
        percent:'1.0-2'}})
        </h4>
    
        <p class="card-text">
        {{description}}
        </p>
    
        <ngb-progressbar [value]="value" [max]="max" [type]="isDanger() ?
        'danger' : 'success'">
        </ngb-progressbar>
    
    </div>
</div>
```
* This template uses a lot of the template syntax capabilities of Angular in a short amount of space. First, we’re using the bootstrap CSS styling to create a card layout for the Metric component, so the card classes are from bootstrap. 
* All the bindings in this template will resolve to the four input properties that were defined in the controller, but notice that we’re using the property names and not any alias names passed to the Input.
* The nav element is used to create the top header bar that contains the title. 
* Using NgClass, it will either apply the bg-danger or bg-success class based on how much of the overall value has been utilized.
* There are a few basic interpolation bindings, such as the title, value, max, and description. We also have an example of a more complex interpolation expression that divides the value by max and formats it as a percentage.
* Finally, we have a sample of using the ng-bootstrap Progress Bar component. Based on the documentation for the progress bar, it accepts the three bindings we declared to give it the bindings for value, max, and type. value and max are numbers, and type is the bootstrap class (such as danger or success) to use for coloring the bar.
* The Progress Bar is a great example of a well-designed component. It has a clear set of input properties. It doesn’t require a lot of effort to consume and internalizes much of the logic.
* Because we added the ng-bootstrap module to our App module earlier, all the components provided by ng-bootstrap are available without having to make any special requests for them. 
* Most third-party library experiences will be similar, and once you’ve added the third-party module to your app, your application can easily consume the values made available by the third party.
* Let’s get this Metric component on the screen. Open up the **src/app/dashboard/dashboard.component.html** file and replace its contents with the code in the following listing. 
* Remember, the Dashboard component contains the data that’s now able to be bound into the component.

```html
<div class="container mt-2">
    <div class="card card-block">
        <nav class="navbar navbar-dark bg-inverse mb-1">
        <h1 class="navbar-brand mb-0">Overall Metrics</h1>
        </nav>
    
        <div class="row">
            <app-metric class="col-sm-6"
            [used]="cpu.used"
            [available]="cpu.available"
            [title]="'CPU'"
            [description]="'utilization of CPU cores'">
            </app-metric>
    
            <app-metric class="col-sm-6"
            [used]="mem.used"
            [available]="mem.available"
            [title]="'Memory'"
            [description]="'utilization of memory in GB'">
            </app-metric>
        </div>
    </div>
</div>
```
* Here we define a bit of markup for the bootstrap styling to create a new container and card block for display purposes. The really meaningful part is the use of the Metric component. Notice we’re still able to apply a class to the element because it’s treated like a regular HTML element when rendered.
* Each of the four properties we defined in our component is listed as an attribute with the binding syntax of brackets around it. Remember, this is the syntax to tell Angular to evaluate the expression against the parent component (the Dashboard component) and return its value to the child component (the Metric component). 
* Because they were marked as inputs, we can bind values to those properties that aren’t standard HTML properties. 
* For the used and available properties, we bind to the corresponding properties on the cpu and mem controller values 
* Then for the title and description, we provide a string literal, because they’re evaluated as an expression.
* Did you notice how this component is more generic? It accepts the four inputs that it needs to display, which even includes some text. We can easily use the same component more than once and have it display for each metric uniquely. This is usually preferred over making a lot of components that do nearly the same task.

### Intercepting inputs
>We’re currently blindly accepting whatever values are passed into the component and using them without any kind of validation. 

* It’s easy to forget to validate or sanitize data inputs, especially when you’re building the component tree and are confident about the types of data that are being passed along.
* The problem is as applications grow or as components get reused in new ways, it becomes harder to keep track of the inputs. 
* It’s also good practice to try and validate inputs when possible to harden your components.
* For instance, if we changed the value of a binding to a Metric component to give it the wrong type, the Metric component would have some issues. Try it out yourself.
* Change `[used]="cpu.used"` to `[used]="'fail'"`. This changes the binding to pass a string instead of a number. The Metric component will throw an error because eventually it will try to divide the string and a number, which isn’t valid.
* I like to use a method to intercept inputs that need validation by using getter and setter methods. This is a feature that has been in JavaScript for a while, but I haven’t seen it widely used until more recently.
* The main idea here is that instead of having a property to bind to directly, you bind the input to the setter method, which stores the real value on a private property so you can protect it from direct access. The setter method is also where you can run any validation logic. Then you use the getter method to return the value of the private property anytime it’s requested.
* Let’s modify our Metric component to use this approach to validate input values and protect our template from division errors with values that aren’t numbers. Open the **src/app/metric/metric.component.ts** file and change the class to what you see in the following listing.

```typescript
export class MetricComponent {

@Input() title: string;
@Input() description: string;

private _value: number = 0;
private _max: number = 100;

@Input('used')

set value(value: number) {
    if (isNaN(value)) value = 0;
    this._value = value;
    }

get value(): number { return this._value; }

@Input('available')

set max(max: number) {
if (isNaN(max)) max = 100;
this._max = max;
}

get max(): number { return this._max; }
    isDanger() {
    return this.value / this.max > 0.7;
    }
}
```
* If you haven’t used a getter or setter method before, they’re functions that are proceeded by the get or set keyword. 
* The name of the method has to match the name of the property, so in this example get value(){} will be called anytime something requests the this.value property. 
* Likewise, anytime something stores a new value to this.value, it will call the setvalue(value){} method and pass the value that was stored.
* We started by creating two new private properties, _value and _max. With the private keyword, TypeScript will ensure they’re not directly exposed in the controller so they can’t be mutated outside of this controller. 
* The underscore before the name is a common convention to notify developers that a property is considered private.
* We implement the value setter method and also decorate it with the Input decorator.
* This registers a normal input property, but, instead, when the binding into the component happens, it will pass through this function. 
* The function does a check to ensure that the value is a number—if not, it sets it to 0 and stores that value on the _value property. Then we implement the getter method to retrieve the _value property.
* We do the same basic thing for the max property, but set the default to 100 if the input is invalid.
* We’ve now guarded our inputs against invalid values, but there are other scenarios where you may want to intercept values. 
* You may want to format some values before they’re used, such as ensuring that words are capitalized.
* The main problem with this approach is that every time you read the property using a getter method, it will run the logic inside the getter method. 
* This probably isn’t a big deal in most cases, such as if you’re only returning a private property as we’re doing here, but if you have more complex logic, the functions might take a toll on rendering speed. 
* Also, don’t do any mutation in the getter function! Doing so will cause unexpected behaviors because you can’t be sure how many times the getter function will be called.
* There’s one more way that we’ll intercept and modify inputs as they come into the component, but we’ll first take a look at how to project content inside your components.

## Content projection
>Imagine that you created a Card component and you wanted the card content area to be flexible to accept any kind of markup that a developer needed to insert.

* We can accomplish this using content projection, which allows us to declare the place to insert external content into our component.
* Content projection is a concept Angular implements that comes from web components.
* If we think about the role of display components, the need to accept markup to display inside the component is fairly common. Tabs, cards, navbars, modals, dialogs, sidebars—the list goes on for types of UI elements that could accept a generic set of markups to display within the component.
* Because we want to create reusable display components, content projection is a key capability that we’ll need to use. The good news is that it’s fairly simple to implement, so let’s go ahead and see it in action.
* We’re going to build two components that help us create a table to display each of the nodes in a cluster of servers. If you think about it, a table already uses content projection because you create a table and then nest rows inside the headers, and then inside the rows you insert the cells. See figure 4.8.
* Using the CLI, generate two new components, one for the Nodes component and another for the Nodes Row component:
`ng generate component nodes`
`ng generate component nodes-row`

* We’ll start by getting the Nodes component up and running, and we’re only going to start with the template. 
* The controller is empty, and for this use case we don’t need any controller logic to use content projection.
* Open **src/app/nodes/nodes.component.html** and replace its contents with the following listing.

```html
<thead>
    <tr>
        <th>Node</th>
        <th [colSpan]="2">CPU</th>
        <th [colSpan]="2">Memory</th>
        <th>Details</th>
    </tr>
</thead>

<ng-content></ng-content>
```
* You can see this template has the markup for a table header. I want to point out the use of property binding to a non-standard name; in this case the attribute is colspan, but the property on the element is colSpan. 
* The only benefit to using the binding here is that normally you need to bind to an expression instead of a static value, so you wouldn’t do this.
* The really interesting aspect is the `NgContent` element, which tells Angular that this component has an insertion point for content. When someone uses this element and they nest additional markup inside the element, it will be placed where the NgContent element currently sits. That allows you to choose exactly where the content is inserted (hence the name insertion point).
* We’ll get to see how this works shortly, but we want to make one small change to the component decorator. * Open the **src/app/nodes/nodes.component.ts** file and change the selector to the following: `selector: '[app-nodes]'`, We’ve changed the selector here to say that it will look for the attribute app-nodes instead of an element called app-nodes. The selector can take any valid CSS selector.
* In this case, we use the attribute CSS selector to target an element with the app-nodes attribute. 
* We do this so we can apply this component onto another element.
* The Nodes component creates our table header, and now we need to create a component that handles displaying the individual rows of content. 
* Open the **src/app/nodesrow/nodes-row.component.html** file and replace its content with the code found in the following listing.

```html
<th scope="row">{{node.name}}</th>
    
    <td [class.table-danger]="isDanger('cpu')">
    {{node.cpu.used}}/{{node.cpu.available}}
    </td>

    <td [class.table-danger]="isDanger('cpu')">
    ({{node.cpu.used / node.cpu.available | percent}})
    </td>

    <td [class.table-danger]="isDanger('mem')">
    {{node.mem.used}}/{{node.mem.available}}
    </td>

    <td [class.table-danger]="isDanger('mem')">
    ({{node.mem.used / node.mem.available | percent}})
    </td>

<td><button class="btn btn-secondary">View</button></td>
```
* The template consists of a set of table cells that display various data. The data cells usethe special class binding to conditionally apply the table-danger CSS class to a cell if the value is over the 70% threshold. 
* It also contains a binding that divides the values to produce a percentage of utilization, and uses the Percent pipe to format the value.
* Now hopefully you’re thinking that the component must receive the node object via an input because, as we already discussed, the Dashboard component holds all the data.
* For that to work, we need to set up our component controller properly and also implement the isDanger() method that is called in this template. To do that, open the src/app/nodes-row/nodes-row.component.ts file and replace its contents with what you see in the following listing.

```typescript
import { Component, Input } from '@angular/core';

@Component({
selector: '[app-nodes-row]',
templateUrl: './nodes-row.component.html',
styleUrls: ['./nodes-row.component.css']
})

export class NodesRowComponent {

@Input() node: any;
    isDanger(prop) {
    return this.node[prop].used / this.node[prop].available > 0.7;
    }
}

```
* We want to use the attribute selector for this component as well, so update the selector accordingly. Then we set up the node property as an input so that we can bind values into this component, and implement the `isDanger()` method to calculate whether the usage exceeds the 70% threshold we’ve set.
* This component doesn’t have any content insertion, because it only describes a table row. 
* But because we’ll use it more than once, it was prudent to abstract it into its own component. 
* This makes it a perfect example of a Display component that modifies its own display based on the data that’s provided as an input.
* Now we can see these two components in action. Open the **src/app/dashboard/dashboard.component.html** file and add the code in the following listing to the bottom of the template (don’t remove anything—add it).

```html
<div class="container mt-2">
    <div class="card card-block">
        <div class="card-body">
    
        <nav class="navbar navbar-dark bg-inverse mb-1">
        <h1 class="navbar-brand mb-0">Cluster 1</h1>
        </nav>
        
        <table app-nodes class="table table-hover">
        <tr app-nodes-row *ngFor="let node of cluster1" [node]="node"></tr>
        </table>
        
        <nav class="navbar navbar-dark bg-inverse mb-1">
        <h1 class="navbar-brand mb-0">Cluster 2</h1>
        </nav>
        
        <table app-nodes class="table table-hover">
        <tr app-nodes-row *ngFor="let node of cluster2" [node]="node"></tr>
        </table>
        </div>
    </div>
</div>
```
* The dashboard now implements another section that contains the two cluster tables.
* Because we used the attribute selector for the Nodes component on the table element, it will apply the component template inside the table. That will insert the table head elements that are in the Nodes component template, but it also contains the `NgContent` insertion point.
* The table has a child element, the table row element, and the Nodes Row component is applied to that row. It also has ngFor to loop over all the nodes in a given cluster, so there will be three instances of it created per table. 
* Finally, the table row also has a binding to capture the specific node value. The table rows show several distinct Angular capabilities working together to easily iterate over a list and display a table row that is abstracted into a component.
* We’ve injected content into one place, but what if we want to have multiple insertion points? We can do that too by naming our insertion points. To demonstrate, let’s replace the way we bind the title and description of the Metric component with child elements.
* Open **src/app/metric/metric.component.html** and update it to reflect the code in the following listing. We can use multiple NgContent elements by adding an attribute that has a CSS selector to use for targeting.

```html
<div class="card card-block">
    <div class="card-body">
        <nav class="navbar navbar-dark bg-primary mb-1" [ngClass]="{'bg-danger':isDanger(), 'bg-success':!isDanger()}">
        <h1 class="navbar-brand mb-0">
            <ng-content select="metric-title"></ngcontent>
        </h1>
        </nav>
    <h4 class="card-title">{{value}}/{{max}} ({{value / max |percent:'1.0-2'}})</h4>
    
    <p class="card-text">
    <ng-content select="metric-description"></ng-content>
    </p>
    
    <ngb-progressbar [value]="value" [max]="max" [type]="isDanger() ? 'danger' : 'success'"></ngb-progressbar>
    
    </div>
</div>
```
* We’ve replaced the interpolation bindings with an NgContent element, and in both cases it has a select attribute. This is a CSS selector that Angular will look for while rendering to determine what content to insert where. In this case, we’re expecting to have two elements, metric-title and metric-description.
* That means the Metric component needs to have two child elements by those names to properly display that content, but if it’s missing, it will be blank. You could use other CSS selectors, and it would locate the elements based on those selectors, such as a class name or attribute.
* We should also remove the title and description inputs from the component, so open **src/app/metric/metric.component.ts** and remove these two lines:
```typescript
@Input() title: string;
@Input() description: string;
```
* Now we need to update our Dashboard component to use these new elements instead of binding directly to properties, because it will throw errors after removing them from the Metric component. 
* Open **src/app/dashboard/dashboard.component.html** and modify the section, as you see in the following listing.

```html
<div class="container mt-2">
    <div class="row">
        <app-metric class="col-sm-6" [used]="cpu.used" [available]="cpu.available">
            <metric-title>CPU</metric-title>
            <metric-description>utilization of CPU cores</metric-description>
        </app-metric>
        <app-metric class="col-sm-6" [used]="mem.used" [available]="mem.available">
            <metric-title>Memory</metric-title>
            <metric-description>utilization of memory in GB</metric-description>
        </app-metric>
    </div>
</div>
```
* As you can see here, we’ve replaced the title and description bindings with custom elements by the names we declared in our NgContent select attribute. The resulting UI appears the same, but it does put the elements in the insertion points as they are, so you could have nested more markup inside of the elements.
* Now if you run the code, you’ll get a compilation error from Angular. It will try to parse these new elements, recognize that they’re not registered components, and throw an error. 
* We can fix that by setting some configuration in the App module that tells Angular not to get upset about finding an element it doesn’t understand.
* Open the **src/app/app.module.ts** file and make two small changes. The first is to import the **NO_ERRORS_SCHEMA** object from **@angular/core**; 

    `import { NgModule, NO_ERRORS_SCHEMA } from '@angular/core';`
* Then you’ll add a new property to the `NgModule` definition called schemas. 
* Add it after the bootstrap property, as you see here:
```typescript
bootstrap: [AppComponent],
schemas: [NO_ERRORS_SCHEMA]
})
```
* Angular will now disable throwing errors on unknown elements and allow you to create content insertion points based on element names. Alternatively, you could have used other CSS selectors for a class or attribute and it wouldn’t have required this schema fix. 
* I find elements to be more accessible and clearer in many cases, so I still recommend doing this. 
* You only lose the error handling of unknown component names, which usually helps to catch typos in your markup.
* Content projection is powerful and quite useful for components that fit the role of display components. As you saw, you can use named or unnamed content insertion points to include markup provided into your component template, all by using the `NgContent` element.
* That wraps up the first chapter about components, and in the next chapter we’ll dig into more advanced topics, like how to optimize change detection and watch for changes in our inputs.

## Summary
* Components are the building blocks of any Angular application, and every other feature of Angular stems from them in some way. We’ve covered 
* Components are self-contained elements that include a component class, a template implemented in `HTML`, and associated `CSS` that styles the component.
* Components can play various roles inside an application. The roles are what I call app, display, data, and routing components. 
* Although they’re not hard-andfast rules, it’s best to design your components to tackle a single set of tasks to keep them focused.
* The `@Component` decorator has a number of configuration capabilities, and though you won’t likely use them all in the same component, you’ll certainly need to harness most of them at some point.
* I talked about how to pass data into a component using input properties defined using the Input decorator or the inputs property. 
* You also saw that input properties aren’t available in the component class’s constructor method, but are available in the `NgOnInit` component lifecycle event handler.
* Components sometimes need to accept additional markup and display it inside the component called content projection. You saw how to use `NgContent` to insert external content into your component.