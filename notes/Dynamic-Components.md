# 🧐 **What is "Programmatic Rendering of Components"?**

Normally in Angular, we do this:

```html
<app-some-component></app-some-component>
```

But **what if** you don’t know **at compile time** which component should be shown?
You want to decide **at runtime** (based on data, conditions, or user interaction).

> 👉 You dynamically **create**, **insert** or **render** a component **in the code** (not in template).

That’s called **Programmatic Rendering**.

---

# 🛠️ **Tools Angular gives us**

| **Tool**              | **What it does**                                           | **When to use**                                                           |
| --------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------- |
| **NgComponentOutlet** | A directive to insert a component dynamically in template  | When component type is **known in template** but set dynamically          |
| **ViewContainerRef**  | A service to programmatically create and insert components | When you want **full control** (create, destroy, move components in code) |

---

# 🚀 **NgComponentOutlet — Example (Easy)**

Let’s say you have 2 components:

```ts
@Component({
  selector: 'app-hello',
  template: `<p>Hello Component</p>`
})
export class HelloComponent {}

@Component({
  selector: 'app-goodbye',
  template: `<p>Goodbye Component</p>`
})
export class GoodbyeComponent {}
```

Now in **ParentComponent**, you want to render one of them based on a condition.

```ts
@Component({
  selector: 'app-parent',
  template: `
    <ng-container *ngComponentOutlet="componentToRender"></ng-container>

    <button (click)="showHello()">Show Hello</button>
    <button (click)="showGoodbye()">Show Goodbye</button>
  `
})
export class ParentComponent {
  componentToRender: any;

  showHello() {
    this.componentToRender = HelloComponent;
  }

  showGoodbye() {
    this.componentToRender = GoodbyeComponent;
  }
}
```

### ✅ **Why use it?**

👉 When **you don't need to pass data or do complex logic** — just swap components dynamically.

---

# ⚡ **ViewContainerRef — Example (Full control)**

Now suppose you want **more power**:

* Pass inputs
* Create/destroy many components dynamically
* Listen outputs

Here comes **ViewContainerRef** (along with ComponentFactoryResolver in older Angular — not needed now with Ivy)

---

## 🛠️ Example

Let’s use same HelloComponent:

```ts
@Component({
  selector: 'app-hello',
  template: `<p>Hello Component - {{name}}</p>`
})
export class HelloComponent {
  @Input() name!: string;
}
```

### In **ParentComponent**

```ts
@Component({
  selector: 'app-parent',
  template: `
    <ng-template #container></ng-template>

    <button (click)="createComponent()">Create Component</button>
    <button (click)="clear()">Clear</button>
  `
})
export class ParentComponent {

  @ViewChild('container', {read: ViewContainerRef}) container!: ViewContainerRef;

  createComponent() {
    const componentRef = this.container.createComponent(HelloComponent);
    componentRef.instance.name = 'Angular Dev';   // pass input
  }

  clear() {
    this.container.clear();   // remove all dynamically created components
  }
}
```

---

# 🥇 **NgComponentOutlet vs ViewContainerRef (Ultimate Summary)**

| **Feature**                 | **NgComponentOutlet**                     | **ViewContainerRef**                                       |
| --------------------------- | ----------------------------------------- | ---------------------------------------------------------- |
| **Usage**                   | Template-driven dynamic component         | Code-driven dynamic component                              |
| **Passing Inputs**          | Via ngComponentOutletInputs (Angular 14+) | Via componentRef.instance                                  |
| **Control**                 | Less control                              | Full control                                               |
| **Destroy/Move Components** | No                                        | Yes                                                        |
| **Use case**                | Simple component swapping                 | Complex dynamic component rendering, dynamic forms, modals |

---

# 🎯 **Common Interview Questions (and sharp answers)**

