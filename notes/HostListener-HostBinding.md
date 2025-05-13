### **`@HostListener` and `@HostBinding` in Angular**

Both **`@HostListener`** and **`@HostBinding`** are decorators in Angular that allow interaction with the **host element** (the element to which the directive or component is attached). These decorators are primarily used to handle events and manipulate properties of the host element.

---

## 🚀 **`@HostListener`**

### **Definition:**

* **`@HostListener`** listens for events on the host element and executes methods in the component or directive in response to those events.

### **Use Case:**

* It’s often used for handling events like `click`, `mouseover`, `resize`, etc., on the host element.

### **Syntax:**

```ts
@HostListener('eventName', ['argument'])
```

### **Example:**

In this example, we use `@HostListener` to listen for a `click` event on the host element and execute a method:

#### **Directive Example**

```ts
import { Directive, HostListener } from '@angular/core';

@Directive({
  selector: '[appClickTracker]'
})
export class ClickTrackerDirective {
  
  @HostListener('click', ['$event']) 
  onClick(event: MouseEvent) {
    console.log('Element clicked!', event);
  }
}
```

### **What Happens:**

* When the user clicks on an element with the `appClickTracker` directive, the `onClick` method is triggered, logging the event to the console.

---

### **`@HostBinding`**

### **Definition:**

* **`@HostBinding`** allows you to bind properties or attributes of the host element to the component or directive’s properties.

### **Use Case:**

* It’s used when you need to change or bind a host element’s property (like `class`, `style`, or attributes) based on the state of the component or directive.

### **Syntax:**

```ts
@HostBinding('property') propertyName: string;
```

### **Example:**

In this example, we use `@HostBinding` to set a `class` on the host element based on a property in the directive:

#### **Directive Example**

```ts
import { Directive, HostBinding } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  
  @HostBinding('class.highlighted') isHighlighted = false;

  // Toggle highlight on mouse enter and mouse leave
  @HostListener('mouseenter') onMouseEnter() {
    this.isHighlighted = true;
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.isHighlighted = false;
  }
}
```

### **What Happens:**

* When the user hovers over the element, the `highlighted` class is applied to the host element, and when the user moves the mouse away, the class is removed.

---

## 📊 **Comparison**

| Feature        | `@HostListener`                                                                                | `@HostBinding`                                                                                             |
| -------------- | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Purpose**    | Listens to events on the host element (like `click`, `mouseover`, etc.) and triggers a method. | Binds a property or attribute of the host element to the directive/component's property.                   |
| **Use case**   | Handling DOM events (e.g., mouse click, mouseover) on the host element.                        | Binding a property like `class`, `style`, or attributes of the host element to the component’s properties. |
| **Syntax**     | `@HostListener('event', [args])`                                                               | `@HostBinding('property') propertyName`                                                                    |
| **Common Use** | Event handling (e.g., listening for `click` events).                                           | Binding `class`, `style`, or attributes to reflect component state.                                        |

---

### **Example with Both `@HostListener` and `@HostBinding`**

Here’s a **combined example** where both `@HostListener` and `@HostBinding` are used to create a directive that changes the background color of an element on hover:

#### **Directive:**

```ts
import { Directive, HostBinding, HostListener } from '@angular/core';

@Directive({
  selector: '[appHover]'
})
export class HoverDirective {
  
  @HostBinding('style.backgroundColor') backgroundColor: string;

  @HostListener('mouseenter') onMouseEnter() {
    this.backgroundColor = 'yellow';  // Change background color on hover
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.backgroundColor = 'transparent';  // Reset background color on mouse leave
  }
}
```

### **What Happens:**

* When the user hovers over the element, the background color changes to **yellow**, and when the mouse leaves, it resets to **transparent**.

---

## 💡 **Use Cases and Tips:**

* **`@HostListener`**: Useful for handling DOM events that happen on the host element or even the global events (e.g., `window` resize).
* **`@HostBinding`**: Useful for binding properties like `class`, `style`, or other element attributes (e.g., `disabled`, `readonly`) to properties within your component or directive.
