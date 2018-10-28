# \#5: 💼 Class

A class is a special programmatic structure. It is defined with **members** which can be **properties** \(variables\) and **methods** \(functions\). Then instances of the class are created, usually by calling the `new` operator on the class: `let myInstance = new myClass();`. The instance created is an object on which you can call the class methods and get and set the values of its properties. Multiple instances can be created from one class.

## In Angular...

Angular takes care of creating instances of the classes you define - if they are recognized as Angular building blocks. The decorators make that connection with Angular.

Each time you use a component in a template, a new instance of it is created. For example, here three instances of the InputButtonUnitComponent class will be created:

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.ts" %}
```markup
// example only

template: `
  <app-input-button-unit></app-input-button-unit>
  <app-input-button-unit></app-input-button-unit>
  <app-input-button-unit></app-input-button-unit>
`,
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Let's take a look at the class `InputButtonUnitComponent`.

## implements OnInit

First, you see something was added to the class declaration:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
export class InputButtonUnitComponent implements OnInit {
  ...
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

`OnInit` is an **interface** - a structure defined but not implemented as a class. It defines which properties and/or methods should exist on the class that implements it. In this case, `OnInit` is an interface for Angular Components which implement the method `ngOnInit`. This method is a **component life-cycle method**. Angular will call this method after the component instance has been created.

The Angular CLI adds this statement to remind us that it's best to initialize things on the component through the `ngOnInit` method. You can see it also added the method in the body of the class:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
ngOnInit() {
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

You can use this method without explicitly indicating that the class implements the `OnInit` interface, but it's useful to use the implementation statement. To see why, delete the `ngOnInit` method. The IDE will tell you there's an error - you must implement `ngOnInit`. How does it know that? Because of `implements OnInit`.

## constructor

Another method we didn't see in the `app-root` component is the constructor. It is a method that is called by JavaScript when an instance of the class is created. Whatever is inside this method is used to create the instance. So it is called before `ngOnInit`.

> A strong feature in Angular that uses the constructor is dependency injection. We'll get to that later on, when we start using services.

## Properties

The property `title` we added is used to store a value, in our case of type string. Each instance of the class will have its own `title` property, meaning you can change the value of `title` in one instance, but it will remain the same in the other instances.

In TypeScript, we must declare members of the class either in the class body outside any method, or pass them to the constructor - as we will see when we use services.

You can declare a property without initializing it:

```typescript
title: string;
```

Then you can assign a value at a later stage, for example in the constructor or in the ngOnInit method. Here we explicitly noted that `title` is of the type `string`. \(The type is inferred by TypeScript when we immediately assign a value, so there's no need to add the type in this case.\)

When referencing a member of the class from within a class method you must prefix it with `this`. It's a special property that points at the current instance.

Try setting a different value for `title` from inside the constructor. See the result in the browser:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
title = 'Hello World';

constructor() { 
  this.title = 'I Love Angular';
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Try changing the value of `title` inside the method `ngOnInit`. Which value will be displayed on the screen?

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
title: string = 'Hello World';

constructor() { 
  this.title = 'I Love Angular';
}

ngOnInit() { 
  this.title = 'Angular CLI Rules!';
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Methods

Let's add a method that changes the value of `title` according to the argument we will pass. We'll call it `changeTitle`. The method will have one parameter of type `string`. Add it **inside the class body** \(but not inside another method\):

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
changeTitle(newTitle: string) {
  this.title = newTitle;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**Note:** Functions and Methods can return a value that can be used when the method is called. For example:

{% code-tabs %}
{% code-tabs-item title="code for example" %}
```typescript
function multiply (x: number, y: number) {
  return x * y;
}

let z = multiptly(4, 5);
console.log(z);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The method `changeTitle` is not used anywhere yet. We can call it from another method or from the template \(which we will see in the following chapters\). Let's call it from the constructor.

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
constructor() { 
  this.changeTitle('My First Angular App');
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

![lab-icon](https://raw.githubusercontent.com/ngHeroes/todo-list-tutorial/master/assets/lab.jpg) **Playground**: You can try calling the method with different arguments \(the string passed inside the brackets\) from `ngOnInit`. Try calling it before or after assigning a value directly to title. Try calling it a few times from the same method. See the result in the browser.

## Debugging Tip

You can always use `console.log(someValue)` inside class methods. Then the value you passed as an argument will be printed in the browser's console. This way you can see the order of the execution of the methods and the value of the argument you pass \(if it's a variable\). For example:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
constructor() { 
  console.log('in constructor');
  this.changeTitle('My First Angular App');
  console.log(this.title);
}

changeTitle(newTitle: string) {
  console.log(newTitle);
  this.title = newTitle;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The browser's console is a part of its Dev Tools. You can see how to open the console in different browsers here: [https://webmasters.stackexchange.com/questions/8525/how-do-i-open-the-javascript-console-in-different-browsers](https://webmasters.stackexchange.com/questions/8525/how-do-i-open-the-javascript-console-in-different-browsers)

