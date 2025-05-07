# 🚀 **What is `::ng-deep` in Angular?**

> "`::ng-deep` is a **special pseudo-class** in Angular that allows your component's styles to **'pierce' through Angular’s view encapsulation** and apply styles **to child components** deeply — even if those components have their own encapsulated styles."

---

# ⚙️ **First, understand View Encapsulation**

By default, Angular **encapsulates component styles**.
This means:

* Styles written in `my-component.component.css` apply **only inside `my-component` template**.
* Angular does this by adding **unique attributes** to elements and CSS rules.

Example 👇

### Component:

```ts
@Component({
  selector: 'my-comp',
  template: `<p>Hello</p>`,
  styleUrls: ['./my-comp.component.css']
})
export class MyComp { }
```

### CSS:

```css
p {
  color: red;
}
```

Angular compiles it like this:

```html
<p _ngcontent-xyz>Hello</p>
```

And styles:

```css
p[_ngcontent-xyz] {
  color: red;
}
```

✅ Styles **scoped to this component only**
✅ They **don’t leak** to other components

---

# ❓ **But what if…**

You want to **style a child component** from the parent’s CSS?

Example:
`parent-comp` contains `child-comp` and you want to style an element inside `child-comp`

### **Normal CSS (FAILS)**

```css
child-comp p {
  color: red;
}
```

❌ This won’t work because **encapsulation blocks it**

---

# ✅ **This is where `::ng-deep` helps**

It **disables** view encapsulation **for that CSS rule** — making it act like **global CSS**

### Example

```css
::ng-deep child-comp p {
  color: red;
}
```

✅ This will style `<p>` inside `child-comp`, even though it has encapsulation

---

# 🧐 **How does Angular do this internally?**

It **removes the scoped attribute** from your rule

Your style turns from:

```css
::ng-deep child-comp p {
  color: red;
}
```

To:

```css
child-comp p {
  color: red;
}
```

💥 **Global style applied** — encapsulation ignored

---

# ⚠️ **Why is `::ng-deep` discouraged?**

* It **breaks encapsulation** — defeats the purpose of component-isolated styles
* It creates **hard-to-maintain, fragile CSS**
* **Angular plans to deprecate/remove it** in future

💡 **Angular team recommends:**
✅ Prefer **Input properties** + **ngClass/ngStyle**
✅ If you really need global styles → put them in **global styles.css** (not via `::ng-deep`)

---

# ✅ **Real-life Example**

Say you use a **3rd party component** (e.g., Angular Material) and want to override its style

```html
<mat-button>Click</mat-button>
```

```css
::ng-deep mat-button {
  background: green;
}
```

✅ Works even if Material component has encapsulation

---

# 🎯 **Crisp Interview Answer (say this)**

> "`::ng-deep` is an Angular pseudo-class that lets component styles apply **deeply** into child components, bypassing view encapsulation.
> It disables scoping for that rule and acts globally.
> It’s discouraged because it breaks encapsulation and is hard to maintain — Angular suggests avoiding it in favor of global styles or Input-based customizations."

---

# 🥇 **Pro Tip to say (to impress interviewer)**

> "I avoid `::ng-deep` whenever possible.
> Instead, I try **customizing components via Input properties**, using **CSS variables**, or applying **global styles** in `styles.css`.
> I only fall back to `::ng-deep` when absolutely necessary — like styling 3rd party libraries that don’t expose config APIs."

---

# ❓ **Bonus — Alternatives to ::ng-deep**

| Use case              | Alternative                          |
| --------------------- | ------------------------------------ |
| Style 3rd party lib   | Global `styles.css` override         |
| Style child component | Use **Input** + `ngClass`, `ngStyle` |
| Reusable styling      | Use **CSS variables**                |

---

# ✅ **Summary (easy to remember)**

\| `::ng-deep` does what? | **Pierces encapsulation** |
\| Why discouraged? | **Breaks component isolation** |
\| Better alternatives? | **Input props, global styles.css, CSS vars** |
\| Will it be removed? | **Yes (in future)** — deprecated |
