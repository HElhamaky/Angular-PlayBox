## Summary
* What is TypeScript?
    * It's a **Superset** of JavaScript
* What is the benefits of using TypeScript?
    * **Strong Typing**
    * **Object Oriented Features( Classes - Interfaces - Constructors - Generics)**
    * **Compile-time Errors**
    * **Tooling (Intellisense)**
* How to install TypeScript on your machine?
    * Using this command `npm install -g typescript` and check it using `tsc --version`
* How to Declare a variable in TypeScript ?
    * There are two ways. Either `var number = 1;` Or `let number = 1;`
*  What is the difference between `var` and `let`?
    * A variable declared with `var` is scoped to the nearest **function block level**.
    * A variable declared with `let` is scoped to the **nearest block** level.
    
    ```typescript
    function doSomething(){
        for(var i = 0 ; i<5 ; i++){
            console.log(i);
        }
        console.log('Finally: ' +i);
    }

    doSomething();
    ```
* What is new types introduced by TypeScript?
    * Number `let a: number = 5;`
    * String  `let b: string = 'a';`
    * Boolean `let c: boolean = true;`
    * Any `let d: any;`
    * Array `let e: number[] = [1, 2, 3];` Or `let f: any[] = [1, true, 'a', false];`
    * enum
        ```typescript
        enum Color { Red, Green, Blue}
        let backgroundColor = Color.Red;
        ```
* How Type Assertion can be done in TypeScript?
    * let's take **string** type for Example 
        * `let messsage = 'ABC';`
        * `let message;` then `<string>message` or `message as string`
* How you write `function(message){ console.log(message)}` as an **Arrow** function?
    * It can be written as `(message) => console.log(message)`
* How to convert Inline Annotation to Interface ? 
    * Inline Annotation
    ```typescript
    let drawPoint = (point:{x:number , y:number}) => {
    //....
    }

    drawPoint({
        x: 1,
        y: 2
    })
    ```
    * Interface
    ```typescript
    interface Point {
        x: number, 
        y: number
    }

    let drawPoint = (point: Point) => {
    //....
    }

    drawPoint({
        x: 1,
        y: 2
    })
    ```
* What is the importance of calsses?
    * It Groups variables(properties) and functions that are **highly related** [Cohesion Princible].

## Notes
* Any Valid JavaScript Code, is a Valid TypeScript Code.
* Interfaces are purely for declarations, they can't include any implementations.
* When a function is part of a class it's called a Method.
* An Object is an Instance of a Class.
