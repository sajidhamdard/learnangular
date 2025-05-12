## ✨ What is a **Mixin**?

A **mixin** is like a **reusable block of CSS rules**.
You write it **once**, and then **reuse it** wherever you need it (like a function in code).

---

### 📦 **Example of a mixin**

```scss
@mixin button-styles {
  padding: 10px 20px;
  border-radius: 5px;
  background-color: blue;
  color: white;
}
```

You can now "apply" (`@include`) this mixin in multiple places:

```scss
.primary-button {
  @include button-styles;
}

.secondary-button {
  @include button-styles;
  background-color: gray;  // you can override as needed
}
```

👆 **Benefit:** DRY code (Don’t Repeat Yourself). Easy to update styles everywhere.

---

## ✨ What is a **SCSS Function**?

A **function** is like a **mixin that returns a value**.
Usually returns **a single value** like a color, size, or string.

---

### 🧩 **Example of a function**

```scss
@function calculate-spacing($factor) {
  @return $factor * 8px;
}
```

Use it like this:

```scss
.card {
  padding: calculate-spacing(2);  // returns 16px (2 * 8px)
}

.container {
  margin-bottom: calculate-spacing(3);  // returns 24px
}
```

👆 **Benefit:** Easy **dynamic values** and **consistency**.

---

## 📊 **Mixin vs Function quick comparison**

| Feature | Mixin (block of styles)  | Function (returns value)    |
| ------- | ------------------------ | --------------------------- |
| Purpose | Reuse multiple CSS rules | Return 1 value (color/size) |
| Usage   | `@include mixin-name`    | `property: function-name()` |
| Returns | Block of CSS             | Single value                |

---

## 🛠️ **Real Angular Component Example** (SCSS)

```scss
// styles.scss (global)
@mixin card-style {
  border: 1px solid #ddd;
  border-radius: 10px;
  padding: 20px;
}

@function spacing($n) {
  @return $n * 8px;
}

// app.component.scss (component)
.card {
  @include card-style;
  margin-bottom: spacing(3);
}
```

---

## ✅ **Summary**

* **Mixin** = reusable styles block (`@mixin ... @include`)
* **Function** = reusable value generator (`@function ...`)
  **Both make SCSS much more powerful than CSS.**
