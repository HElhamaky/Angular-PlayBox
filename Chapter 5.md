# Advanced components
> Chapter 4 covered many of the basics of components, but there is so much more!

Here we’ll dive into additional capabilities that will come in handy as you build more interesting applications. 
We’ll look at how change detection works in more detail, and look at how to use the OnPush capability to reduce the amount of work Angular has to do to improve rendering efficiency.
Although components can use inputs and outputs, there are additional ways to have components talk to one another. We’ll look at why you might choose to use a different approach and how to implement it.
There are three primary ways to render styles for a component, and choosing different modes potentially has a significant impact on the way your components are displayed. 
You may want to internalize the CSS styling as much as possible, or you may not want any styling to be internalized, but to use the global CSS styles instead.
Finally, we’ll look at how to render a component dynamically, and why you might want to do that. It’s not very common, but there are moments where it’s useful.
As discussed in chapter 4, I highly recommend that you keep your components focused. As we learn more about component capabilities, you’ll see that it’s important to avoid overloading your components.
This chapter will continue with the example from chapter 4, so refer to it for how to set up the example. Everything will build upon what you learned, so the examples will be expanded to demonstrate more advanced capabilities of components.
I can say with almost 100% certainty that no component will use every single capability, because it would most likely not function. However, mastery of these additional concepts will help you write more complex and dynamic applications. Let’s start by taking a look at change detection and how to optimize performance.
Change detection and optimizations Angular ships with a change detection framework that determines when components need to be rendered if inputs have changed. 
Components need to react to changes made somewhere in the component tree, and the way they change is through inputs. Changes are always triggered by some asynchronous activity, such as when a user
interacts with the page. When these changes occur, there is a chance (though no guarantee) that the application state or data has changed. Here are some examples:
    * A user clicks a button to trigger a form submission (user activity).
    * An interval fires every x seconds to refresh data (intervals or timers).
    * Callbacks, observables, or promises are resolved (XHR requests, event streams).
These are all events or asynchronous handlers, but they may come from different sources. We’ll dig deeper into the way that observables and XHR requests behave in other chapters, but here we’re curious about how user actions and an interval trigger changes in Angular.
Angular has to know that an asynchronous activity occurred, but the setInterval and setTimeout APIs in JavaScript occur outside Angular’s awareness. Angular has monkey- patched the default implementation of setInterval and setTimeout to have them properly trigger Angular’s change detection when an interval or timeout is resolved.
Likewise, when an event binding is handled in Angular, it knows to trigger change detection. 
There are some special things to do if you write code outside of Angular that needs to trigger change detection, but I won’t cover that here.
One the change detection mechanism is triggered, it will start from the top of the component tree and check each node to see whether the component model has changed and requires rendering. That’s why input properties have to be made known to Angular, or it would fail to know how to detect changes.
Angular has two change detection modes: Default and OnPush. 
The Default mode will always check the component for changes on each change detection cycle. 
Angular has highly optimized this process so that it’s efficient to run these checks—within a couple milliseconds for most cases. That’s important when data is easily mutated between components, and it can be difficult to ensure that values haven’t changed around the application.
You can also use the OnPush mode, which explicitly tells Angular that this component only needs to check for changes if one of the component inputs has changed. That means if a parent component hasn’t changed, it’s known that the child component’s inputs won’t change, so it can skip change detection on that component (and any grandchild components). 
Just because an input has changed doesn’t mean the component itself has to change; perhaps the input is an object with a changed property that the component doesn’t use. Keeping tracking of your data structures in your application can help to optimize when values are passed around and how change detection fires.
Figure 5.1 illustrates the two types of change detection. Imagine there’s a component tree with two properties, and the property 'b' is changed by some user input. The default mode will update the value in the component and then check all components underneath it for changes. The OnPush mode only checks child components that have an input binding specifically for the changed property and skips checking the other components.
I recommend spending time reading about change detection in more detail from one of the people who helped create it, Victor Savkin: https://vsavkin.com/change-detection-in-angular-2-4f216b855d4c.
Our Nodes Row component is a candidate for using OnPush because everything comes into the component via an input and the component’s state is always linked to the values passed in (such as if the utilization is above 70% and the danger classes need to be applied). Open src/app/nodes-row/nodes-row.component.ts and we’ll make a minor adjustment (see the following listing) to enable OnPush for it.

```typescript
import { Component, Input, ChangeDetectionStrategy } from '@angular/core';
@Component({
selector: '[app-nodes-row]',
templateUrl: './nodes-row.component.html',
styleUrls: ['./nodes-row.component.css'],
changeDetection: ChangeDetectionStrategy.OnPush
})
```
That’s it! Import and declare the component changeDetection property to OnPush, and your component is now only going through change detection when the inputs have changed. The strategy is a property of the component metadata, which is set to the Default mode by default (believe it or not!).
Now we can apply the same changes to our Metric component because it also only reflects the input values provided to it when rendered. Try to make the same change to that file yourself.
There’s another way to intercept and detect changes using the OnChanges lifecycle hook, and because we’re already intercepting the inputs with getter/setter methods in the Metric component, let’s modify it again to use OnChanges and OnPush mode.
Open src/app/metric/metric.component.ts and update it to the code you see in listing 5.2. It replaces the getter and setter methods with an OnChanges lifecycle hook and also uses OnPush mode.

```typescript
import { Component, Input, ChangeDetectionStrategy, OnChanges } from
'@angular/core';
@Component({
selector: 'app-metric',
templateUrl: './metric.component.html',
styleUrls: ['./metric.component.css'],
changeDetection: ChangeDetectionStrategy.OnPush
})
export class MetricComponent implements OnChanges {
@Input('used') value: number = 0;
@Input('available') max: number = 100;
    ngOnChanges(changes) {
if (changes.value && isNaN(changes.value.currentValue)) this.value = 0;
if (changes.max && isNaN(changes.max.currentValue)) this.max = 0;
}
isDanger() {
return this.value / this.max > 0.7;
}
}
```
If you implemented the OnPush mode yourself, it should start by importing the strategy helper and then adding the changeDetection property. Here you also import the OnChanges interface and declare the class to implement it.
We still need to declare our input properties, so we go back to the previous way to declare them without the getter and setter methods. The ngOnChanges method implements the lifecycle hook for OnChanges, and it provides a single parameter as an object populated with any changed inputs, which then have their current and previous values available. 
For example, if only the value input was changed in the parent, then only the change.value property will be set on the lifecycle hook.
Inside the OnChange lifecycle hook, we run the same basic checks for whether the values are numeric or not, and if validation fails we reset the value. It should be noted that any changes to the inputs would propagate to any child components (should there be any).
The value of using this approach is that we aren’t creating private properties while still intercepting and validating the inputs, and the logic doesn’t run when the component requests a property. It will only run the lifecycle hook for a component when the inputs have changed for that specific component.
If you ever need to run code every time that change detection is run on a component (regardless of whether you use OnPush or not), you can use the DoCheck lifecycle hook. This allows you to run some logic that can check for changes that exist in your component that Angular can’t detect automatically. If you need to trigger your own type of change detection, this is the lifecycle hook that will help you do that. It’s
less commonly used, but do be aware of its existence for situations where OnChanges doesn’t get you what you need. In this section we’ve optimized the Nodes Row and Metric components' change detection by only checking them when an input has changed. The Metric component also now uses the OnChanges lifecycle hook to validate inputs, which can be more efficient and hooks into the change detection lifecycle.