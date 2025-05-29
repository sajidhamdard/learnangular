### **What is a Decorator in Angular?**

A **decorator** is a **function** that adds **metadata** to a class, method, property, or parameter, telling Angular **how to process** that element.

They are **TypeScript features** (not Angular-specific), but Angular uses them heavily to **define and configure** components, services, directives, modules, pipes, etc.

---

### **Types of Decorators in Angular** (give examples!)

1. **Class Decorators** — Attach metadata to classes
   ➡️ Used to tell Angular what kind of thing the class is.

| Decorator       | Purpose                                                           |
| --------------- | ----------------------------------------------------------------- |
| `@Component()`  | Marks a class as an Angular component and provides configuration. |
| `@Directive()`  | Marks a class as an Angular directive.                            |
| `@Injectable()` | Marks a class as available to be injected as a dependency.        |
| `@NgModule()`   | Marks a class as an Angular module.                               |

```ts
@Component({
  selector: 'app-example',
  templateUrl: './example.component.html'
})
export class ExampleComponent {}
```

2. **Property Decorators** — Attach metadata to class properties
   ➡️ Used for **Input/Output binding** or **Dependency Injection**

| Decorator        | Purpose                                      |
| ---------------- | -------------------------------------------- |
| `@Input()`       | Declares a data-bound input property.        |
| `@Output()`      | Declares an event-bound output property.     |
| `@HostBinding()` | Binds a property to a host element property. |

```ts
@Input() userId: number;
@Output() userChanged = new EventEmitter<number>();
```

3. **Method Decorators** — Attach metadata to methods
   ➡️ Less common, but used in cases like **HostListeners**.

| Decorator         | Purpose                                 |
| ----------------- | --------------------------------------- |
| `@HostListener()` | Binds a method to a host element event. |

```ts
@HostListener('click', ['$event'])
onClick(event: Event) {
  console.log('Element clicked', event);
}
```

4. **Parameter Decorators** — Attach metadata to constructor parameters
   ➡️ Used in **dependency injection**.

| Decorator                           | Purpose                               |
| ----------------------------------- | ------------------------------------- |
| `@Inject()`                         | Specifies a custom provider token.    |
| `@Optional()`                       | Marks a dependency as optional.       |
| `@Self()`, `@SkipSelf()`, `@Host()` | Control injector resolution behavior. |

```ts
constructor(@Inject('API_URL') private apiUrl: string) {}
```

---

### **Behind the scenes** — Why does Angular use decorators?

Because Angular uses **Reflection + metadata** to understand your classes at runtime.
Decorators attach that metadata, and Angular uses it during **compilation** and **runtime DI (Dependency Injection)**.

---

### **When asked in interview — Good structured answer** 👇

> “Decorators in Angular are special functions that attach metadata to classes, properties, methods, or parameters. Angular uses this metadata to understand how to process and instantiate those elements.
>
> The most common decorators are:
>
> * Class decorators like `@Component`, `@Directive`, `@Injectable`, `@NgModule`.
> * Property decorators like `@Input`, `@Output`, `@HostBinding`.
> * Method decorators like `@HostListener`.
> * Parameter decorators like `@Inject`, `@Optional`.
>
> These decorators help Angular know whether a class is a component, service, or directive; or help with binding and dependency injection.”

---

### **Bonus Tip (for senior interviews)**

You can add:

> "Under the hood, decorators leverage TypeScript’s **experimental decorator feature**, and Angular uses **Reflect Metadata API** to store and read metadata attached by these decorators."

---
Short answer: **No**, they are **not mandatory**.

Let’s make this clear (so you can answer confidently in interviews):

---

### **In Angular, a component requires only two things** to be valid:

1. **A selector** (to identify the component in the template)
2. **A template** (which defines the view)

BUT —
That "template" can be **empty**, **inline**, or **external**.

---

### ✅ **These are all valid components**

1️⃣ **Empty template**

```ts
@Component({
  selector: 'app-empty',
  template: ''
})
export class EmptyComponent {}
```

2️⃣ **Inline template (with no CSS)**

```ts
@Component({
  selector: 'app-inline',
  template: '<p>Hello World</p>'
})
export class InlineComponent {}
```

3️⃣ **Template + Styles — (optional)**

```ts
@Component({
  selector: 'app-styled',
  template: '<p>Styled component</p>',
  styles: ['p { color: red; }']
})
export class StyledComponent {}
```

---

### **Key Point for Interview**

> "In Angular, a component **must** have a template (even an empty one), but **CSS is optional**.
> If I don't provide `styles` or `styleUrls`, Angular simply applies no extra styles to the component."

---

### **Caution (Common mistake)**

If you forget both `template` **and** `templateUrl` →
⚠️ Angular will throw an error — because it doesn't know **what to render**.

```ts
@Component({
  selector: 'app-invalid'
  // Missing template/templateUrl → ERROR
})
export class InvalidComponent {}
```

---

### **Crisp Interview Answer**

> "In Angular, the `template` is required but can be empty.
> `styles` or `styleUrls` for CSS are completely optional.
> If neither template nor templateUrl is provided, Angular throws an error because it doesn't know what to render."

---
