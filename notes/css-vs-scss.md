## 🌿 **CSS (Cascading Style Sheets)**

Plain, traditional styling language. Basic styling rules.

**Example CSS**

```css
.button {
  color: blue;
}

.button:hover {
  color: darkblue;
}
```

* No variables
* No nesting
* No functions/mixins
* Very basic — but universally supported

---

## 🌿 **SCSS (Sassy CSS — a syntax of Sass)**

**Superset of CSS** → Everything in CSS is valid SCSS
**+ More powerful features** (developer-friendly)

**Example SCSS**

```scss
$primary-color: blue;

.button {
  color: $primary-color;
  
  &:hover {
    color: darken($primary-color, 10%);
  }
}
```

* ✅ **Variables** (`$primary-color`)
* ✅ **Nesting** (`.button { &:hover { } }`)
* ✅ **Mixins and functions**
* ✅ **Reusable code**, easier to maintain

---

### 📊 **Quick Comparison Table**

| Feature          | CSS                      | SCSS (Sass) |
| ---------------- | ------------------------ | ----------- |
| Variables        | ❌ No                     | ✅ Yes       |
| Nesting          | ❌ No                     | ✅ Yes       |
| Mixins/functions | ❌ No                     | ✅ Yes       |
| Code reuse       | ❌ Hard                   | ✅ Easy      |
| Maintenance      | 😓 Tough in big projects | 😌 Easier   |

---

### 🛠️ **Real-World Advice**

* **Small project → CSS is fine**
* **Big project or Angular/React/Vue → SCSS is better** (cleaner, DRY code)

---

👉 **Summary**
SCSS = CSS + **developer-friendly features** to write **cleaner, scalable styles**.
That's why in Angular projects `--style=scss` is very common.
