### 1️⃣ **`ngOnInit()`**

**When?** Runs once after all component’s inputs are initialized.
**Use case:** Initialize data, fetch data from API, or set up component-specific logic.

```typescript
@Component({
  selector: 'app-example',
  template: `Hello {{name}}`
})
export class ExampleComponent implements OnInit {
  @Input() name!: string;

  ngOnInit() {
    console.log('ngOnInit called');
    // Example: API call or initializing default values
  }
}
```

👉 **Example:**
If `<app-example [name]="'John'"></app-example>` is rendered, `ngOnInit()` runs once and logs `ngOnInit called`.

---

### 2️⃣ **`ngOnChanges()`**

**When?** Called **whenever @Input() values change** (before `ngOnInit` and also on every change after that).
**Use case:** React to changes in @Input() values.

```typescript
export class ExampleComponent implements OnChanges {
  @Input() name!: string;

  ngOnChanges(changes: SimpleChanges) {
    console.log('ngOnChanges', changes);
  }
}
```

👉 **Example:**
If `name` changes from `'John'` to `'Doe'`, Angular calls `ngOnChanges()` and logs something like:

```js
{ name: { previousValue: 'John', currentValue: 'Doe', ... } }
```

---

### 3️⃣ **`ngDoCheck()`**

**When?** Called **every time change detection runs**, even if @Input() hasn't changed.
**Use case:** Custom change detection logic (e.g., check deep object mutations).

```typescript
export class ExampleComponent implements DoCheck {
  ngDoCheck() {
    console.log('ngDoCheck called');
  }
}
```

👉 **Example:**
Even if no inputs change but a local variable updates (or parent triggers change detection), this hook fires.
Be **careful**, overusing `ngDoCheck` can hurt performance.

---

### 4️⃣ **`ngAfterContentInit()`**

**When?** Runs once after **content projected** into the component (via `<ng-content>`) is initialized.
**Use case:** Interact with projected content.

```typescript
@Component({
  selector: 'app-child',
  template: `<ng-content></ng-content>`
})
export class ChildComponent implements AfterContentInit {
  ngAfterContentInit() {
    console.log('ngAfterContentInit called');
  }
}
```

👉 **Example:**

```html
<app-child>
  <p>Projected content here</p>
</app-child>
```

After projecting the `<p>` into `<ng-content>`, `ngAfterContentInit()` runs once.

---

### 5️⃣ **`ngAfterContentChecked()`**

**When?** Called **every time** the projected content is checked.
**Use case:** Respond to projected content updates.

```typescript
export class ChildComponent implements AfterContentChecked {
  ngAfterContentChecked() {
    console.log('ngAfterContentChecked called');
  }
}
```

👉 **Example:**
If projected content updates (e.g., bound variables in the projected content change), this hook runs again.

---

### 6️⃣ **`ngAfterViewInit()`**

**When?** Runs once after component’s **view** (template + child components) is initialized.
**Use case:** Interact with template references, ViewChild elements, child components.

```typescript
export class ExampleComponent implements AfterViewInit {
  @ViewChild('myDiv') myDiv!: ElementRef;

  ngAfterViewInit() {
    console.log('ngAfterViewInit called');
    console.log(this.myDiv.nativeElement);
  }
}
```

👉 **Example:**

```html
<div #myDiv>Hello View</div>
```

After view initializes, `myDiv` is available to use (e.g., for DOM manipulation).

---

### 7️⃣ **`ngAfterViewChecked()`**

**When?** Called **every time** the component view and child views are checked.
**Use case:** Respond after the view updates.

```typescript
export class ExampleComponent implements AfterViewChecked {
  ngAfterViewChecked() {
    console.log('ngAfterViewChecked called');
  }
}
```

👉 **Example:**
If something in the view (like a child component output) updates and triggers change detection, this hook runs again.

---

### 🚦 **Summary Table**