| **Q (Interviewer)**                                        | **Smart Answer (You)**                                                                                                               |
| ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| When do you use ViewContainerRef?                          | When I need full programmatic control over component creation, destruction, positioning or interaction (e.g., dynamic forms, modals) |
| Can you pass @Input() to dynamically created component?    | Yes — via `componentRef.instance.inputName = value`                                                                                  |
| Difference between NgComponentOutlet and ViewContainerRef? | NgComponentOutlet is for simple swapping in template. ViewContainerRef is for complex control fully in TypeScript                    |
| How do you clear dynamically created components?           | `viewContainerRef.clear()`                                                                                                           |
| Can NgComponentOutlet pass inputs?                         | Yes, from Angular 14 onwards → `*ngComponentOutlet="comp; ngComponentOutletInputs: {input: value}"`                                  |

---

# 🔥 **Real Practical Use Cases (Interview friendly)**

| **Use case**                          | **What tool?**    |
| ------------------------------------- | ----------------- |
| Dynamic modal/popups                  | ViewContainerRef  |
| Form builder (dynamic fields)         | ViewContainerRef  |
| Dynamic dashboards/widgets            | ViewContainerRef  |
| Simple tab-switching                  | NgComponentOutlet |
| Dynamic component based on route/data | NgComponentOutlet |

---

# 💡 **How to answer confidently (in interview)**

> *"If I want simple component switching, I’ll use NgComponentOutlet.
> If I want to programmatically create, configure, or control multiple dynamic components (like modals, form fields, widgets), I’ll use ViewContainerRef."*

---

# 🛠️ **Scenario**

We will dynamically create a **ChildComponent** that:

* Accepts an `@Input()` called `name`
* Emits an `@Output()` called `greet`

Then we’ll:
✅ Pass the input when creating the component dynamically
✅ Listen to output and handle the event

---

# 1️⃣ **ChildComponent (to be created dynamically)**

```ts
@Component({
  selector: 'app-child',
  template: `
    <p>Hello {{name}}</p>
    <button (click)="sayHello()">Greet</button>
  `
})
export class ChildComponent {
  @Input() name!: string;
  @Output() greet = new EventEmitter<string>();

  sayHello() {
    this.greet.emit(`Hello from ${this.name}`);
  }
}
```

---

# 2️⃣ **ParentComponent (where we dynamically render it)**

```ts
@Component({
  selector: 'app-parent',
  template: `
    <ng-template #container></ng-template>

    <button (click)="createComponent()">Create Dynamic Child</button>
    <button (click)="clear()">Clear</button>
  `
})
export class ParentComponent {
  @ViewChild('container', {read: ViewContainerRef}) container!: ViewContainerRef;

  createComponent() {
    const componentRef = this.container.createComponent(ChildComponent);

    // ✅ Pass @Input() value
    componentRef.instance.name = 'Angular Dev';

    // ✅ Listen to @Output() event
    componentRef.instance.greet.subscribe((message: string) => {
      console.log('Received from child:', message);
      alert(message);   // Show the greeting
    });
  }

  clear() {
    this.container.clear();
  }
}
```

---

# ✅ **How it works**

* We create component → `container.createComponent(ChildComponent)`
* We assign input → `componentRef.instance.name = 'Angular Dev'`
* We subscribe to output → `componentRef.instance.greet.subscribe(...)`

---

# 🥇 **How to explain this in interview (confidently)**

> *"After dynamically creating the component with ViewContainerRef, I can pass @Input() values directly on `componentRef.instance`. For @Output(), I simply subscribe to the EventEmitter exposed on `componentRef.instance`. It works the same as normal component communication, just done manually in code."*

---

# ⚡ **Pro Tip (Interviewer might ask)**

| **Q**                                                       | **Sharp Answer**                                                                                                                               |
| ----------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| What happens to subscription when component is destroyed?   | If I don't unsubscribe manually, Angular automatically cleans up the EventEmitter when the component is destroyed (since component is removed) |
| Can I pass multiple @Input() and listen multiple @Output()? | Yes — I just assign each input and subscribe to each output separately on `componentRef.instance`                                              |

