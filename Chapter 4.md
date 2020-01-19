# Components Basics
Components are so central to how Angular applications are structured that
almost every feature is somehow linked to them. It’s impossible to make an Angular
application without a component, after all. This means being able to harness
the capabilities of components is vital to any developer. They are so important,
I’ve dedicated the next chapter to additional, more advanced topics involving
components.
You saw a couple of components in action in the chapter 2 examples, but in this
chapter we’ll start with the basics of components to ensure that you have a clear overview
of how they’re declared and designed. We’ll then look at some of the additional
capabilities of components that you’ll use most frequently.
A component includes a template, which is the HTML markup used to describe
its visual layout and behavior. We’ll look at how to make the most of these templates,
understand how they’re rendered, and give them individual stylings.
A component also creates a view, which is the rendered result of a component that
the user can interact with and is comprised of rendering the component template.
Templates may include references to other components, which will trigger them to also
be rendered as part of the rendering of the parent component. As discussed in chapter 3,
an Angular application is a tree of components that all start with the App component.
During this chapter, we’ll build a realistic-looking dashboard that contains several
components, and we’ll use mock data to simplify the implementation and focus purely
on the components themselves. Let’s set up the example.