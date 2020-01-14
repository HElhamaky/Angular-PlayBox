# Building Your First Angular App
#### In this chapter we cover 
* **Angular components** and how they form a basis
for your app
* Defining a number of **types of components**,
using decorators
* Learning **how services can be used** to share
data across your app
* **Setting up routing** to display different pages

#### In this project you will learn about 
* Bootstrapping the app
    * To start the app, we’ll use the **bootstrap feature** to kick things off once they’re loaded.
    * This happens once during the **app lifecycle**, and we’ll bootstrap the App component.
* Creating components 
    * Angular is all about components, and we’ll create several components for different purposes. We’ll learn about how they’re built and **how they nest** to create complex applications.
* Creating services and using HttpClient 
    * For code reuse, we’ll **encapsulate** some logic that helps manage the list of stocks into a service and also uses the HttpClient service from Angular to load stock quote data.
* Using pipes and directives in templates 
    * Using pipes, we can **transform data** from one format into another during display, such as formatting a timestamp into a local date format. 
    * Directives are useful tools to **modify the behavior of DOM elements** inside a template, such as the ability to repeat pieces or conditionally show elements.
* Setting up routing 
    * Most applications need the ability to allow users to navigate around the application, and by using the router we can see how to **route between different components**.

## Project Preview
* First, there’s an API that loads current stock price data from **Yahoo! Finance**; it’s
deployed on Heroku and isn’t covered in this chapter, but you can view the code for the
API at https://github.com/angular-in-action/api. 
    * It’s a standard **REST API** and doesn’t require authentication. We’ll create a **service** to help us access and load data from the API.
* When the app loads, it shows the dashboard page with a **list of cards**. 
* Each card contains a **single stock**, the **current price**, and the **day’s change in price** (as a currency *value* and as a *percentage*). 
* The **background** of the cards will be **red for negative change** and **green for a positive change**, or **gray for no change**. 
* Each of these cards is an **instance of a component** that takes the stock data and determines how to render the card.
* Lastly, the top navbar has two links, to the **dashboard** and **manage views**, which allow
for general navigation between the views. 
* We’ll use the Angular Router to **set up these routes** and manage how the browser determines which to display.
* When you click the **Manage link** in the navbar, you’ll see the manage page with a list of the stocks. 
* Here you can remove any of the stocks by clicking the **Remove button**. 
* You can also **add new stocks** by typing the stock symbol into the text area and pressing the Enter key.
* This page is a **single component**, but contains a form that’s updated immediately upon changes input by the user. 
    * The list can be **extended** by putting a new stock symbol in the input field and hitting Enter, or the list can be **reduced** by clicking the Remove button.
* In both cases, the list of symbols is **immediately changed**, and if you go back to the dashboard you’ll see the updated list appear.
* This project has a **few limitations** you should be aware of. To keep the example focused and simple, there are a few details that aren’t included in the app:
    * **No persistence** — Anytime you refresh the app in the browser, the list of stocks resets to the default list.
    * **Lack of error checking** — Some situations can throw an error or cause strange behavior, such as trying to add a stock that doesn’t exist.
    * **No unit tests** — For this example, I kept the focus on the code and intentionally left out unit tests, which are covered later.

* This example is intended to provide you with an overview of how Angular apps can be built—not to provide a bulletproof app. I provide you a number of interesting challenges you can attempt near the end of the chapter, and there are many possible features that can be imagined.

## Setting up the project
* In the terminal, start from a directory that you want to generate a new project folder
inside. 
* Then you can run the following command to generate a new project, and then start the development server:

        ng new stocks
        cd stocks
        ng serve
        
* This will take a few moments as the CLI installs a number of packages from npm, and
this depends greatly on the speed of your network and how busy the registry is. 
* Once it has completed, you can use your browser to view the app at http://localhost:4200. You should see a simple page that says something about being a new Angular app.

## The basic app scaffolding
##### The CLI generated a new project that contains a lot of files. We’ll look at the most important ones for the moment and learn more about the rest over time.

* The project contains several directories and files. Most of these are configuration for various aspects of the development, such as linting rules, unit test configuration, and CLI configuration.
<br>


|Asset  | Role
| --- | ---
| **e2e End-to-end** |testing folder, contains a basic stub test
| **node_modules** |Standard NPM modules directory, no code should be placed here
| **src** |Source directory for the application
| **.editorconfig** |Editor configuration defaults
| **.angular-cli.json** |Configuration file for the CLI about this project
| **karma.conf.js** |Karma configuration file for unit test runner
| **package.json** |Standard NPM package manifest file
| **protractor.conf.js** |Protractor configuration file for e2e test runner
| **README.md** |Standard readme file, contains starter information
| **tsconfig.json** |Default configuration file for TypeScript compiler
| **tslint.json** |TSLint configuration file for TypeScript linting rules

* In this chapter, you’ll only modify files that exist inside the **src directory**, which contains all the application code. 
Next table contains a listing of all the assets generated inside **src**. This may seem like a lot of files, but they each play a role, and if you aren’t sure what one does, leave it alone for now.

|Asset|Role|
|--- | --- |
|**app**| Contains the primary App component and module
|**assets**| Empty directory to store static assets like images
|**environments**| Environment configurations allow you to build for different targets, like dev or production
|**favicon.ico**| Image displayed as browser favorite icon
|**index.html**| Root HTML file for the application
|**main.ts**| Entry point for the web application code
|**polyfills.ts**| Imports some common polyfills required to run Angular properly on some browsers
|**styles.css**| Global stylesheet
|**test.ts**| Unit test entry point, not part of application
|**tsconfig.app.json**| TypeScript compiler configuration for apps
|**tsconfig.spec.json** |TypeScript compiler configuration for unit tests
|**typings.d.ts** |Typings configuration