---

# 🎁 **Bonus Tip (Optional but impressive)**

If you're creating **many dynamic components** and want **auto-unsub**, use **`takeUntilDestroyed()`** pipe (Angular 16+)
I can show that too if you want.

---

# 🖼️ **Summary Diagram (Easy to remember)**

```
createComponent() → componentRef.instance.input = value
                 → componentRef.instance.output.subscribe()
```

---


# 🛠️ **Scenario**

We want to dynamically render **form fields** (based on config).
Example config:

```ts
fields = [
  { type: 'text', label: 'First Name', name: 'firstName' },
  { type: 'text', label: 'Last Name', name: 'lastName' },
  { type: 'email', label: 'Email', name: 'email' }
];
```

We’ll:
✅ Dynamically create input components
✅ Bind them to a **FormGroup**
✅ Capture the form value

---

# 1️⃣ **TextFieldComponent (dynamic input component)**

```ts
@Component({
  selector: 'app-text-field',
  template: `
    <label>{{label}}</label>
    <input [type]="type" [formControl]="control" />
  `
})
export class TextFieldComponent {
  @Input() label!: string;
  @Input() type: string = 'text';
  @Input() control!: FormControl;
}
```

---

# 2️⃣ **ParentComponent (dynamic form generator)**

```ts
@Component({
  selector: 'app-dynamic-form',
  template: `
    <form [formGroup]="form">
      <ng-template #container></ng-template>

      <button (click)="submit()">Submit</button>
    </form>
  `
})
export class DynamicFormComponent {
  @ViewChild('container', {read: ViewContainerRef}) container!: ViewContainerRef;

  form = new FormGroup({});
  fields = [
    { type: 'text', label: 'First Name', name: 'firstName' },
    { type: 'text', label: 'Last Name', name: 'lastName' },
    { type: 'email', label: 'Email', name: 'email' }
  ];

  ngAfterViewInit() {
    this.generateForm();
  }

  generateForm() {
    this.fields.forEach(field => {
      // Create a control for the field
      const control = new FormControl('');
      this.form.addControl(field.name, control);

      // Dynamically create TextFieldComponent
      const componentRef = this.container.createComponent(TextFieldComponent);

      // Pass @Input() values
      componentRef.instance.label = field.label;
      componentRef.instance.type = field.type;
      componentRef.instance.control = control;
    });
  }

  submit() {
    console.log(this.form.value);
    alert(JSON.stringify(this.form.value, null, 2));
  }
}
```

---

# ✅ **How it works**

1. We have config → **fields array**

2. For each field:

   * We create a **FormControl** and add to **FormGroup**
   * We dynamically create **TextFieldComponent**
   * We pass `label`, `type`, and bind `control`

3. The form works like normal, but fields are **generated dynamically**

---

# 🥇 **How to explain this in interview**

> *"We generate form fields dynamically by iterating config, creating FormControls, and rendering TextFieldComponent dynamically with ViewContainerRef. We pass required inputs and bind each component to its FormControl. This allows highly flexible dynamic forms where fields come from backend or config at runtime."*

---

# 🎯 **Where is this used in real world?**

| **Use case**              | **Why dynamic form?**                  |
| ------------------------- | -------------------------------------- |
| Form builder apps         | Users define fields at runtime         |
| Survey/questionnaire apps | Questions change based on backend data |
| CMS content forms         | Fields vary based on content type      |

---

# ⚡ **Bonus (Advanced — Interview Gold)**

If you want to support **multiple field types** (dropdown, checkbox, etc.),
you can:

* Create multiple components (`DropdownFieldComponent`, etc.)
* Dynamically decide **which component to create** based on `field.type`

> I can show **multi-field dynamic form** too if you want (very interview friendly)

---
