# \#6: 📥 Property binding

We now have our input-button-unit component, but it does not do much. We want to bring it to life.

Let's add an HTML input element and make its control text reflect the value of the `title` property.

We'll revert the component to its state before our experiments with its methods:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-input-button-unit',
  template: `
    <p>
      input-button-unit works!
      The title is: {{ title }}
    </p>
  `,  
  styleUrls: ['./input-button-unit.component.css']  
})    
export class InputButtonUnitComponent implements OnInit {
  title = 'Hello World';           

  constructor() { }                     

  ngOnInit() {
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Let's add an input element and a button to the template:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
template: `
  <p>
    input-button-unit works!
    The title is: {{ title }}
  </p>
  
  <input>
  <button>Save</button>
`,
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Reminder: We use interpolation to present the value of the `title` property: `{{ title }}`. Angular then presents the value of `title` each time that our `app-input-button-unit` component is shown.

What if we want to show the title value inside the HTML input control itself?

Every `input` element has a property called `value`, which holds the string that is displayed inside the `input` box. In HTML, we can pass a string directly to the element's `value` attribute:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<input value="Hello World">
```
{% endcode-tabs-item %}
{% endcode-tabs %}

But we lose the dynamic binding between the properties in the controller and the template.

Angular lets us bind properties to the template easily and conveniently; we saw that with interpolation. Now we'll see how to bind to an **element's property** \(not to be confused with class properties\). **We surround the wanted property with square brackets and pass it the class member**:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<input [value]="title">
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Try this out and see the result in the browser!

## Binding to Methods

The expressions that we can bind to in the template are not limited to class properties. They can be a method call or almost any other valid JavaScript expression.

![lab-icon](.gitbook/assets/lab%20%281%29.jpg) **Playground**: For example, let's bind the input value to a method call that returns a value. First, let's add the method `generateTitle` anywhere inside the class, but not inside any of its methods.

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
generateTitle(): string {
  return 'This title was generated by a method.';
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Replace one or both of the bindings of the title in the template with the method call \(don't forget the parenthesis!\):

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
  <input [value]="generateTitle()">

  {{ generateTitle() }}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Change Detection

Angular has a very efficient change detection mechanism. It looks for bindings in the components' templates, and then updates the value each time the bound expression is changed.

![lab-icon](.gitbook/assets/lab%20%281%29.jpg) **Playground**: To show this, let's change the value of the title after a few seconds and see what happens. Call the `setTimeout` function inside `ngOnInit`:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
ngOnInit() {
  setTimeout(() => {
    this.title = 'This is not the title you are looking for';
  }, 3000);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

`setTimeout` is a JavaScript function. Its first parameter is what we want to happen - a function of our choice. The second parameter is how much we want to delay it, in milliseconds. In this example, we pass an **inline anonymous function** which sets the value of `this.title`. For this we use one of the new features in JavaScript ES6: an **arrow function**.

## Binding to Methods

The expressions that we can bind to in the template are not limited to class properties. They can be a method call or almost any other valid Angular template expression.

## Resources

[Angular Guide - Template Property Binding](https://angular.io/guide/template-syntax#property-binding--property-)

## A note about accessing the DOM

Using regular JavaScript, we can insert the value to the input via its properties. We'll fetch the element from the DOM and assign the value of the member `title` to the element's `value` property.

{% code-tabs %}
{% code-tabs-item title="code for example" %}
```typescript
let inputElement = document.getElementById('my-input');
inputElement.value = this.title;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

In JavaScript, we find the `input` element in the DOM by its id, and then set its `value` property to the value of the title property. We need to add the id to the `input` element then:

{% code-tabs %}
{% code-tabs-item title="code for example" %}
```markup
<input id="my-input">
```
{% endcode-tabs-item %}
{% endcode-tabs %}

This will work in the browser.

However, **this is highly discouraged in Angular. You should never access the DOM directly!** That's because you can assign different renderers to Angular and run the application on different platforms. They may be renderers for mobile, desktop, or even a robot. These platforms will not have a `document` object from which you can manipulate the result!