## How Angular renders the base application
* Angular requires at least one component and one module. 
* A component is the basic building block of Angular applications and acts much like any other HTML element. 
* A module is a way for Angular to organize different parts of the application into a single unit that Angular can understand. 
* You might think of **components as LEGO® bricks**, which can be many different shapes, sizes, and colors, and **modules would be the packaging the LEGOs come in**. 
* Components are for functionality and structure, whereas modules are for packaging and distribution.

### App component

* We’re going to start by looking at the src/app/app.component.ts file. 
* This contains what’s called the *App component*, which is the **root** of the application. 
* In LEGO® terms, you could picture this component as the big green platform you use to start building from. 

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'StockMarket';
}
```

* **First**, you **import** the Component annotation. It’s used to **decorate** the App component by adding details that are related to the component but **aren’t** part of its controller logic, which is the AppComponent class. 
* Angular looks at these annotations and uses them with the AppComponent controller **class** to create the component at runtime.
* The **@Component** annotation declares that **this class is a component** by accepting an
object. 
* It has a **selector property** that declares the HTML selector of the component. That means the component is used in the template by adding an HTML tag 
<br>
```
<app-root> </app-root>
```
* The **templateUrl** property declares a **link to a template** containing an HTML template.
* Likewise, the **styleUrls** property contains an **array of links** to any CSS files that should be loaded for this component. 
* The **@Component** annotation can have more properties, and you’ll see a few more in action in this chapter.
* **Finally**, you see that the **AppComponent class** has a single **property** called title.
* The value is what you should see rendered in the browser, so this is the source of the value that ultimately appears. 
* Angular relies greatly on ES2015 **classes to create objects**, and almost all entities in Angular are created with classes and annotations.
* Now let’s look at the markup associated with the App component by opening **src/app/app.component.html**, shown here:
<br>
```
<h1>
{{title}}
</h1>
```
* As you can see, this is just a simple header tag, but there’s the **title property** defined
between double curly braces. 
* This is a common convention for how to **bind a value into a template** (perhaps you’re familiar with Mustache templates), and it means **Angular will replace {{title}} with the value of the title propert**y from the component. 
* This is called **interpolation** and is frequently used to **display data in a template**.

### App module
* App module is the **packaging** that helps tell Angular **what’s available to render**.
* Just as most food items have packaging that describes the various ingredients inside and other important values, a module describes the various dependencies that are needed to render the module.
* There’s at least **one module** in an application, but it’s possible to create multiple
modules for different reasons. 
* In this case, it’s the **App component from earlier plus additional capabilities** that are needed in most applications (such as routing, forms, and HttpClient).
* The CLI generated the module for us, so we can look at it in **src/app/app.module.ts**
* Just like a component, a module is an **object with an decorator**. 
* The object here is called AppModule, and **NgModule is the decorator**. 

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
* The first block is to import any **Angular dependencies** that are common to most apps and the App component.
* The **NgModule decorator** takes an **object** with a few different **properties**. 
* The **declarations** property is to provide a list of any components and directives to make available to the entire application.
* The **imports** property is an **array of other modules** upon which this module
depends—in this case, the Browser module (a collection of required capabilities). 
    * If you ever include other modules, such as **third-party modules** or **ones you’ve created**, they also need to be **listed here**.
* The **providers** property, which is empty by default. **Any services** that are created are to be listed here.
* Lastly, the **bootstrap** property defines which components to bootstrap at runtime.

### Bootstrapping the app

* **The application must be bootstrapped at runtime to start the process of rendering**.
* So far, we’ve only declared code, but now we’ll see how it gets executed. 
* The CLI takes care of **wiring up the build tooling**, which is based on **webpack**.
* Start by taking a look at the **.angular-cli.json** file. 
    * You’ll see an array of apps, and one of the properties is the main property. By default, it points to the **src/app/main.ts** file.
    
    ```typescript
    "main": "src/main.ts",
    ```
* This means that **when the application gets built, it will automatically call the contents of the main.ts** file as the **first set of instructions**.
* The role of **main.ts** is to **bootstrap the Angular application**. 
* The contents of the **main.ts** file are included in the following listing, and contain only a few basic instructions.

```typescript
import { enableProdMode } from '@angular/core';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app/app.module';
import { environment } from './environments/environment';

if (environment.production) {
  enableProdMode();
}

platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => console.error(err));
```
* The first section imports some **dependencies**, particularly **platformBrowserDynamic** and **AppModule**. 
* The name is a bit long, but the **platformBrowserDynamic** object is used to **tell Angular which module is being loading**, and in this case that’s the **AppModule** from earlier. 
* There’s one last piece to review by looking at the **index.html** file. 
* If you remember from the App component code, there was a selector of app-root used to identify the component in markup. 
* You should see the following in the src/index.html file:
<br>
```html
<body>
<app-root></app-root>
</body>
```
* Once the app is bootstrapped, Angular will **look for the app-root element** and replace it with the rendered component. 
* That’s what you end up seeing in the screen, but while it’s loading everything, you’ll see a “Loading ...” message. 
* It can take a moment for all the assets to load and initialize before the component renders.
* This is known as **Just in Time compilation (JiT)**, meaning that **everything is loaded and rendered on demand in the browser**. 
* **JiT** is only meant for **development**, and may be removed in future releases.

## Let's Start Building
* I’d like to add a couple of small touches that will help us style the rest of the application, by adding some basic CSS and markup. 
* First we need to add two link tags to our **src/index.html**:
<br>
```html
<link rel="stylesheet" href="//storage.googleapis.com/code.getmdl.io/1.0.1/
material.indigo-orange.min.css">
<link rel="stylesheet" href="//fonts.googleapis.com/
icon?family=Material+Icons">
```
* This will **load some font icons** and the global styles for the application, which are based on the **Material Design** Lite project. 
* This is one way you can load **external references** to style libraries or other assets.
* We’d like to give our application some **global styles**. Add the following to the **src/ styles.css** file—it will give the application a light gray background:

```css
body {
background: #f3f3f3;
}
```
* Lastly we want to set up some base markup to structure our application. Let’s replace the content of the **src/app/app.component.html** file with the markup in the following listing.

```html
<div class="mdl-layout mdl-js-layout mdl-layout--fixed-header">
<header class="mdl-layout__header">
<div class="mdl-layout__header-row">
<span class="mdl-layout-title">Stock Tracker</span>
<div class="mdl-layout-spacer"></div>
<nav class="mdl-navigation mdl-layout--large-screen-only">
<a class="mdl-navigation__link">Dashboard</a>
<a class="mdl-navigation__link">Manage</a>
</nav>
</div>
</header>
<main class="mdl-layout__content" style="padding: 20px;">
</main>
</div>
```

* All right, we’ve created the **base app scaffolding** using the CLI, seen the **App component**, **App module**, and **bootstrap logic**, and found the **markup** that renders out the component. **Congratulations**, you’ve made your first Angular app!

# Building services

* Services are objects that abstract some common logic that you plan to reuse in multiple places.
* They can do about anything you need them to do, because they’re objects.
* Using ES2015 modules, these classes are exported, and so any component can import them as necessary. 
* They could also have functions or even static values, like a string or number, as a way to share data between various parts of your application.
* Another way to think of services is as sharable objects that any part of your app can import as needed. 
* They’re able to abstract some logic or data (such as the logic necessary to load some data from a source), so it’s easy to use inside of any component.
* Although services will often help manage data, they’re not restricted to any particular job.
* The intention of a service is to enable reuse of code. 
* A service might be a set of common methods that need to be shared. 
* You could have various “helper methods” that you don’t want to write over and over, such as utilities to parse data formats or authentication logic that needs to be run in multiple places.
* In the app, you’ll want to have a list of the stocks for both the dashboard and manage pages to use. 
* This is a perfect scenario of when to use a service to help manage the data and share it across different components.
* The CLI gives us a nice way to create a service that has the scaffolding we need to get started. It will generate a simple service and a test stub for that service, as well. 
To generate a service, you run the following:
        ng generate service services/stocks
* The CLI will generate the files in the **src/app/services** directory. 
* It contains the most basic service, which does nothing. 
* The Stocks service will have an array that contains a list of the stock symbols and expose a set of methods to retrieve or modify the list of stocks.

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

//Declares a stock array and API variables
let stocks: Array<string> = ['AAPL', 'GOOG', 'FB', 'AMZN', 'TWTR'];
let service: string = 'https://angular2-in-action-api.herokuapp.com';


//Defines and exports the TypeScript interface for a stock object
export interface StockInterface {
  symbol: string;
  lastTradePriceOnly: number;
  change: number;
  changeInPercent: number;
}

// Annotates with Injectable to wire up dependency injection
@Injectable()
export class StocksService {
  
  //Constructor method to inject HttpClient service into class http property
  constructor(private http: HttpClient) {}

    //Method to get the stocks
      get() {
      return stocks.slice();
      }
      
      //Method to add a new stock to list
      add(stock) {
      stocks.push(stock);
      return this.get();
      }

      //Method to remove a stock from list
      remove(stock) {
      stocks.splice(stocks.indexOf(stock), 1);
      return this.get();
      }

      //Method to call HttpClient service to load stock values from API
      load(symbols) {
      if (symbols) {
      return this.http.get<Array<StockInterface>>(service + '/stocks/snapshot?symbols=' + symbols.join());
    }}
  }

```
1. The service first needs to **import its dependencies**; one is the **decorator for a service**, and the other is the **HttpClient service**. 
* Then it **declares two variables**; one is to **track the list of stock symbols**, and the other is the **API endpoint URL**.
* Then the **StockInterface interface** is defined and exported for other components to use. 
    * This provides a TypeScript definition of what a *stock object should contain*, which is used by TypeScript to ensure the use of the data remains **consistent**. 
    * We’ll use this later to ensure that we’re *typing our stock objects correctly* when they’re used.
* The StocksService class is **exported and is decorated** by the Injectable decorator.
    * The decorator is used to *set up the proper wiring* for Angular to know how to use it elsewhere, so if you forget to include the decorator, the class might not be injectable into your application.
* In the **constructor method**, the **HttpClient service is injected** using the TypeScript technique of **declaring a private variable called *http* and then giving it a type of HttpClient**. 
    * Angular can *inspect the type definition* and determine how to inject the requested object into the class. 
    * If you’re new to TypeScript, keep in mind that anytime you see a colon after a variable declaration, you’re *defining the object type* that should be assigned to that variable.
* The service contains **four methods**. 
    * The **get()**method is a simple method that **returns the current value of the stocks array**, but it always **returns a copy** instead of the direct value. This is done to encapsulate the stock values and prevent them from being directly modified. 
    * The **add()**method **adds a new item to the stocks array** and returns the newly modified value.  
    * The **remove()** method will **drop an item from the stocks array**.
    * Finally, the **load()** method makes a call to the **HttpClient service** to load the data for **current stock price values**. 
* The **HttpClient service** is called and **returns an observable**,which is a construct for handling **asynchronous events**, such as data from an API call. 
* There is a little **feature** of the HttpClient that appears as part of the **get()** method
and is put between two angle brackets:
```typescript
        this.http.get<Array<StockInterface>>(...
```
* This is known as a **type variable**, which is a feature of TypeScript that allows you to **tell the http.get() method what type of object it should expect**, and in this case it will expect to get an array of objects that conform to the **StockInterface** (our stock objects). 
    * This is optional, but it’s very helpful to alert the compiler if you try to access properties that don’t exist.
