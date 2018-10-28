# \#10: ➕ New component: todo-item

We will create a new component which will be used for each todo item that is displayed on the list. It will be a simple component at first, but it will grow later on. What's important is that **it will get the todo item as an input from its parent component**. This way it can be a reusable component, and not rely directly on the application's data and state.

Create a new component called `todo-item`:

```text
ng g c todo-item
```

You can see a new folder was created - `src/app/todo-item`, with the component files inside.

Use the new component in the template of `app-root` component - inside the `<li>` element:

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.ts" %}
```markup
<ul>
  <li *ngFor="let item of todoList">
    <app-todo-item></app-todo-item>
  </li>
</ul>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Check out the result in the browser. What do you see? Why?

## @Input\(\)

We want to display the title of each item within the `todo-item` component. We need to pass the current item in the loop to the `todo-item` component.

Again, Angular makes it really easy for us, by providing the `Input` decorator.

Inside the newly generated `TodoItemComponent` class in `todo-item.component.ts` add the line:

{% code-tabs %}
{% code-tabs-item title="src/app/todo-item/todo-item.component.ts" %}
```typescript
@Input() item;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

It tells the component to expect an input and to assign it to the class member called `item`. Make sure that `Input` is added to the `import` statement in the first line in the file. Now we can use it inside the `todo-item` template and extract the item's title with interpolation: `{{ item.title }}`

The component should look like this now:

{% code-tabs %}
{% code-tabs-item title="src/app/todo-item/todo-item.component.ts" %}
```typescript
import { Component, Input, OnInit } from '@angular/core';

@Component({
  selector: 'app-todo-item',
  template: `
    {{ item.title }}
  `,
  styleUrls: ['./todo-item.component.css']
})
export class TodoItemComponent implements OnInit {
  @Input() item;

  constructor() { }

  ngOnInit() {
  }

}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now we need to pass an item where we use the component. Go back to `app-root` component and pass the item title to the `todo-item`:

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.ts" %}
```markup
<ul>
  <li *ngFor="let todoItem of todoList">
    <app-todo-item [item]="todoItem"></app-todo-item>
  </li>
</ul>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The `item` here in square brackets is the same as declared as the component's `@Input`.

We used property binding on an element we created ourselves! And now we can actually see and understand that property binding binds to an actual property of the component. Soon we'll see how this list can be dynamic.