| Lifecycle Hook            | Runs...                                        | Example use case                   |
| ------------------------- | ---------------------------------------------- | ---------------------------------- |
| `ngOnInit()`              | Once after inputs initialized                  | Fetch API data, init values        |
| `ngOnChanges()`           | Every time @Input() changes                    | React to input changes             |
| `ngDoCheck()`             | Every time change detection runs               | Custom change detection            |
| `ngAfterContentInit()`    | Once after projected content initialized       | Interact with ng-content           |
| `ngAfterContentChecked()` | Every time projected content is checked        | Respond to content updates         |
| `ngAfterViewInit()`       | Once after view (template + child) initialized | Access ViewChild, DOM manipulation |
| `ngAfterViewChecked()`    | Every time view + child views checked          | Respond after view updates         |

---

### 🌟 **Rule of thumb**

✅ For API calls and setup → `ngOnInit`
✅ For reacting to input changes → `ngOnChanges`
✅ For projected content → `ngAfterContentInit`, `ngAfterContentChecked`
✅ For ViewChild / template refs → `ngAfterViewInit`, `ngAfterViewChecked`

---

Excellent — you're now asking **exactly the right question** that interviewers love:

> *“I don't want the textbook definition; I want to know **when should I use ngAfterViewInit over ngOnInit** and why?”*

Let’s cut the fluff and get practical.

---

## 🥇 **Golden Rule**

\*\*It all depends on **"When is the thing you're accessing ready?"**
The hooks are not for *random choice* — they’re tied to **timing of availability** of component elements.

---

## 1️⃣ **ngOnInit vs ngAfterViewInit** — **Biggest confusion**

| If you want to...                                                                              | Use               |
| ---------------------------------------------------------------------------------------------- | ----------------- |
| Work with **@Input() values** (parent → child data)                                            | `ngOnInit`        |
| Access **template elements**, **@ViewChild()**, or **child components** (DOM, child comp refs) | `ngAfterViewInit` |

---

### **Why?**

Because **@Input()** is available **before** the view is ready, but **@ViewChild()** is only available **after** the view renders.

> ❌ **Mistake:** Accessing `@ViewChild` in `ngOnInit` returns `undefined`.
> ✅ **Correct:** Access `@ViewChild` in `ngAfterViewInit`.

---

### **Example**

```ts
@Input() name: string;              // available in ngOnInit
@ViewChild('myDiv') myDiv!: ElementRef; // available in ngAfterViewInit
```

```ts
ngOnInit() {
  console.log(this.name); // ✅ Available
  console.log(this.myDiv); // ❌ undefined
}

ngAfterViewInit() {
  console.log(this.myDiv); // ✅ Available
}
```

---

## 2️⃣ **ngOnInit vs ngAfterContentInit**

| If you want to...                                                               | Use                  |
| ------------------------------------------------------------------------------- | -------------------- |
| Initialize component’s own logic or @Input() data                               | `ngOnInit`           |
| Interact with **content projected** via `<ng-content>` (parent injects content) | `ngAfterContentInit` |

---

### **Why?**

Because **projected content** is inserted **after** ngOnInit.
If you want to read/access or modify **projected content**, use `ngAfterContentInit`.

---

### **Example**

```html
<app-child> <p #projected>Project me</p> </app-child>
```

```ts
@Component({
  template: `<ng-content></ng-content>`
})
export class ChildComponent {
  @ContentChild('projected') projected!: ElementRef;
}
```

| Hook                 | Is `projected` available? |
| -------------------- | ------------------------- |
| `ngOnInit`           | ❌ undefined               |
| `ngAfterContentInit` | ✅ Available               |

---

## ✅ **So practically — here’s how I think during interview**

### Q: *When do you use ngOnInit?*

A: **When I only care about @Input() values, API calls, or initializing local variables — I don't need DOM or child/template refs.**

---

### Q: *When do you use ngAfterViewInit?*

A: **When I need to access template elements (`@ViewChild`) or interact with DOM/child components.**

---

### Q: *When do you use ngAfterContentInit?*

A: **When I need to work with content projected via `<ng-content>` and I want to access that projected content's references.**

---

## 🎯 **Interview Tips (sharp answer)**

> 🔥 **"I use `ngOnInit` when I need @Input data, `ngAfterViewInit` when I need template refs / DOM (`@ViewChild`), and `ngAfterContentInit` when I need projected content (`@ContentChild`). It all depends on *when the thing is available* in component lifecycle."**

---

## 1️⃣ **Q:**

*"If I want to access a DOM element using `@ViewChild`, should I do it in `ngOnInit` or `ngAfterViewInit`? Why?"*