* There’s one more step we have to do, because the **CLI doesn’t automatically register the service** with the App module, and we need to **register HttpClient** with the application as well.
    * Open the **src/app/app.module.ts** file and near the top add these two imports:
```typescript
import { HttpClientModule } from '@angular/common/http';
import { StocksService } from './services/stocks.service';
```
* This will import the Stocks service and HttpClientModule into the file, but we need to register the HttpClientModule with the application. Find the imports section as defined in the NgModule, and update it like you see here to include the HttpClientModule: 
```typescript
imports: [
BrowserModule,
HttpClientModule
],
```
* Now we need to register the new StocksService with the providers property to
inform Angular that it should be made available for the module to use:
```typescript
providers: [StocksService],
```
* Your service is wired up and ready to consume, but we haven’t used it yet anywhere in our application. The next section looks at how to consume it.
* This service is not too complex. It’s mostly designed to abstract the modification of the array so it’s not directly modified and load the data from the API. 
* While the application runs, the stocks array can be modified, and changes are reflected in both the dashboard and manage components, as you’ll see shortly. Because it’s exported, it’s easily imported when needed.
* Now you’ll create a component that uses some default directives and allow configurable properties to modify the component’s display.

# Creating your first component
* We’re going to create a component that **displays a basic summary card** of the stock price information.
* This component will only **receive data to display from its parent component** and modify its own display based on that input value. 
    * For example, a parent component will pass along the current data for a particular stock, and the Summary component will **use the daily change to determine whether the background should be green or red based on whether the stock went up or down**.
* The key goals of this component are to do the following:
    * Accept stock data and **display** it.
    * **Change background color** depending on the day’s activity (green for increase, red for decrease).
    * **Format values** for proper display, such as currency or percentage values and we’ll even wire it up to load the data from the API. 
    * Eventually, we’ll instantiate **multiple copies** of this component to display a card for each of the stocks.
* Obviously, when you run this the stock values will change based on the **latest data**, but
you can see the card displaying the **current data**.
* **Let’s dig into building this card and then we’ll walk through the individual parts of how it results in this output**. 
* Go back to the terminal and run the following:
```bash
ng generate component components/summary
```
* The CLI will generate a new component inside the **src/app/components/summary**
directory. 
* We had to create the **src/app/components** directory first, because the **CLI doesn’t make new folders** for you automatically if they’re missing. This helps **organize** the components into a **single directory**, though you could choose to generate them elsewhere.
* Now the **contents of the component** are pretty similar to how the App component appeared originally. 
    * It contains 
        * an empty CSS file, 
        * basic HTML template, 
        * test stub, 
        * and empty class already initialized with the component annotation.
* We’ll start by setting up the template for our component and then we’ll create the controller to manage it. 
* Open the **src/app/components/summary/summary.component.html** file and replace the contents with what you see in the following listing.
```html
<div class="mdl-card stock-card mdl-shadow--2dp" [ngClass]="{increase:isPositive(), decrease:isNegative()}" style="width: 100%;">
    <span>
        <div class="mdl-card__title">
            <h4 style="color: #fff; margin: 0">
                {{stock?.symbol?.toUpperCase()}}
                <br/>
                {{stock?.lastTradePriceOnly | currency:'USD':'symbol':'.2'}}
                <br/>
                {{stock?.change | currency:'USD':'symbol':'.2'}} ({{stock?.changeInPercent|percent:'.2'}})
            </h4>
        </div>
    </span>
</div>
```
* The template contains some **markup to structure** the card like a **material design card**.
* If we look at the first line, we see this snippet as an attribute on the **div** element:
        [ngClass]="{increase: isPositive(), decrease: isNegative()}"
* This is a **special kind of attribute called a directive**. 
* **Directives** allow you to modify the behavior and display of DOM elements in a template. 
* Think of them as **attributes** on HTML elements that cause the element to **change its behavior**, such as the **disabled attribute** that disables an HTML input element. 
* Directives make it possible to **add some conditional logic** or otherwise modify the way the template **behaves or is rendered**.
* The **NgClass** directive is able to **add or remove CSS classes** to and from the element.
* It’s assigned a value, which is an object that contains properties that are the **CSS class
names**, and those properties map to a method on the controller (to be written). 
* **If the method returns true, it will add the class**; if false, it will be removed. 
* In this snippet, **the card will get the increase CSS class when it’s positive**, or the **decrease CSS class when it’s negative**, for the day’s trading.
* Angular has a **few directives built in**, and you’ll see a couple more in this chapter.
* **Directives usually take an expression**, which is evaluated by Angular and passed to the directive. 
* The expression might **evaluate to a Boolean** or other primitive value or **resolve to a function call** that would be run to return a value before the directive runs. 
* Based on the value of the expression, the directive might do different things, such as **show or hide** whether the expression is **true or false**.
* We saw an example of **interpolation** earlier, but we now have a more complex example
that **displays the symbol of the stock**. 
* The controller is expected to have a property called stock, which is an object with various values:
        {{stock?.symbol?.toUpperCase()}}
* The **double curly braces** syntax is the way to display some value in the page and it is called **interpolation**. 
* The content **between the braces** is called an **Angular expression** and is evaluated against the controller (like the directive), meaning that **it will try to find a property on the controller** to display. 
* **If it fails**, normally it will throw an **error**, but the **safe navigation** operator **?.** will **silently fail** and not display anything if the property is missing.
* This block will display the stock symbol, but as `uppercase`. 
* Most JavaScript expressions are valid Angular expressions, though some things are different, such as the safe navigation operator. 
* The ability to call prototype methods like `toUpperCase()` remains, and that’s how it’s able to render the text **as uppercase**.
* The next **interpolatio**n shows the last trade price and adds another feature called **pipes**, which are added directly into the expression **to format the output**.
* The **interpolationexpression** is extended with a **pipe symbol**, |, and then a pipe is named and **optionally configured with values separated with the colon : symbol**. 
* The price value comes back as a **normal float** (like `111.8`), which is not the same format as currency, which should appear like `$111.80`:
        {{stock?.lastTradePriceOnly | currency:'USD':'symbol':'.2'}}
