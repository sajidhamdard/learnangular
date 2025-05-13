## 🎯 **Selector priority & execution — Components & Directives**

> "In Angular, only **one component** is allowed per element — if multiple components match, Angular throws an error.
> For **directives**, multiple can apply together — Angular doesn't prioritize selectors like CSS does; it simply applies **all matching directives**.
> The **execution order** of directives is based on **their order in the component's module declaration array**, not selector specificity.
> Structural directives (like `*ngIf`, `*ngFor`) have additional constraints:
> only one structural directive can be on an element because they change the DOM layout — multiple structural directives throw an error.
> For normal components, as soon as Angular finds a matching component selector for an element, that component is applied and no other component can apply to the same element."

---

## ✅ **In short:**

* **Only 1 component per element** → multiple = ❌ Error
* **Multiple directives allowed** → all executed (no CSS-like specificity)
* **Directive execution order** = order of **module declaration array**
* **Only 1 structural directive per element** → multiple = ❌ Error

---

## 💡 **Simple Example**

```ts
@Component({ selector: 'button[type="reset"]' }) export class ResetButton {}
@Directive({ selector: '[logClick]' }) export class LogClickDirective {}
@Directive({ selector: '[highlight]' }) export class HighlightDirective {}
```

```html
<button type="reset" logClick highlight>Reset</button>
```

* **ResetButton** component applied
* **LogClickDirective** and **HighlightDirective** both applied (execution order = module declaration order)

---

This is **exactly** the answer interviewers expect:
✅ Clear
✅ Concise
✅ Technically precise

---

## 📝 **Demo Code**

```ts
import { Directive, ElementRef } from '@angular/core';

@Directive({
  selector: '[firstDirective]'
})
export class FirstDirective {
  constructor(el: ElementRef) {
    console.log('FirstDirective executed');
  }
}

@Directive({
  selector: '[secondDirective]'
})
export class SecondDirective {
  constructor(el: ElementRef) {
    console.log('SecondDirective executed');
  }
}
```

---

## 🛠️ **AppModule (Important — this order matters)**

```ts
@NgModule({
  declarations: [
    AppComponent,
    FirstDirective,
    SecondDirective
  ],
  ...
})
export class AppModule { }
```

---

## ⚡ **HTML Usage**

```html
<button firstDirective secondDirective>Click me</button>
```

---

## 🖥️ **Console Output**

```
FirstDirective executed
SecondDirective executed
```

✅ **Directives executed in module declaration order**

---

## 🎯 **Crisp Interview line**

> "Directive execution order is based on their order in the module's declarations array, not selector specificity or HTML attribute order."

---

If you swap the order like this 👇

```ts
@NgModule({
  declarations: [
    AppComponent,
    SecondDirective,
    FirstDirective
  ],
  ...
})
```

🖥️ Output:

```
SecondDirective executed
FirstDirective executed
```

---

## ⚡ Bottomline

👉 **Module declaration order controls directive execution order**
👉 **HTML attribute order does NOT matter**

---

## 📝 **Custom Structural Directives Demo**

(We will simulate two structural directives)

---

### ✅ **First structural directive**

```ts
import { Directive, TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[firstStructural]'
})
export class FirstStructuralDirective {
  constructor(template: TemplateRef<any>, vcr: ViewContainerRef) {
    console.log('FirstStructuralDirective executed');
    vcr.createEmbeddedView(template);
  }
}
```

---

### ✅ **Second structural directive**

```ts
@Directive({
  selector: '[secondStructural]'
})
export class SecondStructuralDirective {
  constructor(template: TemplateRef<any>, vcr: ViewContainerRef) {
    console.log('SecondStructuralDirective executed');
    vcr.createEmbeddedView(template);
  }
}
```

---

## 🛠️ **AppModule**

```ts
@NgModule({
  declarations: [
    AppComponent,
    FirstStructuralDirective,
    SecondStructuralDirective
  ],
  ...
})
export class AppModule { }
```

---

## ⚡ **HTML Usage (Intentional Error)**

```html
<div *firstStructural *secondStructural>Test</div>
```

---

## 🛑 **Output / Error**

```
NG03041: Can't have multiple structural directives on the same host element.
```

✅ **Angular blocks it**
Because **both structural directives** try to change DOM layout on same element

---

## 🎯 **Correct Usage (Nest them)**

```html
<ng-container *firstStructural>
  <div *secondStructural>Test</div>
</ng-container>
```

Now **both directives execute cleanly**
Console output:

```
FirstStructuralDirective executed
SecondStructuralDirective executed
```

---

## 🎯 **Crisp Interview Answer (Structural Directives)**

> "Only one structural directive can apply to an element.
> If multiple are used on same element, Angular throws an error because structural directives manipulate the DOM layout.
> We can nest them using `<ng-container>` to apply multiple structural behaviors safely."

---


## 🎯 **Crisp Interview Answer (Selector priority & execution — Components & Directives)**

> "In Angular, only **one component** is allowed per element — if multiple components match, Angular throws an error.
> For **directives**, multiple can apply together — Angular doesn't prioritize selectors like CSS does; it simply applies **all matching directives**.
> The **execution order** of directives is based on **their order in the component's module declaration array**, not selector specificity.
> Structural directives (like `*ngIf`, `*ngFor`) have additional constraints:
> only one structural directive can be on an element because they change the DOM layout — multiple structural directives throw an error.
> For normal components, as soon as Angular finds a matching component selector for an element, that component is applied and no other component can apply to the same element."

---

## ✅ **In short:**

* **Only 1 component per element** → multiple = ❌ Error
* **Multiple directives allowed** → all executed (no CSS-like specificity)
* **Directive execution order** = order of **module declaration array**
* **Only 1 structural directive per element** → multiple = ❌ Error

---

## 💡 **Simple Example**

```ts
@Component({ selector: 'button[type="reset"]' }) export class ResetButton {}
@Directive({ selector: '[logClick]' }) export class LogClickDirective {}
@Directive({ selector: '[highlight]' }) export class HighlightDirective {}
```

```html
<button type="reset" logClick highlight>Reset</button>
```

* **ResetButton** component applied
* **LogClickDirective** and **HighlightDirective** both applied (execution order = module declaration order)

---

This is **exactly** the answer interviewers expect:
✅ Clear
✅ Concise
✅ Technically precise

---
