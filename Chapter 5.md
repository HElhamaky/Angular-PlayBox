# Advanced components
> Chapter 4 covered many of the basics of components, but there is so much more!
Here we’ll dive into additional capabilities that will come in handy as you build
more interesting applications.
We’ll look at how change detection works in more detail, and look at how to use
the OnPush capability to reduce the amount of work Angular has to do to improve
rendering efficiency.
Although components can use inputs and outputs, there are additional ways to
have components talk to one another. We’ll look at why you might choose to use a
different approach and how to implement it.
There are three primary ways to render styles for a component, and choosing
different modes potentially has a significant impact on the way your components are
displayed. You may want to internalize the CSS styling as much as possible, or you may
not want any styling to be internalized, but to use the global CSS styles instead.
Finally, we’ll look at how to render a component dynamically, and why you might
want to do that. It’s not very common, but there are moments where it’s useful.
As discussed in chapter 4, I highly recommend that you keep your components
focused. As we learn more about component capabilities, you’ll see that it’s important
to avoid overloading your components.
This chapter will continue with the example from chapter 4, so refer to it for how to
set up the example. Everything will build upon what you learned, so the examples will
be expanded to demonstrate more advanced capabilities of components.
I can say with almost 100% certainty that no component will use every single capability,
because it would most likely not function. However, mastery of these additional concepts
will help you write more complex and dynamic applications. Let’s start by taking a
look at change detection and how to optimize performance.
Change detection and optimizations
Angular ships with a change detection framework that determines when components
need to be rendered if inputs have changed. Components need to react to changes
made somewhere in the component tree, and the way they change is through inputs.
Changes are always triggered by some asynchronous activity, such as when a user
interacts with the page. When these changes occur, there is a chance (though no guarantee)
that the application state or data has changed. Here are some examples:
¡ A user clicks a button to trigger a form submission (user activity).
¡ An interval fires every x seconds to refresh data (intervals or timers).
¡ Callbacks, observables, or promises are resolved (XHR requests, event streams).
These are all events or asynchronous handlers, but they may come from different
sources. We’ll dig deeper into the way that observables and XHR requests behave in
other chapters, but here we’re curious about how user actions and an interval trigger
changes in Angular.
Angular has to know that an asynchronous activity occurred, but the setInterval and
setTimeout APIs in JavaScript occur outside Angular’s awareness. Angular has monkey-
patched the default implementation of setInterval and setTimeout to have them
properly trigger Angular’s change detection when an interval or timeout is resolved.
Likewise, when an event binding is handled in Angular, it knows to trigger change
detection. There are some special things to do if you write code outside of Angular that
needs to trigger change detection, but I won’t cover that here.
One the change detection mechanism is triggered, it will start from the top of
the component tree and check each node to see whether the component model has
changed and requires rendering. That’s why input properties have to be made known
to Angular, or it would fail to know how to detect changes.
Angular has two change detection modes: Default and OnPush. The Default mode
will always check the component for changes on each change detection cycle. Angular
has highly optimized this process so that it’s efficient to run these checks—within
a couple milliseconds for most cases. That’s important when data is easily mutated
between components, and it can be difficult to ensure that values haven’t changed
around the application.
You can also use the OnPush mode, which explicitly tells Angular that this component
only needs to check for changes if one of the component inputs has changed. That
means if a parent component hasn’t changed, it’s known that the child component’s
inputs won’t change, so it can skip change detection on that component (and any grandchild
components). Just because an input has changed doesn’t mean the component
itself has to change; perhaps the input is an object with a changed property that the
component doesn’t use. Keeping tracking of your data structures in your application can
help to optimize when values are passed around and how change detection fires.
Figure 5.1 illustrates the two types of change detection. Imagine there’s a component
tree with two properties, and the property 'b' is changed by some user input. The
default mode will update the value in the component and then check all components
underneath it for changes. The OnPush mode only checks child components that have
an input binding specifically for the changed property and skips checking the other
components.