* **Pipes only modify the data before it is displayed**, and **do not change the value** in the controller. 
* In this code, the double **curly braces** indicate that you want to **bind the data stored in the stock.lastTradePriceOnly property to display it**. 
* The data is piped through the **Currency pipe**, which **converts the value into a financial figure** based on a **`USD` figure**, and **rounds to two decimal points**. 
* Now let’s look at the next line:
        {{stock?.change | currency:'USD':'symbol':'.2'}} ({{stock?.changeInPercent | percent:'.2'}})
* The next line also has **two different interpolation** bindings with a **Currency or Percentage** pipe. 
* The **first will convert to the same currency format**, but the second will **take a percentage as a decimal, such as `0.06`, and turn it into `6%`**. 
* The Angular documentation can detail all the options available and how to use them for each pipe.
* This template doesn’t work in isolation; it requires a controller to wire up the data and the methods. 
* Let’s open the **src/app/components/summary/summary.component.ts** file and replace the code, as you see in the following listing.

```typescript
import { Component, Input } from '@angular/core';
//Declares the component metadata
@Component({
selector: 'summary',
styleUrls: ['./summary.component.css'],
templateUrl: './summary.component.html'
})

//Exports the Summary component class
export class SummaryComponent {
    //Declares a property that is an input value
    @Input() stock: any;
    //Method to check if stock is negative                           
    isNegative() {
    return (this.stock && this.stock.change < 0);
    }
    //Method to check if stock is positive                           
    isPositive() {
    return (this.stock && this.stock.change > 0);
    }
}
```
* This controller **imports dependencies**, which is almost always the **first block** of any file written in TypeScript. 
The component **metadata** describes the **selector**, **linked styles**, and **linked template files** that comprise the component. We’ll add some CSS to the styles in a moment.
* The summary **controller class** starts with a **property** called *stock*, which is preceded with the **Input annotation**. 
* This indicates that **this property is to be provided to the component by a parent component** passing it to the summary. 
* Properties are bound to an element using an **attribute**, as you can see here—this example will set the value of **stockData** of the parent component in the **stock property** of the Summary component:
        <summary [stock]="stockData"></summary>
* Because input is passed through a binding attribute, it will **evaluate the expression** and pass it into that property for the **Summary component** to consume. 
* Angular expressions behave the same anytime there’s a binding. They **try to find a corresponding value in the controller to bind to the property**.
* Lastly, there are the two methods for **checking** whether the **stock value** is positive or negative. 
* The stock could also be neutral, so that’s the default state, and only if the stock changes will one of the methods return true. 
* These methods are used by the `NgClass` directive to determine whether it should **add a particular CSS class**, as described earlier in the template.
* The final piece we want to add are the CSS classes themselves. 
* Angular has some interesting ways to **encapsulate CSS styles** so they only apply to a single component.
We’ll dig into the specifics later, but open the **src/app/components/summary/summary.component.css** file and add the styles, as shown in the following listing.
```css
:host .stock-card {
background: #333333;
}
:host .stock-card.increase {
background: #558B2F;
color: #fff;
}
:host .stock-card.decrease {
background: #C62828;
color: #fff;
}
```
* This is typical CSS, though you may not have seen or used the :host selector in the past. 
* Because components need to be as self-contained as possible, they rely on the Shadow DOM concepts discussed in chapter 1. 
* When Angular renders this component, it will modify the output to ensure that the CSS selector is unique and doesn’t accidentally interfere with other elements on the page. 
* This behavior is configurable, but that will be covered later.
* The **host selector** is a way to specify that you want the styles to **apply to the element that hosts the element**, so in this case it will look at the **Summary component element itself** rather than the contents inside it. 
* The primary purpose of the CSS here is to establish the **Summary component background color**.
* We’ve walked through the Summary component generation and built out a functional component. 
* Let’s quickly use it to get a glimpse of how it behaves.
* Look at the **src/app/app.module.ts** file and you’ll see that the CLI already modified the module to include the Summary component in the App module. 
    * There’s nothing to do here, but I wanted to point it out.
* Now look at **src/app/app.component.ts** and update it to the contents of the following listing. 
    * This will include the **Stocks service** and use it to **store the stock data onto a property**. We’ll then use this to **display the summary card**.

```typescript
import { Component } from '@angular/core';
import { StocksService, StockInterface } from './services/stocks.service';

@Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css']
})

export class AppComponent {
    //Declares a property of an array of stocks
    stocks: Array<StockInterface>;
    constructor(service: StocksService) {
            service.load(['AAPL']).subscribe(stocks => {
            //When the data loads, it will store it on the stocks property
            this.stocks = stocks;                                                    
            });
    }
}
```
* Here we **store the loaded stock data onto a property called stocks**. 
* We also **provide some typing information**, which is imported from our Stocks service, so that TypeScript knows **what kind of value to expect**. 
* Finally, instead of logging the data to the console, we **store it on the stocks property**.
* Now we’ll need to update the **src/app/app.component.html** file to use the Summary component. 
* Here is the snippet you need to **update** from the template:

```html
<main class="mdl-layout__content" style="padding: 20px;" *ngIf="stocks">
    <summary [stock]="stocks[0]"></summary>
</main>
```
* The first line added `*ngIf="stocks"`, which is a directive that will **only render** the contents inside the element **when the expression is true**. 
* In this case, **it won’t render** the Summary component **until the stock data has been loaded**.
* The middle line shows the **instantiation of a single Summary component**, and the first value of the stocks array is bound into the stock property. 
* The **data returns as an array**, so we’re directly accessing the first value. 
* Recall the **input value** we declared in the Summary component, which is also named stock.
* Once you save this and run the app, it should finally display a single summary card with the current stock data for Apple’s stock. 
* We’ve made our first component and displayed it inside our application!
* Next you’ll create another component and use it together with the Summary component to create the dashboard that displays the list of stocks and their current statuses.

## Components that use components and services
##### We’re ready to combine the previously created Summary component and Stocks service into a working Dashboard component.

* This component will 
    * **manage the loading of the data using the Stocks service** 
    * and then **display each stock using a copy of the Summary component**.
* To get started, we can use the CLI again to generate another component:
`ng generate component components/dashboard`
    * This will output new files into the **src/app/components/dashboard** directory for the
HTML, CSS, controller, and unit test.
    * It also adds the component to the App module to be immediately consumable.
    
    
1. Let’s reset our working project to *display this new component* by modifying the
**src/app/app.component.html** file with the content here:
```html
<main class="mdl-layout__content" style="padding: 20px;">
<dashboard></dashboard>
</main>
```
    * This should display the *default component message* in the application, since that’s the default code generated by the CLI.
    * We also need to remove some logic from the App component controller; it should now appear as you see here. This removes the imports and loading of stock data in the App component itself, and we’ll put it instead into the dashboard in a moment. 
2. Replace the contents of **src/app/app.component.ts** with the following:
```typescript
import { Component } from '@angular/core';
@Component({
selector: 'app-root',
templateUrl: './app.component.html',
styleUrls: ['./app.component.css']
})
export class AppComponent {}
```
3. Great! We’ve now **cleaned up the App component** and are ready to start **building out the
dashboard**. Our first order of business is to set up the dashboard controller. 
    * Its job is to **use the Stocks service to load data** and make it available for the component to consume.
    
4. Open the controller at **src/app/components/dashboard/dashboard.component.ts** and replace it with the code in the following listing.

```typescript
import { Component, OnInit } from '@angular/core';
import { StocksService, StockInterface } from '../../services/stocks.service';

@Component({
    selector: 'dashboard',
    templateUrl: './dashboard.component.html',
    styleUrls: ['./dashboard.component.css']
    })

export class DashboardComponent implements OnInit {
    //Declares a property for holding an array of stocks
    stocks: Array<StockInterface>;
    //Declares a property for holding an array of stock symbols
    symbols: Array<string>;
    //Gets the stock symbols from the service when the component is first constructed
    constructor(private service: StocksService) {
    this.symbols = service.get();
    }
    //Implements the ngOnInit method and calls the service to load stock data over Http
    ngOnInit() {
    this.service.load(this.symbols).subscribe(stocks => this.stocks = stocks);
    }
}
```
1. The controller starts by **importing the Component annotation and the OnInit interface**. 
    * If you haven’t implemented an interface before, an interface is a means to enforce that a class contains a required method—in this case, the method named `ngOnInit`.
2. The **DashboardComponent class is the component controller**, and it declares that **it must implement the requirements of** `OnInit`. 
    * If it doesn’t, TypeScript will fail to compile the code and throw an error. 
    * It then has **two properties**: an **array of stocks** and an array of strings that represent the **stock symbols to display**. 
    * Initially they’re **empty arrays**, so we’ll need to get them loaded for the component to render.
3. The **constructor method** runs as soon as the component is **created**. 
    * It will **import the Stocks service onto the service property** and then **request the current list of stock symbols from it**. 
    * This works because this is a **synchronous action that loads a value directly from memory**.
    * But **we don’t load data from the service in the constructor for a number of reasons**, the primary reason is **due to the way that components are rendered**.
    * The **constructor fires early in the rendering of a component**, which means that **often, values are not yet ready** to be consumed. 
4. Components expose a number of **lifecycle hooks** that allow you to **execute commands at various stages of rendering**, giving you greater control over when things occur.
    * In our code, we use the `ngOnInit` *lifecycle hook* **to call the service to load the stock data**. 
    * It uses the list of stock symbols that was **loaded in the constructor**. 
    * We then **subscribe to wait** for the results to return and **store them in the stocks property**. 
    * This uses the **observable approach to handling asynchronous requests**. 
    * We’ll look at observables in depth later as well. 
    * Here we are using them because the **HttpClient returns an observable for us to receive the response**. 
    * This is exposed as a **stream of data**, even though it is a **single event**. 
5. Now we need to complete the component by *adding the template*. 
    * Open the **src/ app/components/dashboard/dashboard.component.html** file and replace it with the contents of the following listing.
    
```html
<div class="mdl-grid">
    <div class="mdl-cell mdl-cell--12-col" *ngIf="!stocks" style="text-align:center;"> Loading</div>
    <div class="mdl-cell mdl-cell--3-col" *ngFor="let stock of stocks">
    <summary [stock]="stock"></summary>
    </div>
</div>
```

* The template has some classes to use the *Material Design Lite* UI framework for a *grid structure*. 
* The template contains another `NgIf` attribute to **show a loading message while the data is loaded**. 
* Once the stock data has returned from the API, the loading message will be hidden.
* Then we see another element that has a new directive, `NgFor`. Like `NgIf`, it starts with an `*`, and the expression is similar to what you would use in a traditional JavaScript for loop. 
The expression contains `let stock of stocks`, which means that it will **loop over each of the items in the stocks array** and expose a local variable by the name of stock.
* Again, this is the same kind of behavior that you would see in a JavaScript for loop, but applied in the context of HTML elements.
* `NgFor` will then create an instance of the Summary component for each of the stock items. 
* It binds the stock data into the component. 
* Each copy of the Summary component is **distinct** from the others, and they **don’t directly share** data.
* You’ve now completed the dashboard view, which uses a service and another component to render the experience. 
* When you **run** the application now, you should see the five default stocks appearing as separate cards in the page. 
* The grid layout should lay them out in four columns.
* Next you’ll build a new component that has a form that manages the list of stock symbols to use when displaying the stocks.