### **A:**

> **`ngAfterViewInit`** — because `@ViewChild` elements are only available **after** Angular has rendered the component's template.
> In `ngOnInit`, they are still `undefined`.

---

## 2️⃣ **Q:**

*"If I have projected content using `<ng-content>`, when should I access it — in `ngOnInit` or `ngAfterContentInit`?"*

### **A:**

> **`ngAfterContentInit`** — because **projected content** (via `<ng-content>`) is inserted **after** component initialization.
> In `ngOnInit`, the content doesn't exist yet.

---

## 3️⃣ **Q:**

*"If my component receives @Input() from a parent, when is it safe to use those values?"*

### **A:**

> **`ngOnInit`** — because Angular sets all @Input() values **before** calling `ngOnInit`.
> I can safely use them there.

---

## 4️⃣ **Q:**

*"If @Input() value changes multiple times after component creation, which hook catches every change?"*

### **A:**

> **`ngOnChanges`** — it runs **every time** an @Input() value changes (even after first render).

---

## 5️⃣ **Q:**

*"What is the difference between `ngOnChanges` and `ngDoCheck`? Both run on change detection, right?"*

### **A:**

> **Yes, but:**

* `ngOnChanges` only fires **when @Input() changes** (and gives you previous/current values).
* `ngDoCheck` fires **on every change detection cycle**, even if nothing actually changed.
  It’s for **custom dirty checking**, e.g., deep object mutations not caught by Angular.

---

## 6️⃣ **Q:**

*"I want to call a method of a child component (using @ViewChild). Where do I do it?"*

### **A:**

> **`ngAfterViewInit`** — because child component instance (via `@ViewChild`) is available only after view is initialized.

---

## 7️⃣ **Q:**

*"I want to modify projected content (via `<ng-content>`). Where should I do it?"*

### **A:**

> **`ngAfterContentInit`** — because that’s when projected content is available.

---

## 8️⃣ **Q:**

*"Can I use `@ViewChild` in `ngAfterContentInit`?"*

### **A:**

> **No — use `ngAfterViewInit` for ViewChild.**
> `ngAfterContentInit` is only for **ContentChild** (projected content).
> **ViewChild = template elements**, **ContentChild = projected content**

---

## 9️⃣ **Q:**

*"What’s the difference between ngAfterViewInit and ngAfterViewChecked?"*

### **A:**

> * `ngAfterViewInit` runs **once** after view initialized.
> * `ngAfterViewChecked` runs **every time** view is checked (including after updates).
>   Use `ngAfterViewChecked` if you want to respond to view **updates**, not just initial load.

---

## 🔟 **Q:**

*"My component has both projected content and template elements. Which hooks do I use to access both?"*

### **A:**

> * For projected content → `ngAfterContentInit` (`@ContentChild`)
> * For template elements → `ngAfterViewInit` (`@ViewChild`)

---

## ✨ **BONUS: Interviewer tricky scenario**

> *"I have a child component with @Input(). I also want to call its method using @ViewChild. What lifecycle hooks do I need?"*

### **A:**

> * Use **`ngOnInit`** to access **@Input()**
> * Use **`ngAfterViewInit`** to access **@ViewChild** and call child method

---

## 🥇 **Ultimate Summary Table** (for interviews)

| Goal                                    | Hook                    | Why?                            |
| --------------------------------------- | ----------------------- | ------------------------------- |
| Use @Input() values                     | `ngOnInit`              | Inputs are ready before this    |
| React to input change                   | `ngOnChanges`           | Fires every time @Input changes |
| Access template refs / ViewChild        | `ngAfterViewInit`       | View rendered                   |
| Access projected content / ContentChild | `ngAfterContentInit`    | Content projected               |
| Respond to view updates                 | `ngAfterViewChecked`    | View updated                    |
| Respond to projected content updates    | `ngAfterContentChecked` | Projected content updated       |
| Custom change detection                 | `ngDoCheck`             | Called every CD cycle           |

---

## ⚡ **How to sound confident? (exact words)**

> "It depends on *when the thing I need becomes available*. If it’s @Input — ngOnInit, if it’s ViewChild — ngAfterViewInit, if it’s projected content — ngAfterContentInit. I choose hooks based on availability, not randomly."

---
