# \#3: 📐 Component

One approach in Web development \(and software development generally\) is component-based architecture. In the past years it has gained a lot of popularity. What is a component?

In [Service-Oriented Architecture \(SOA\) vs. Component Based Architecture](http://petritsch.co.at/download/SOA_vs_component_based.pdf), Helmut Petritsch defines a component as follows:

> A component is a software object, meant to interact with other components, encapsulating certain functionality or a set of functionalities. A component has a clearly defined interface and conforms to a prescribed behaviour common to all components within an architecture.

In Web applications, **a component controls a patch of screen called a view**. It's a part of what you will eventually see on the screen. It has a template, which defines its visual structure. It also has logic which defines the behavior and the dynamic values. The logic part is JavaScript code and is called the controller.

Here's a diagram of a component in Angular, with the result below.

![](.gitbook/assets/image.png)

Directives, pipes and services are other building blocks in Angular, which can be used in a component \(in the diagram we see only the usage of a pipe\). We will discuss them later in the tutorial.

Let's take a look at the component that was created by the Angular CLI. All the relevant files exist in the folder `src/app`. Open the file `app.component.ts`.

Just like ngModules that we saw in the previous chapter, a component is also defined by a class with a decorator. This is the class definition:

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.ts" %}
```typescript
export class AppComponent {
  title = 'app';
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

It has one member called "title". It is a property to which you can assign a value. The value assigned to it here is the string "app".

Angular takes care of synchronizing the members of the component with the component template. So we can easily use the member `title` in the template. Take a look at the template attached to the component in the file `app.component.html`. Near the top, you'll see code like this:

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.html" %}
```markup
<h1>
  Welcome to {{ title }}!
</h1>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The double curly braces and their content are called **Interpolation**. This is one form of **data binding** in Angular. As we mentioned before, the code in this file is not used as-is when the browser renders the component. Angular compiles it to JavaScript code. In one of the compilation steps, it looks for Interpolations inside the template. The content of the Interpolation is an "Angular template expression" \(which is a lot like JavaScript\). The expression is evaluated at runtime, and then you see the result.

Interpolation is one of the strongest, most basic features in Angular. It has existed from the very beginning of Angular - in the first version \(AngularJS\). It makes it really simple to insert dynamic data into the view.

In this component, the expression is simply the member of the component class, `title`. **Let's try to change it.** Try out the following and see the result in the browser. \(With every change you make in the file, the browser will refresh automatically!\)

* Remove the curly braces and keep just the content `title`.
* Put the curly braces back and replace the content with some mathematical expression, for example: `{{ 2 + 2 }}`. \(The spaces are not mandatory, they just make the code more readable.\)
* Write a mathematical expression combined with the `title` member: `{{ title + 10 }}`
* Pass an undefined variable to the expression - a variable which was not declared in the component class. For example: `{{ x }}`
* Try out anything you'd like. Don't worry - you can't do any harm to the browser or the computer! In the worst case, the browser will run out of memory and will get stuck. \(But you'll have to write something really complicated to make that happen!\)

This is one way that you can bind members of the component's controller to its template. How does Angular know that this is the template of the App component?

Let's go back to the file `app.component.ts` and look at the component's metadata defined in the decorator `@Component` right above the class definition:

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.ts" %}
```typescript
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
```
{% endcode-tabs-item %}
{% endcode-tabs %}

We pass an object of definitions to the decorator, just like we saw in the previous chapter with ngModule. The second property, `templateUrl`, tells Angular where to look for the template attached to the component. There is another option to point to the template, which we'll discuss later: to write the whole template inline here, in the component definition.

The third property, `styleUrls`, tells Angular where to look for the CSS files that define the style of this component. It can have multiple CSS files. That's why the value of `styleUrls` is an array. You can take a look at the CSS file `app.component.css` - you'll see that it's empty. You can add some CSS style here, for example:

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.css" %}
```css
h1 {
  color: red;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

We'll add more style later on.

**Note:** the Angular CLI supports css-extension languages out-of-the-box: sass, less, and stylus.

The first property, `selector`, tells Angular what will be the name of the tag that we'll use to call the component. As we saw in the file `src/index.html`, we use the app component inside the body:

{% code-tabs %}
{% code-tabs-item title="src/index.html" %}
```markup
<body>
  <app-root></app-root>
</body>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The element `app-root` is not an HTML element. It is the component that was created with the selector `app-root`. Try changing the selector. You'll see that if you change it in only one of the files, nothing will be displayed, since the element is no longer replaced with an Angular component. You can see an error message in the browser's console.

One last thing: the first line in the component file imports the code that defines the decorator `@Component`. It is needed to use the decorator, which is defined in the imported file \(or actually, in one of its own imports\). Try removing this line, and see the error.

## Inline Template

Let's move the template to be **inline** in the component definition. This will help us manage the template while looking at its functionality. In the file `app.component.ts` replace the line

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.ts" %}
```typescript
templateUrl: './app.component.html',
```
{% endcode-tabs-item %}
{% endcode-tabs %}

with

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.ts" %}
```typescript
template: ``,
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Notice the **backticks** - **\`** - they are used to define Template Literals, which are new in JavaScript \(ES6\). This way you can define multi-line strings. They have another cool ability: to easily use JavaScript variables and expressions within the string \(with no relation to Angular binding expressions in the template\). Read about it in the [MDN documentation](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals).

Make sure you replace `templateUrl` with `template`, and don't forget the comma at the end of the line.

Now instead of taking the content from the original template, we'll insert a more simple template which we can work with. When the cursor is between the backticks, press Enter and insert the HTML template:

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.ts" %}
```markup
template: `
  <h1>
    Welcome to {{ title }}!
  </h1>  
`,
```
{% endcode-tabs-item %}
{% endcode-tabs %}

It is easier to manage the template when you see its controller at the same time. This is true as long as the template doesn't get too big and the controller doesn't get too complicated. If they do, it's a sign you should refactor your code by breaking it down to child components.

At this point you can delete the file `app.component.html`.

**We recommend continuing this tutorial using inline templates in the components.** Especially if you're working on a laptop with a small screen, where there isn't enough space to open two files side-by-side.

Let's configure the Angular CLI to give us inline-template as a default. In the terminal run the command: `ng set defaults.component.inlineTemplate true`. Now every component that you'll generate will have an inline template, and an HTML file will not be created.

If you wish to continue this tutorial with templates in separate HTML files, do not run this command, and use the generated `.html` files for the templates.

> **Note:** You can specify that you'd like to use inline-template throughout the project in several ways:
>
> * When generating a project, pass the flag `-it` or `--inline-template` like this: `ng new todo-list -it`
> * After generating a project, add it to the configuration so that components generated from this point on will have an inline template: `ng set defaults.component.inlineTemplate true`. \(From version 6 you'll need to use the command `ng config projects.YOURPROJECTNAME.schematics.@schematics/angular:component.inlineTemplate true` instead\) This adds the line `inlineTemplate: true` in the Angular CLI configuration file `.angular-cli.json` \(`angular.json` in version 6\). You can also edit the file directly.
> * If you haven't configured to have inline templates as a default, you can specify this per component when you generate it, by passing the flag `-it` or `--inline-template`. For example: `ng generate header -it`.

The same way we use inline template, we can use also inline styles. But for now we will keep the styles in a separate file.

## Summary

We have explored the root component that was generated for us by the Angular CLI, and even refactored it. In the next chapter, we will create a new component. We will start building the tree of components, which defines the structure of the application.

