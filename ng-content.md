### **`ng-content` in Angular**

**`ng-content`** is a directive in Angular that allows you to **project content** from the parent component into the child component’s template. It’s commonly referred to as **content projection**. This helps in creating reusable components where content is provided by the parent component but rendered in the child component’s template.

---

## 🚀 **Key Points:**

1. **Content Projection**: `ng-content` is used to project the content passed from the parent component into a child component’s template.
2. **Component Reusability**: It allows you to create flexible, reusable components where the parent can pass custom content to be displayed by the child.
3. **Similar to Slots in Web Components**: The functionality of `ng-content` is similar to slots in Web Components.

---

## 🛠️ **Basic Example**

### **Child Component (with `ng-content`)**

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
    <div class="child">
      <h3>Child Component</h3>
      <ng-content></ng-content>  <!-- Content will be projected here -->
    </div>
  `
})
export class ChildComponent {}
```

### **Parent Component**

```html
<app-child>
  <p>This content is projected into the child component!</p>
</app-child>
```

### **What Happens?**

* The content inside the `<app-child>` tag (in the parent component) will be projected into the `ng-content` tag in the child component’s template.
* **Output**:

  ```
  Child Component
  This content is projected into the child component!
  ```

---

## 🚀 **`ng-content` with Multiple Slots**

You can use **named slots** (using `select` attribute) to project different parts of content into different places.

### **Child Component (Multiple Slots)**

```ts
@Component({
  selector: 'app-child',
  template: `
    <div class="child">
      <h3>Header Content:</h3>
      <ng-content select="[header]"></ng-content> <!-- Header slot -->

      <h3>Body Content:</h3>
      <ng-content select="[body]"></ng-content>   <!-- Body slot -->
    </div>
  `
})
export class ChildComponent {}
```

### **Parent Component (Projecting Named Content)**

```html
<app-child>
  <div header>
    <h1>Welcome Header</h1>
  </div>
  <div body>
    <p>This is the body content.</p>
  </div>
</app-child>
```

### **What Happens?**

* The **header** content is projected into the first `<ng-content>` slot.
* The **body** content is projected into the second `<ng-content>` slot.

---

## ⚡ **Benefits of `ng-content`**

1. **Reusable Components**: You can create highly flexible and reusable components where the parent provides its own content.
2. **Separation of Concerns**: The child component doesn’t need to know about the content passed by the parent. It just uses `ng-content` to render it.
3. **Customizable Components**: Makes components customizable without altering their structure or functionality.

---

### **Example with ng-content in a Card Component**

#### **Card Component (with `ng-content`)**

```ts
@Component({
  selector: 'app-card',
  template: `
    <div class="card">
      <div class="card-header">
        <ng-content select=".header"></ng-content> <!-- Projected Header -->
      </div>
      <div class="card-body">
        <ng-content></ng-content> <!-- Default Content -->
      </div>
    </div>
  `,
  styles: [`
    .card { border: 1px solid #ccc; padding: 10px; }
    .card-header { font-weight: bold; }
  `]
})
export class CardComponent {}
```

#### **Parent Component**

```html
<app-card>
  <div class="header">
    Card Title
  </div>
  <div class="content">
    This is the body of the card.
  </div>
</app-card>
```

### **What Happens?**

* The **header** content goes into the first `ng-content` tag (`select=".header"`).
* The **body** content goes into the default `ng-content` tag.

---

## 📊 **Summary**

| Feature         | Description                                                            |
| --------------- | ---------------------------------------------------------------------- |
| **Purpose**     | Allows projecting content from the parent to the child component       |
| **Syntax**      | `<ng-content></ng-content>`                                            |
| **Named Slots** | `<ng-content select="[header]"></ng-content>`                          |
| **Use Case**    | When you want a parent to inject custom content into a child component |
