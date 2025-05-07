# 🚀 **What are Model Inputs in Angular 16?**

✅ **Model Inputs** = **Bidirectional Inputs** (Input + Output together)
✅ Introduced in **Angular 16**
✅ Let you **sync values between parent & child** **automatically** — **without** manually writing both `@Input()` and `@Output()`
✅ It works **with Angular’s Signals**

---

# 🧐 **But wait… What is the problem with old @Input/@Output?**

Before Angular 16, if we wanted **2-way binding** between parent and child component:

We had to do **both**:

* **@Input()** property to accept value from parent
* **@Output()** EventEmitter to send updated value back to parent

### Example (old way)

```ts
@Input() value: number = 0;
@Output() valueChange = new EventEmitter<number>();

updateValue(newVal: number) {
  this.value = newVal;
  this.valueChange.emit(newVal);
}
```

### In Parent

```html
<my-child [(value)]="parentValue"></my-child>
```

✅ **\[(value)]** = **banana-in-a-box** syntax = 2-way binding

---

# ✨ **Angular 16 Model Inputs = Cleaner Alternative**

```ts
value = model<number>(0);
```

✅ **`model()`** creates **Input + Output** together
✅ **No need** to manually create EventEmitter
✅ **Simpler** and **Signal-based**

---

# ✅ **Full Example — Model Input**

---

## **Child Component**

```ts
import { Component, model } from '@angular/core';

@Component({
  selector: 'my-child',
  standalone: true,
  template: `
    <p>Child value is {{ value() }}</p>
    <button (click)="increment()">Increment</button>
  `
})
export class MyChild {
  value = model(0);  // 👈 Model Input with default value

  increment() {
    this.value.set(this.value() + 1);
  }
}
```

---

## **Parent Component Template**

```html
<my-child [(value)]="parentValue"></my-child>

<p>Parent value is {{ parentValue }}</p>
```

---

✅ **What happens?**

* Parent passes value via **`[(value)]`**
* Child reads value with **`value()`**
* Child updates value with **`value.set()`**
* **Parent automatically gets updated** — thanks to **model()**

---

# ⚙️ **Under the hood — model() is shorthand for:**

```ts
@Input() value: number;
@Output() valueChange = new EventEmitter<number>();
```

But done **automatically and Signal-backed**

---

# 🎯 **Crisp Interview Answer (say this)**

> "Angular 16 introduced **Model Inputs** via the `model()` function, which creates **bidirectional input** (Input + Output) as a **Signal**.
> It allows 2-way binding using **`[(prop)]`** syntax without needing to manually define `@Input()` and `@Output()`.
> We read the value using **`value()`** and update it using **`value.set()`** — the parent automatically syncs."

---

# 📊 **Old vs New Summary Table**

| Feature       | Old way                  | Angular 16 Model Input |
| ------------- | ------------------------ | ---------------------- |
| Input         | `@Input()`               | `value = model()`      |
| Output        | `@Output()` EventEmitter | **Auto-created**       |
| Read value    | `value`                  | `value()`              |
| Update value  | emit() manually          | `value.set()`          |
| 2-way binding | `[(prop)]`               | `[(prop)]`             |

---

# ⚡ **Usage of Model Inputs — When to use?**

✅ When child **both reads and updates** the input
✅ To simplify **Forms-like components** (example: Input fields, sliders)

---

# ❌ **When NOT to use Model Inputs?**

❌ If child only **reads** value but does not update → just use `input()`
❌ If **one-way binding** is enough

---

```typescript
@Component({ /* ... */})
export class CustomSlider {
  // Define a model input named "value".
  value = model(0);
  increment() {
    // Update the model input with a new value, propagating the value to any bindings.
    this.value.update(oldValue => oldValue + 10);
  }
}
@Component({
  /* ... */
  // Using the two-way binding syntax means that any changes to the slider's
  // value automatically propagate back to the `volume` signal.
  // Note that this binding uses the signal *instance*, not the signal value.
  template: `<custom-slider [(value)]="volume" />`,
})
export class MediaControls {
  // Create a writable signal for the `volume` local state.
  volume = signal(0);
}
```

# ✅ **Final Summary (easy to remember)**

> **`model()`** = `@Input()` + `@Output()` + Signals
> 2-way binding with **`[(prop)]`**
> Read with `value()`
> Write with `value.set()`
