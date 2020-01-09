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