## Components with forms and events
* We want to **manage the stocks** that are displayed, so we’ll need to add another component that has a **form to edit the list of stocks**. 
* This form will allow users to **input new stock symbols** to add to the list and will have a list of the **current stocks** with a button that will **remove a stock** from the list. 
* This list of stocks is shared throughout the entire application, so any **changes will replicate** elsewhere.
* Forms are essential in applications, and Angular comes with built-in support for building complex forms with many features. 
* Forms in Angular are comprised of **any number of controls**, which are the various types of inputs and fields the form may contain (such as a *text input*, a *checkbox*, or some custom element).
* Let’s start by **generating a new component** for the manage view. 
* Using the CLI, run the following command, and remember, this will also automatically register the component with the App module so it’s ready to consume: `ng generate component components/manage`
* Now update the **src/app/app.component.html** file and change the content of the main element, as you see in the following code, so the Manage component will display in the application. 
* Then when you run the application, it will display the **default message** you see with any new component:
```html
<main class="mdl-layout__content" style="padding: 20px;">
<manage></manage>
</main>
```
* We also need to add the `FormsModule` to our application, because we are going to use the **form features** that aren’t automatically included by Angular. 
* Open up the **src/app/app.module.ts** file and add a new import:
`import { FormsModule } from '@angular/forms';`
* Then update the imports definition of the module to declare the `FormsModule` like you see here:
```typescript
imports: [
BrowserModule,
HttpClientModule,
FormsModule,
],
```
* Let’s start making **our Manage component** by updating the controller with some logic.
* There will also need to be two methods: 
    * one to **handle the removal of a stock** 
    * and another to **add a new stock symbol to the list**. 
* Open **src/app/components/manage/manage.component.ts** and update it to match the following listing.
* This will comprise the additional methods and setup required for this view.

```typescript
import { Component } from '@angular/core';
import { StocksService } from '../../services/stocks.service';

@Component({
    selector: 'manage',
    templateUrl: './manage.component.html',
    styleUrls: ['./manage.component.css']
})

export class ManageComponent {
    //Defines class and two properties for storing the array of symbols and a string for input
    symbols: Array<string>;
    stock: string;
    //Gets the current list of symbols during class instantiation
    constructor(private service: StocksService) {
    this.symbols = service.get();
    }
    //Method to add a new stock to the list
    add() {
    this.symbols = this.service.add(this.stock.toUpperCase());
    this.stock = '';
    }
    //Method to remove a stock symbol from the list
    remove(symbol) {
    this.symbols = this.service.remove(symbol);
    }
}
```
* As usual, we start by **importing dependencies** for the component. 
* Then the **component metadata** is declared using the `@Component` annotation. 
* The **class object** is then declared, which contains **two properties**: 
    * the first is the **array of symbols** that’s retrieved *from* the **Stocks service**, 
    * and the second is a property to **hold the value of the input**.
* We’ll see how the **stock property** is linked to the input field in the template, but this is where it’s first defined.
* The **constructor uses the service** to get the array of stock symbols and **store** it on the **symbols property**. 
* This doesn’t require the `OnInit` lifecycle hook, because it’s a **synchronous request** to get data that exists in memory.
* Then we have the two methods to **add** or **remove** the symbols from the list. 
* The **service always returns a copy of the stocks symbol array**, so we have to use the service methods to manage the list (which is encapsulated inside the service and isn’t directly modifiable). 
    * The `add` method will **add a new item to the list of symbols**, and then **store the modified list onto the symbols list**. 
    * Conversely, the `remove` method will **remove the item from the array** and **refresh the symbols list in the controller**.
* This controller satisfies our needs for **handling the actions of the form**, but now we need to create the template to display the form and its contents. 
* Open **src/app/components/manage/manage.component.html** and add the contents from the following listing.

```html
<div class="mdl-grid">
    <div class="mdl-cell mdl-cell--4-col"></div>
        <div class="mdl-cell mdl-cell--4-col">
            
            <form style="margin-bottom: 5px;" (submit)="add()">
            <input name="stock" [(ngModel)]="stock" class="mdl-textfield__input"
            type="text" placeholder="Add Stock" />
            </form>
            
            <table class="mdl-data-table mdl-data-table--selectable mdl-shadow--2dp"
            style="width: 100%;">
                <tbody>
                <tr *ngFor="let symbol of symbols">
                    <td class="mdl-data-table__cell--non-numeric">{{symbol}}</td>
                    <td style="padding-top: 6px;">
                    <button class="mdl-button" (click)="remove(symbol)">
                    Remove</button>
                    </td>
                </tr>
                </tbody>
            </table>
        </div>
        
        <div class="mdl-cell mdl-cell--4-col"></div>
</div>
```

