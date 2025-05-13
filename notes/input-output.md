## 🚀 **`@Input()` in Angular**

### **Definition**

* **`@Input()`** is used to pass data **from parent to child** components.
* Marks a property in the **child component** that can be bound to a value from the **parent component**.

### **Syntax**

```ts
@Input() propertyName: Type;
```

### **Example**

#### **Child Component**

```ts
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<p>Received Value: {{ value }}</p>`
})
export class ChildComponent {
  @Input() value: string = '';  // Receiving value from Parent
}
```

#### **Parent Component**

```html
<app-child [value]="parentValue"></app-child>
```

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  template: `<app-child [value]="parentValue"></app-child>`
})
export class ParentComponent {
  parentValue = 'Hello from Parent';
}
```

---

## 🚀 **`@Output()` in Angular**

### **Definition**

* **`@Output()`** is used to **send data from the child to the parent** component.
* It uses **EventEmitter** to emit events from the **child** to the **parent**.

### **Syntax**

```ts
@Output() eventName = new EventEmitter<Type>();
```

### **Example**

#### **Child Component**

```ts
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<button (click)="sendData()">Send Data to Parent</button>`
})
export class ChildComponent {
  @Output() notify: EventEmitter<string> = new EventEmitter();

  sendData() {
    this.notify.emit('Hello from Child');
  }
}
```

#### **Parent Component**

```html
<app-child (notify)="receiveData($event)"></app-child>
```

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  template: `<app-child (notify)="receiveData($event)"></app-child>`
})
export class ParentComponent {
  receiveData(message: string) {
    console.log(message);  // Output: Hello from Child
  }
}
```

---

## 📊 **Quick Comparison:**

| Feature   | `@Input()`                              | `@Output()`                                       |
| --------- | --------------------------------------- | ------------------------------------------------- |
| Purpose   | Pass data from **Parent** to **Child**  | Send data or events from **Child** to **Parent**  |
| Syntax    | `@Input() propertyName: Type;`          | `@Output() eventName = new EventEmitter<Type>();` |
| Use case  | Bind data to child component's property | Emit events or send data to parent component      |
| Data flow | Parent → Child                          | Child → Parent                                    |

---

### **Key Points to Remember:**

* **`@Input()`**: You bind a property in the **parent** template to the **child** component’s input.
* **`@Output()`**: You emit events from the **child** to the **parent** component, allowing the parent to listen to those events.