* In this template there’s a decent amount of markup only for the grid layout. 
* Any class that starts with `mdl-` is part of the styles provided by *Material Design Lite’s grid* and *UI library*. 
* The first interesting section is the form, which has a new type of attribute we haven’t seen before.
* The `(submit)="add()"` attribute is a **way to add an event listener**, known as an **event binding**. 
* When the **form is submitted** (which is done by pressing **Enter**), it will **call the add method**. 
* **Any attribute that’s surrounded by parentheses is an event binding**, and the **name of the event should match the event** without the on (onsubmit is submit). 
* The form contains a single input element, which has another new type of attribute. 
* The `[(ngModel)]="stock"` attribute is a **two-way binding that will sync the value of the input and the value of the property in the controller** anytime it changes from either location. 
* This way, as the user types into the text field, the value will be immediately available for the controller to consume. 
* When the **user hits Enter**, the **submit event fires** and will use the **value of the stock** property when adding the new symbol. 
* The next section **loops over the list** of symbols using `NgFor`. 
* For each symbol, it will create a **local variable** called `symbol`, create a **new table row** that **binds the value**, and a button that’s for **removing the item**. 
* The `remove button` contains another **event binding**, this one to **handle the click event**. 
* The `(click)="remove(symbol)"` attribute **adds an event listener** to the click event and will **call** the `remove method` in the controller, passing along the symbol. 
* Because there are **multiple instances** of the button, each one passes along the **local variable** to know which symbol to remove. 
* The last task is to **add routing** to the application to **activate routes** for the **two views** to act like **two different pages**.

## Application routing
#### The final piece of the application is the routing, which configures the different pages that the application can render. Most applications need some form of routing so it can display the correct part of the application at the expected time. 
* Angular has a **router** that works well with the Angular architecture by mapping components to routes. 
* The router works by **declaring an outlet in the template**, which is the place in the template that the **final rendered** component will be displayed. 
* Think of the **outlet** as the **default placeholder** for the content, and until the content is ready to be displayed, it will be **empty**.
* To set up our routes, we’ll **link the Manage and Dashboard components to two routes**. 
* We’ll handle the configuration ourselves, because the **CLI doesn’t support setting up routes** in this particular release.
* To begin, create a new file at **src/app/app.routes.ts** and fill it with the code from the following listing.

```typescript
import { Routes, RouterModule } from '@angular/router';
import { DashboardComponent } from './components/dashboard/dashboard.component';
import { ManageComponent } from './components/manage/manage.component';

//Defines a route configuration array
const routes: Routes = [
    {path: '',
    component: DashboardComponent},
    
    {path: 'manage',
     component: ManageComponent}];

//Exports the routes for use
export const AppRoutes = RouterModule.forRoot(routes);
```
* This file’s main purpose is to configure the routes for the application, and we **start by importing** the `RouterModule` and the **Route type definition**. 
* The `RouterModule` is used to **activate the router** and **accepts the routes configuration** when it’s initialized. 
* We also import the **two routable components**, the *Dashboard* and *Manage* components, so we can reference them properly in our routes configuration.
* The routes are defined as an **array of objects** that have **at least one property**—in this case two, for a URL path and a component. 
* For the *first route*, there’s **no path**, so it acts as the **application index** (which will be `http://localhost:4200`) and links to the *Dashboard* component. 
* The *second route* provides a URL path of *manage* (which will be `http://localhost:4200/manage`) and links to the *Manage* component. 
* This is the most likely type of routing that you’ll do with Angular, though there are many ways to configure and nest routes.
* Finally, we create a **new value AppRoutes**, which is assigned to the result of `RouterModule.forRoot(routes)`. 
* We’ll dig further into how the `forRoot` method behaves later, but it’s **a way to pass configuration to the module**. 
    * In this case, we’re passing the **array of routes**. 
* We **export** this so we can **import** it into our **App module** and register it. 
* Open the **src/app/app.module.ts** file and add a new line at the end of the imports that imports the AppRoutes object you created in the previous file:

```
import { AppRoutes } from './app.routes';
```
* Now update the **imports property** of your module to include the **AppRoutes** object.
* This will **register the Router module** and our configuration with our application:

```typescript
imports: [
BrowserModule,
HttpClientModule,
FormsModule,
AppRoutes
],
```
* The **final step** is to **declare a place for the router to render**, and update the links to use the router to navigate. 
* Open the **src/app/app.component.html** file one last time and make a few modifications. 
* First you’ll change the contents of the *main element* to have a different element, the **router outlet**:

```html
<main class="mdl-layout__content" style="padding: 20px;">
<router-outlet></router-outlet>
</main>
```
* This declares the **specific location in the application that the router should render the component**. 
* It’s the same place that we’ve put our components while building them, so it should make sense that this is the best place.
* Then we need to update the links to **use a new directive** that will **set up the navigation between routes**. 
* The **RouterLink directive** binds to an **array of paths** that are used to build a URL:

```html
<nav class="mdl-navigation mdl-layout--large-screen-only">
<a class="mdl-navigation__link" [routerLink]="['/']">Dashboard</a>
<a class="mdl-navigation__link" [routerLink]="['/manage']">Manage</a>
</nav>
```
* The **directive parses the array and tries to match to a known route**. 
* Once it matches a route, it will add an `href` to the **anchor tag** that correctly links to that route.
* The router is capable of **more advanced configuration**, such as *nested routes*, *accepting parameters*, and *having multiple outlets*.
* Now your project is complete, and you can reload the application in the browser to see it running, as previewed earlier. 
* Congratulations! You’ve got a working Angular app running, and now you can try to make it do some more things.

## Summary
Congratulations on making it through a functional Angular app! We went through a lot of Angular features quickly, but you should now have an understanding of how various parts are assembled into an app. Here is a quick recap of the primary takeaways:

* Angular apps are components that contain a **tree of components**. The root app is **bootstrapped** on page load to initialize the application.
* A component is an ES6 class with an `@Component` annotation that **adds metadata** to the class for Angular to properly render it.
* Services are also ES6 modules and should be designed for **portability**. Any ES6 class could be used, even if it isn’t specifically meant for Angular.
* **Directives are attributes** that modify the template in some way, such as `NgIf`, which conditionally **shows or hides** the DOM element based on the value of an *expression*.
* Angular has **built-in form support** that includes the ability to automatically *validate*, *group*, and *bind data* with any form control, as well as *use events*.
* Routing in Angular is based around **paths mapping to a component**. Routes will **render a single component**, and that component will also be able to **render any** additional components it needs.